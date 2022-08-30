# Mobile Programming
### Introduction
##### August 22nd, 2022
---

do android studio

---
##### August 26th, 2022

### Applications/Activity

#### Android Applications
Android applications normally have one or more of the following:

- Activity: Provides a screen which a user interacts with. An activity can also start other activities, including activities in seperate applications
- Service:

- BroadcastReceiver
	- This is a way to receive information from other applications and the OS.

- ContentProvider
	- Your app is providing information to another app and itself via an encapsulated data structure
	- Where the data is stored or how it is stored is not relevent.

#### Activity
We use activity as AppCompatActivity, which comes out the AndroidX library.

After every screen there is an activity. We have a start screen which can change screen to screen. This fragment is the *login* fragment, *settings* fragment and so on. There is something called an *intent* which is a chunk of data which is also a security model. 

##### Manifest File (.xml)
Includes a package address and icon, label, and theme. 

##### Closing Activities

### Model-View-Controller

MVC is a good coding style, software engineering model.

#### Model View Controller Stuff
- Model 
	- Represents the knowledge, ie: data and maybe the "business logic"
		- The Data could be a single object or a "structure of objects"
		- Simple as a java class to a database or data on another system
	- There is no controller or view code in the model
- View
	- Is the visual representation
		- The GUI interface, Layout, html
- Controller
	- Ties the view and the model together.

##### Advantages
Keeps your data and code seperate, by adding more data, the program still functions correctly. 

## Images and Vector Stuff
You can add an image (PNG) or a vector image (SVG) in the drawable directory. Examples of a vector graphic us a grid of pixels with each in its own color data, and the vector a data path between many nodes. 

---
### How to put stuff on the screen!
We got an activity that handles a user interaction and displays information on the screen.

Implement new activities

1. Define layout in XML
2. ...

Go to design view and design the XML

Go into app and define mainActivity

Connect Activity with layout

*Look at the Activity Lifecycle Slide*

---
