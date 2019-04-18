Unity iOS Basics
================


This section covers the most common and important questions that come up when starting to work with iOS.

先决条件
-------------


###I've just received iPhone Developer approval from Apple, but I've never developed for iOS before. What do I do first?
**A**: Download the SDK, get up and running on the Apple developer site, and set up your team, devices, and provisioning. We've provided a [basic list of steps](iphone-accountsetup.html) to get you started.

###Can Unity-built games run in the iPhone Simulator?
**A**: No, but Unity iOS can build to iPad Simulator if you're using the latest SDK. However the simulator itself is not very useful for Unity because it does not simulate all inputs from iOS or properly emulate the performance you get on the iPhone/iPad. You should test out gameplay directly inside Unity using the iPhone/iPad as a remote control while it is running the [Unity Remote](#UnityRemote5) application. Then, when you are ready to test performance and optimize the game, you should publish to iOS devices.

Unity Features
--------------


###How do I work with the touch screen and accelerometer?
**A**: In the scripting reference inside your Unity iOS installation, you will find classes that provide the hooks into the device functionality that you will need to build your apps. Consult the [Input page](Input.html) for more information.

###My existing particle systems seem to run very slowly on iOS. What should I do?
**A**: iOS has a relatively low fillrate. If your particles cover a large portion of the screen with multiple layers, it will kill iOS performance even with the simplest shader. We suggest baking your particle effects into a series of textures offline. Then, at run-time, you can use 1-2 particles to display them via animated textures. You can get ok results with a minimum amount of overdraw this way.

###Can I make a game that uses heavy physics?
**A**: Physics can be expensive on iOS as it requires a lot of floating point number calculations. You should completely avoid MeshColliders if at all possible, but they can be used if they are really necessary. To improve performance, use a low fixed framerate using __Edit-&gt;Time-&gt;Fixed Delta Time__. A framerate of 10-30 is recommended. Enable rigidbody interpolation to achieve smooth motion while using low physics frame rates. In order to achieve completely fluid framerate without oscillations, it is best to pick a fixed `deltaTime` value based on the average framerate your game is getting on iOS. Either 1:1 or half the frame rate is recommended. For example, if you get 30 fps, you should use 15 or 30 fps for fixed frame rate (0.033 or 0.066)

###Can I access the gallery, music library or the native iPod player in Unity iOS?
**A**: Yes, if you implement it. Unity iPhone supports the native plugin system, where you can add any feature you need -- including access to Gallery, Music library, iPod Player and any other feature that the iOS SDK exposes. Unity iOS does not provide an API for accessing the listed features through Unity scripts.


UnityGUI Considerations
-----------------------

###What kind of performance impact will UnityGUI make on my games?
**A**: UnityGUI consumes more resources when more controls are used. It is ideal to limit your use of UnityGUI to game menus or very minimal GUI Controls while your game is running. It is important to note that every object with a script containing an `OnGUI()` call will require additional processor time -- even if it is an empty `OnGUI()` block. It is best to disable any scripts that have an `OnGUI()` call if the GUI Controls are not being used. You can do this by marking the script as `enabled = false`.

###Any other tips for using UnityGUI?
**A**: Try using GUILayout as little as possible. If you are not using GUILayout at all from one `OnGUI()` call, you can disable all GUILayout rendering using `MonoBehaviour.useGUILayout = false;` This doubles GUI rendering performance. Finally, use as few GUI elements while rendering 3D scenes as possible.

---

* <span class="page-edit">2018-06-14 Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
