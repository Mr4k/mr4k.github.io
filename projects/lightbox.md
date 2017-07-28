---
layout: project
title:  "Lightbox"
excerpt: A quick raytracer in python.
project: 4
---

<p align="center">
	<img src="/lightbox.png"> 
</p>

**Lightbox is a basic extendable raytracer I wrote with python.** Most of the work was done over two days. If you are interested feel free to check it out on my [github](https://github.com/Mr4k/lightbox).  
<br>
**Structure** - Lightbox breaks down everything into components as follows:  
<br>
**Renderables** - 3D geometry like a sphere, or a plane, or a box. You can easily extend this component by providing your own ray to surface intersection function.  
Here is a Renderable which represents a horizontal plane:
{%highlight python%}
class Plane(Geom):
	def __init__(self, y):
		self.y = y

	def hitsRay(self, ray, cameraPos, epsilon):
		intersectionNormal = None
		if ray.dot([0,-1,0]) == 0:
			intersectionDepth = -1
		else:
			intersectionDepth = (self.y - cameraPos[1]) / ray[1]
			intersectionNormal = np.array([0,-1.0,0])
		return (intersectionDepth, intersectionNormal)
{%endhighlight%}



<br>
**Materials** - Materials are assigned to renderables. They control how an object looks. They have the following properties:  
Color - The color of the material  
Surface Specularity - How reflective or "shiny" the material is  
Transparency - How transparent the material is  
Refraction Index - Used to determine how light will bend when it passes through a transparent object.  
Sample Texture - This is a function that determines texture at a specific point on (or inside) an object  
<br>
**Extensibilty**
Lightbox was built to be extensible. The texture functions as well as the renderable class allow you to easily add new objects and materials to the scenes. Furthermore the underlying ray tracing code can also be rewritten without having to change how the objects work. The seperation of components leads lightbox to be easily changed.  
<br>

**What's next?**
Lightbox was supposed to be a short project (more like a toy) but some possible extensions could be to make it faster, potentially real time by parallelizing it and offloading the ray tracing to the gpu using something like CUDA.

