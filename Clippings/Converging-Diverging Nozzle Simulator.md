# Converging-Diverging Nozzle Simulator
[Converging-Diverging Nozzle Simulator](https://devenport.aoe.vt.edu/aoe3114/CD%20Nozzle%20Sim/) 

 * * *

Instructions

* * *

**_Background_**  
The purpose of this Matlab program is to simulate the operation of a converging-diverging nozzle, perhaps the most important and basic piece of engineering hardware associated with propulsion and the high speed flow of gases. This device was invented by Carl de Laval toward the end of the l9th century and is thus often referred to as the 'de Laval' nozzle. This program is intended to help students of compressible aerodynamics visualize the flow through this type of nozzle at a range of conditions.

**_Technical Background_**  
The usual configuration for a converging diverging (CD) nozzle is shown in the figure. Gas flows through the nozzle from a region of high pressure (usually referred to as the chamber) to one of low pressure (referred to as the ambient or tank). The chamber is usually big enough so that any flow velocities here are negligible. The pressure here is denoted by the symbol _pc_. Gas flows from the chamber into the converging portion of the nozzle, past the throat, through the diverging portion and then exhausts into the ambient as a jet. The pressure of the ambient is referred to as the 'back pressure' and given the symbol _pb_.

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-5-16%2017-47-37/2ce8acd3-0a1a-469e-b2d3-733de400cbc2.gif?raw=true)

A simple example  
To get a basic feel for the behavior of the nozzle imagine performing the simple experiment shown in figure 2. Here we use a converging diverging nozzle to connect two air cylinders. Cylinder A contains air at high pressure, and takes the place of the chamber. The CD nozzle exhausts this air into cylinder B, which takes the place of the tank. ![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-5-16%2017-47-37/b30299e0-b965-404c-87e0-c573cf0258dc.gif?raw=true)

Imagine you are controlling the pressure in cylinder B, and measuring the resulting mass flow rate through the nozzle. You may expect that the lower you make the pressure in B the more mass flow you'll get through the nozzle. This is true, but only up to a point. If you lower the back pressure enough you come to a place where the flow rate suddenly stops increasing all together and it doesn't matter how much lower you make the back pressure (even if you make it a vacuum) you can't get any more mass flow out of the nozzle. (At least, that is, without increasing the pressure or density in cylinder A.) We say that the nozzle has become 'choked'. You could delay this behavior by making the nozzle throat bigger (e.g. grey line) but eventually the same thing would happen. The nozzle will become choked even if you eliminated the throat altogether and just had a converging nozzle.

The reason for this behavior has to do with the way the flows behave at Mach 1, i.e. when the flow speed reaches the speed of sound. In a steady internal flow (like a nozzle) the Mach number can only reach 1 at a minimum in the cross-sectional area. When the nozzle isn't choked, the flow through it is entirely subsonic and, if you lower the back pressure a little, the flow goes faster and the flow rate increases. As you lower the back pressure further the flow speed at the throat eventually reaches the speed of sound (Mach 1). Any further lowering of the back pressure can't accelerate the flow through the nozzle any more, because that would entail moving the point where M=1 away from the throat where the area is a minimum, and so the flow gets stuck. The flow pattern downstream of the nozzle (in the diverging section and jet) can still change if you lower the back pressure further, but the mass flow rate is now fixed because the flow in the throat (and for that matter in the entire converging section) is now fixed too.

The changes in the flow pattern after the nozzle has become choked are not very important in our thought experiment because they don't change the mass flow rate. They are, however, very important however if you were using this nozzle to accelerate the flow out of a jet engine or rocket and create propulsion, or if you just want to understand how high-speed flows work.

The flow pattern ![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-5-16%2017-47-37/6df032a9-d575-44ba-abf2-e71e430122de.gif?raw=true)
  
Figure 3a shows the flow through the nozzle when it is completely subsonic (i.e. the nozzle isn't choked). The flow accelerates out of the chamber through the converging section, reaching its maximum (subsonic) speed at the throat. The flow then decelerates through the diverging section and exhausts into the ambient as a subsonic jet. Lowering the back pressure in this state increases the flow speed everywhere in the nozzle.

Lower it far enough and we eventually get to the situation shown in figure 3b. The flow pattern is exactly the same as in subsonic flow, except that the flow speed at the throat has just reached Mach 1. Flow through the nozzle is now choked since further reductions in the back pressure can't move the point of M=1 away from the throat. However, the flow pattern in the diverging section does change as you lower the back pressure further.

As _pb_ is lowered below that needed to just choke the flow a region of supersonic flow forms just downstream of the throat. Unlike a subsonic flow, the supersonic flow accelerates as the area gets bigger. This region of supersonic acceleration is terminated by a normal shock wave. The shock wave produces a near-instantaneous deceleration of the flow to subsonic speed. This subsonic flow then decelerates through the remainder of the diverging section and exhausts as a subsonic jet. In this regime if you lower or raise the back pressure you increase or decrease the length of supersonic flow in the diverging section before the shock wave.

If you lower _pb_ enough you can extend the supersonic region all the way down the nozzle until the shock is sitting at the nozzle exit (figure 3d). Because you have a very long region of acceleration (the entire nozzle length) in this case the flow speed just before the shock will be very large in this case. However, after the shock the flow in the jet will still be subsonic.

Lowering the back pressure further causes the shock to bend out into the jet (figure 3e), and a complex pattern of shocks and reflections is set up in the jet which will now involve a mixture of subsonic and supersonic flow, or (if the back pressure is low enough) just supersonic flow. Because the shock is no longer perpendicular to the flow near the nozzle walls, it deflects it inward as it leaves the exit producing an initially contracting jet. We refer to this as overexpanded flow because in this case the pressure at the nozzle exit is lower than that in the ambient (the back pressure)- i.e. the flow has been expanded by the nozzle to much.

A further lowering of the back pressure changes and weakens the wave pattern in the jet. Eventually we will have lowered the back pressure enough so that it is now equal to the pressure at the nozzle exit. In this case, the waves in the jet disappear altogether (figure 3f), and the jet will be uniformly supersonic. This situation, since it is often desirable, is referred to as the 'design condition'. ![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-5-16%2017-47-37/49c0820f-d4a0-4ded-8f55-ce111c30ab4a.gif?raw=true)

Finally, if we lower the back pressure even further we will create a new imbalance between the exit and back pressures (exit pressure greater than back pressure), figure 3g. In this situation (called 'underexpanded') what we call expansion waves (that produce gradual turning and acceleration in the jet) form at the nozzle exit, initially turning the flow at the jet edges outward in a plume and setting up a different type of complex wave pattern.

The pressure distribution in the nozzle  
A plot of the pressure distribution along the nozzle (figure 4) provides a good way of summarizing its behavior. To understand how the pressure behaves you have to remember only a few basic rules

*   When the flow accelerates (sub or supersonically) the pressure drops
*   The pressure rises instantaneously across a shock
*   The pressure throughout the jet is always the same as the ambient (i.e. the back pressure) unless the jet is supersonic and there are shocks or expansion waves in the jet to produce pressure differences.
*   The pressure falls across an expansion wave.

The labels on figure 4 indicate the back pressure and pressure distribution for each of the flow regimes illustrated in figure 3. Notice how, once the flow is choked, the pressure distribution in the converging section doesn't change with the back pressure at all.

**_Operating Instructions for the Program._**  
All of the above description is quite a lot to understand and remember without actually having a converging diverging nozzle to look at. The idea of the program is to give you a model of a nozzle that you can play around with and get experience of. Download the program [here](https://devenport.aoe.vt.edu/aoe3114/CD%20Nozzle%20Sim/CDN.m), run in Matlab and a window like that shown below will appear.

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-5-16%2017-47-37/adf9b893-d00f-4325-b5b3-21b9cce5f865.jpeg?raw=true)

On the left hand side of the window there are plots showing; the geometry of the nozzle (in terms of cross sectional area divided by the throat area _A/At_, the Mach number distribution along it _M_, and the pressure distribution along it normalized on the chamber pressure _p/pc_. These are used for plotting the flow and its features. On the right of the window are the controls; a slider for controlling the back pressure to chamber pressure ratio _pb/pc_, a slider for controlling the exit to throat area ratio of the nozzle _Ae/At_, a slider that controls 'gamma', the ratio of specific heats _Cp/Cv_, as well as buttons that automatically set the pressure ratio _pb/pc_ to one of the three borderline flow cases for the nozzle. These are; 'Subsonic choked' where the flow in throat just reaches Mach 1 but everywhere else is subsonic, 'Shock at exit' where the back pressure is low enough to have drawn a length of supersonic flow in the diverging section terminated by a normal shock at the exit, and 'Design' where the back pressure matches the pressure in the supersonic flow at the nozzle exit.

You can use the program simply by adjusting the sliders or pressing the buttons and observing what happens to the flow. For whatever conditions you set, the top plot will show the region of airflow (in blue), any shocks produced (in red, depth of color and width of line increasing with strength), and any expansion waves downstream of the nozzle exit (in green, depth of color increasing with strength). The plot will also show the initial flow behavior downstream of the nozzle exit, in terms of initial waves produced and orientation of the jet edge. The figure below, for example, shows the case of overexpanded flow.

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-5-16%2017-47-37/d5ccb9d2-fed7-479f-9305-107970ced13a.jpeg?raw=true)

The top plot shows the initial strong shock and jet edge deflection in the nozzle exhaust. Note that the bottom part of the shock is dotted since in real life the wave will undergo reflection, and reflection effects are not modelled in this program. The middle and bottom plots show the Mach number and pressure distributions associated with the flow. Note that downstream of the nozzle exit the pressure distribution shows the back pressure connected to the nozzle exit pressure with a dotted line. This merely highlights when there is a missmatch between the two, and that the pressure within the jet must eventually settle to the back pressure.

**_How the program works._**  
The program works by computing the flow using the one dimensional equations for the isentropic flow of a perfect gas, and the Rankine Hugoniot relations for normal shock waves in perfect gases. You can learn about these relations by reading, for example, _Modern Compressible Flow_, 3rd Edition, 2003, by John D. Anderson Jr. You can use the [Compressible Aerodynamics _Calculator_](http://www.dept.aoe.vt.edu/~devenpor/aoe3114/calc.html) to help you use these relations in your own calculations.