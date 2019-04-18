Preparing your application for In-App Purchases (IAP)
=================================================


This chapter does **not** describe how to integrate your game with Apple's _StoreKit_ API. It is assumed that you already have integration with StoreKit via a [native code plugin](Plugins.html).

The Apple StoreKit documentation defines four kinds of products that can be sold via the _In-App Purchase_ process: 

* Content
* Functionality
* Services
* Subscriptions

This chapter covers the first case only and focuses mainly on the downloadable content concept. [AssetBundles](../ScriptReference/AssetBundle.html) are recommended for implementing downloadable content in Unity and both the creation and runtime usage of AssetBundles will be covered here.

Exporting your assets for use on iOS
------------------------------------

It is sometimes useful to maintain separate projects for your main application and the downloadable AssetBundles that it will use. However, you should note that all _scripts_ referenced by objects in the AssetBundle must be present in the main game executable. The project you use to create the AssetBundle must have iOS as the selected build target since the content of AssetBundle files is not compatible between iOS and other platforms.

AssetBundles are created using editor scripts - a simple example is given below:


````
using UnityEngine;
using UnityEditor;

public class ExportBundle : MonoBehaviour {
	[MenuItem ("Assets/Build AssetBundle From Selection - Track dependencies")]
	static void DoExport() {
		string str = EditorUtility.SaveFilePanel("Save Bundle...", Application.dataPath, Selection.activeObject.name, "assetbundle");
		if (str.Length != 0) {
			BuildPipeline.BuildAssetBundle(Selection.activeObject, Selection.objects, str, BuildAssetBundleOptions.CompleteAssets, BuildTarget.iPhone);
		}
	}
}
````

You should save this code in a file called _ExportBundle_ and place it inside a folder called _Editor_ (you can just create this if it isn't already present in the project). The script will add a menu item entitled _Build AssetBundle From Selection - Track dependencies_ on the _Assets_ menu in the editor.

The content you want to include in the bundle should be prepared in the form of prefabs. Select a prefab in the Project view and then select _Assets &gt; Build AssetBundle From Selection - Track dependencies_ (i.e. the menu item added by the ExportBundle script). This command will bring up a save dialog where you can choose the name and location of your AssetBundle file.


Downloading your assets on iOS
------------------------------


**Note:** Apple may change the folder locations where you are permitted to write data. Always check the latest Apple documentation to be sure your application will be compliant. The following advice was correct as of early 2018.

AssetBundles can be downloaded using the [WWW class](../ScriptReference/WWW.html) and once transfer is complete, the enclosed assets can be accessed. The recommended way to download an AssetBundle is to use [LoadFromCacheOrDownload](../ScriptReference/WWW.LoadFromCacheOrDownload.html) as shown in the following sample:


````
IEnumerator GetAssetBundle() {
	WWW download;
	string url = "http://somehost/somepath/someassetbundle.assetbundle";

	while (!Caching.ready)
		yield return null;

	download = WWW.LoadFromCacheOrDownload(url, 0);

	yield return download;

	AssetBundle assetBundle = download.assetBundle;
	if (assetBundle != null) {
		// Alternatively you can also load an asset by name (assetBundle.Load("my asset name"))
		Object go = assetBundle.mainAsset;
			
		if (go != null)
			Instantiate(go);
		else
			Debug.Log("Couldn't load resource");	
	} else {
		Debug.Log("Couldn't load resource");	
	}
}
````

The downloaded asset bundle files are stored in the _Library_ folder of the iOS application sandbox and have the _No Backup_ flag set on them. This means the OS won't delete these files accidentally and these files won't be backed up to iCloud.

If you need to choose exactly where the AssetBundle file is stored, you can use a standard WWW download (i.e. just use the constructor instead of LoadFromCacheOrDownload) and then save the downloaded data on disk using the .NET file API. You can save required files to the [Application.temporaryCachePath](../ScriptReference/Application-temporaryCachePath.html) folder (stored in _Library/Caches_ which is regularly "cleaned out" by the OS) or the [Application.persistentDataPath](../ScriptReference/Application-persistentDataPath.html) folder (stored in _Documents_ which is not cleaned out by the OS). You should set the _No Backup_ flag on these files with [iOS.Device.SetNoBackupFlag](../ScriptReference/iOS.Device.SetNoBackupFlag.html) to prevent them being backed up to iCloud.

**Note:** If you don't set the _No Backup_ flag, your app may be rejected when you submit it to the App Store.

You can access saved files by creating a WWW object with a URL in the form `file:///pathtoyourapplication/Library/savedassetbundle.assetbundle`:-



````
string cachedAssetBundle = Application.temporaryCachePath + "/savedassetbundle.assetbundle"; 
System.IO.FileStream cache = new System.IO.FileStream(cachedAssetBundle, System.IO.FileMode.Create);
cache.Write(download.bytes, 0, download.bytes.Length);
cache.Close();

iOS.Device.SetNoBackupFlag(cachedAssetBundle);

Debug.Log("Cache saved: " + cachedAssetBundle);
````


**Note:** You can test the reading of stored files in the Documents folder if you enable _file sharing_ (setting `UIFileSharingEnabled` to true in your `Info.plist` allows you to access the _Documents_ folder from iTunes). Note that the contents of the Documents folder are cached to iCloud, so you should not use this location to store AssetBundles in the final build to be shipped. See [File System Basics](http://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/FileSystemOverview/FileSystemOverview.html) in the Apple iOS documentation for further details.

---

* <span class="page-edit">2018-06-14  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
