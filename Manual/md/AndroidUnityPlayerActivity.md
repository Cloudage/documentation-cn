#扩展 UnityPlayerActivity Java 代码

在开发 Unity Android 应用程序时，可使用插件来扩展标准的 UnityPlayerActivity 类（这是 Android 上的 Unity 播放器的主 Java 类，类似于 Unity iOS 上的 AppController.mm）。应用程序可以覆盖 Android 操作系统和 Unity Android 应用程序之间的任何及所有基本交互。


需要通过两个步骤来覆盖默认活动：

* 创建从 UnityPlayerActivity 派生的新[活动](http://developer.android.com/reference/android/app/Activity.html)；

* 修改 [Android 清单](android-manifest.html)以将新活动作为应用程序的入口点。


为实现此目的，最简单的方法是导出项目，并在 Android Studio 中对 UnityPlayerActivity 类进行必要的修改。

要用新活动代码创建插件并将其添加到 Unity 项目，必须执行以下步骤：

1.扩展 UnityPlayerActivity。UnityPlayerActivity.java 文件位于 Mac 上的 _/Applications/Unity/Unity.app/Contents/PlaybackEngines/AndroidPlayer/src/com/unity3d/player_ 和 Windows 上的 _C:\Program Files\Unity\Editor\Data\PlaybackEngines\AndroidPlayer\src\com\unity3d\player_。要扩展 UnityPlayerActivity，请找到 Unity 附带的 classes.jar。该文件位于安装文件夹（通常为 C:\Program Files\Unity\Editor\Data（Windows 上）或 /Applications/Unity（Mac 上））的子文件夹中，子文件夹名为 PlaybackEngines/AndroidPlayer/Variations/mono 或者是 il2cpp/Development 或 Release/Classes/。然后将 classes.jar 添加到用于编译新活动的类路径中。编译活动源文件并将其打包到 JAR 或 AAR 包中，然后将其复制到项目文件夹。


2.创建新的 Android 清单以将新活动设置为应用程序的入口点。将 AndroidManifest.xml 文件放在项目的 Assets/Plugins/Android 文件夹中。


以下是 UnityPlayerActivity 文件的示例：

```
OverrideExample.java:
package com.company.product;
import com.unity3d.player.UnityPlayerActivity;
import android.os.Bundle;
import android.util.Log;

public class OverrideExample extends UnityPlayerActivity {
  protected void onCreate(Bundle savedInstanceState) {
    // 调用 UnityPlayerActivity.onCreate()
    super.onCreate(savedInstanceState);
    // 将调试消息打印至 logcat
    Log.d("OverrideActivity", "onCreate called!");
  }
  public void onBackPressed()
  {
    // 不调用 UnityPlayerActivity.onBackPressed()，而是直接忽略 Back 按钮事件
    // super.onBackPressed();
  }
}
```

```
以下便是相应 AndroidManifest.xml 文件的内容：
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.company.product">
  <application android:icon="@drawable/app_icon" android:label="@string/app_name">
    <activity android:name=".OverrideExample"
             android:label="@string/app_name"
             android:configChanges="fontScale|keyboard|keyboardHidden|locale|mnc|mcc|navigation|orientation|screenLayout|screenSize|smallestScreenSize|uiMode|touchscreen">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
  </application>
</manifest>
```

<br/> 

----

* <span class="page-edit">2017-05-18  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.5 版中的更新功能</span>
