动画混合形状
======================

准备原始作品
---------------------

在 Maya 中设置好混合形状后：

* 将选择的内容导出到 FBX，确保选中动画复选框并选中变形模型下的混合形状。

* 将 FBX 文件导入 Unity（Assets > Import New Assets > [文件名].fbx）。

* 将资源拖动到 Hierarchy 窗口中。如果在层级视图中选择对象并查看检视面板，则会在 SkinnedMeshRenderer 组件下看到列出的混合形状。在此位置，您可以调整混合形状对默认形状的影响，0 表示混合形状没有影响，100 表示混合形状具有最大影响。

在 Unity 中创建动画
--------------------------

也可以使用 Unity 中的 Animation 窗口来创建混合动画，步骤如下：

* 在 Window > Animation 下打开 Animation 窗口。

* 在窗口左侧，单击“Add Curve”，然后添加 Blend Shape，它将位于 Skinned Mesh Renderer 下。

从此处，您可以操作关键帧和混合权重来创建所需动画。

完成动画编辑后，可在 Editor 窗口或 Animation 窗口中单击 Play 以预览动画。

脚本访问
----------------

也可以使用 GetBlendShapeWeight 和 SetBlendShapeWeight 等函数来通过代码设置混合权重。

您还可以通过访问 blendShapeCount 变量以及其他有用的函数来检查 Mesh 上有多少混合形状。

以下代码示例连接到具有 3 个或更多混合形状的游戏对象时，随着时间推移，会将默认形状与其他两个混合形状混合在一起：

````
//使用 C#
 
using UnityEngine;
using System.Collections;
 
public class BlendShapeExample : MonoBehaviour
{
 
       int blendShapeCount;
       SkinnedMeshRenderer skinnedMeshRenderer;
       Mesh skinnedMesh;
       float blendOne = 0f;
       float blendTwo = 0f;
       float blendSpeed = 1f;
       bool blendOneFinished = false;
 
       void Awake ()
       {
          skinnedMeshRenderer = GetComponent<SkinnedMeshRenderer> ();
          skinnedMesh = GetComponent<SkinnedMeshRenderer> ().sharedMesh;
       }
 
       void Start ()
       {
          blendShapeCount = skinnedMesh.blendShapeCount; 
       }
 
       void Update ()
       {
          if (blendShapeCount > 2) {
 
                 if (blendOne < 100f) {
                    skinnedMeshRenderer.SetBlendShapeWeight (0, blendOne);
                    blendOne += blendSpeed;
                 } else {
                    blendOneFinished = true;
                 }
 
                 if (blendOneFinished == true && blendTwo < 100f) {
                    skinnedMeshRenderer.SetBlendShapeWeight (1, blendTwo);
                    blendTwo += blendSpeed;
                 }
 
          }
       }
}
````

