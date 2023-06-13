---
title: OllieLeaf
date: 2020-07-04 
categories: [Projects, Personal]
tags: [led, arduino, electrical design, 3d printing, product design]     # TAG names should always be lowercase
toc: true
image:
  path: /assets/img/OllieLeaf/20200704_152355.jpg  
  alt: Hexagons are the Bestagons
---

Best use of 3D printing: DIY versions of existing products you can't afford. In this case, I created the "OllieLeaf" inspired by the mind-bending prices of the [nanoleaf](https://nanoleaf.me/en-CA/){:target="_blank"} LED panels.  


## Summary
- **COST**:  $75 CAD
    + *[WS2812B RGB LED](https://shorturl.at/cefMS){:target="_blank"} - 5 meters*:&nbsp;&nbsp; $34
    + *[LED Controller](https://shorturl.at/bhIO6){:target="_blank"} or ESP32*:&nbsp;&nbsp; $5-10
    + *[PLA Filament - White](https://shorturl.at/wxBD3){:target="_blank"}*:&nbsp;&nbsp; $30
    + *DC Power Adapter (5-28V, 2-5A)*:&nbsp;&nbsp; "Permenantly borrow" your best friends laptop charger ;)
   
- **TIME TO COMPLETE**: 5 days


## Overview
![OllieLeaf](/assets/img/OllieLeaf/OllieLeaf.gif){: .rounded-10 w='400' h='auto'}
_Showing the 'Slow Ripple' color changing pattern_


This project is the perfect introduction to 3D printing, soldering, and basic microcontroller programming. Many DIY color-changing LED panel solutions exist online for inspiration and guidance. I decided to have a go at designing my own as I wanted a really seamless gap between the panels unlike many I could find [online](https://www.thingiverse.com/thing:3354082){:target="_blank"}.


The three biggest tips I would have for anyone designing their own would be:


1. *Plan out the pattern*: Anything from rough drawing to CAD model and STICK with it after deciding. This will allow you to properly link the LED strips inside so the color patterns flow nicely through all the tiles.


2. *Choose a >4000K White PLA Filament*: I made the mistake of choosing a white filament labeled "Ivory" at first and it makes the tiles look old and all colors yellowed.


3. *Print the Tiles on a Mirror or Glass Plate*: I bought some 30x30cm Ikea mirrors for dirt cheap and taped them to the print bed for printing. It requires lots of first layers and Z-height adjustment but gives the tile a lovely glossy finish on the outside. You can vary the light diffusion by printing multiple layers on the front of the tile but I stuck with one 0.3mm layer.  


![OllieLeaf Internals](/assets/img/OllieLeaf/20200328_110930.jpg){: .rounded-10 w='auto' h='auto'}
_Backside of OllieLeaf showing mounted LED strips, connecting wires and double side tape mounts._


> People really love this product - have sold 3 units to friends and family! Change up the shapes (i.e.: Molecular structure for Serotonin is a popular choice) and fire up that 3D printer to make a little cash.
{: .prompt-tip }
