3D 纹理
===========

只能从脚本创建 3D 纹理。以下代码片段将创建 3D 纹理，其中每个轴 X、Y 和 Z 对应于红色、绿色和蓝色的颜色值。


````
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Example : MonoBehaviour
{

	Texture3D texture;

	void Start ()
	{
		texture = CreateTexture3D (256);
	}

	Texture3D CreateTexture3D (int size)
	{
		Color[] colorArray = new Color[size * size * size];
		texture = new Texture3D (size, size, size, TextureFormat.RGBA32, true);
		float r = 1.0f / (size - 1.0f);
		for (int x = 0; x < size; x++) {
			for (int y = 0; y < size; y++) {
				for (int z = 0; z < size; z++) {
					Color c = new Color (x * r, y * r, z * r, 1.0f);
					colorArray[x + (y * size) + (z * size * size)] = c;
				}
			}
		}
		texture.SetPixels (colorArray);
		texture.Apply ();
		return texture;
	}
		
}


````
