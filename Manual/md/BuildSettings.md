# Build Settings

The Build Settings window allows you to choose your target platform, adjust settings for your build, and start the build process. To access the Build Settings window, select __File__ > __Build Settings...__ . Once you have specified your build settings, you can click __Build__ to create your build, or click the __Build And Run__ to create and run your build on the platform you have specified.

![Build Settings window](../uploads/Main/BuildSettings.png)

## Scenes in Build

This part of the window shows you the scenes from your project that will be included in your build.  If no scenes are shown then you can use the *Add Current* button to add the current scene to the build, or you can drag scene assets into this window from your project window. You can also untick scenes in this list to exclude them from the build without removing it from the list.  If a scene is never needed in the build you can remove it from the list of scenes by pressing the delete key.

Scenes that are ticked and added to the Scenes in Build list will be included in the build. The list of scenes will be used to control the order the scenes are loaded. You can adjust the order of the scenes by dragging them up or down.

## Platform List

The Platform area beneath the Scenes in Build area list all the platforms which are available to your Unity version.  Some platforms may be greyed out to indicate they are not part of your version or invite you to download the platform specific build options.  Selecting one of the platforms will control which platform will be built. If you change the target platform, you need to press the "Switch Platform" button to apply your change.  This may take some time making the switch, because your assets may need to be re-imported in formats that match your target platform.  The currently selected platform is indicated with a Unity icon to the right of the platform name.

The selected platform will show a list of options that can be adjusted for the build.  Each platform may have different options.  These options are listed below.  Options that are common across many platforms are listed at the very bottom of this section under the "Generic items across builds" details.

### PC, Mac & Linux Standalone
|Option |Purpose |
|---|---|
|Target Platform ||
|&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;Windows |Build for Windows|
|&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;Mac OS X|Build for Mac|
|&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;Linux|Build for Linux|
|Architecture |x86|
|&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;x86|32-bit CPU |
|&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;x86_64|64-bit CPU |
|&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;Universal|All CPU devices |
|&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;x86 + x86_64 (Universal) |All CPU devices for Linux |
|Copy PDB files |(Windows only) Include Microsoft program database (PDB) files in the built Standalone Player. PDB files contain application debugging information that is useful for debugging, but may increase the size of your Player. This setting is disabled by default. |
|Headless Mode |Build game for server use and with no visual elements |


### iOS
|Option ||Purpose |
|---|---|
|Run in Xcode ||Select the version of Xcode to use in the build. When set to Latest version,the build uses the most recent version of Xcode on your machine. |
|Run in Xcode as || |
||Release|Shipping version  |
||Debug|Testing version  |
|Symlink Unity libraries ||Reference to the Unity libraries instead of copying them into the XCode project.  (Reduces the XCode project size.) |

### Android

|Option |Purpose |
|---|---|
|Texture Compression |Choose from the following texture compression formats:<br/>__Don't override__<br/>__DXT (Tegra)__<br/>__PVRTC (PowerVR)__<br/>__ATC (Adreno)__<br/>__ETC (default)__<br/>__ETC2 (GLES 3.0)__<br/>__ASTC__<br/>For advice on using these formats, see [Getting started with Android development](android-GettingStarted.html#compression).|
|ETC2 fallback |For Android devices that don't support ETC2, override the default ETC2 texture decompression by choosing from 32 or 16 bit format, or 32 bit format with half the resolution.|
|Build System |
|&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;Internal (Default)| Generate the output package (APK) using the internal Unity build process, based on Android SDK utilities. |
|&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;Gradle (New)| Generate the output package (APK)  using the Gradle build system. Supports direct Build and Run and exporting the project to a directory. This is the preferred option for exporting a project, as Gradle is the native format for Android Studio. |
|&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;ADT (Legacy)| Export the project in ADT (eclipse) format. The output package (APK) can be built in eclipse or Android Studio. This project type is no longer supported by Google and is considered obsolete. |
|SDKs for App Stores|Select which third party app stores to integrate with. To include an integration, click __Add__ next to an App Store name. The Unity Package Manager automatically downloads and includes the relevant integration package.|


### WebGL
Build Settings for WebGL use the generic settings shown later on this page.

### Samsung TV
Build Settings for Samsung TV use the generic settings shown later on this page.

### Xiaomi 

For information about building projects for Xiaomi Game Center, see [Unity Xiaomi documentation](https://unity3d.com/cn/partners/xiaomi/guide).


### Other platforms

Console platforms and devices which require a Unity license will be documented in the Platform Specific section of the User Guide.

### Generic items across builds

|Option ||Purpose |
|---|---|
|Development Build ||Allow the developer to test and work out how the build is coming along.|
|Autoconnect Profiler ||When the Development Build option is selected allow the profiler to connect to the build.|
|Script Debugging ||When the Development Build option is selected allow the script code to be debugged.  Not available on WebGL.|
|Scripts Only Build ||Build just the scripts in the current Project.|
|Compression Method ||Compress the data in your Project when building the Player. This includes [Assets](AssetTypes.html), [Scenes](CreatingScenes.html), [Player Settings](class-PlayerSettings.html) and [GI data](GICache.html). Choose between the following methods:|
||Default | On PC, Mac, Linux Standalone, and iOS, there is no compression by default. On Android, the default compression is ZIP, which gives slightly better compression results than LZ4HC, but data is slower to decompress.|
||LZ4 |A fast compression format that is useful for development builds. For more information, see [BuildOptions.CompressWithLz4](../ScriptReference/BuildOptions.CompressWithLz4.html).|
||LZ4HC | A high compression variant of LZ4 that is slower to build but produces better results for release builds. For more information, see [BuildOptions.CompressWithLz4HC](../ScriptReference/BuildOptions.CompressWithLz4HC.html).|

---
* <span class="page-edit">• 2017-08-24  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>

* <span class="page-history">Xiaomi build target added in [2017.2](https://docs.unity3d.com/2017.2/Documentation/Manual/30_search.html?q=newin20172) <span class="search-words">NewIn20172</span></span>

* <span class="page-history">在 [2017.3](https://docs.unity3d.com/2017.3/Documentation/Manual/30_search.html?q=newin20173) 中停止了 Tizen 支持 <span class="search-words">NewIn20173</span></span>

