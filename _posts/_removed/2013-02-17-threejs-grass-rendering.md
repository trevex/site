---
layout: post
title: Three.js simple grass rendering
image: thumb_threejs_grass.png
comments: true
---
For my provisional summer project I will need grass, so I started investigating how to deal with grass efficiently in WebGL. This simple approach was a technique I used to render grass in one of my first OpenGL (2.1) programs years ago.

The theory is simple: Loads of instances of the same mesh. The geometry buffers can then be manipulated every frame to make the grass waving. Z-Ordering is avoided for the transparency by using AlphaTests. With batching or proper instancing in OpenGL it was possible to achieve decent performance. Unfortunately this last step is not possible with WebGL therefore the performance is limited, because the vast amount of issued drawing calls. Therefore dealing with the grass within a shader would be a better solution.

Anyway here is a video:

{% youtube rAGeAJ8tyzQ 600 300 %}

And the [demo](https://dl.dropbox.com/u/43243793/examples/webgl_geometry_grass.html).

If waving grass is not necessary and three.js is used. It is also possible to use billboards along with three.js’s inbuilt particle system. I mocked up a version using this approach [here](https://dl.dropbox.com/u/43243793/examples/webgl_particles_grass.html). Unfortunately at least on my machine the resizing of the billboards does not work.

A completely different approach I used last is to completely generate the grass procedurally. Using a few triangles I simply generate each leaf and then merge the complete geometry. This performs reasonably well, but have a look for yourself:

{% youtube RocjbKKg1W4 600 300 %}

Or try the final [demo](https://dl.dropbox.com/u/43243793/examples/webgl_shader_grass.html).

Overall three.js is quite powerful, but if I have some spare time left I will try to write a completely shader-based version from scratch to see how far this can be pushed within the boundaries of the browser environment.