#Scriptable Render Pipeline#

The Scriptable Render Pipeline (SRP) is an alternative to the Unity [built-in pipeline](SL-RenderPipeline.html). 
With the SRP, you can control and tailor rendering via C# scripts. This way, you can either slightly modify or completely build and customize the render pipeline to your needs.

The SRP gives you more granularity and customization options than the built-in Unity render pipeline. And you can use one of the pre-built SRPs to target your specific needs.

Using an SRP is different from using the built-in Unity render pipeline. For example, the pre-built SRPs come with a new Shader library, because the built-in Unity Shaders do not work with SRPs.


##Pre-built SRPs##

Unity has built two Scriptable Render Pipelines that use the SRP framework: the High Definition Render Pipeline (HDRP) and the Lightweight Render Pipeline (LWRP). Each render pipeline targets a specific set of use-case scenarios and hardware needs. The pre-built render pipelines are available as templates for new Projects.

Both of these render pipelines are delivered as packages, and include the [Shader Graph](https://github.com/Unity-Technologies/ScriptableRenderPipeline/wiki/Shader-Graph) and [Post Processing](https://github.com/Unity-Technologies/PostProcessing/wiki) packages.

**Note:** Projects made using HDRP are not compatible with the Lightweight Render Pipeline or the built-in Unity rendering pipeline. Before you start development, you must decide which render pipeline to use in your Project. 


###High Definition Render Pipeline###

The HDRP targets high-end hardware like consoles and PCs. With the HDRP, you’re able to achieve realistic graphics in demanding scenarios. The HDRP uses Compute Shader technology and therefore requires compatible GPU hardware. 

Use HDRP for AAA quality games, automotive demos, architectural applications and anything that favors high-fidelity graphics over performance. HDRP uses physically-based Lighting and Materials, and supports both Forward and Deferred rendering. 

The High Definition Render Pipeline is provided in a single template.

For more information on HDRP, see the [HDRP overview on the SRP GitHub Wiki.](https://github.com/Unity-Technologies/ScriptableRenderPipeline/wiki/High-Definition-Render-Pipeline-overview)


###Lightweight Render Pipeline###

You can use the LWRP across a wide range of hardware. The technology is scalable to mobile platforms, and you can also use it for higher-end consoles and PCs. You’re able to achieve quick rendering at a high quality. LWRP uses simplified, physically based Lighting and Materials.

The LWRP uses single-pass forward rendering. Use this pipeline to get optimized real-time performance on several platforms. 

The Lightweight Render Pipeline is available via two templates: LWRP and LWRP-VR. The  LWRP-VR comes with pre-enabled settings specifically for VR. 

**Note:**  Built-in and custom Lit Shaders do not work with the Lightweight Render Pipeline. Instead, LWRP has a new set of standard shaders. If you upgrade a current Project to LWRP, you can upgrade built-in shaders to the new ones.

Fore more information on LWRP, see the [LWRP overview on the SRP GitHub Wiki.](https://github.com/Unity-Technologies/ScriptableRenderPipeline/wiki/Lightweight-Render-PipelineOverview)


##Setting up a Scriptable Render Pipeline##

There are several different ways to install SRP, HDRP and LWRP: 
If you want to use one of the pre-built SRPs in a new or current Project, continue reading this section. If you are an advanced user, and you want to modify the SRP scripts to directly, see [Creating a custom SRP](#custom).

###Using a pre-built SRP with a new Project###

If you want to use the HDRP or the LWRP in a new Project, and you don’t need to customise the render pipeline, you can create a new Project using Templates.

To create a Project using Templates:

1. Open Unity. On the Home page, click __New__ to start a new Project. 
2. Under __Template__, select one of the render pipeline templates.
3. Click __Create Project__. Unity automatically creates a new Project for you, complete with all the functionalities of the included built-in pipeline.

For more information on using Templates, see [Project Templates](ProjectTemplates.html).



###Installing the latest SRP into an existing Project###
You can download and install the latest version of HDRP, LWRP or SRP to your existing Project via the [Package Manager system](https://docs.unity3d.com/Packages/com.unity.package-manager-ui@1.8/manual/index.html). 

Switching to an SRP from an existing Project consumes a lot of time and resources. HDRP, LWRP and custom SRPs all use custom shaders. They are not compatible with the built-in Unity shaders. You will have to manually change or convert many elements. Instead, consider starting a new Project with your SRP of choice.

To install an SRP into an existing Project:

1. In Unity, open your Project. 
2. In the Unity menu bar, go to __Window &gt; Package Manager__ to open the __Package Manager__ window. 
3. Select the __All__ tab. This tab displays the list of available packages for the version of Unity that you are currently running.
4. Select the render pipeline that you want to use from the list of packages. In the top right corner of the window, click __Install__. This installs the render pipeline directly in to your Project.



###Configuring and using a pre-built render pipeline###

Before you can use either of the pre-built render pipelines, you must create a Render Pipeline Asset and add it to your Project settings. The following sections explain how to create the SRP Asset and how to add it to a HDRP or LWRP.





####Creating Scriptable Render Pipeline Assets####

To properly use your chosen SRP, you must create a Scriptable Render Pipeline Asset.

The Scriptable Render Pipeline Asset controls the global rendering quality settings of your Project and creates the rendering pipeline instance. The rendering pipeline instance contains intermediate resources and the render pipeline implementation.

You can create multiple Pipeline Assets to store settings for different platforms or for different testing environments.

To create a Render Pipeline Asset:

1. In the Editor, go to the Project window, and navigate to a directory outside of the Scriptable Render Pipeline Folder. 
2. Right-click in the Project window, and select __Create &gt; Render Pipeline__.
3. Select either  __High Definition__ or __Lightweight__, depending on your needs. Click __Render Pipeline/Pipeline Asset__.


####Configuring and using HDRP####

To use HDRP, you must edit your Project’s __Player__ and __Graphics__ settings as follows:


1. In the Editor, go to __Edit__ &gt; __Project Settings &gt; Player__. In the __Color Space__ dropdown, select __Linear__. HDRP does not support Gamma lighting.
2. In the Project window, navigate to a directory outside of the _Scriptable Render Pipeline_ folder. 
3. Right-click the Project window, and select __Create &gt; Render Pipeline &gt; High Definition &gt; Render Pipeline__.
4. Navigate to __Edit &gt; Project Settings &gt; Graphics__. 
5. In the __Render Pipeline Settings__ field, add the High Definition Render Pipeline Asset you created earlier.

**Tip:** Always store your High Definition Render Pipeline Asset outside of the Scriptable Render Pipeline folder. This ensures that your HDRP settings are not lost when merging new changes from the SRP GitHub repository.


####Configuring and using LWRP####

To use the LWRP, you must edit your Project’s __Graphics__ settings as follows:

1. In the Project window, navigate to a directory outside of the _Scriptable Render Pipeline_ folder. 
2. Right-click in the Project window, and select __Create &gt; Render Pipeline &gt; Lightweight &gt; Pipeline Asset__.
3. Navigate to __Edit &gt; Project Settings &gt; Graphics__. 
4. In the __Render Pipeline Settings_ field, add the Lightweight Render Pipeline Asset you created earlier.

**Tip:** Always store your new Render Pipeline Asset outside of the _Scriptable Render Pipeline_ folder. This ensures that your Lightweight settings are not lost when merging new changes from the SRP GitHub repository.


<a name="custom"></a>
##Creating a custom SRP ##

The Scriptable Render Pipelines are available in an open Project on GitHub. You can clone an SRP and make modifications in your local version. 

To configure local script files for a new or existing Unity Project:

1. Create a clone of the SRP repository. Place the clone in the root of the Project directory, next to the _Assets_ folder. For information on cloning repositories, see the GitHub help on [Cloning a repository](https://help.github.com/articles/cloning-a-repository/).
2. Checkout a branch that is compatible with your version of Unity. Use the command `git checkout`, and type the branch name.
3. Use the `git submodule update --init` command to find and initialize all submodules related to the SRP. 
4. In your Project manifest, update `dependencies` to so that they point to the SRP packages.  To read more about Project manifests, see the [ Package Manager documentation](https://docs.unity3d.com/Packages/com.unity.package-manager-ui@1.8/manual/index.html#advanced-package-topics). Here is an example of what your script should look like:


    ```
    
    {

       "dependencies": {
           "com.unity.postprocessing": "file:../ScriptableRenderPipeline/com.unity.postprocessing",

           "com.unity.render-pipelines.core": "file:../ScriptableRenderPipeline/com.unity.render-pipelines.core",

           "com.unity.shadergraph": "file:../ScriptableRenderPipeline/com.unity.shadergraph",

           "com.unity.render-pipelines.lightweight": "file:../ScriptableRenderPipeline/com.unity.render-pipelines.lightweight"

       }
    }
    ```

5. Open your Project in Unity. Your packages are now installed locally. When you open the Project solution in an integrated development environment, you can debug and modify the script directly.

---
* <span class="page-edit">2018-07-03 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">Scriptable Render Pipline added in [2018.1] (https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin2018) <span class="search-words">NewIn20181</span></span>

