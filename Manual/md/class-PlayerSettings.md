#Player Settings

The __Player Settings__ (menu: __Edit > Project Settings > Player__) let you set various options for the final game built by Unity. There are a few settings that are the same regardless of the build target but most are platform-specific and divided into the following sections:

* **Resolution and Presentation**: settings for screen resolution and other presentation details such as whether the game should default to fullscreen mode.
* **Icon**: the game icon(s) as shown on the desktop.
* **Splash Image**: the image shown while the game is launching.
* **Other Settings**: any remaining settings specific to the platform.
* **Publishing Settings**: details of how the built application is prepared for delivery from the app store or host webpage.

The general settings are covered below. Settings specific to a platform can be found separately in the [platform's own manual section](PlatformSpecific.html).

See also [Unity Splash Screen](class-PlayerSettingsSplashScreen.html) settings.

##General Settings

![](../uploads/Main/PlayerSetInspTop.png)


|**_属性：_** |**_功能：_** |
|:---|:---|
|**Cross-Platform Properties** ||
|__Company Name__ |The name of your company. This is used to locate the preferences file. |
|__Product Name__ |The name that will appear on the menu bar when your game is running and is used to locate the preferences file also. |
|__Default Icon__ |The default icon that the application will have on every platform. You can override this for specific platforms. |
|__Default Cursor__ |The default cursor that the application will have on every supported platform. The supported platforms are PC, MAC and Linux standalone, WebGL, PS4, and Windows store.|
|__Cursor Hotspot__ |Cursor hotspot in pixels from the top left of the default cursor. |

