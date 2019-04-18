# Integrated development environment (IDE) support

An integrated development environment ([IDE](https://en.wikipedia.org/wiki/Integrated_development_environment)) is a piece of computer software that provides tools and facilities to make it easier to develop other pieces of software. Unity supports the following IDEs:

## Visual Studio (default IDE on Windows and macOS)

Unity installs Visual Studio by default when you install Unity on Windows and macOS. On Windows, you can choose to exclude it when you select which components to download and install. Visual Studio is set as the __External Script Editor__ in the Preferences (menu: __Unity &gt; Preferences &gt; External Tools &gt; External Script Editor__). With this option enabled, Unity launches Visual Studio and uses it as the default editor for all script files.

On macOS, Unity includes [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) as the C# IDE. Visual Studio Tools for Unity (VSTU) provides Unity integration for Visual Studio for Mac (VS4M). For information on setting up and using Visual Studio for Mac, see the following Microsoft documentation pages:

* [Visual Studio Tools for Unity](https://docs.microsoft.com/en-us/visualstudio/cross-platform/visual-studio-tools-for-unity)
* [Setup Visual Studio for Mac Tools for Unity](https://docs.microsoft.com/en-us/visualstudio/mac/setup-vsmac-tools-unity)
* [Using Visual Studio for Mac Tools for Unity](https://docs.microsoft.com/en-us/visualstudio/mac/using-vsmac-tools-unity)

On Windows, Unity also includes [Visual Studio 2017 Community](https://www.visualstudio.com/downloads/).

## Visual Studio Code (Windows, macOS, Linux)

Unity supports opening scripts in [Visual Studio Code](https://code.visualstudio.com/) (VS Code). To open scripts in VS Code,  select it as the __External Script Editor__ in the Editor Preferences (menu: __Unity &gt; Preferences &gt; External Tools &gt; External Script Editor__). For information on using VS Code with Unity, see Visual Studio's documentation on [Unity Development with VS Code](https://code.visualstudio.com/docs/other/unity). 

### Prerequisites

To use Visual Studio Code for C# code editing and Unity C# debugging support, you need to install:

* [Mono](http://www.mono-project.com/download/) (only required on macOS and Linux)
* [Visual Studio C# Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [Visual Studio Unity Debugger Extension](https://marketplace.visualstudio.com/items?itemName=Unity.unity-debug) (does not support debugging on .NET Framework 4.6)

## JetBrains Rider (Windows, macOS, Linux)

Unity supports opening scripts in [JetBrains Rider](https://www.jetbrains.com/rider/). To open scripts in Rider, select it as the __External Script Editor__ in the Editor Preferences (menu: __Unity &gt; Preferences &gt; External Tools &gt; External Script Editor__).

Rider is built on top of [ReSharper](https://www.jetbrains.com/resharper/) and includes most of its features. It supports all of C# 6.0’s features as well as C# debugging on the .NET 4.6 scripting runtime in Unity. For more information, see JetBrains documentation on [Rider for Unity](https://www.jetbrains.com/dotnet/promo/unity/).

---
* <span class="page-edit">2018-07-03 Page published with [editorial review](DocumentationEditorialReview.html)
</span>
