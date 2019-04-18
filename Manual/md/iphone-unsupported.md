#Features currently not supported by Unity iOS

##Graphics

* DXT texture compression is not supported; use other formats instead. Please see the [Texture2D Component page](class-TextureImporter.html) for more information.
* Rectangular textures cannot be compressed to PVRTC formats.
* Movie Textures are not supported; use a full-screen streaming playback instead. Please see the [Movie playback page](class-MovieTexture.html) for more information.

##脚本

* Dynamic features like Duck Typing are not supported. Use `#pragma strict` for your scripts to force the compiler to report dynamic features as errors.
* Video streaming via `WWW` and `WebRequest` classes is not supported.
* FTP support by `WWW` and `WebRequest` classes is limited.


##Features restricted to Unity iOS Pro License

* Splash-screen customization


##External libraries

It is recommended to minimize your references to external libraries, because 1 MB of .NET CIL code roughly translates to 3-4 MB of ARM code. For example, if your application references _System.dll_ and _System.Xml.dll_ then it means additional 6 MB of ARM code **if stripping is not used**. At some point the application will reach this limit and the linker will have trouble linking the code.

---

* <span class="page-edit">2018-06-14  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
