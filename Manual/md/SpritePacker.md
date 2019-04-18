#Sprite Packer


在设计精灵图形时，可以方便地为每个角色使用单独的纹理文件。但是，精灵纹理的很大一部分通常会被图形元素之间的空白空间所占据，而此空间将导致运行时浪费视频内存。为了获得最佳性能，最好将几个精灵纹理中的图形紧密地打包在一个称为图集的纹理中。Unity 提供了一个 __Sprite Packer__ 实用程序来自动执行从各个精灵纹理生成图集的过程。

Unity 在后台处理精灵图集纹理的生成和使用，这样用户就不需要进行手动分配。可选择在进入播放模式时或在构建期间打包图集，而图集一旦生成，便会从图集获取精灵对象的图形。

用户需要在纹理导入器 (Texture Importer) 中指定一个打包标签 (Packing Tag)，从而为该纹理的精灵启用打包。


##使用 Sprite Packer

默认情况下已禁用 Sprite Packer，但可从 Editor Settings（菜单：__Edit &gt; Project Settings &gt; Editor__）中对其进行配置。精灵打包模式可从 __Disabled__ 更改为 __Enabled for Builds__（即打包用于构建但不用于播放模式）或 __Always Enabled__（即播放模式和构建都启用打包）。

如果打开 Sprite Packer 窗口（菜单：__Window &gt; Sprite Packer__）并单击左上角的 __Pack__ 按钮，则会显示图集内包含的纹理排列结果。

![](../uploads/Main/SpritePackerMain.png) 

如果在 Project 面板中选择一个精灵，该精灵也会突出显示以显示其在图集内的位置。轮廓实际上是渲染网格轮廓，还定义了用于紧密打包的区域。

Sprite Packer 窗口顶部的工具栏有许多影响打包和查看的控件。__Pack__ 按钮可启动打包操作，但如果图集自上次打包后没有更改，则不会强制进行任何更新。（按__自定义 Sprite Packer__ 中所述实现自定义打包策略时，将出现一个相关的 __Repack__ 按钮）。__View Atlas__ 和 __Page #__ 菜单允许选择在窗口中显示哪个图集的哪个页面（如果没有足够的空间来容纳最大纹理大小内的所有精灵，则单个图集可能拆分成多个“页面”）。页面编号旁边的菜单用于选择对图集使用的“打包策略”（见下文）。在工具栏的右侧有两个控件用于缩放视图以及在图集的颜色和 Alpha 显示之间切换。

##打包策略


Sprite Packer 使用__打包策略__来决定如何将精灵分配到图集。您可以创建自己的打包策略（见下文），但也可以随时使用 __Default Packer Policy__、__Tight Packer Policy__ 或 __Tight Rotate Enabled Sprite Packer Policy__ 选项。通过使用这些策略，[纹理导入器](class-TextureImporter.html)中的 __Packing Tag__ 属性可直接选择将精灵打包到的图集的名称，因此具有相同打包标签 (Packing Tag) 的所有精灵将打包到同一图集内。然后，根据纹理导入设置对图集进行进一步排序，使图集符合用户为源纹理进行的任何设置。具有相同纹理压缩设置的精灵将尽可能分组到相同的图集。

* DefaultPackerPolicy 将默认使用矩形打包方式，除非在 __Packing Tag__ 中指定了“[TIGHT]”（即，将打包标签设置为“[TIGHT]Character”将允许紧密打包）。
* TightPackerPolicy 将默认使用紧密打包（如果精灵具有紧密网格）。如果在 __Packing Tag__ 中指定了“[RECT]”，则将执行矩形打包方式（即，将打包标签设置为“[RECT]UI_Elements”将强制进行矩形打包）。
* TightRotateEnabledSpritePackerPolicy 将默认使用紧密打包（如果精灵具有紧密网格）并会启用精灵旋转。如果在 __Packing Tag__ 中指定了“[RECT]”，则将执行矩形打包方式（即，将打包标签设置为“[RECT]UI_Elements”将强制进行矩形打包）。

##自定义 Sprite Packer


__DefaultPackerPolicy__ 选项足以满足大多数用途，但如果需要，也可以实施自己的自定义打包策略。为此，需要实现编辑器脚本中的一个类的 UnityEditor.Sprites.IPackerPolicy 接口。此接口需要以下方法：

* GetVersion - 返回打包策略的版本值。如果对策略脚本进行了修改，则应该更换版本，并且此策略将保存到版本控制系统。
* OnGroupAtlases - 在此处实现打包逻辑。在 PackerJob 上定义图集，并从给定的 TextureImporter 分配精灵。

DefaultPackerPolicy 默认使用矩形打包（请参阅 SpritePackingMode）。如果正在创建纹理空间效果或想要使用不同的网格来渲染精灵，此设置将非常有用。自定义策略可覆盖此设置，并改用紧密打包。

* 仅在选择自定义策略时才会启用 Repack 按钮。
    * 除非 TextureImporter 元数据或所选的 PackerPolicy 版本值发生变化，否则不会调用 OnGroupAtlases。
    * 仅在实施自定义策略时才应使用 Repack 按钮。
* 可使用 TightRotateEnabledSpritePackerPolicy 自动对精灵打包并旋转。
    * SpritePackingRotation 是保留供未来 Unity 版本使用的类型。


##其他


* 图集缓存在 Project\Library\AtlasCache 中。
    * 删除此文件夹再启动 Unity 将强制对图集重新打包。这样做时必须关闭 Unity。
* 启动时不加载图集缓存。
    * 重新启动 Unity 后首次打包时必须选中所有纹理。此操作可能需要一些时间，具体取决于项目中的纹理总数。
    * 仅加载必需的图集。
* 默认最大图集大小为 2048x2048。
* 如果设置了 PackingTag，则不会压缩纹理，这样 SpritePacker 就可以获取原始像素值，然后对图集进行压缩。

DefaultPackerPolicy
-

````
using System;
using System.Linq;
using UnityEngine;
using UnityEditor;
using System.Collections.Generic;


public class DefaultPackerPolicySample : UnityEditor.Sprites.IPackerPolicy
{
		protected class Entry
		{
			public Sprite            sprite;
		public UnityEditor.Sprites.AtlasSettings settings;
			public string            atlasName;
			public SpritePackingMode packingMode;
			public int               anisoLevel;
		}

		private const uint kDefaultPaddingPower = 3; // 适合基础级别和两个 Mip 级别。

		public virtual int GetVersion() { return 1; }
		protected virtual string TagPrefix { get { return"[TIGHT]"; } }
		protected virtual bool AllowTightWhenTagged { get { return true; } }
		protected virtual bool AllowRotationFlipping { get { return false; } }

	public static bool IsCompressedFormat(TextureFormat fmt)
	{
		if (fmt >= TextureFormat.DXT1 && fmt <= TextureFormat.DXT5)
			return true;
		if (fmt >= TextureFormat.DXT1Crunched && fmt <= TextureFormat.DXT5Crunched)
			return true;
		if (fmt >= TextureFormat.PVRTC_RGB2 && fmt <= TextureFormat.PVRTC_RGBA4)
			return true;
		if (fmt == TextureFormat.ETC_RGB4)
			return true;
		if (fmt >= TextureFormat.ATC_RGB4 && fmt <= TextureFormat.ATC_RGBA8)
			return true;
		if (fmt >= TextureFormat.EAC_R && fmt <= TextureFormat.EAC_RG_SIGNED)
			return true;
		if (fmt >= TextureFormat.ETC2_RGB && fmt <= TextureFormat.ETC2_RGBA8)
			return true;
		if (fmt >= TextureFormat.ASTC_RGB_4x4 && fmt <= TextureFormat.ASTC_RGBA_12x12)
			return true;
		if (fmt >= TextureFormat.DXT1Crunched && fmt <= TextureFormat.DXT5Crunched)
			return true;
		return false;
	}

	public void OnGroupAtlases(BuildTarget target, UnityEditor.Sprites.PackerJob job, int[] textureImporterInstanceIDs)
		{
			List<Entry> entries = new List<Entry>();

			foreach (int instanceID in textureImporterInstanceIDs)
			{
				TextureImporter ti = EditorUtility.InstanceIDToObject(instanceID) as TextureImporter;

				TextureFormat desiredFormat;
				ColorSpace colorSpace;
				int compressionQuality;
				ti.ReadTextureImportInstructions(target, out desiredFormat, out colorSpace, out compressionQuality);

				TextureImporterSettings tis = new TextureImporterSettings();
				ti.ReadTextureSettings(tis);

			Sprite[] sprites =
				AssetDatabase.LoadAllAssetRepresentationsAtPath(ti.assetPath)
					.Select(x => x as Sprite)
					.Where(x => x != null)
					.ToArray();
				foreach (Sprite sprite in sprites)
				{
					Entry entry = new Entry();
					entry.sprite = sprite;
					entry.settings.format = desiredFormat;
					entry.settings.colorSpace = colorSpace;
					// 仅针对压缩格式使用压缩质量进行稍后分组。否则，将其保留为空。
				entry.settings.compressionQuality = IsCompressedFormat(desiredFormat) ? compressionQuality : 0;
				entry.settings.filterMode = Enum.IsDefined(typeof(FilterMode), ti.filterMode)
					? ti.filterMode
					: FilterMode.Bilinear;
					entry.settings.maxWidth = 2048;
					entry.settings.maxHeight = 2048;
					entry.settings.generateMipMaps = ti.mipmapEnabled;
					entry.settings.enableRotation = AllowRotationFlipping;
					if (ti.mipmapEnabled)
						entry.settings.paddingPower = kDefaultPaddingPower;
					else
						entry.settings.paddingPower = (uint)EditorSettings.spritePackerPaddingPower;
			    	#if ENABLE_ANDROID_ATLAS_ETC1_COMPRESSION
                        entry.settings.allowsAlphaSplitting = ti.GetAllowsAlphaSplitting ();
                    #endif //ENABLE_ANDROID_ATLAS_ETC1_COMPRESSION

					entry.atlasName = ParseAtlasName(ti.spritePackingTag);
					entry.packingMode = GetPackingMode(ti.spritePackingTag, tis.spriteMeshType);
					entry.anisoLevel = ti.anisoLevel;

					entries.Add(entry);
				}

				Resources.UnloadAsset(ti);
			}

			// 首先根据图集名称将精灵分成几组
			var atlasGroups =
				from e in entries
				group e by e.atlasName;
			foreach (var atlasGroup in atlasGroups)
			{
				int page = 0;
				// 然后根据纹理设置将这些组拆分为更小的组
				var settingsGroups =
					from t in atlasGroup
					group t by t.settings;
				foreach (var settingsGroup in settingsGroups)
				{
					string atlasName = atlasGroup.Key;
					if (settingsGroups.Count() > 1)
						atlasName += string.Format(" (Group {0})", page);

				UnityEditor.Sprites.AtlasSettings settings = settingsGroup.Key;
					settings.anisoLevel = 1;
					// 使用此图集内所有条目的最高各向异性级别
					if (settings.generateMipMaps)
						foreach (Entry entry in settingsGroup)
							if (entry.anisoLevel > settings.anisoLevel)
								settings.anisoLevel = entry.anisoLevel;

					job.AddAtlas(atlasName, settings);
					foreach (Entry entry in settingsGroup)
					{
						job.AssignToAtlas(atlasName, entry.sprite, entry.packingMode, SpritePackingRotation.None);
					}

					++page;
				}
			}
		}

		protected bool IsTagPrefixed(string packingTag)
		{
			packingTag = packingTag.Trim();
			if (packingTag.Length < TagPrefix.Length)
				return false;
			return (packingTag.Substring(0, TagPrefix.Length) == TagPrefix);
		}

		private string ParseAtlasName(string packingTag)
		{
			string name = packingTag.Trim();
			if (IsTagPrefixed(name))
				name = name.Substring(TagPrefix.Length).Trim();
			return (name.Length == 0) ? "(unnamed)" : name;
		}

		private SpritePackingMode GetPackingMode(string packingTag, SpriteMeshType meshType)
		{
			if (meshType == SpriteMeshType.Tight)
				if (IsTagPrefixed(packingTag) == AllowTightWhenTagged)
					return SpritePackingMode.Tight;
			return SpritePackingMode.Rectangle;
		}
}
````


TightPackerPolicy
-

````
using System;
using System.Linq;
using UnityEngine;
using UnityEditor;
using UnityEditor.Sprites;
using System.Collections.Generic;

// 除非打包标签包含 "[RECT]"，否则 TightPackerPolicy 将对非矩形精灵进行紧密打包。
class TightPackerPolicySample : DefaultPackerPolicySample
{
		protected override string TagPrefix { get { return "[RECT]"; } }
		protected override bool AllowTightWhenTagged { get { return false; } }
		protected override bool AllowRotationFlipping { get { return false; } }
}
````

TightRotateEnabledSpritePackerPolicy
-

````
using System;
using System.Linq;
using UnityEngine;
using UnityEditor;
using UnityEditor.Sprites;
using System.Collections.Generic;

// 除非打包标签包含 "[RECT]"，否则 TightPackerPolicy 将对非矩形精灵进行紧密打包。
class TightRotateEnabledSpritePackerPolicySample : DefaultPackerPolicySample
{
		protected override string TagPrefix { get { return "[RECT]"; } }
		protected override bool AllowTightWhenTagged { get { return false; } }
		protected override bool AllowRotationFlipping { get { return true; } }
}

````
