---
layout: post
title: Force fields for crowd simulation
image: thumb_force_field.png
comments: false
---
While playing around with Maya I started trying to achieve a form of crowd-simulation without scripting. What I did was simple: Every object in the scene had an invisible sphere attached that was pointy where the object was looking. The sphere itself was an elastic physics body without contact friction. 

The final result was a bunch of objects trying to avoid each other but ending up being stuck in each over, but it lead me to an idea how I could implement simple crowd-simulation. If every object similar to a magnetic field has a force field pushing other objects away, they should avoid each other. The result can be even improved by adding in a determination factor, describing how strongly an object is influenced by other objects force fields. The major down-side of this approach is that the work to calculate the force applied on each objects grows with each object by  `T(n) = O(n*n)`, but this shouldn’t be importent for using it within Maya.