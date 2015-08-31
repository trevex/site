---
layout: post
title: Pathfinding in Supreme Commander 2
image: thumb_supreme_commander.png
comments: false
---
Since I am still working on and researching techniques for crowd simulation I stumbled upon the pathfinding in Supreme Commander 2. Everyone who ever worked with pathfinding probably used or at least knows of a variation of the A\*-algorithm. When a few objects move around A\* has proven to be a good and fast solution for pathfinding. Unfortunately A\* is not reliable when a lot of objects move around. 

This problem was researched by the Supreme Commander developers and the University of Washington. The resulting algorithm to allow seamless pathfinding of vast amounts of objects is called Flowfield-Pathfinding. Flowfields represent a grid of velocities indicators, basically directing the units similar to streams influencing plankton in the sea. The downside is that the resulting behaviour of the objects is swarm-like and doesn’t allow high behaviour variety.

The paper on Flowfields in general can be found [here](http://www.aaai.org/Papers/AIIDE/2008/AIIDE08-031.pdf) and the research by the University of Washington is available [here](http://grail.cs.washington.edu/projects/crowd-flows/).