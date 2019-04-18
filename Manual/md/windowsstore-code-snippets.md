通用 Windows 平台：代码片段
========================


####如何实现“Rate Us”功能？

将以下代码行与按钮点击行为相关联。

````
# if ENABLE_WINMD_SUPPORT
			string productID =  Windows.ApplicationModel.Package.Current.Id.FamilyName;
			Application.OpenURL("ms-windows-store://pdp/?ProductId=" + productID);
# endif
````

---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
