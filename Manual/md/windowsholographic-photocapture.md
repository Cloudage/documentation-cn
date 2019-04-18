#HoloLens 照片捕捉
<!-- https://trello.com/c/Qw7imxOL -->
使用 [PhotoCapture](../ScriptReference/XR.WSA.WebCam.PhotoCapture.html) API 可从 HoloLens 网络摄像头拍照。必须启用__网络摄像头 (WebCam)__ 和__麦克风 (Microphone)__ 功能才能使用 PhotoCapture API。以下示例将演示如何使用 PhotoCapture 功能拍摄照片并将照片显示在 Unity [游戏对象](../ScriptReference/GameObject.html)上。

````
using UnityEngine;
using System.Collections;
using System.Linq;
using UnityEngine.XR.WSA.WebCam;

public class PhotoCaptureExample : MonoBehaviour {
    PhotoCapture photoCaptureObject = null;
    Texture2D targetTexture = null;

    // 使用此函数进行初始化
    void Start() {
        Resolution cameraResolution = PhotoCapture.SupportedResolutions.OrderByDescending((res) => res.width * res.height).First();
        targetTexture = new Texture2D(cameraResolution.width, cameraResolution.height);

        // 创建 PhotoCapture 对象
        PhotoCapture.CreateAsync(false, delegate (PhotoCapture captureObject) {
            photoCaptureObject = captureObject;
            CameraParameters cameraParameters = new CameraParameters();
            cameraParameters.hologramOpacity = 0.0f;
            cameraParameters.cameraResolutionWidth = cameraResolution.width;
            cameraParameters.cameraResolutionHeight = cameraResolution.height;
            cameraParameters.pixelFormat = CapturePixelFormat.BGRA32;

            // 激活摄像头
            photoCaptureObject.StartPhotoModeAsync(cameraParameters, delegate (PhotoCapture.PhotoCaptureResult result) {
                // 拍摄照片
                photoCaptureObject.TakePhotoAsync(OnCapturedPhotoToMemory);
            });
        });
    }

    void OnCapturedPhotoToMemory(PhotoCapture.PhotoCaptureResult result, PhotoCaptureFrame photoCaptureFrame) {
        // 将原始图像数据复制到目标纹理中
        photoCaptureFrame.UploadImageDataToTexture(targetTexture);

        // 创建可以应用纹理的游戏对象
        GameObject quad = GameObject.CreatePrimitive(PrimitiveType.Quad);
        Renderer quadRenderer = quad.GetComponent<Renderer>() as Renderer;
        quadRenderer.material = new Material(Shader.Find("Custom/Unlit/UnlitTexture"));

        quad.transform.parent = this.transform;
        quad.transform.localPosition = new Vector3(0.0f, 0.0f, 3.0f);

        quadRenderer.material.SetTexture("_MainTex", targetTexture);

        // 停用摄像头
        photoCaptureObject.StopPhotoModeAsync(OnStoppedPhotoMode);
    }

    void OnStoppedPhotoMode(PhotoCapture.PhotoCaptureResult result) {
        // 关闭照片捕捉资源
        photoCaptureObject.Dispose();
        photoCaptureObject = null;
    }
}
````
##将照片捕捉到内存

将图像捕捉到内存时，将返回 [PhotoCaptureFrame](../ScriptReference/XR.WSA.WebCam.PhotoCaptureFrame.html)。`PhotoCaptureFrame` 包含原生图像数据以及指示图像拍摄位置的空间矩阵。

将图像捕捉到内存可以在着色器中引用捕捉的图像，或将其应用于游戏对象。可通过三种方式从 `PhotoCaptureFrame` 中提取图像数据。如下：

1.以 [Texture2D](../ScriptReference/Texture2D.html) 的形式访问图像数据。这是提取图像数据的最常用方法，因为 Unity 中的大多数组件都了解如何访问 Texture2D 中的图像数据。将图像捕捉到内存后，需要将图像数据上传到 Texture2D。将图像数据上传到 Texture2D 后，就可以开始在材质、脚本或项目的任何其他相关元素中引用该图像数据。Unity API 文档有一个示例演示了如何将照片捕捉到内存，然后将其上传到 Texture2D。请参阅 [WebCam.PhotoCapture.TakePhotoAsync](../ScriptReference/XR.WSA.WebCam.PhotoCapture.TakePhotoAsync.html) 了解如何将照片捕捉到 Texture2D。通过上传命令将图像数据上传到 Texture2D 是开始在 Unity 中处理图像数据的最简单方式。上传操作发生在主线程上。此操作是资源密集型操作，可能会影响项目的性能。

2.以 __WinRT IMFMediaBuffer__ 的形式捕捉原生图像数据。请参阅 Microsoft 的 [IMFMediaBuffer](https://msdn.microsoft.com/en-us/library/windows/desktop/ms696261.aspx) 文档以了解更多信息。通过将一个字节列表传入 [PhotoFrame.CopyRawImageDataIntoBuffer](../ScriptReference/XR.WSA.WebCam.PhotoCaptureFrame.CopyRawImageDataIntoBuffer.html) 函数来创建原生图像数据的副本。请注意，复制操作发生在主线程上。此操作是资源密集型操作，可能会影响项目的性能。

3.如果自行创建插件，或在单独的线程上处理图像数据，可通过 [PhotoFrame.GetUnsafePointerToBuffer](../ScriptReference/XR.WSA.WebCam.PhotoCaptureFrame.GetUnsafePointerToBuffer.html) 函数获取指向原生图像数据的指针。返回的指针是指向 IMFMediaBuffer 组件对象模型 (COM) 的指针。请参阅 Microsoft 的 [IMFMediaBuffer](https://msdn.microsoft.com/en-us/library/windows/desktop/ms696261.aspx) 和[组件对象模型 (Component Object Models)](https://msdn.microsoft.com/en-us/library/windows/desktop/ms694363.aspx) 文档以了解更多信息。调用此函数后，将向 COM 对象添加引用。不再需要资源时，您需要负责释放该引用。
