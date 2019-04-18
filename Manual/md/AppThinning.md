# App thinning

The Apple store and iOS optimize the installation of iOS, tvOS, and watchOS apps by tailoring app delivery to the capabilities of a user’s particular device. This optimization is called app thinning. App thinning lets you create apps that use the most device features, occupy minimum disk space, and accommodate future updates that can be applied by Apple. Learn more about this optimization process on [Apple's Developer Library page on App Thinning](https://developer.apple.com/library/tvos/documentation/IDEs/Conceptual/AppDistributionGuide/AppThinning/AppThinning.html).

This chapter describes the two major components of app thinning that can be implemented in Unity:

* [On-demand resources (ODR)](#OnDemandResources)
* [App slicing](#AppSlicing)

<a name="OnDemandResources"></a> 
# On-demand resources

On-demand resources (ODR) is a feature available for the iOS and tvOS platforms, from version 9.0 of iOS and tvOS onwards. It allows you to reduce the size of your application by separating the core Assets (those that are needed from application startup) from Assets which may be optional, or which appear in later levels of your game. These additional Assets are called [AssetBundles](AssetBundlesIntro.html). They’re available across all Unity build targets, but extra steps must be taken to enable them to be hosted via the App Store.

AssetBundles can contain Asset files such as models, Materials, Textures and Scenes, but they cannot include scripts. This means that all your scripting logic must reside in the main application. Apple recommends that an AssetBundle be no larger than 64MB in size, to reduce loading time (particularly over-the-air) and minimize the amount of storage space used on the device.

## Setting up ODR

To set up your project for ODR, first check that your build type is set to iOS. In the menu bar, go to __File__ > __Build Settings__.

![](../uploads/Main/AppThinning-BuildSettings.png)

If it isn’t already selected, click on __iOS__, then click the __Switch Platform__ button. Next, click the __Player Settings__ button. In the Inspector window, open __Other Settings,__ navigate to the __Configuration__ section, and enable the __Use on demand resources__ checkbox.


![](../uploads/Main/AppThinning-OtherSettings.png)


## Creating an AssetBundle

First, create a new folder to group the Assets you want to place into an AssetBundle. To do this, right-click inside the Project window and go to __Create__ > __Folder__ (or click __Create__ > __Folder__ in the top-left of the Project window). 


![](../uploads/Main/AppThinning-CreateFolder.png)


 Select the Asset files you wish to add to the AssetBundle, then drag-and-drop them into your new folder.

When creating an AssetBundle, you need to assign it a tag, which is used as an identifier when requesting the AssetBundle download. It’s good practice to create a label that matches the bundle’s filename; this ensure a unique label, and makes it easier to recognise when you are working with it later.

To create or assign a tag, select your new folder and navigate to the Asset Labels section at the bottom of the Inspector window. Click the left-hand drop-down menu, select __New__, and enter the name of your new label. Note that AssetBundle labels must be lower-case.


![](../uploads/Main/AppThinning-AssetLabels.png)


In this example, a folder called _Textures_ is being given a new label

To generate the new AssetBundle file, the new label must be referenced in an Editor script. To create an Editor script, create a new folder inside the Project window called _Editor_. Right-click on the _Editor_ folder and select __Create__ > __C# Script__. Name the new script __BuildiOSAssetBundles.cs__.

![](../uploads/Main/AppThinning-NewScript.png)

In this example, the _Textures_ folder has been given the label _textures_. The new script __BuildiOSAssetBundles.cs__ has been created in the _Editor_ folder.

Open __BuildiOSAssetBundles.cs__ and copy in the code below. In this example, the label _textures_ has been used; change this throughout with the name of your label (in lower-case text).

Note that this example uses uncompressed bundles; however, this is not a requirement for app thinning.

````
using UnityEngine;
using UnityEditor;


public class BuildiOSAssetBundles : MonoBehaviour
{
    [InitializeOnLoadMethod]
    static void SetupResourcesBuild( )
    {
        UnityEditor.iOS.BuildPipeline.collectResources += CollectResources;
    }

    static UnityEditor.iOS.Resource[] CollectResources( )
    {
        return new UnityEditor.iOS.Resource[]
        {
            new UnityEditor.iOS.Resource( "textures", "Assets/ODR/textures" ).AddOnDemandResourceTags( "textures" ),
            new UnityEditor.iOS.Resource( "bundle", "Assets/Bundles/bundle.unity3d" ).AddOnDemandResourceTags( "bundle" ),
        };
    }

    [MenuItem( "Bundle/Build iOS AssetBundle" )]
    static void BuildAssetBundles( )
    {
        var options = BuildAssetBundleOptions.None;

        bool shouldCheckODR = EditorUserBuildSettings.activeBuildTarget == BuildTarget.iOS;

#if UNITY_TVOS
            shouldCheckODR |= EditorUserBuildSettings.activeBuildTarget == BuildTarget.tvOS;
#endif

        if( shouldCheckODR )
        {
#if ENABLE_IOS_ON_DEMAND_RESOURCES
            if( PlayerSettings.iOS.useOnDemandResources )
                options |= BuildAssetBundleOptions.UncompressedAssetBundle;
#endif

#if ENABLE_IOS_APP_SLICING
            options |= BuildAssetBundleOptions.UncompressedAssetBundle;
#endif
        }

        BuildPipeline.BuildAssetBundles( "Assets/ODR", options, EditorUserBuildSettings.activeBuildTarget );
    }

}
````


The significant line in the above code sample is the following, which takes the files tagged with the _textures_ label and creates an AssetBundle file called _textures_ within the Assets/ODR folder:

````
new UnityEditor.iOS.Resource( "textures", "Assets/ODR/textures" ).AddOnDemandResourceTags( "textures" )
````

For demonstration purposes, the above code sample also includes the following line, which adds an AssetBundle called _bundle_ that has already been built - for example, from another project or a third-party vendor:

````
new UnityEditor.iOS.Resource( "bundle", "Assets/Bundles/bundle.unity3d" ).AddOnDemandResourceTags( "bundle" )
````


The whole code sample creates a new menu in the Unity Editor menu bar. Go to __Bundle__ > __Build iOS  AssetBundle__. This generates the AssetBundles in the _ODR_ folder.

![](../uploads/Main/AppThinning-BuildiOSAssetBundle.png)

The following script downloads the _textures_ ODR Asset Bundle, assigning it to the public member __TextureBundle__. Place this somewhere in your project.

````
using UnityEngine;
using UnityEngine.iOS;
using System;
using System.Collections;

public class LoadBundle : MonoBehaviour
{
    public AssetBundle     TextureBundle;


    void Start( )
    {
        StartCoroutine( LoadAsset( "textures", "textures" ) );
    }

    public IEnumerator LoadAsset( string resourceName, string odrTag )
    {
        // Create the request
        OnDemandResourcesRequest request = OnDemandResources.PreloadAsync( new string[] { odrTag } );

        // Wait until request is completed
        yield return request;

        // Check for errors
        if( request.error != null )
            throw new Exception( "ODR request failed: " + request.error );

        TextureBundle = AssetBundle.LoadFromFile( "res://" + resourceName );

        request.Dispose( );
    }
}
````


The next step is to generate an Xcode project, build an .IPA, and upload it to iTunes Connect’s TestFlight. As part of the TestFlight processing procedure, the embedded ODR AssetBundles are removed from the app and hosted on Apple’s servers, ready for downloading.

Before you build the .IPA in Xcode, check in XCode’s Build Settings to make sure that in the Assets section, __Embed Asset packs In Product Bundle__ is set to __No__, and __Enable On Demand Resources__ is set to __Yes__.

![](../uploads/Main/AppThinning-XCodeBuildSettings.png)

Once iTunes Connect has finished processing your app upload, click on the build version in TestFlight Builds to see more information about it:

![](../uploads/Main/AppThinning-XCodeTestFlightBuilds.png)

<a name="AppSlicing"></a>
## App slicing

App slicing follows a similar process to on-demand resourcing, allowing you to dynamically download Assets according the specification of the device the app is running on (for example, to download high-resolution Assets for retina iPads, and low-resolution Assets for smaller devices like iPhones and the iPad Mini). This is achieved by defining AssetBundles, with the added provision of __variants__. This way, you can decide at startup which variant to use, and automatically append this to the Asset file name upon download.

To create a Variant, click on your new folder and navigate to the __Asset Labels__ section at the bottom of the Inspector window. Click on the right-hand drop-down menu, select __New__, and enter the name of your new variant. Note that AssetBundle variants must be lower-case.

![](../uploads/Main/AppThinning-AssetBundleVariants.png)

The new variant must be referenced in an Editor script. To create an Editor script, create a new folder inside the Project window called _Editor_. Right-click on the Editor folder and select __Create__ > __C# Script__. Name the new script __BuildiOSAppSlices.cs__.

Copy and paste the code below, replacing the example label ("textures") and variants (“hd” and “sd”) with your own. In this code example, multiple folders are referred to: one containing HD textures, and one containing SD textures. These have been given the variants “hd” and “sd” respectively.

````
using UnityEngine;
using UnityEditor;


public class BuildiOSAppSlices : MonoBehaviour
{
	[InitializeOnLoadMethod]
	static void SetupResourcesBuild( )
	{
		UnityEditor.iOS.BuildPipeline.collectResources += CollectResources;
	}

	static UnityEditor.iOS.Resource[] CollectResources( )
	{
		return new UnityEditor.iOS.Resource[]
		{
			new UnityEditor.iOS.Resource("textures").BindVariant( "Assets/ODR/textures.hd", "hd" )
                                     .BindVariant( "Assets/ODR/textures.sd", "sd" )
					 .AddOnDemandResourceTags( "textures" ),
	};
	}

	[MenuItem( "Bundle/Build iOS App Slices" )]
	static void BuildAssetBundles( )
	{
		var options = BuildAssetBundleOptions.None;

		bool shouldCheckODR = EditorUserBuildSettings.activeBuildTarget == BuildTarget.iOS;

#if UNITY_TVOS
            shouldCheckODR |= EditorUserBuildSettings.activeBuildTarget == BuildTarget.tvOS;
#endif

		if( shouldCheckODR )
		{
#if ENABLE_IOS_ON_DEMAND_RESOURCES
			if( PlayerSettings.iOS.useOnDemandResources )
				options |= BuildAssetBundleOptions.UncompressedAssetBundle;
#endif

#if ENABLE_IOS_APP_SLICING
			options |= BuildAssetBundleOptions.UncompressedAssetBundle;
#endif
		}

		BuildPipeline.BuildAssetBundles( "Assets/ODR", options, EditorUserBuildSettings.activeBuildTarget );
	}

}
````

This creates a new menu in the Unity Editor menu bar called __Bundle__. Click this and select the only item in the list, __Build iOS App Slices__. This generates the AssetBundles in the __ODR__ folder.

Then, to load an Asset, place this class somewhere in your project and pass in the name of the Variant you wish to load:

````
using UnityEngine;
using UnityEngine.iOS;
using System;
using System.Collections;

public class LoadBundle : MonoBehaviour
{
    public AssetBundle     TextureBundle;


    void Start( )
    {
        StartCoroutine( LoadAsset( "textures.hd", "textures" ) );
    }

    public IEnumerator LoadAsset( string resourceName, string odrTag )
    {
        // Create the request
        OnDemandResourcesRequest request = OnDemandResources.PreloadAsync( new string[] { odrTag } );

        // Wait until request is completed
        yield return request;

        // Check for errors
        if( request.error != null )
            throw new Exception( "ODR request failed: " + request.error );

        TextureBundle = AssetBundle.LoadFromFile( "res://" + resourceName );

        request.Dispose( );
    }
}
````

You can now see and modify variants in __Player Settings > Other Settings > Configuration__ in a __Variant map for app slicing__ drop-down menu (the menu is only visible if __Use on demand resources__ is enabled in the Player Settings).

![](../uploads/Main/AppThinning-VariantMap.png)


To learn more about AssetBundles and on-demand resources, see Unity’s [AssetBundle Manager demo project](https://bitbucket.org/Unity-Technologies/assetbundledemo), hosted on Bitbucket. The landing page offers a comprehensive description of how to use and tweak the demo.

---

* <span class="page-edit">2018-06-14  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
