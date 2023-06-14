---
title: TI - Rapid, Large Size FDM 3D Printing
date: 2022-05-31 
categories: [Technical Investigation, Work]
tags: [product design, 3d printing]     # TAG names should always be lowercase
toc: true
image:
  path: /assets/img/TI_BR/20220604_011136 (1).jpg 
  alt: This 3D print was later used as the "plug" to create a fiberglass hull.
---
This Technical Investigation (TI) was focused on how to create a large high-surface resolution plug for fiberglassing. Traditional methods of ["waffle structures"](https://alecgmckay.myportfolio.com/bench-waffle-structure){:target="_blank"} from laser cutting or 5-axis foam carving was not available due to limited tooling.  


## Introduction
As mentioned in the [800W CNC Machine](https://www.oliverk.ca/posts/800W-CNC-Machine/){:target="_blank"} post that:
>"*3D printing is always the best manufacturing method unless you need high strength or large size*"


and this project required large size with a total length of around 2.2m. 3D printing starts to take a long time with larger volume objects as it suffers from the square-cube law and the infill pattern starts to take ~60%+ of the print time as it has to draw long interconnecting lines in this larger volume. Luckily the smart folks in the 3D printing world offer a "Vase Mode" or "Spiralize Outer Contour" in slicers which is when the nozzle continuously prints the outer contour of the object (1 wall layer wide).


![Vase Mode GIF](/assets/img/TI_BR/c7q2a92s5mcz.gif){: .rounded-10 w='auto' h='auto'}
_Credits: [/r/3Dprinting](https://www.reddit.com/r/3Dprinting/comments/6qe3rr/vase_mode_on_a_low_poly_model/){:target="_blank"}_


Vase mode is a fantastic feature for....well...vases that don't require any rigidity but would collapse under any load when fiberglassing which would be detrimental.

## Solution


The solution is to trick the slicer into creating rib supports inside the structure without lifting the nozzle during printing. This is done by creating cuts in the solid body in CAD that are 0.35x the line width you are using and extend 2.0x the line width from the outer walls.


![Example Cross Section](/assets/img/TI_BR/image1.png){: .rounded-10 w='500' h='auto'}
_Close-up of rib features bracing both sides of print and welded together at end_
![Finished part CS](/assets/img/TI_BR/20220530_143326%20(2).jpg){: .rounded-10 w='500' h='auto'}
_Example of the finished 3D Print with rib supports_


The 2.0x spacing multiplier mentioned above are so that the print head will turn right before it reaches the outer wall but not before wiping into that wall and fusing the layers. Additionally, the 0.35x multiplier is such that the there (green) and back (red) lines also fuse together.


The cuts are fairly easy to make in Solidworks as you can extrude a thin feature (sketch line) "offset from surface" and choose the offset. Completing this operation in some programs such as Fusion360 was found to be more challenging and created more error messages. This method is also 3D printer specific and the spacings need to be tweaked if you change slicer settings or nozzle diameter. However, it is incredible at creating semi-rigid prints that take hours or sometimes DAYS less time to print than the conventional FDM printing method.


3D printing creates very high-resolution smooth surfaces that are continuous, unlike laser-cut waffle structures which saves you hours down the road when fiberglassing as you need way less filler and sanding to create a smooth plug.


![Example Slicer of TI](/assets/img/TI_BR/ezgif.com-video-to-gif%20(7).gif){: .rounded-10 w='1250' h='auto'}
_Showing path of the nozzle in slicer as it creates rib features_


