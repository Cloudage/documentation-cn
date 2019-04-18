# 光照探针：技术信息

光照探针中的光照信息被编码为球谐 (SH) 基函数。我们使用三阶多项式，也称为 L2 球谐函数。它们采用 27 个浮点值进行存储，每个颜色通道使用 9 个浮点值。尽管 Unity 使用 Geomerics 的 Enlighten，但我们使用的 SH 基函数与您在他们博客上看到的不同（y 轴和 z 轴进行了交换）。Unity 使用的是 Peter-Pike Sloan 的论文[愚蠢的球谐函数 (SH) 技巧 (Stupid Spherical Harmonics (SH) Tricks)](http://www.ppsloan.org/publications/StupidSH36.pdf) 中的表示法和重建方法，而 Geomerics 使用的是 Ramamoorthi/Hanrahan 的论文[辐射环境贴图的高效表示 (An Efficient Representation for Irradiance Environment Maps)](http://cseweb.ucsd.edu/~ravir/papers/envmap/envmap.pdf) 中的表示法和重建方法。

用于重建的 Unity 着色器代码可在 UnityCG.cginc 中找到，并使用 Peter-Pikes 论文的“附录 A10 辐射环境贴图的着色器/CPU 代码 (Appendix A10 Shader/CPU code for Irradiance Environment Maps)”中的方法。

数据在内部的排列顺序如下：

```
                    	[L00:  DC]

                    		[L1-1:  y] [L10:   z] [L11:   x]

                    	  [L2-2: xy] [L2-1: yz] [L20:  zz] [L21:  xz]  [L22:  xx - yy]
```
 

R、G 和 B 的 9 个系数的排列顺序如下：

```
L00, L1-1,  L10,  L11, L2-2, L2-1,  L20,  L21,  L22, // 红色通道

L00, L1-1,  L10,  L11, L2-2, L2-1,  L20,  L21,  L22, // 蓝色通道

L00, L1-1,  L10,  L11, L2-2, L2-1,  L20,  L21,  L22  // 绿色通道
```

有关 Unity 光照探针系统的更多背景信息，可阅读 Robert Cupisz 在 2012 年游戏开发者大会 (GDC 2012) 上的演讲报告[“使用四面体曲面细分的光照探针插值 (Light Probe Interpolation Using Tetrahedral Tessellations)”- GDC 2012](http://gdcvault.com/play/1015312/Light-Probe-Interpolation-Using-Tetrahedral)

---

* <span class="page-edit"> 2017-06-08  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 5.6 版更新了光照探针</span>
