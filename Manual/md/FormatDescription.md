格式的描述
=========================


Unity 的场景格式是用 YAML 数据序列化语言实现的。YAML 是一种开放格式，但我们无法在此对其进行深入介绍，可在 [YAML 网站](http://yaml.org/spec/1.2/spec.html)中免费获取此格式的规范。场景中的每个对象都作为单独 YAML 文档写入到文件（通过 --- 序列加入到文件中）。注意，在此背景下，术语“对象”通指游戏对象、组件和其他场景数据；每一项都需要在场景文件中有自己的 YAML 文档。可以从以下示例中理解序列化对象的基本结构：



````
--- !u!1 &6
GameObject:
  m_ObjectHideFlags: 0
  m_PrefabParentObject: {fileID: 0}
  m_PrefabInternal: {fileID: 0}
  importerVersion: 3
  m_Component:
  - 4: {fileID: 8}
  - 33: {fileID: 12}
  - 65: {fileID: 13}
  - 23: {fileID: 11}
  m_Layer: 0
  m_Name: Cube
  m_TagString: Untagged
  m_Icon: {fileID: 0}
  m_NavMeshLayer: 0
  m_StaticEditorFlags: 0
  m_IsActive: 1

````

第一行在文档标记后面包含字符串“!u!1 &6”。“!u!”部分之后的第一个数字表示对象的类（在此示例中表示游戏对象）。＆ 符号后面的数字是文件中非重复的对象 ID 号，但该数字是任意分配给每个对象的。每个对象的可序列化属性由如下行表示：


	 m_Name: Cube

属性通常以“m_”为前缀，但其他部分遵循脚本参考中定义的属性名称。在文件中接下来定义的第二个对象可能如下所示：



````
--- !u!4 &8
Transform:
  m_ObjectHideFlags: 0
  m_PrefabParentObject: {fileID: 0}
  m_PrefabInternal: {fileID: 0}
  m_GameObject: {fileID: 6}
  m_LocalRotation: {x: 0.000000, y: 0.000000, z: 0.000000, w: 1.000000}
  m_LocalPosition: {x: -2.618721, y: 1.028581, z: 1.131627}
  m_LocalScale: {x: 1.000000, y: 1.000000, z: 1.000000}
  m_Children: []
  m_Father: {fileID: 0}

````

这是一个附加到由上面 YAML 文档定义的游戏对象的变换组件。附加方式用以下行表示：


	 m_GameObject: {fileID: 6}

...因为游戏对象在文件中的对象 ID 是 6。

浮点数可以用十进制形式表示，也可以用 IEEE 754 格式的十六进制数表示（用 0x 前缀表示）。IEEE 754 表示法可对值进行无损编码。在写入没有短十进制表示的浮点值时，Unity 会使用这种表示法。Unity 以十六进制写入数字时，总是会在括号中也写入十进制格式以便于调试，但在加载文件时实际只解析十六进制。如果要手动编辑此类值，只需删除十六进制，然后仅输入十进制数。以下是一些浮点值（均代表数字 1）的有效表示：



````
myValue: 0x3F800000
myValue: 1
myValue: 1.000
myValue: 0x3f800000(1)
myValue: 0.1e1

````
