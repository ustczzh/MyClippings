# Sphere packing in AutoCAD: filling an arbitrary solid using .NET - Through the Interface
[Sphere packing in AutoCAD: filling an arbitrary solid using .NET - Through the Interface](https://webcache.googleusercontent.com/search?q=cache:4F_p8WwlQ38J:https://www.keanw.com/2012/02/sphere-packing-in-autocad-filling-an-arbitrary-solid-using-net.html&cd=1&hl=zh-CN&ct=clnk&gl=jp) 

 As we’re nearing the end of this series, it seems a good time to do a quick recap of where we’ve been with the posts leading up to this point. Here goes…

Until now, the focus on the series has largely been around relatively pure mathematical approaches to fill simple (in our case circular or spherical) areas/volumes: we’ve so far avoided having to adapt the filling/packing algorithm depending on the form selected.

Today’s post changes that: it uses a very different approach to fill a selected Solid3d with spheres. A bit like the “hyperbolic tessellation” patterns we saw, early on, we want to create smaller spheres at the surface of the solid, with the radius gradually increasing, layer upon layer, as we get towards the centre.

Something like this sketch, created by my colleague Francesco Tonioni (as mentioned in the previous post) as we brainstormed optimal filling strategies (from a strength – rather than material – perspective):

There are a host of devils in the detail, of course: to reduce the chances of any of the outer layer of spheres intersecting the surface of the solid, we need to know the curvature of the surface at a particular point (and while we get that right for relatively uniform surfaces, I’ll bet it’ll fail for highly irregular ones), which allows us to adjust the depth of the sphere at that point to make sure it fits.

In fact, here are the back-of-the-envelope sketches I drew to work out some of the trigonometry needed to find out how adjacent rings of spheres should be positioned:

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-15%2014-40-18/52fd3811-788c-40cf-a9d7-28585e7bff3f.png?raw=true)
](http://through-the-interface.typepad.com/.a/6a00d83452464869e2016301d59fa7970d-pi)These calculations are far from foolproof, so we iterate adjusting the values to see if we can get a better fit. Once again, this probably works well with solids with a uniform shape along the direction of our slices (the normal to the section vector) – so rotations, etc., can work very well, but I’d expect less uniform shapes to cause problems.

Here’s the C# code, also contained in [the project with the various code files for this series](http://through-the-interface.typepad.com/files/CircleAndSpherePacking.zip). I ended up using C# for this post, as it had a lot to do with accessing and managing AutoCAD geometry: the times I’ve used F# of late have been when I’ve had more “pure” calculations to perform. There’s no strict need for this division, but I do like having a split between core algorithms and AutoCAD-specific code. And while it might seem a bit artificial to use a language barrier to enforce it, it does feel like it plays to the strengths of the respective languages.

using Autodesk.AutoCAD.ApplicationServices;

using Autodesk.AutoCAD.DatabaseServices;

using Autodesk.AutoCAD.EditorInput;

using Autodesk.AutoCAD.Geometry;

using Autodesk.AutoCAD.Runtime;

using System;

namespace SpherePacking

{

 public  class  PackingCommands

  {

    \[CommandMethod("PACKSPHERES")\]

 static  public  void PackSolidWithSpheres()

    {

 Document doc =

 Application.DocumentManager.MdiActiveDocument;

 Database db = doc.Database;

 Editor ed = doc.Editor;

 // We need a solid

 PromptEntityOptions peo =

 new  PromptEntityOptions("\\nSelect solid to fill");

      peo.SetRejectMessage("\\nMust be a Solid3d.");

      peo.AddAllowedClass(typeof(Solid3d), false);

 PromptEntityResult per = ed.GetEntity(peo);

 if (per.Status != PromptStatus.OK)

 return;

 ObjectId solId = per.ObjectId;

 // And an initial section plane

 // (the view direction is used to define the third point)

 PromptPointOptions ppo =

 new  PromptPointOptions(

 "\\nSelect first point for initial section plane"

        );

 PromptPointResult ppr = ed.GetPoint(ppo);

 if (ppr.Status != PromptStatus.OK)

 return;

 Point3d first = ppr.Value;

      ppo.Message =

 "\\nSelect second point for initial section plane";

      ppo.BasePoint = first;

ppo.UseBasePoint = true;

ppo.UseDashedLine = true;

      ppr = ed.GetPoint(ppo);

 if (ppr.Status != PromptStatus.OK)

 return;

 Point3d second = ppr.Value;

 // We either generate spheres or subtract them from the

 // selected solid

 PromptKeywordOptions pko =

 new  PromptKeywordOptions(

 "\\nSubtract spheres from original?"

        );

pko.AllowNone = true;

      pko.Keywords.Add("Yes");

      pko.Keywords.Add("No");

pko.Keywords.Default = "No";

 PromptResult pkr = ed.GetKeywords(pko);

 if (pkr.Status != PromptStatus.OK)

 return;

 bool subtract = (pkr.StringResult == "Yes");

 // We need a transaction

 Transaction tr =

        db.TransactionManager.StartTransaction();

 using (tr)

      {

 // And we also need the modelspace open for write

 BlockTable bt =

          (BlockTable)tr.GetObject(

            db.BlockTableId,

 OpenMode.ForRead,

 false

          );

 BlockTableRecord btr =

          (BlockTableRecord)tr.GetObject(

            bt\[BlockTableRecord.ModelSpace\],

 OpenMode.ForWrite,

 false

          );

 // Get the view direction

 ViewportTableRecord vtr =

          (ViewportTableRecord)tr.GetObject(

ed.ActiveViewportId, OpenMode.ForRead

          );

 Vector3d viewDir = vtr.ViewDirection;

 // Open the selected solid for write

 Solid3d outer =

tr.GetObject(solId, OpenMode.ForWrite) as  Solid3d;

 bool cancelled = false;

 if (outer != null)

        {

 try

          {

 // Create our sphere packer and use it to fill the

 // selected solid with spheres

 SpherePacker sp =

 new  SpherePacker(

                db, tr, btr, outer, first, second, viewDir, subtract

              );

            sp.FillWithSpheres();

          }

 catch

          {

cancelled = true;

          }

        }

 if (!cancelled)

          tr.Commit();

      }

    }

  }

 public  class  SpherePacker

  {

 Database _db = null;

 Transaction _tr = null;

 BlockTableRecord _btr = null;

 Solid3d _parent = null, _sol = null;

 Point3d \_first, \_second;

 Vector3d _viewDir;

 bool _sub = false;

 ProgressMeter _pm = null;

 // Constructor

 public SpherePacker(

 Database db, Transaction tr, BlockTableRecord btr,

 Solid3d parent, Point3d first, Point3d second,

 Vector3d viewDir, bool subtract

    )

    {

 // Set the member data from the arguments

      _db = db;

      _tr = tr;

      _btr = btr;

      _parent = parent;

      _sub = subtract;

      _first = first;

      _second = second;

      _viewDir = viewDir;

 // Make a copy of our outer solid, which we will

 // repeatedly shrink for each layer of spheres

_sol = new  Solid3d();

      \_sol.CopyFrom(\_parent);

 // Add it to the drawing

      AddToDrawing(_sol);

 // Make sure we have target layers in place for each layer

 // of spheres

      CreateLayers();

    }

 // Get the name for a layer based on its integer level

 // (a very simple function, but nice to have it centralised

 // in case we want to do things differently)

 private  string GetLayer(int layer)

    {

 return layer.ToString();

    }

 // Create a set of layers for our spheres

 internal  void CreateLayers()

    {

 // Open the layer table for write

 LayerTable lt =

        (LayerTable)_tr.GetObject(

_db.LayerTableId, OpenMode.ForWrite

        );

 // Loop through, creating 10 layers

 for (short i = 1; i <= 10; i++)

      {

 string name = GetLayer(i);

 if (!lt.Has(name))

        {

 LayerTableRecord ltr = new  LayerTableRecord();

          ltr.Color =

            Autodesk.AutoCAD.Colors.Color.FromColorIndex(

              Autodesk.AutoCAD.Colors.ColorMethod.ByAci, i

            );

          ltr.Name = name;

          lt.Add(ltr);

_tr.AddNewlyCreatedDBObject(ltr, true);

        }

      }

    }

 // Our main method

 internal  void FillWithSpheres()

    {

 // Create and start a progress meter (the limit is slightly

 // arbitrary - it works well for spheres, but is likely to

 // work less well for other forms)

_pm = new  ProgressMeter();

      _pm.SetLimit(180);

      _pm.Start(

        _sub ?

 "Subtracting spheres from selected solid" :

 "Filling solid with spheres"

      );

 try

      {

 // Start with layer 1 and a radius of 0.2 (which should

 // get overwritten immediately of the solid has valid

 // extents)

 int layer = 1;

 double radius = 0.2;

 if (_sol.Bounds.HasValue)

        {

 // Set the radius relative to the solid's extents

 Extents3d ext = _sol.Bounds.Value;

 Vector3d vec = ext.MaxPoint - ext.MinPoint;

 double mag = vec.Length;

          radius = mag / 150;

        }

 // Shrink the body by half the radius of the first layer,

 // just to give us a little gap between the spheres

 // and the outer solid

        _sol.OffsetBody(-0.5 * radius);

 // Create layers of spheres, starting with the initial

 // radius value and multiplying by a factor for each

 // new layer

 for (double r = radius; r < radius * 20; r *= 1.3)

        {

          GenerateSectionsAcrossSolid(r, layer++);

 // Shrink the body by the diameter of the layer's spheres

 try

          {

            _sol.OffsetBody(-2 * r);

          }

 catch

          {

 break;

          }

 // We won't attempt to fill volumes that aren't large

 // enough (we check the radius against the volume)

 if (_sol.MassProperties.Volume < 350 * r)

 break;

        }

 // Set our shrinking solid to be the next level down

 // (via its layer)

        _sol.Layer = GetLayer(layer);

 // If subtracting, do so for this remaining solid

 if (_sub)

        {

          _parent.BooleanOperation(

 BooleanOperationType.BoolSubtract, _sol

          );

        }

      }

 catch

      {

 // Rethrow any exception, letting is get caught in our

 // command and dealt with there

 throw;

      }

 finally

      {

 // Make sure we stop our progress meter

        _pm.Stop();

      }

    }

 // Generate a layer of spheres across the complete solid

 internal  void GenerateSectionsAcrossSolid(

 double radius, int layer

    )

    {

 // Use a coordinate system object to work out the direction

 // vector for us to move the section plane

 // (should be perpendicular to the section plane, basically)

 CoordinateSystem3d cs =

 new  CoordinateSystem3d(

 Point3d.Origin, \_viewDir, \_second - _first

        );

 Vector3d unitVec = cs.Zaxis / cs.Zaxis.Length;

 // Local variables that get adjusted by each call to our

 // GenerateSpheresAlongSection method

 Point3d pt1 = _first,

              pt2 = _second,

prevCen = Point3d.Origin;

 Curve prevCur = null;

 double prevDep = 0.0;

 // Create a ring of spheres around our initial section curve

      GenerateSpheresAlongSection(

 ref pt1, ref pt2, ref prevCen, ref prevCur, ref prevDep,

        unitVec, radius, 0, layer

      );

 if (prevCur != null)

        prevCur.Dispose();

prevCur = null;

 bool done = false;

 Point3d initCen = prevCen;

 double initDep = prevDep;

 // Now go all the way across in one direction

 do

      {

        done =

          !GenerateSpheresAlongSection(

 ref pt1, ref pt2, ref prevCen, ref prevCur, ref prevDep,

            unitVec, radius, 1, layer

          );

      }

 while (!done);

 if (prevCur != null)

        prevCur.Dispose();

prevCur = null;

 // And then go all the way in the other direction

      pt1 = _first;

      pt2 = _second;

      prevCen = initCen;

      prevDep = initDep;

 do

      {

        done =

          !GenerateSpheresAlongSection(

 ref pt1, ref pt2, ref prevCen, ref prevCur, ref prevDep,

            unitVec, radius, -1, layer

          );

      }

 while (!done);

 if (prevCur != null)

        prevCur.Dispose();

    }

 // Generate a ring of spheres along a single section

 private  bool GenerateSpheresAlongSection(

 ref  Point3d pt1, ref  Point3d pt2, ref  Point3d prevCen,

 ref  Curve prevCur, ref  double prevDep,

 Vector3d unitVec, double radius,

 int direction, int layer

    )

    {

 // Tick our progress meter and check for user break

      _pm.MeterProgress();

      System.Windows.Forms.Application.DoEvents();

 if (HostApplicationServices.Current.UserBreak())

      {

 if (prevCur != null)

          prevCur.Dispose();

 throw  new System.Exception("User break");

      }

 // Take local copies of the points defining the section

 Point3d first = pt1, second = pt2;

 // Approximate the distance from the outer curve for the first

 // pass. Set some limits for our target distance from the last

 // curve

 double bestDist = radius,

             bestDepth = radius,

             upperDistLimit = 2.1 * radius,

             lowerDistLimit = 2 * radius;

 // We'll count the number of iterations

 int tries = 0;

 bool res = true;

 Curve cur = null;

 do

      {

 // If we're creating ring in a direction along the surface,

 // we need to transform the points defining the section

 if (direction != 0)

        {

 Matrix3d disp =

 Matrix3d.Displacement(direction * unitVec * bestDist);

          pt1 = pt1.TransformBy(disp);

          pt2 = pt2.TransformBy(disp);

        }

 // Get the curve from this section

        cur = CurveFromSection(pt1, pt2);

 if (cur == null)

        {

          FillRemainingCurve(prevCur, radius, prevDep, layer);

res = false;

 break;

        }

 // Get the distance between the proposed position of the

 // first sphere and the first sphere of the previous ring

 Point3d cen = GetFirstCenter(cur, bestDepth);

 double center2center = (cen - prevCen).Length;

 // If this is either the first time run (i.e. no previous

 // sphere) or the spheres are at an acceptable distance,

 // skip the trigonometric calculations

 if (

          prevCen.DistanceTo(Point3d.Origin) >

 Tolerance.Global.EqualPoint &&

          (center2center < lowerDistLimit ||

            center2center > upperDistLimit)

        )

        {

 // Only do the calculations if this is the first run,

 // otherwise we will just adjust the values

 if (tries == 0)

          {

 // Calculate the distance to the next section

 // Get slope at the current point on the surface

 // (ideal would be to get the tangent at the mid-

 //  point of the spheres, but we have to approximate)

 Vector3d slope =

              GetSlopeAtPoint(pt1, pt2, unitVec, radius);

 if (slope.Length < Tolerance.Global.EqualPoint)

            {

res = false;

 break;

            }

 // Get the "down" vector based on the curve direction

 Vector3d down =

              cur.GetPointAtParameter(cur.EndParam / 2) -

              cur.GetPointAtParameter(cur.StartParam);

 // Get the viewing plane

 using (Plane p = new  Plane(Point3d.Origin, _viewDir))

            {

 // Project the various vectors onto the plane

 Vector2d unit2d = unitVec.Convert2d(p),

                        slope2d = slope.Convert2d(p),

                        down2d = down.Convert2d(p);

 // Get the angle of the slope to the offset direction

 double theta = unit2d.GetAngleTo(slope2d);

 // Get the angle of the slope to the down direction

 double theta2 = down2d.GetAngleTo(slope2d);

 // Calculate the distance and the depth

bestDist = Math.Abs(2 * radius * Math.Cos(theta));

bestDepth = Math.Abs(radius / Math.Sin(theta2));

            }

          }

 else

          {

 // Incrementally adjust the distance upwards or

 // downwards, depending on which side of ideal we are

 if (center2center < lowerDistLimit)

              bestDist *= 1.05;

 else  if (center2center > upperDistLimit)

              bestDist *= 0.95;

          }

 // Reset the points, so that we start from scratch

 // but with (hopefully) a better distance value

          pt1 = first;

          pt2 = second;

          tries++;

 // Max out at 20 iterations (gives much better coverage

 // than 10)

 if (tries > 20)

          {

 double curLen =

              cur.GetDistanceAtParameter(cur.EndParam);

 double prevLen =

              prevCur.GetDistanceAtParameter(prevCur.EndParam);

 // Only fill previous curves when the current one

 // is much shorter than the previous one

 if (curLen < prevLen / 4)

              FillRemainingCurve(prevCur, radius, prevDep, layer);

res = false;

 break;

          }

        }

 else

        {

 // We have spheres at an acceptable distance from

 // each other (or this is the first one)

          prevCen = cen;

          GenerateSpheresAlongCurve(

            cur, radius, bestDepth, layer

          );

res = true;

 break;

        }

      }

 while (true);

 if (prevCur != null)

        prevCur.Dispose();

      prevCur = cur;

 return res;

    }

 private  void FillRemainingCurve(

 Curve cur, double radius, double initDepth, int layer

    )

    {

 Curve inner = null;

 try

      {

        inner =

          (initDepth == 0.0 ? cur : GetInnerCurve(cur, initDepth));

 if (inner == null)

 return;

 // Fill the remaining space as best we can

 double inc = 2 * radius,

                depth = inc;

 bool done = false;

 while (!done)

        {

          done =

            !GenerateSpheresAlongCurve(

              inner, radius, depth, layer

            );

          depth += inc;

        }

      }

 catch { }

 finally

      {

 if (inner != null && initDepth > 0.0)

          inner.Dispose();

      }

    }

 // Return the slope of the solid's surface at a particular

 // section location

 private  Vector3d GetSlopeAtPoint(

 Point3d pt1, Point3d pt2, Vector3d unitVec, double radius

    )

    {

 // To get the slope of the surface at the point we care

 // about, we'll take a section either side of the

 // point - using 5% of the radius as the distance in

 // each direction

 const  double factor = 0.05;

 // Transform the points defining our section line in one

 // direction

 Matrix3d disp =

 Matrix3d.Displacement(radius * factor * unitVec);

 Point3d pt11 = pt1.TransformBy(disp);

 Point3d pt12 = pt2.TransformBy(disp);

 // And then the other direction

disp = Matrix3d.Displacement(-radius * factor * unitVec);

 Point3d pt21 = pt1.TransformBy(disp);

 Point3d pt22 = pt2.TransformBy(disp);

 // Get the defining curves of these two sections

 Curve c1 = CurveFromSection(pt11, pt12);

 Curve c2 = CurveFromSection(pt21, pt22);

 // Assume their start points are at the same relative point

 if (c1 != null && c2 != null)

      {

 // Return the angle between the two

 return c1.StartPoint - c2.StartPoint;

      }

 return  new  Vector3d(0, 0, 0);

    }

 private  Curve CurveFromSection(Point3d pt1, Point3d pt2)

    {

 // Make a collection from our defining points

 Point3dCollection pts =

 new  Point3dCollection(new  Point3d\[2\] { pt1, pt2 });

 // Declare the arrays for our results

 Array fillEnts, bgEnts, fgEnts, fTangEnts, cTangEnts;

 // Create the section and generate the geometry

 using (Section sec = new  Section(pts, _viewDir))

      {

        sec.GenerateSectionGeometry(

_sol, out fillEnts, out bgEnts,

 out fgEnts, out fTangEnts, out cTangEnts

        );

 // We only care about the fillEnts - dispose of the rest

 foreach (Entity e in bgEnts)

          e.Dispose();

 foreach (Entity e in fTangEnts)

          e.Dispose();

 foreach (Entity e in cTangEnts)

          e.Dispose();

 if (fillEnts.Length == 0)

 return  null;

 // Return the first curve, dispose of any remaining

 Curve c = fillEnts.GetValue(0) as  Curve;

 for (int i = 1; i < fillEnts.Length; i++)

        {

 DBObject o = fillEnts.GetValue(i) as  DBObject;

 if (o != null)

            o.Dispose();

        }

 return c;

      }

    }

 // Get an inner curve offset at a certain depth

 private  static  Curve GetInnerCurve(Curve c, double depth)

    {

 try

      {

 // We only return a single curve, if there is one.

 // If there are more, we return null

 DBObjectCollection inners = c.GetOffsetCurves(-depth);

 if (inners.Count == 1)

 return inners\[0\] as  Curve;

 foreach (DBObject o in inners)

          o.Dispose();

      }

 catch { }

 return  null;

    }

 // Return the center of the 1st sphere to be placed on a curve

 private  static  Point3d GetFirstCenter(Curve c, double radius)

    {

 // Only handle closed curves

 if (c.Closed)

      {

 // Get the inner curve at the depth of the radius

 using (Curve inner = GetInnerCurve(c, radius))

        {

 // Return the start point of the curve (which we assume

 // will be at a consistent point across the various

 // section profiles)

 if (inner != null)

 return inner.StartPoint;

        }

      }

 return  Point3d.Origin;

    }

 // Generate adjacent spheres along a curve

 private  bool GenerateSpheresAlongCurve(

 Curve curve, double radius, double depth, int layer

    )

    {

 // Only deal with closed curves

 if (!curve.Closed)

 return  false;

 try

      {

 // Get the inner curve at a certain depth

 using (Curve inner = GetInnerCurve(curve, depth))

        {

 // Return null if we didn't get a curve or if it's too

 // short to realistically fill with spheres

 if (

inner == null ||

            inner.GetDistanceAtParameter(inner.EndParam) <

              8.85 * radius

          )

          {

 if (inner != null)

            {

 // For shorter curves we'll create one last

 // sphere at the center of the curve, assuming

 // it's a circle or an ellipse (spheres

 // generally generate ellipse section curves,

 // it seems)

 if (inner is  Ellipse || inner is  Circle)

              {

 Point3d lastCen =

(inner is  Ellipse ?

                  ((Ellipse)inner).Center :

                  ((Circle)inner).Center);

                CreateSphereAtPoint(lastCen, radius, layer);

              }

            }

 return  false;

          }

 // Get the curve's plane

 Plane p = inner.GetPlane();

 // Get the center of the first sphere (the curve's start

 // point) and make a local copy of it

 Point3d cen = inner.StartPoint,

                  initial = cen;

 // We'll step along the curve parametrically

 double curParam = inner.StartParam;

 // A flag to indication completion

 bool done = false;

 // We'll loop until we can't create more spheres

 while (!done)

          {

 // Create a circle at the first location

            CreateSphereAtPoint(cen, radius, layer);

 // Get the center of the next sphere: cen and curParam

 // will get updated, and lastRadius will only get set

 // in the case where there's only space to create a

 // smaller sphere

 double lastRadius;

            done =

              !GetNextSphereCenter(

inner, p, radius, initial, ref cen, ref curParam,

 out lastRadius

              );

 // If we're at the end of the ring, create the last

 // sphere

 if (done && lastRadius > 0.0)

              CreateSphereAtPoint(cen, lastRadius, layer);

          }

 return  true;

        }

      }

 catch

      {

 return  false;

      }

    }

 // Get the next sphere's center along the ring

 private  bool GetNextSphereCenter(

 Curve curve, Plane p, double radius, Point3d initial,

 ref  Point3d cen, ref  double curParam, out  double lastRadius

    )

    {

 // We may not return a lastRadius, so default to 0

      lastRadius = 0.0;

 // Make a local copy of the previous sphere's center

 Point3d prevCen = cen;

 // We're going to use a circle on the plane of the curve

 // to find the center of our next sphere

 using (Circle c = new  Circle(cen, p.Normal, 2 * radius))

      {

 // The first - at the previous sphere's center - gets

 // intersected with the curve

 Point3dCollection pts = new  Point3dCollection();

        curve.IntersectWith(

c, Intersect.OnBothOperands, pts,

 IntPtr.Zero, IntPtr.Zero

        );

 // For each of the intersection points, we get the parameter

 foreach (Point3d pt in pts)

        {

 double param = curve.GetParameterAtPoint(pt);

 // If the point's parameter is greater than the current

 // one, we'll use it

 if (param > curParam)

          {

 double distFromInitial = (pt - initial).Length;

 if (distFromInitial >= (2 * radius * 0.98))

            {

              cen = pt;

              curParam = param;

 return  true;

            }

 else

            {

 // Fill the gap at the end of a ring by reducing

 // the radius and adjusting the center to be the

 // mid-point on the curve between the first and

 // the previous spheres' centers

              curParam =

                curParam + ((curve.EndParam - curParam) / 2);

              cen = curve.GetPointAtParameter(curParam);

 double distFromInitial2 = (cen - initial).Length;

              lastRadius = distFromInitial2 - radius;

 return  false;

            }

          }

 break;

        }

      }

 return  false;

    }

 // Create a sphere at the given point

 private  void CreateSphereAtPoint(

 Point3d cen, double radius, int layer

    )

    {

 // Just make sure the center isn't at the origin (which

 // we've naughtily been using to denote an error) and

 // that the radius is greater than zero

 if (

        cen.DistanceTo(Point3d.Origin) > Tolerance.Global.EqualPoint

&& radius > Tolerance.Global.EqualPoint

      )

      {

 // Create and position our sphere

 Solid3d sol = new  Solid3d();

 // If we're subtracting, make the sphere 2% smaller, so

 // they don't touch each other

        sol.CreateSphere(_sub ? radius * 0.98 : radius);

 // Move the sphere to the desired location

 Matrix3d disp = Matrix3d.Displacement(cen - Point3d.Origin);

        sol.TransformBy(disp);

 // Set its layer

        sol.Layer = GetLayer(layer);

 // Add it to the modelspace and the transaction

        AddToDrawing(sol);

 // If we're subtracting from the outer solid, do so

 if (_sub)

        {

          _parent.BooleanOperation(

 BooleanOperationType.BoolSubtract, sol

          );

        }

      }

    }

 // Add an entity to the open block table record and the

 // current transaction

 private  void AddToDrawing(Entity ent)

    {

      _btr.AppendEntity(ent);

_tr.AddNewlyCreatedDBObject(ent, true);

    }

  }

}

Let’s see what we get when we run the PACKSPHERES command selecting a sphere and a section line right through the middle.

At the top-left of the below image is the wireframe view of the sphere prior to packing, top-middle is the packed sphere, also in wireframe (and prior to graphics regeneration, which would lead to the inner layers not showing as well), and the subsequent shots are of the various layers of the solid in realistic view (one more layer of the onion is peeled off in each shot).

I didn’t bother hiding the last layer, as we can – in any case – see the inner solid and I prefer the symmetry of the below image as it stands. The inner solid is the one we have been using to generate each layer, and has been created by iterative offsets from the original sphere: by design we choose to stop the process - leaving the inner body – when the radius of the spheres we would be creating would be relatively too large to create a useful layer.

If you look at the model from a different angle, you’ll see some quirks: the poles have some gaps, for instance, and as spheres of a particular radius are not guaranteed to fill a ring completely, we fill the remainder with smaller spheres, where needed.

There are alternative approaches which would avoid this “fault line” of small spheres: we could adjust the radius of the spheres very slightly to make sure they are even over the length of the curve, or we could rotate the whole curve by a random amount. Each approach ultimately has advantages and disadvantages, so I’ve left it as is for this initial implementation.

One benefit of an approach that takes sections across the length of a solid is that it can pack different forms full of spheres, too. For instance, here’s a bottle:

And here’s a tree-like form (the algorithm needs some work to deal with sudden changes in size of the section curve, such as when we go from the lower part of the “leafy part” to the trunk):

The PACKSPHERES command does have an option we haven’t looked at in this post, which is to automatically subtract the inner spheres from the outer solid. This can take a long time to work, but generates results that are quite like those in the last post (where we performed more-or-less the same process manually).

Here’s a section view of the resultant hollowed bottle when that option is selected (the size of the individual holes clearly depends on where the spheres happen to be relative to the section plane):

AutoCAD’s STLOUT command generated a 257 MB STL file for the above solid. Thanks to Alex Fielder for suggesting [netfabb](https://www.netfabb.com/) as a tool for viewing STL files (with the caveat that he hadn’t tested it on files quite that large): while netfabb Studio Basic managed to load the file, it unfortunately had difficulty slicing it.

But anyway – until someone invents a way to 3D print enclosed, hollow spheres without needing support material (perhaps [microgravity fabs in lunar orbit](https://en.wikipedia.org/wiki/Space_manufacturing) would help – which is actually [less sci-fi than you might think](http://www.businessweek.com/technology/3d-printing-coming-to-the-manufacturing-spaceand-outer-space-01092012.html) :-), this remains an intellectual – albeit entertaining (for me, at least) – exercise.

Next week we’re going to have a change of pace: as the [news about](http://autodesk.blogs.com/between_the_lines/2012/02/autocad-2013-news.html) [AutoCAD 2013](http://lynn.blogs.com/lynn_allens_blog/2012/02/autocad-2013-is-approaching-the-finish-line.html) is starting to hit the web, I’ll spend the week talking about the coming developer-oriented features in the 2013 release of AutoCAD.