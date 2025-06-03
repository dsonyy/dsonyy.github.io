---
layout: post
title: "I built a DIY 3D scanning device"
date: 2018-01-01
categories:
---

When I was 15, we built a 3D scanning device with my friends. Here's a note to document the journey a little.

## Idea

The idea was to build a device into which we put an object, press a button, wait a while, and receive a digitalized 3D model of the object.
The motivation was pure curiosity and a need to hack something.
Curiosity supplemented by a 3D-printing boom.

<figure>
    <img src="{{ '/assets/posts/scan-and-print/scanning.gif' | relative_url }}" alt="Scanning" width=300>
    <figcaption>Scanning process from the inside.</figcaption>
</figure>

## Building

We used a set of 3 cheap webcams and 3 line laser pointers. 
When the laser turned on, the corresponding camera took a photo.
Assuming the room is dark, the laser line is visible in the photo and is curved by the object.
After some trigonometric calculations, we estimated the 3D coordinates of the scanned object.

This sequence was repeated for each camera/laser pair, and then the stepper rotated the plate with the object by a defined angle.
At the end of the scan, we resulted in a list of points in a cylindrical coordinate system.

## Darkness

Keeping the room dark was a problem. We decided to build a black box and keep the object inside. The requirements were to:
* ensure there is no ambient light inside,
* provide a sufficiently large working space for our scanning method,
* be easy to assemble and disassemble,
* be cheap.

We used Autodesk Fusion 360 to design the housing and commissioned to make most of the parts from plexiglass.

<figure>
    <img src="{{ '/assets/posts/scan-and-print/render_0.png' | relative_url }}" alt="Scanning" height=200>
    <img src="{{ '/assets/posts/scan-and-print/render_1.png' | relative_url }}" alt="Scanning" height=200>
    <img src="{{ '/assets/posts/scan-and-print/render_2.png' | relative_url }}" alt="Scanning" height=200>
    <figcaption>Renders of the scanner.</figcaption>
</figure>

## Results

They were more or less accurate, depending on the object's gloss, camera quality, and physical imperfections. 
But overall, the approach worked.

To visualize the results, I wrote a very slow Python script for converting point clouds to gifs. It's available on [here](https://github.com/dsonyy/point-cloud-to-gif).

<figure>
    <img src="{{ '/assets/posts/scan-and-print/scans/bag.gif' | relative_url }}" alt="Scanning" height=300>
    <figcaption>3D scan of a bag. Glossy surfaces cause the visible artifacts.</figcaption>
</figure>

<figure>
    <img src="{{ '/assets/posts/scan-and-print/scans/can.gif' | relative_url }}" alt="Scanning" height=300>
    <figcaption>3D scan of a matt can.</figcaption>
</figure>

<figure>
    <img src="{{ '/assets/posts/scan-and-print/scans/marszalek.gif' | relative_url }}" alt="Scanning" height=300>
    <figcaption>3D scan of a the first Marshal of Poland figure.</figcaption>
</figure>


## Community

Everything started in Coder Dojo Gliwice, Poland - a computer science club where young teenagers were able to contribute to team projects and learn some programming skills. 
It was the first hacking organization I joined.

Once a year, all Coder Dojos from Poland met in a single place, which was called the SuperDojo conference.
During SuperDojo 2017, an idea came to build a device that is able to scan small 3D objects. So the project began.

As of 2025, CoderDojo is a part of [Code Club by Raspberry Pi Foundation](https://www.codeclub.org.uk/).

After some time, I really admired the community and space created for young creative people. It was quite difficult for me to find such a place when I was a teenager.