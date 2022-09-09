# Computer Graphics
---
##### September 6th, 2022
---

First Project Info (Think MSPaint)
- Put together 2D graphical elements with OpenGL
- Elements to implement
	- Drawing Stuff
		- Draw with a mouse input
		- Add a draw line element
		- Be able to erase with the mouse
	- Change Size of brush
		- Three sizes
		- Three colors
		- Three Shapes
		- Use parameterization, sampling loops and transformation commands.
	- Toolbar to select all these things
		- Viewports can be used to do this.
---
### Mouse Code

```cs
...

//update called once per frame
void update() {
	if (Input.GetMouseButtonDown(0)) { 
		Debug.Log("left mouse click");
		
		Debug.Log("My mouse: " + Input.mousePosition.x + ", " + Input.mousePosition.y);
		Vector3 WorldMousePt = new Vector3();
		WorldMousePt = new vector3 (Camera.main.ScreenToWorldPoint(Input.mousePosition.x, Input.mousePosition.y, Camera.main.nearClipPlane));
		Debug.Log("World mouse: " + WorldMousePt.x + ", " + WorldMousePt.y + ", " + WorldMousePt.z);
	}
}

...
```

When we need to store the points of the click, we will need to log the data into a variable.

---
##### September 8th, 2022'
