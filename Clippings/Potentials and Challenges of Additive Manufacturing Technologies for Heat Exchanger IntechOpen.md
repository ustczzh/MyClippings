# Potentials and Challenges of Additive Manufacturing Technologies for Heat Exchanger | IntechOpen
![](https://cdnintech.com/web/frontend/www/assets/42.58/series/icons/ic-unlock.svg)
Open access peer-reviewed chapter

Uwe Scheithauer, Richard Kordaß, Kevin Noack, Martin F. Eichenauer, Mathias Hartmann, Johannes Abel, Gregor Ganzer and Daniel Lordick

Submitted: February 26th, 2018 Reviewed: July 6th, 2018 Published: November 5th, 2018

DOI: 10.5772/intechopen.80010

Abstract
--------

The rapid development of additive manufacturing (AM) technologies enables a radical paradigm shift in the construction of heat exchangers. In place of a layout limited to the use of planar or tubular starting materials, heat exchangers can now be optimized, reflecting their function and application in a particular environment. The complexity of form is no longer a restriction but a quality. Instead of brazing elements, resulting in rather inflexible standard components prone to leakages, with AM, we finally can create seamless integrated and custom solutions from monolithic material. To address AM for heat exchangers we both focus on the processes, materials, and connections as well as on the construction abilities within certain modeling and simulation tools. AM is not the total loss of restrictions. Depending on the processes used, delicate constraints have to be considered. But on the other hand, we can access materials, which can operate in a much wider heat range. It is evident that conventional modeling techniques cannot match the requirements of a flexible and adaptive form finding. Instead, we exploit biomimetic and mathematical approaches with parametric modeling. This results in unseen configurations and pushes the limits of how we should think about heat exchangers today.

### Keywords

*   additive manufacturing
*   computer-aided design
*   flow simulation
*   metals
*   ceramics
*   fractal geometry

*   #### Uwe Scheithauer\*
    
    *   Fraunhofer Institute for Ceramic Technologies and Systems IKTS, Germany
    
*   #### Richard Kordaß
    
    *   Fraunhofer Institute for Machine Tools and Forming Technology IWU, Germany
    
*   #### Kevin Noack
    
    *   TU Dresden, Team Geometric Modeling and Visualization, Germany
    
*   #### Martin F. Eichenauer
    
    *   TU Dresden, Team Geometric Modeling and Visualization, Germany
    
*   #### Mathias Hartmann
    
    *   Fraunhofer Institute for Ceramic Technologies and Systems IKTS, Germany
    
*   #### Johannes Abel
    
    *   Fraunhofer Institute for Ceramic Technologies and Systems IKTS, Germany
    
*   #### Gregor Ganzer
    
    *   Fraunhofer Institute for Ceramic Technologies and Systems IKTS, Germany
    
*   #### Daniel Lordick
    
    *   TU Dresden, Team Geometric Modeling and Visualization, Germany
    

\*Address all correspondence to: uwe.scheithauer@ikts.fraunhofer.de

1\. Introduction
----------------

Heat exchangers are used to transfer heat energy from one to another medium without intermixing them. There are different types available like plate, bundled tube or rotary heat exchangers. [Figure 1](#F1) shows an example of a conventional plate heat exchanger.

![](https://www.intechopen.com/media/chapter/63132/media/F1.png)

#### Figure 1.

Setup of a conventional plate heat exchanger \[1\].

In addition, heat exchangers also differ in their working principle (counterflow, direct flow, or cross flow) and can consist of differently shaped plates or tubes with, for example, smooth, buckled, or rippled surfaces. A typical wall thickness reaches from 0.4 to 2.5 mm and is mainly designed to withstand blockage, corrosion, active pressure, or abrasive media. Such heat exchangers are very cost-effective.

In conventional heat exchangers, a lot of restrictions and disadvantages exist, concerning the realizable geometry, the operating temperature, as well as the manufacturing costs:

1.  Heat exchanger manufactured by the combination of different planar parts is limited in the realizable design and compactness (the ratio between heat exchanging surface and total volume).
    
2.  The assembly of the different parts can result in assembly failures.
    
3.  The realization of mechanical and fluidic interfaces is very challenging (often, the cross-flow principle is realized instead of the superiorly counterflow principle because of the feeding system for the different fluid channels).
    
4.  The joining of the different parts is often realized by brazing. But the brazing material limits the operating temperature, and the brazing process can result in leakage.
    
5.  Because of the used standardized geometries for the parts as well as the whole heat exchanger components, their outer geometry can hardly be individualized. Furthermore, no adjustment of the outer geometry on the shape of the surrounding system can be realized.
    
6.  Some examples for ceramic-based heat exchangers for high temperature or high corrosive or abrasion applications exist, but their design is limited because of the ceramic shaping and finishing technologies. Furthermore, the operation temperature is limited because of the needed joining additives (e.g., solders or brazes) for the different ceramic parts.
    

Additive manufacturing (AM) is a new class of manufacturing technologies, which has been developed for polymers, metals, and ceramics during the last three decades and keeps evolving. Based on computer-aided design (CAD) files in 3D, typically a layer-wise manufacturing process follows, which allows the realization of component designs as well as inner and outer geometries which were previously regarded as not producible. Concerning the manufacturing of heat exchanger, AM technologies open the door to overcome all of the restrictions mentioned above:

1.  The manufacturing of the heat exchanger as one component with integrated mechanical and fluidic interfaces becomes possible.
    
2.  No joining steps are needed, and the same properties are available in the whole component.
    
3.  Very complex designs can be realized, and the ratio between the heat exchanging surfaces to the total volume of the heat exchanger can be increased significantly. The increased performance allows the miniaturization of the heat exchanger.
    
4.  The adjustment of the outer geometry becomes possible, and the required volume for the implementation of heat exchanger and the surrounding system can be decreased.
    
5.  AM of ceramics opens the door for complex heat exchangers for demanding applications concerning operation temperature, abrasion, or corrosion.
    

Thus, heat exchangers seem to be very interesting for AM while prices are falling especially for AM of metal components \[[3](#B3)\]. Also, an integrated manufacturing process for heat exchangers seems to be positive on their pressure resistance and against leakage. Today, only a few designs are described or commercially available. EOS and 3TRPD have designed a heat exchanger, which was manufactured with laser beam melting ([Figure 2](#F2)). Unfortunately, they have not published any performance data for comparison.

![](https://www.intechopen.com/media/chapter/63132/media/F2.png)

#### Figure 2.

Additively manufactured heat exchanger by EOS and 3TRPD \[2\].

Furthermore, at Fraunhofer IFAM, a counterflow heat exchanger was developed to improve the efficiency of a micro-gas turbine system ([Figure 3](#F3)). In this case study, the hot exhaust gas should heat up the inflowing cold air to improve the overall combustion efficiency of the system. The heat exchanger was particularly designed for laser beam melting, so there was no conventional way to manufacture the part. The heat exchanger combines 18 layers of channels in the limited design space. Furthermore, the complex inner channels were designed in a wave shape combined with a very small spacing to each other in order to maximize the surface for heat transfer ([Figure 3](#F3)). With the special design for laser beam melting, it was possible to reduce the required time and costs for postprocessing. It was only necessary to machine the inlet and outlet at the side toward the build platform after the LBM process \[C4\].

![](https://www.intechopen.com/media/chapter/63132/media/F3.png)

#### Figure 3.

Counterflow heat exchanger manufactured by LBM; left: manufactured component \[4\]; right: rendering of complex internal structure inside the heat exchanger \[4\].

But still, a lot of challenges exist, which have to be overcome, to allow the AM of high-performance heat exchanger.

Design stage:

1.  designing and dimensioning of the heat exchanger;
    
2.  generation of the CAD files;
    
3.  modeling and simulation of the fluid flow and heat flux;
    
4.  providing software tools for the different tasks, coupling, and automatization of all tasks.
    

Manufacturing stage:

1.  enhancement of the material portfolios for the different AM technologies;
    
2.  increasing productivity (higher building speed, larger building space with more components manufactured simultaneously, less reject rate) will result in decreasing manufacturing costs.
    

To overcome the current restrictions, we are working on all links of the process chain. In this chapter, we want to introduce different AM technologies for metals (Laser beam Melting—LBM) and ceramics (Fused Filament Fabrication—FFF and Lithography-based Ceramic Manufacturing—LCM) as well as two different approaches for designing and creating the CAD files, one based on conventional software tools and one based on mathematical algorithms. In addition, the specification of the operation conditions of a solid oxide fuel cell system with an operation temperature of 850°C and higher will exemplarily illustrate the requirements for heat exchangers suitable for high-temperature applications and will justify the need for AM of high-temperature materials like ceramics.

[Advertisement](https://ehealthcaresolutions.com/contact-us/)

2\. Design stage
----------------

### 2.1. Different heat exchanger designs based on conventional CAD tools

For additively manufactured heat exchangers, a high heat exchange capacity is essential. It is represented by the heat flow, which can be calculated from the thermal conductivity λ, the heat transfer area A, the wall thickness s, and the temperature difference ΔT. For different materials, λ is summarized in [Table 1](#tab1). It can be seen that the thermal conductivity is an important factor, because it has a linear influence on the heat flow. It becomes evident that steel should not be used as a material for heat exchangers. With copper or silver instead of steel, the heat exchange rate can almost be increased by the factor of 20. But aluminum would be an adequate material of choice, as well.

| Material | Thermal conductivity WmK |
| --- | --- |
| Silver | 429 |
| Copper | 401 |
| Aluminum | 237 |
| High alloyed steel | ∼20 |
| Low alloyed steel | ∼30 |

### Table 1.

Specific thermal conductivity for different materials \[[5](#B5), [6](#B6)\].

Usually, the temperature gradient ΔT cannot be influenced by outer parameters and is defined by the process in which the heat exchanger is used. Therefore, the quotient of the heat exchanging area A and the thickness of the wall s are the factors to vary in terms of geometric adaption. The goal for improved structures for a high heat exchange rate should be that they have a high ratio A/s and have a considerable high thermal conductivity. The favorable material becomes aluminum because it can be processed by LBM, and the minimal reproducible wall thickness accounts to 300 μm. Furthermore, the structures have to be self-supporting. Inside the channels, a mechanic finishing process is not feasible, so the wall roughness has to be in an acceptable range, and minimal angles of areas reclining from the normal of the build plate should be lower than 45°.

In this work, principle insights into the design of structures with a high heat exchange capability and coincident low pressure drop shall be given. Therefore, simulation of the flow is inevitable, and the structures were designed using CAD tools. The aim is to obtain a structure, which can be individualized at its outer geometry and optimizes the flow problems inside of the structure.

On the basis of the constraints explained before, basic sketches were developed and compared on its contour length of all the channels that will be involved in the heat transfer. To enhance the performance of the heat exchanger, the aim is to maximize the surface for the heat transfer. Therefore, the contour length of the channels is maximized in each evolution of the sketches by retaining the hydraulic diameter. Basic sketches were made and afterwards extruded in height with a helical-shaped structure for generating a longer streamline and for maximizing the heat transfer area. All in all, seven basic sketches were defined with nine resulting structures, as shown in [Figure 4](#F4). Structures 1 and 2 show a simple geometry, which could also be produced with conventional manufacturing processes. Structures 3–5 are very complicated in terms of connecting geometries. Structures 6.1 and 6.2 are highly optimized for production with straight walls which can be efficiently produced with LBM with special slicer options. At last, structures 7.1 and 7.2 are optimized for production and fluid flow by using special slicer options and not having sharp edges like structures 6.1 and 6.2 which lead to a higher pressure drop.

![](https://www.intechopen.com/media/chapter/63132/media/F4.png)

#### Figure 4.

Different designs of inner structures for heat transfer.

A comparison of the geometrical and heat flow characteristics of these structures is given in [Table 2](#tab2). Also, the structures show a surface roughness as build. This roughness is assumed to be the same value for all side surfaces since they are all in the same build direction (same as the orientation of the pictures).

|  | Compactness m2m3 | Heat flow W | Performance per volume Wcm3 | Pressure drop \[Pa\] |
| --- | --- | --- | --- | --- |
| 1 | 267 | 270 | 38.20 | 0.05 |
| 2 | 243 | 170 | 24.05 | 0.06 |
| 3 | 664 | 200 | 28.29 | 0.06 |
| 4 | 651 | 350 | 49.51 | 0.50 |
| 5 | 1576 | 420 | 59.42 | 0.15 |
| 6.1 | 1471 | 520 | 65.47 | 0.10 |
| 6.2 | **1711** | 550 | **80.54** | 0.20 |
| 7.1 | 1264 | 750 | 50.71 | **0.10** |
| 7.2 | 1246 | **900** | 60.86 | **0.10** |

### Table 2.

Simulation results for the different designed structures (compactness, heat flow, performance per volume and pressure drop; possible choice highlighted).

As stated above, connecting structures for fluid guidance inside the heat exchanging structure are important, but rather complex and difficult to design. As a first attempt for structures 5 and 6.2, connecting structures are shown in [Figure 5](#F5), which were derived from biomimetic role models like fennel.

![](https://www.intechopen.com/media/chapter/63132/media/F5.png)

#### Figure 5.

Exemplary connecting structures for designed structures 5 and 6.2.

To avoid such a complexity of the connecting structures and at the same time to enable the engineer to model them economically, a new approach was chosen. The design process now starts with the connecting structures, and the inner geometry is optimized afterwards. The design ideas were adopted from nature, too. Some possible basic structures are shown in [Figure 6](#F6). The purpose of these structures is to transform a round connector with a 6-mm diameter to any amount of complex-shaped inner structures like tubes. In addition, the flow has to be equally distributed in all inner tubes, and heat transfer should also start within the connecting structure to minimize losses. Certainly, manufacturability has to be guaranteed as well by allowing a minimum angle of 45° to the building plane. For the same reason, gaps inside the structures have to be avoided, and the connecting structures have to be as narrow as possible.

![](https://www.intechopen.com/media/chapter/63132/media/F6.png)

#### Figure 6.

Sampled possible structures for connecting geometries adopted from nature.

It is obvious that from these connecting structures, such immersed structures like those shown in [Figure 4](#F4) (structures 6 and 7) cannot be accessed. This means that ideally nestable inner structures are preferred, which can be enveloped with one larger outer channel to gain a tube-in-tube-like design known from conventional heat exchangers.

Designing the inner structures, which are applicable to the previous connecting structures, is the next step. The profile shape may not be too complex due to terms of a steady connection to connecting structures. Furthermore, analytical calculation cannot be used for dimensioning these structures since they should all have the same hydraulic diameter. Therefore, fluid simulation was used for dimensioning. To validate these findings, experimental parameter evaluation should be conducted in further investigations. In [Figure 7](#F7), some possible designed inner structures are compared. Herein, the lowest pressure drop per performance as indicating value was used to choose the optimal inner structure. These profiles are based on mathematical algorithms after Sierpinski (left profile) and Hilpert (right profile).

![](https://www.intechopen.com/media/chapter/63132/media/F7.png)

#### Figure 7.

Comparison between inner structures with the same hydraulic diameter and optimal designs selected using fluid simulation.

Furthermore, the heat exchanging volume has to be filled with the inner structures using optimal arrangement options to fill the volume using curves, planes, or lines.

For combining the presented structures to a whole heat exchanger, a connecting structure has to be chosen and specified. In [Figure 8](#F8), the possible structures emerged from biomimetic structures of [Figure 3](#F3) are preselected in terms of producibility, compactness, and capability. The final selection was done by simulating the connecting structures with CFD using Ansys CFX to gain a uniform flow distribution over all profiles and minimum losses in flow distribution.

![](https://www.intechopen.com/media/chapter/63132/media/F8.png)

#### Figure 8.

Selection of an optimal connecting structure.

Finally, three different complete heat exchanging structures were designed for comparison and to gain an optimal structure. The structures were combined to a whole exchanger geometry, and the capability of the models was simulated as shown in [Figure 9](#F9).

![](https://www.intechopen.com/media/chapter/63132/media/F9.png)

#### Figure 9.

Whole heat exchanger structures and flow in inner and outer channels.

As depicted, a uniform flow distribution can be achieved in the inner flow. The outer flow, however, shows a highly turbulent and uneven distribution especially in designs 2 and 3, leading to a high pressure drop. The obtained values from these structures are depicted in [Table 3](#tab3). The different values for design no. 1 are based on different lengths of the inner structure (50, 100, and 150 mm).

|  | 1 | 2 | 3 |
| --- | --- | --- | --- |
| Compactness \[m2/m3\] | 736–1291–**1348** | 735 | 1161 |
| Heat flow Q̇ \[W\] | 3465–6202–**9372** | 4482 | 6985 |
| Capacity per volume Q̇/V \[W/cm3\] | 376–**435**–421 | 139 | 272 |
| Pressure drop Δp \[bar\] | 1.87–1.95–2.50 | **5.49** | **0.97** (inner), **2.58** (outer) |

### Table 3.

Calculated values for designed heat exchangers (outstanding values highlighted).

It can be seen that the compactness is highly dependent on the length of the structure (design no. 1) because the influence of connecting structures decreases with an increasing length. With this example, a good scalability with different performances can be achieved, especially in terms of individualization. All in all, individualized and complex-shaped heat exchanging structures can be obtained for optimized production with additive manufacturing. But still, optimization has to be done to increase the performance and to lower the pressure drop. Also, experimental validation of the simulation has to be carried out even since the convergence in simulation was not satisfying.

### 2.2. Alternative ways to generate new designs for heat exchanger

#### 2.2.1. New approach

In the following section, a new approach to generate efficient design structures for heat exchangers is presented. This new approach is one of the main topics of the instaf project.

Fractal macrostructures are used to generate a large inner surface, which implicates a better energy transfer between the heat-exchanging fluids. The creation of microstructures with roughness (with respect to the process-based roughness of AM), induced by partial Brownian motions, leads to turbulences. This raises the performance even more.

Two irreconcilable goals define the design scope. On the one hand, the surface for heat exchanging should be maximal and the fluids should remain in the heat exchanger as long as possible. But on the other hand, the restrictions concerning the manufacturing process (e.g., minimal wall thickness, resolution, waiver of support structures, etc.) and the operation as heat exchanger (e.g., pressure drop, mechanical strength, etc.) have to be considered as well.

#### 2.2.2. Space-filling curves

To generate a maximum heat exchange surface, the so-called fractals were studied. Fractals are inspired by nature and are a branch of research essentially introduced to mathematics by Benoit Mandelbrot \[[7](#B7)\]. Stochastic fractals can be found in lung alveoli and other breathing organs or in the capillaries in the fin of whales with a heat- and energy-saving component.

For the design of the macrostructures, our special focus lies on the construction of space-filling curves, which belong to the group of FASS curves. The acronym FASS stands for space-filling, self-avoiding, simple, and self-similar. This class of curves traverses every vertex of a polygonal grid so that every point is reached once. Because they are not allowed to cut themselves, they separate to areas perfectly and are therefore well suited for the construction of heat exchangers.

A Lindenmayer system (L-system) was used to generate the curves. An L-system is a way to describe a repetitive structure with a small number of rules. It is a character-based rewriting system, which consists of constants. These are representing draw commands and variables, which are replaced in every iteration step through a replacement rule. In [Figure 10](#F10), the system is visualized by means of the Hilbert curve with the variables X, Y, the constants F (straight line), −(clockwise rotation), and +(counterclockwise rotation) with a starting value of X. The rules are X → +YF − XFX − FY+ and Y → −XF + YFY + FX−.

![](https://www.intechopen.com/media/chapter/63132/media/F10.png)

#### Figure 10.

A visualization of the Lindenmayer-code demonstrated by means of the Hilbert Curve.

A process was developed to generate a large number of different curves with a small number of basic motifs considering only 2 × 2- and 3 × 3 grids in order to investigate the best properties. There are four ways to traverse through these grids, which leads to seven basic motifs, paying attention to reflection. Every motif can also be used as a mapping structure. Therefore, it represents the connecting pieces of the following iteration step. [Figure 11](#F11) shows a curve where the basic motif of a Peano curve (represented by the purple lines) and the mapping structure of the Hilbert curve (represented by the green lines) are combined. The ratio of curve length to the surface area or in higher dimension from surface to volume is always the same for the same grid size. Therefore, the criteria of evaluation are turbulences in flow and the velocity of the fluids so as the basic conditions of the used AM technology.

![](https://www.intechopen.com/media/chapter/63132/media/F11.png)

#### Figure 11.

One possible combination of the motifs.

#### 2.2.3. Generation of CAD files

To construct a three-dimensional structure, successive iteration steps of a curve were placed in a predefined distance. They were combined with an NURBS-based (non-uniform rational basic spline) surface. The surface is lofted over the curves and defines a closed structure in combination with the outer skin. [Figure 12](#F12) illustrates the process for the first four iteration steps of a Peano curve. Since separate areas should always be consistent, the offset must be adjusted. As the curve gets longer in each iteration step, the selected offset also decreases. [Figure 13](#F13) demonstrates the result, visualizing one of the two separate liquids.

![](https://www.intechopen.com/media/chapter/63132/media/F12.png)

#### Figure 12.

The first four iteration steps of the Peano curve. They lead to a feeding structure for which we combined the four layers with an NURBS-based surface.

![](https://www.intechopen.com/media/chapter/63132/media/F13.png)

#### Figure 13.

Rendering of the dispersion of one fluid in a feeding structure designed with a Peano curve down to the fourth iteration step.

In the illustrated structure, the thickness of the partition wall decreases from 4.5 to 0.2 mm, which corresponds to a factor of 22.5. The length of the partition wall increases in the same range, simultaneously. In addition, the extrusion of the final geometry results in a heat exchanger with a compactness of about 3000 m2/m3, which can be operated as a counterflow heat exchanger. The structure was created using the CAD software Rhinoceros 3D. In [Figure 13](#F13), some renderings of the fluid paths are shown.

Another structure for a heat exchanger is presented in [Figure 14](#F14). In this case, a curve was chosen, which can be closed. It can be used without an outer wrapping, and this is why it could also be used as an immersion heater. For a better view into the internal structure, it is presented cut open. The outer structure could be represented by a cuboid. The curves are constructed with the end points of the elements as control points. This makes the curves even longer and smoothens them evenly.

![](https://www.intechopen.com/media/chapter/63132/media/F14.png)

#### Figure 14.

Alternative structure as a concept for a heat exchanger. From left to right: a 5/8 cutout to show the inner structure; the whole component (lying); the curves used for designing the structure.

[Advertisement](https://ehealthcaresolutions.com/contact-us/)

3\. Laser beam melting (LBM): AM of metal components
----------------------------------------------------

The LBM process has a lot of different specific names such as LaserCUSING®, selective laser melting (SLM®), direct metal laser sintering (DMLS®), and direct metal printing, to name only a few. All of these names describe the powder bed-based laser process, where a part is manufactured by means of thin layers of a powder material, which are applied by a scraper and molten selectively by laser energy.

The digital process chain begins with the 3D CAD file (\*.stl) of the part, which has to be manufactured. This file is transferred to a software program where the support generation and the positioning in the building chamber of the machine are done. Afterwards, the so-called build job is sliced into layers of 20–100 μm, dependent on the material and the laser parameters and carried over to the machine \[[8](#B8)\]. A principle schematic of an LBM machine is shown in [Figure 15](#F15).

![](https://www.intechopen.com/media/chapter/63132/media/F15.png)

#### Figure 15.

A principle schematic of an LBM machine.

To avoid oxidation, the process itself as well as the preparation and postprocessing of the powder has to be done under inert gas atmosphere. Overhanging structures have to be supported by supporting structures which have to be produced as well for stabilizing the model and to improve the dissipation of heat below these geometries \[[9](#B9)\].

The process is suitable for producing individual and highly complex parts and hollow structures such as topology-optimized components as shown in [Figure 16](#F16). Also, very fine structures like lattice structures can be produced, which is also depicted therein.

![](https://www.intechopen.com/media/chapter/63132/media/F16.png)

#### Figure 16.

Complexly designed skateboard trunk manufactured with LBM.

After the manufacturing process, the part is separated from the build platform, and the support structure is removed. To adjust the mechanical properties of the part, a heat treatment can be applied. Other conventional methods like machining or polishing can be utilized to achieve a better surface quality, dependent on the requirements.

Depending on slicing and scanning strategy, the quality of manufactured parts can widely vary, concerning cracks, pores, residual stresses, distortions, tightness, and fatigue properties. However, LBM processes are becoming relevant in series and tool production, while the reliability of such manufacturing technologies and the resulting component quality are of high importance \[[10](#B10)\].

At Fraunhofer IWU, a counterflow heat exchanger was specially developed for the LBM process with the focus on a low pressure drop, flexible, as well as compact design ([Figure 17](#F17)). To reduce the pressure drop and validate the best version, a fluid analysis was executed on each design. To get a maximum heat transfer and integrated insulation to the outer atmosphere, the cold channel is wrapped around the hot channel within the heat exchanger (heat exchange surface/volume: 405 m2/m3; performance: 36.2 W/cm3). Also, the wall between the cold and the hot channel system is reduced to a minimum of 0.8 mm to enhance the heat transfer. As a result of the special design for the laser beam melting process, no support structure and postprocessing were needed to manufacture the heat exchanger. The pressure drop (0.20 bar) as well as heat flow (1.3 kW) of the final design was calculated with flow simulations using Ansys CFX. The propagated heat exchanger shows one imperfection since it produces a thermal short circuit in the area of the connection geometries.

![](https://www.intechopen.com/media/chapter/63132/media/F17.png)

#### Figure 17.

A counterflow heat exchanger with a low pressure drop and compact, flexible design.

The next step will be to avoid the thermal short circuit and to implement the designs described before within the metal heat exchanger in consideration of the general conditions of LBM.

[Advertisement](https://ehealthcaresolutions.com/contact-us/)

4\. AM of ceramic components
----------------------------

### 4.1. Operating conditions in a high-temperature fuel cell system

The research and development of new technologies is sometimes limited by the availability of commercial peripheral components. These new technologies change the requirements on state-of-the-art products.

One of those new technologies is the fuel cell. A fuel cell has the benefit of directly converting chemical energy to electrical power without the conventional process steps in between. This reduces the losses on the whole conversion path. Fuel cells are available in various types with different characteristics ([Figure 5](#F5)). The low-temperature fuel cells are mostly used for portable or mobile applications because of the high power density, for example, as a battery charger for mobile devices. High-temperature fuel cells are typically used for stationary applications like power and heat supply for households and facilities \[[11](#B11)\].

Most of the low-temperature fuel cells like the PEFC (polymer electrolyte fuel cell) need pure hydrogen or at least “clean” fuel. Not allowed substances like carbon monoxide have to be filtered before operation. The high-temperature solid oxide fuel cell (SOFC) is designed to generate electrical and/or thermal power with a high efficiency based on the usage of worldwide available fuels such as natural gas and LPG (liquefied petroleum gas). These fuels are reformed to a gas mixture of hydrogen and carbon monoxide directly inside the SOFC System. In contrast to most of the common fuel cells, an SOFC can also generate electrical power by carbon monoxide conversion ([Figure 18](#F18)).

![](https://www.intechopen.com/media/chapter/63132/media/F18.png)

#### Figure 18.

An overview of fuel cell technologies, including temperatures, reactions, and allowed and not allowed chemical components.

In order to achieve this high efficiency, high temperatures are necessary. The standard operation temperature of an SOFC is above 700°C. As mentioned before, these systems have different gas processing steps included \[[12](#B12)\], which is illustrated in [Figure 19](#F19) as well:

1.  the reforming process (REF),
    
2.  the conversion of hydrogen and carbon monoxide to water and carbon dioxide inside the fuel cell itself, and
    
3.  furthermore, a postprocessing step typically called tail gas oxidation (TOX).
    

![](https://www.intechopen.com/media/chapter/63132/media/F19.png)

#### Figure 19.

An overview of SOFC system with integrated components.

The fuel cell itself cannot convert 100% of the fuel for various reasons, which will not be further explained in this context but are discussed in \[[13](#B13)\]. The rest of the fuel has to be oxidized in order to avoid emission of hydrogen and carbon monoxide. This kind of reaction leads to temperatures of 900°C and higher.

The different reactors REF and SOFC have different requirements on heat treatment. The REF needs heat for a high efficiency. The SOFC receives the reaction products of the REF with a temperature of approx. 800°C on the anode side. On the cathode side, typically air is used. In order to avoid thermal stress and to realize the necessary operating temperature of the SOFC, this air has to be preheated.

The realization of the heat treatment requires a rather complex packaging of all components. This packaging is realized in the HotBox. The efficiency of all steps is based on a minimum of losses inside the system. Therefore, an adapted heat exchange from TOX to air and an overall packaging for optimal heat management are necessary.

Commercial heat exchangers are not aware of these requirements. The used materials and/or joining technologies cannot handle this high temperature, and due to the standard design on commercial heat exchangers, especially plate heat exchangers, the integration of such components inside the HotBox is complicated and space-consuming.

In order to achieve the highest possible efficiency of such systems, heat exchangers that combine an adaptable design with high-temperature resistance are indispensable.

### 4.2. Fused filament fabrication (FFF)

Fused filament fabrication (FFF) is a thermoplastic AM technology which bases on nearly endless filaments which are used as a semi-finished products and which are melted and deposited under a heated nozzle. To generate ceramic components, particle-filled filaments are used to manufacture the so-called green bodies additively \[[14](#B14)\]. These green bodies have to be debinded, to remove all organic materials, and sintered to densify the microstructure and to achieve the typical ceramic properties. The benefits of this AM technology are the high productivity and the large building space of the available devices. The existing challenges of FFF of ceramic components are the development of highly particle-filled filaments and the defect-free debinding and sintering of the components \[[14](#B14)\].

SiC filaments were developed to allow the AM of large volume SiC heat exchanger, which can be used for operation temperatures of 1000°C and higher. [Figure 20](#F20) shows some ceramic test components and demonstrators manufactured by FFF.

![](https://www.intechopen.com/media/chapter/63132/media/F20.png)

#### Figure 20.

Different ceramic demonstrators additively manufactured by FFF; left: SiC (green state); right: various ceramics (sintered state).

The next steps will be the investigation of FFF with SiC filaments concerning the realizable geometries and the tightness of the sintered structures.

### 4.3. Lithography-based ceramic manufacturing (LCM)

The LCM technology was developed and commercialized by Lithoz GmbH, Austria \[[15](#B15)\]. As a special kind of stereolithography, free radical polymerization of the binder system takes place with light of a defined wavelength, causing the suspension to solidify. Via a DLP module, the suspension is selectively irradiated with a blue light, whereby all areas to be cross-linked on a given plane are exposed at the same time. The ceramic particles dispersed in the suspensions are fixed in the solid polymer matrix (green body). A final debinding and sintering step is necessary for this AM technologies for ceramics, as well \[[16](#B16)\].

The LCM technology impresses with a very high resolution (wall thickness down to 100 μm possible) \[[16](#B16)\] and very good surface properties (Ra < 1 μm) of the sintered components. The challenges are the small building area ((76 ×43 × 150) mm3) and the low productivity, both resulting in relatively high manufacturing costs as well as the cleaning and the debinding process for the green components \[[17](#B17)\].

Both FASS structures which were described before were additively manufactured via LCM technology and Al2O3 suspension of Lithoz. The sintering occurred at 1650°C which allows operation temperatures of significantly more than 1000°C. [Figure 21](#F21) shows the feeding structure at the sintered state (left) and the alternative heat exchanger structure (green state).

![](https://www.intechopen.com/media/chapter/63132/media/F21.png)

#### Figure 21.

The FASS structures as ceramic component in the sintered (left; 35 × 35 × 35 mm3) or green state (right), additively manufactured via LCM technology.

[Advertisement](https://ehealthcaresolutions.com/contact-us/)

5\. Conclusion
--------------

The rapid development of AM technologies enables a radical paradigm shift in the construction of heat exchangers. In place of a layout limited to the use of planar or tubular starting materials, heat exchangers can now be optimized, reflecting their function and application in a particular environment. The investigations show the potential of the technologies concerning increasing heat exchanging surface and compactness as well as the designing of the fluidic systems. The AM of ceramics will pave the way to realize heat exchanger for operation temperatures highly above 1000°C.

The new approach for designing can also be used for bent structures. They provide more potential than the straight heat exchangers and open a wider field of possible technical applications. A possible, curved geometry is shown in [Figure 22](#F22).

![](https://www.intechopen.com/media/chapter/63132/media/F22.png)

#### Figure 22.

A computer-aided designed curved heat exchanger. Its geometry bases on FASS.

To increase the inner surface even more, rough surfaces, which induce beneficial turbulences, can be generated by modeling partial Brownian motion.

[Advertisement](https://ehealthcaresolutions.com/contact-us/)

Acknowledgments
---------------

The authors would like to thank the German Federal Ministry of Education and Research (BMBF) for funding the project “**FunGeoS**“ within the Framework Concept Zwanzig20— partnership for innovation” in the consortium AGENT-3D (fund number 03ZZ0208A) as well as European Union which funded parts of this work under the European Union’s Horizon 2020 Research and Innovation Programme (“**CerAMfacturing,**” Grant Agreement No 678503). Other parts of this chapter are based on results from **instaf**, a research project in the European network IraSME, which is carried out by partners in Austria and Germany. It is funded by the Federal Ministry for Economical Affairs and Energy (BMWi) on the basis of a decision of the German Bundestag.

[Advertisement](https://ehealthcaresolutions.com/contact-us/)

Conflict of interest
--------------------

There are no conflicts of interest and nothing else to declare.

References
----------

1.  1. Tranter GmbH. Plattenwärmeübertrager. Available from: https://www.tranter.com/de/products/plate-heat-exchangers \[cited 13 October 2017\]
2.  2. 3TRPD. Available from: https://www.3trpd.co.uk/ \[cited 16 October 2017\]
3.  3. Langefeld R, Veenker H, Schäff C, Balzer C. Additive Manufacturing—Next generation (AMnx): Study. München. Available from: http://www.rolandberger.com/media/studies/2016-04-11-rbsc-pub-Additive\_Manufacturing-next\_generation.html 11 April 2016
4.  4. Schnabel T, Oettel M, Müller B. Design for Additive Manufacturing: Guidelines and Case Studies for Metal Applications. Ottawa: Fraunhofer-Institut für Werkzeugmaschinen und Umformtechnik; 05/2017. Available from: http://canadamakes.ca/wp-content/uploads/2017/05/2017-05-15\_Industry-Canada\_Design4AM\_141283.pdf \[cited 2017 9 October 2017\]
5.  5. Choi SUS, Eastman JA. Enhancing thermal conductivity of fluids with nanoparticles. ASME-Publications-Fed 1995;231:99-106 \[cited 2017 Oct 11\]
6.  6. Schatt W, Blumenauer H, editors. Werkstoffwissenschaft. 8., neu bearb. Aufl. Stuttgart: Dt. Verl. für Grundstoffindustrie; 1996
7.  7. Mandelbrot B. The Fractal Geometry of Nature. New York: W.H. Freeman and Company; 1982
8.  8. Gebhardt A. Additive Fertigungsverfahren: Additive Manufacturing Und 3D-Drucken für Prototyping-Tooling-Produktion. München: Carl Hanser Verlag GmbH Co KG; 2016
9.  9. Rombouts M, Kruth JP, Froyen L, Mercelis P. Fundamentals of selective laser melting of alloyed steel powders. CIRP Annals—Manufacturing Technology. 2006;55(1):187-192. DOI: 10.1016/S0007-8506(07)60395-3
10.  10. Bremen S, Buchbinder D, Meiners W, Wissenbach K. Selective Laser Melting—A Manufacturing for Series Production. In: Fraunhofer Generativ, editor. DDMC—Direct Digital Manufacturing Conference. 2012. p. 2012
11.  11. Viswanathan B, Aulice Scibioh M. Fuel Cells: Principles and Applications. Abingdon: Taylor & Francis Group; 2007
12.  12. Singhal SC, Kendall K. High-Temperature Solid Oxide Fuel Cells: Fundamentals, Design and Applications. Oxford: Elsevier; 2003
13.  13. Lee S, Kim H, Yoon KJ, Son J, Lee J, Kim B, Choi W, Hong J. The effect of fuel utilization on heat and mass transfer within solid oxide fuel cells examined by three-dimensional numerical simulations. International Journal of Heat and Mass Transfer. 2016;97:77-93
14.  14. Abel J, Scheithauer U, Klemm H, Moritz T, Michaelis A. Fused filament fabrication (FFF) of technical ceramics. Ceramic Applications. 2018;6:2-4
15.  15. Homa J. Rapid prototyping of high-performance ceramics opens new opportunities for the CIM industry. Powder Injection Moulding International. 2012;6:73-80
16.  16. Scheithauer U, Schwarzer E, Moritz T, Michaelis A. Additive manufacturing of ceramic heat exchanger: Opportunities and limits of the lithography-based ceramic manufacturing (LCM). Journal of Materials Engineering and Performance. 2018;27:14-10. DOI: 10.1007/s11665-017-2843-z
17.  17. Schwarzer E, Götz M, Markova D, Stafford D, Scheithauer U, Moritz T. Lithography-based ceramic manufacturing (LCM)—Viscosity and cleaning as two quality influencing steps in the process chain of printing green parts. Journal of the Electrochemical Society. 2017;37:5329-5338. DOI: 10.1016/j.jeurceramsoc.2017.05.046

Written By

Uwe Scheithauer, Richard Kordaß, Kevin Noack, Martin F. Eichenauer, Mathias Hartmann, Johannes Abel, Gregor Ganzer and Daniel Lordick

Submitted: February 26th, 2018 Reviewed: July 6th, 2018 Published: November 5th, 2018

© 2018 The Author(s). Licensee IntechOpen. This chapter is distributed under the terms of the [Creative Commons Attribution 3.0 License](http://creativecommons.org/licenses/by/3.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original work is properly cited.