---
title: TextureView
ms.topic: article
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2017
ms.openlocfilehash: d2d9c455f2ddd652a76177527586673901edd012
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="textureview"></a>TextureView

`TextureView` Sınıfı, bir video veya içerik akışı görüntülenecek OpenGL etkinleştirmek için donanım hızlandırılmış 2B işleme kullanan bir görünümü. Örneğin, aşağıdaki ekran gösterilir `TextureView` bir canlı akış cihazın kameradan görüntüleme:

[![Cihazın kameradan canlı görüntüsünün örnek ekran görüntüsü](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

Farklı `SurfaceView` de OpenGL veya video içeriği görüntülemek için kullanılabilir, sınıf TextureView değil işlenen ayrı bir pencerede.
Bu nedenle, `TextureView` görünüm dönüşümleri başka bir görünüm gibi destekleyebilirler. Örneğin, döndürme bir `TextureView` yalnızca ayarlayarak gerçekleştirilebilir kendi `Rotation` özelliği, ayarlayarak saydamlığını kendi `Alpha` özelliği ve benzeri.

Bu nedenle `TextureView` biz artık kameradan canlı akış bu görüntü gibi şeyler ve aşağıdaki kodda gösterildiği gibi Dönüştür:

```csharp
public class TextureViewActivity : Activity,
    TextureView.ISurfaceTextureListener
{
    Camera _camera;
    TextureView _textureView;
       
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        _textureView = new TextureView (this);
        _textureView.SurfaceTextureListener = this;
           
        SetContentView (_textureView);
    }
       
    public void OnSurfaceTextureAvailable (
        Android.Graphics.SurfaceTexture surface,
        int width, int height)
    {
        _camera = Camera.Open ();
        var previewSize = _camera.GetParameters ().PreviewSize;
        _textureView.LayoutParameters =
            new FrameLayout.LayoutParams (previewSize.Width,
                previewSize.Height, (int)GravityFlags.Center);

        try {
            _camera.SetPreviewTexture (surface);
            _camera.StartPreview ();
        } catch (Java.IO.IOException ex) {
            Console.WriteLine (ex.Message);
        }
           
        // this is the sort of thing TextureView enables
        _textureView.Rotation = 45.0f;
        _textureView.Alpha = 0.5f;
    }
    …
}
```

Yukarıdaki kod oluşturur bir `TextureView` etkinliğin örneğinde `OnCreate` yöntemi ve etkinlik olarak ayarlar `TextureView`'s `SurfaceTextureListener`. Olmasını `SurfaceTextureListener`, etkinlik uygulayan `TextureView.ISurfaceTextureListener` arabirimi. Sistem çağıracak `OnSurfaceTextAvailable` yöntemi zaman `SurfaceTexture` kullanıma hazırdır. Bu yöntemde, biz ele `SurfaceTexture` geçirilen ve kameranın Önizleme doku olarak ayarlayın. Biz ayarı gibi normal görünüm temel işlemleri gerçekleştirmek ücretsiz sonra `Rotation` ve `Alpha`, olarak yukarıdaki örnekte. Bir cihazda çalışan elde edilen uygulama aşağıda gösterilmiştir:

[![Bir görüntü görüntüleme bir cihazda çalışan uygulama örneği](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

Kullanılacak `TextureView`, donanım hızlandırmasını etkinleştirilmesi gerekir, API Düzey 14 sürümünden itibaren varsayılan olacak. Ayrıca, bu örnek kamerayı kullandığından hem `android.permission.CAMERA` izni ve `android.hardware.camera` özelliğini ayarlayın, **AndroidManifest.xml**.



## <a name="related-links"></a>İlgili bağlantılar

- [TextureViewDemo (örnek)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
