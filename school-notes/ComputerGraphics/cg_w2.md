# Computer Graphics
## Week 2
---
##### August 30th, 2022
---
### Cameras & Viewpoints
#### Coordinate System
- Modeling Task
	- Coordinates used to describe Geometric Objects.
- Viewing Task
	- Coordinates used to size and position the pictures of those geometric on the display.

##### Viewport

Define a rectangular viewport in the screen window on the display. 

How?
- A mapping is established by OpenGL

#### Code Examples (Useful Wrappers)

```cs
// Setting the Window
void setWindow(GLdouble left, GLdouble right, GLdouble bottom, GLdouble top) {
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(left, right, bottom, top);
}

// Setting the Viewport
void setViewport(GLint left, GLint right, GLint bottom, GLint top) {
glViewport(left, bottom, right - left, top - bottom);
}
```

###### Exercises to do
Alter the camera with multiple viewpoints.

##### More

We can map window to viewport

$s_x = A x + C$
$s_y = B y + D$

...

##### Preserving Aspect Ratio
We want the largest viewport which preserves the aspect ratio *R* of the world window. 

The screen window has width W and height H:
If R > W/H
	- The viewport should be width W and height W/R

If R < W/H
	The vieport should be width H*F

---
##### September 1st, 2022
---

### CG Transformations

#### The Camera and Perspective Projection
There are three planes defined perpendicular to the VPN: the **near plane**, the **view plane**, and the **far plane** (Things past the far plane do not render. Like a pyramid. The windows have an **aspect ratio** which can be set in the program.

```cs
UnityEngine.Matrix4x4 projM = Matrix4x4.identity;
projM = UnityEngine.Matrix4x4.Ortho(left, right, bottom, top, near, far);

...
```

#### Vectors/Points and transformation code

Points
- Have postion $(x,y,z)$

Vectors and Coordinate Systems
- A vector $v$ between points

Scaling an object
```cs
glScaled(sx, sy, sz);
```

Translating an object
```cs
glScaled(tx, ty, tz);
```

Rotating an object
```cs
glRotated(angle, ux, uy, uz);
```

#### Transfromations

- An **Object Transformation** alters the coordinates of each point on the object according to the same rule, leaving the underlying coordinate system fixed.
- A **Coordinate Transformation** defines a new coordinate system in terms of the old one, then represnets all the object's points in this new system.


---
