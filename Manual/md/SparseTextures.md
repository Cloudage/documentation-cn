#稀疏纹理

__稀疏纹理 (Sparse Textures)__（也称为“区块纹理”或“超级纹理”）是太大而无法完全存入显存的纹理。为了处理它们，Unity 将主纹理分解为更小的矩形部分，称为“区块”(tile)。然后，可根据需要加载各个区块。例如，如果摄像机只能看到稀疏纹理的一小块区域，那么只有当前可见的区块需要加载到内存中。

除了区块特性之外，稀疏纹理的行为与目前所用的任何其他纹理相似。无需进行特殊修改，着色器即可使用它们，而且它们可以有 Mipmap，使用所有纹理过滤模式等。如果由于某种原因无法加载特定的区块，则结果是不明的：有些 GPU 在缺少区块的位置显示黑色区域，但此行为未标准化。

Not all hardware and platforms support sparse textures. For example, on DirectX systems they require [DX11.2](http://msdn.microsoft.com/en-us/library/windows/desktop/dn312084.aspx) (Windows 8.1) and a fairly recent GPU. On OpenGL they require [ARB_sparse_texture](http://www.opengl.org/registry/specs/ARB/sparse_texture.txt) extension support. Sparse textures only support non-compressed texture formats.

请参阅 [SparseTexture](../ScriptReference/SparseTexture.html) 脚本参考页面，了解有关使用脚本处理稀疏纹理的更多详细信息。


##示例项目

[此处](../uploads/Examples/SparseTextureExample.zip)提供了稀疏纹理的一个最小示例项目。

![示例项目中所示的稀疏纹理](../uploads/Main/SparseTextureExample.jpg)
 
该示例显示了一个简单的程序化纹理图案，允许您移动摄像机以查看它的不同部分。请注意，该项目需要最新的 GPU 和 DirectX 11.2 (Windows 8.1) 系统，或者使用支持 _ARB\_sparse\_texture_ 的 OpenGL。
