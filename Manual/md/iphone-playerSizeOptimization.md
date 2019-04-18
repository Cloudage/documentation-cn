#Optimizing the size of the built iOS Player

The two main ways of reducing the size of the Player are by making a proper __Release build__ within Xcode and by changing the __Stripping Level__ within Unity.

##Building for distribution

It is expected that final release builds are made using the Xcode command __Product__ &gt; __Archive__. Using this command ensures that build is made with release configuration **and** all the debug symbols are stripped.
After issuing this command, Xcode switches to the Organizer window __Archives__ tab (you can navigate there manually via __Window__ &gt; __Organizer__ menu). For guidelines on how to calculate app size and other size-reducing tips, see Apple’s Technical Q&A on [Reducing the size of my App] (https://developer.apple.com/library/content/qa/qa1795/_index.html).

**Note**: We recommend you have some small extra margin for error when aiming for an over-the-air download limit (which currently is 150MB).

##iOS stripping level

Activate the size optimizations for Mono scripting backend builds by stripping work in the following way:


1. __Strip assemblies__ level: the scripts' bytecode is analyzed so that classes and methods that are not referenced from the scripts can be removed from the DLLs and thereby excluded from the AOT compilation phase. This optimization reduces the size of the main binary and accompanying DLLs and is safe as long as no reflection is used.


1. __Strip ByteCode__ level: any .NET DLLs (stored in the Data folder) are stripped down to metadata only. This is possible because all the code is already precompiled during the AOT phase and linked into the main binary.


1. __Use micro mscorlib__ level: a special, smaller version of mscorlib is used. Some components are removed from this library, for example, Security, Reflection.Emit, Remoting, non Gregorian calendars, etc. Also, interdependencies between internal components are minimized. This optimization reduces the main binary and mscorlib.dll size but it is not compatible with some System and System.Xml assembly classes, so use it with care.

These levels are cumulative, so level 3 optimization implicitly includes levels 2 and 1, while level 2 optimization includes level 1.

Note that __Micro mscorlib__ is a heavily stripped-down version of the core library. Only those items that are required by the Mono runtime in Unity remain. Best practice for using micro mscorlib is not to use any classes or other features of .NET that are not required by your application. GUIDs are a good example of something you could omit; they can easily be replaced with custom made pseudo GUIDs and doing this would result in better performance and app size. 

## Stripping with IL2CPP

Refer to documentation on [managed bytecode stripping with IL2CPP](IL2CPP-BytecodeStripping.html) for more information

**Note:** Tt can sometimes be difficult to determine which classes are getting stripped in error even though the application requires them. You can often get useful information about this by running the stripped application on the simulator and checking the Xcode console for error messages.

###Simple checklist for making your distribution as small as possible


1. Minimize your assets: enable compression for textures and reduce their resolution as far as possible. Also, minimize the number of uncompressed sounds. There are some additional tips for file size reduction [here](ReducingFilesize.html).
1. On Mono, set the __iOS Stripping Level__ to __Use micro mscorlib__, or on IL2CPP enable __Strip Engine Code__.
1. Set the script call optimization level to __Fast but no exceptions__.
1. Don't use anything that lives in System.dll or System.Xml.dll in your code. These libraries are **not** compatible with micro mscorlib.
1. Remove unnecessary code dependencies.
1. Set the API Compatibility Level to __.Net 2.0 subset__. Note that .Net 2.0 subset has limited compatibility with other libraries.
1. Avoid generic containers in combination with value types, including structs.

###How small can an app be made with Unity?
An empty project would take less than 22 MB in the App Store if all the size optimizations were turned off. With code stripping, the empty scene with just the main camera can be reduced to less than 12 MB in the App Store (zipped and DRM attached). 

###Why did my app increase in size after being released to the App Store?
When publishing your app, Apple App Store service first encrypts the binary file and then compresses it via zip. Encryption increases ''randomness' of the code segment and thus makes it worse for compression. Check "Building for distribution" chapter above how to estimate App Store size before submission.

---

* <span class="page-edit">2018-06-14  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">2017-14-06 - Upated Stripping with IL2CPP section</span>  


