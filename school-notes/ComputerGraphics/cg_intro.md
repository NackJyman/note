# Computer Graphics
### Introduction
##### August 23rd, 2022
---

Download Unity and OpenGL

---

### Intro to OpenGL
##### August 25th, 2022

Displays information frame by frame and is event-driven.

Events 

- Event Queue
- Callback Functions
- Event Loop
- Registering Callback Functions

#### OpenGL Graphics Pipeline

Application ->  Geometry -> Rasterization -> Screen

- Everything in the world is defined in 3 dimensions
	- 2D is a special case
- Objects and Cameras
- Define
	- Objects and Cameras
	- Lighting and shading
	- Final pixel values must be computed

#### Graphics Primitives
 - Points
 - Lines
 - Triangle Strips
 - Quad Strip
 - Line Strip
 - Line Loop
 - Polylines
	 - Joints 
	 - Thicknesses
- Text
- Filled Regions

##### Drawing Lines in OpenGL
```cs
glBegin(GL_LINES);
	glVertex3(100,50,0);
	glVertex3(100, 130, 0);
glend();
```
---
```c
glBegin(GL_LINES);
	glVertex3(40,100,0);
	glVertex3(202, 96, 0);
	glVertex3(50,100,0);
	glVertex3(21, 13 ,0);
glend();
```
---
```c
Void drawLine (Glint x1, Glint y2, Glint x2, Glint y1);

glBegin(GL_LINES);
	glVertex3(100,50,0);
	glVertex3(100, 130, 0);
glend();
```
---
```c
glBegin(GL_LINE_STRIP);
	glVertex3(40,100,0);
	glVertex3(202, 96, 0);
	glVertex3(50,100,0);
	glVertex3(21, 13 ,0);
glend();
```

---
### Example Code
```c
using System.Collections;  
using System.Collections.Generic;  
using UnityEngine;  
public class myFirstOpenGLprogram: MonoBehaviour  
{  
	static Material drawMaterial;  
	// Start is called before the first frame update  
	void Start()  
{  
  
}  
// Update is called once per frame  
void Update()  
{  
  
}  
  
void OnPostRender()  
	{  
	GL.Clear(false, true, Color.white, 0.0f);  
	if (!drawMaterial)  
	{  
		Shader myShader = Shader.Find("Hidden/Internal-Colored");  
		drawMaterial = new Material(myShader);  
	}  
	drawMaterial.SetPass(0);  
	GL.LoadIdentity();  
	GL.LoadOrtho(); //from 0,0,-1 to (1,1,100)  
	GL.PushMatrix();  
		GL.Begin(GL.QUADS);  
		GL.Color(Color.blue);  
		GL.Vertex3(0, 0.2f, 0);  
		GL.Vertex3(0.2f, 0.5f, 0);  
		//GL.Color(new Color(1, 0, 1)); //try adding color in between the vertices  
		GL.Vertex3(0.5f, 0.2f, 0);  
		GL.Vertex3(0.5f, 0, 0);  
	GL.End();  
	/*  
	GL.Begin(GL.QUADS);  
	GL.Color(new Color(1, 0, 0));  
	GL.Vertex3(0, 0.5f, 0);  
	GL.Vertex3(0.5f, 1, 0);  
	GL.Vertex3(1, 0.5f, 0);  
	GL.Vertex3(0.5f, 0, 0);  
	GL.Color(Color.cyan);  
	GL.Vertex3(0, 0, 0);  
	GL.Vertex3(0, 0.25f, 0);
	
	GL.Vertex3(0.25f, 0.25f, 0);  
	GL.Vertex3(0.25f, 0, 0);  
	GL.End();  
	*/  
	GL.PopMatrix();  
	}  
}

```
