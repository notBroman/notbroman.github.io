---
laout: Page
title: Micromouse Chasis

permalink: micromouse-chasis
---

# CAD design software

To design the chasis for the mouse [FreeCAD](https://www.freecad.org/index.php) is used, which has just released its version 1.0.
I also tried to use [blender](https://www.blender.org/) with the parametric design plugin [cadsketcher](https://www.cadsketcher.com/), but I could not make it work the way I wanted it to.
My experience in [Creo PTC](https://www.ptc.com/en/products/creo) and [Autodesk AutoCAD](https://www.autodesk.com/uk) made FreeCAD a more enjoyable and intuitive experience.
Blender, although good for modelling, is not intended to be used for parametric design and [cadsketcher](https://www.cadsketcher.com/) is more geared towards people that already use blender and want to try parametric design.
Since this is not my background or expertise FreeCAD was more intuitive to use.
After freshening up on how the UI works by following one of they FreeCAD [tutorials](https://wiki.freecad.org/Basic_Part_Design_Tutorial_019), I was off to a good start.

# Requirements for the part

The basic requirements are as follows:

- Mount the IR ToF sensor
- Mount the motors for the wheels
- Mount the PCB

I wanted this part to be as simple as possible and to "just work"&trade;, spoiler it did not work on the first try; cause why would it, but more on that later.

# Manufactoring

This part was 3D printed using a printer the university robotics club has.
Despite there being countersinks on the bottom, they are quite small and need very little support.

# Designing the chasis

This is what it ended up looking like.
It's preety simple, it's basically a block with holes and a slot.

![chasis](media/chasis_model.png)

The slots are a recepticle for the [IR ToF sensor](https://thepihut.com/products/tf-luna-lidar-ranging-sensor), it is going to be considered the front of the vehicle.
This slot kinda gets around screw and is easier to manufacture than holes for bolts and nuts or threads for screws.

The countersunk holes are to mount the motor brackets.
The height of the platform was chosen to position the motors so that the tires are just touching the ground.
There is some clearance, but it's only 1 mm.
