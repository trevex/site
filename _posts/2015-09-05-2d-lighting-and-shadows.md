---
layout: post
title: GPU 2D Lighting and shadows
---

### Background

Lighting and shadowing techniques for 2D games seem to be an all-time favorite in any game development oriented community. And recently another wave of people started presenting their approaches/implementations.

Getting lighting working in 2D games is fairly straight forward. Either you blend radial gradient or simply fallback to traditional shader-based lighting techniques.

The harder part is usually to implement shadows efficiently. There are two popular approaches:

* CPU-computed light volumes [[1]](#reference-1) [[2]](#reference-2) [[3]](#reference-3)
* 1D shadow maps [[4]](#reference-4) [[5]](#reference-5)

Both techniques have certain issues. CPU-computed light volumes are fairly flexible and easily extendable, but they are CPU bound and can result in major slow downs depending on the light count and furthermore dependent on the floating point accuracy visual artifacts are possible. I should mention though, that performance is usually not an issue, but with more complex effects the computation gets needlessly more complicated. On the other handside 1D shadow maps are fast on modern GPUs, but the presented techniques are quite limited in visual fidelity. The shadows are always _underneath_ the rendered objects.

My hobby project is a top-down RPG and both techniques proved insufficient for my personal expectations and desired visual fidelity. Therefore I implemented a different technique (based on [[6]](#reference-6)), that allows _self-shadowing_ and different _light-types_.


### Algorithm

The algorithm is tailored to fit the needs of a low-resolution top-down game, but should be flexible and scalable enough to be applicable for a multitude of games.

#### Deferred shading

To seperate geometry and lighting, common deferred shading is used. The _G-Buffer_ has the following layout:

`IMAGE`

The depth is used to compute the position in view-space, therefore the position is omitted.
The normals are encoded using spheremapping to reduce the necessary texture bandwidth [[7]](#reference-7). The layout includes one uncommon extension, that is important for shadowing fragments: The height of the fragment (y-axis) is stored in a 16bit floating point texture (depth could be used to compute view-space height, see [possible improvements](#improvements-and-notes)). 

#### G-Buffer size

`IMAGE`

#### Lighting

#### Shadows


### <a name="improvements-and-notes"></a>Possible Improvment and other notes

> trade computations for texture bandwidth (height vs depth)




[1]<a name="reference-1"></a> <https://github.com/libgdx/box2dlights> <br />
[2]<a name="reference-2"></a> <http://ncase.me/sight-and-light/> <br />
[3]<a name="reference-3"></a> <http://www.redblobgames.com/articles/visibility/> <br />
[4]<a name="reference-4"></a> <http://www.catalinzima.com/2010/07/my-technique-for-the-shader-based-dynamic-2d-shadows/> <br />
[5]<a name="reference-5"></a> <https://github.com/mattdesl/lwjgl-basics/wiki/2D-Pixel-Perfect-Shadows> <br />
[6]<a name="reference-6"></a> <http://www.mattgreer.org/articles/dynamic-lighting-and-shadows/> <br />
[7]<a name="reference-7"></a> <http://aras-p.info/texts/CompactNormalStorage.html> <br />