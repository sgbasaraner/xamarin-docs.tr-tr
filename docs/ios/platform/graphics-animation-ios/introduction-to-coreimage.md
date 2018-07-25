---
title: Xamarin.iOS, temel görüntü
description: Temel görüntü, görüntü işleme sağlamak ve canlı video geliştirme işlevleri için 5 iOS ile tanıtılan yeni bir çerçevedir. Bu makalede, Xamarin.iOS örnekleri ile bu özellikler sunar.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 7af57856079813e8cb1831a7f22a0a098a6be771
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242172"
---
# <a name="core-image-in-xamarinios"></a>Xamarin.iOS, temel görüntü

_Temel görüntü, görüntü işleme sağlamak ve canlı video geliştirme işlevleri için 5 iOS ile tanıtılan yeni bir çerçevedir. Bu makalede, Xamarin.iOS örnekleri ile bu özellikler sunar._

Temel görüntü, bir dizi yerleşik filtreler ve görüntüler ve videolar, yüz algılama dahil olmak üzere uygulanacak efektleri sağlayan bir iOS 5 sürümünde yeni bir çerçevedir.

Bu belge, basit örnekleri içerir:

-  Yüz algılama.
-  Bir görüntü için filtreler uygulanıyor
-  Kullanılabilir filtrelerin listesi.


Bu örnekler Xamarin.iOS uygulamalarınıza temel görüntü özellikleri başlamanıza yardımcı olacaktır.

## <a name="requirements"></a>Gereksinimler

En yeni Xcode sürümünü kullanmanız gerekir.

## <a name="face-detection"></a>Yüz algılama

Temel görüntü yüz algılama özelliği, yalnızca ne diyor yapar: Fotoğraf yüzlerini tanımlamaya çalışır ve tanıdığı tüm yüzleri koordinatlarını döndürür. Bu bilgiler görüntü kişilerin sayısını göstergeleri (ör. görüntü üzerinde çizmek için kullanılabilir 'kişiler hello'nun etiketleme için'), veya'nın aklınıza.

Bu koddan CoreImage\SampleCode.cs, oluşturma ve yüz algılama katıştırılmış bir resim üzerinde kullanma gösterilmektedir:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Özellik dizisi ile doldurulur `CIFaceFeature` (tüm yüzleri algılanırsa) nesneleri. Var olan bir `CIFaceFeature` her yüz için. `CIFaceFeature` aşağıdaki özelliklere sahiptir:

-  HasMouthPosition – için bu yüz ağız olup olmadığı algılandı.
-  HasLeftEyePosition – Bu yüz için sol göz olup olmadığı algılandı.
-  HasRightEyePosition – Bu yüz için doğru göz olup olmadığı algılandı. 
-  MouthPosition – Bu yüz için ağız koordinatları.
-  LeftEyePosition – Bu yüz için sol göz koordinatları.
-  RightEyePosition – Bu yüz için doğru göz koordinatları.


Koordinatları tüm bu özellikler için sol alt – sol üst kaynağı kullandığı Uıkit aksine kaynağa sahip. Koordinatları kullanırken `CIFaceFeature` 'bunları çevirmek ' emin olun. Bu çok basit bir özel görüntü görünümde CoreImage\CoreImageViewController.cs 'yüzey gösterge' üçgenler resmi çizim gösterilmektedir (Not `FlipForBottomOrigin` yöntemi):

```csharp
public class FaceDetectImageView : UIView
{
    public Xamarin.iOS.CoreImage.CIFeature[] Features;
    public UIImage Image;
    public FaceDetectImageView (RectangleF rect) : base(rect) {}
    CGPath path;
    public override void Draw (RectangleF rect) {
        base.Draw (rect);
        if (Image != null)
            Image.Draw(rect);

        using (var context = UIGraphics.GetCurrentContext()) {
            context.SetLineWidth(4);
            UIColor.Red.SetStroke ();
            UIColor.Clear.SetFill ();
            if (Features != null) {
                foreach (var feature in Features) { // for each face
                    var facefeature = (CIFaceFeature)feature;
                    path = new CGPath ();
                    path.AddLines(new PointF[]{ // assumes all 3 features found
                        FlipForBottomOrigin(facefeature.LeftEyePosition, 200),
                        FlipForBottomOrigin(facefeature.RightEyePosition, 200),
                        FlipForBottomOrigin(facefeature.MouthPosition, 200)
                    });
                    path.CloseSubpath();
                    context.AddPath(path);
                    context.DrawPath(CGPathDrawingMode.FillStroke);
                }
            }
        }
    }
    /// <summary>
    /// Face recognition coordinates have their origin in the bottom-left
    /// but we are drawing with the origin in the top-left, so "flip" the point
    /// </summary>
    PointF FlipForBottomOrigin (PointF point, int height)
    {
        return new PointF(point.X, height - point.Y);
    }
}
```

Görüntüyü yeniden önce SampleCode.cs dosyasında görüntü ve özelliklerini atanır:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

Ekran görüntüsünde örnek çıktı gösterilmektedir: algılanan yüz özellikleri konumlarını içinde bir UITextView görüntülenir ve CoreGraphics kullanarak kaynak görüntünün çizilir.

Yüz tanıma çalıştığını şekli nedeniyle (örneğin, bu çocuğunun monkeys!) İnsan yüzlerini yanı sıra şeyler bazen algılar.

## <a name="filters"></a>FilTReleri

50'den fazla farklı yerleşik filtreler vardır ve böylece yeni filtreler uygulanabilir genişletilebilir bir çerçevedir.

## <a name="using-filters"></a>Filtreleri kullanma

Görüntünün bir filtre uygulanıyor dört ayrı adımdan oluşur: yükleme görüntüsünü, filtresi oluşturma, filtre uygulama ve kaydetme (veya görüntüleme) sonucu.

Görüntüye ilk olarak, yükleme bir `CIImage` nesne.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

İkinci olarak, filtre sınıfı oluşturun ve özelliklerini ayarlayın.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

Üçüncü olarak, erişim `OutputImage` özelliği ve çağrı `CreateCGImage` sonucunu işlemek için yöntemi.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Son olarak, görüntü, sonuçları görmek için bir görünüm atayın. Gerçek bir uygulamada elde edilen görüntü dosya sistemi, fotoğraf albümü, Tweet veya e-posta için kaydedilmiş olabilir.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Bu ekran görüntüleri sonucu göster `CISepia` ve `CIHueAdjust` CoreImage.zip içinde gösterilen filtreleri örnek kodu.

Bkz: [sözleşme ayarlamak ve görüntü tarif parlaklığını](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) ilişkin bir örnek `CIColorControls` filtre.

```csharp
var uiimage = UIImage.FromFile("photo.JPG");
var ciimage = new CIImage(uiimage);
var hueAdjust = new CIHueAdjust();   // first filter
hueAdjust.Image = ciimage;
hueAdjust.Angle = 2.094f;
var sepia = new CISepiaTone();       // second filter
sepia.Image = hueAdjust.OutputImage; // output from last filter, input to this one
sepia.Intensity = 0.3f;
CIFilter color = new CIColorControls() { // third filter
    Saturation = 2,
    Brightness = 1,
    Contrast = 3,
    Image = sepia.OutputImage    // output from last filter, input to this one
};
var output = color.OutputImage;
var context = CIContext.FromOptions(null);
// ONLY when CreateCGImage is called do all the effects get rendered
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

```csharp
var context = CIContext.FromOptions (null);
```

```csharp
var context = CIContext.FromOptions(new CIContextOptions() {
    UseSoftwareRenderer = true  // CPU
});
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

### <a name="listing-filters-and-their-properties"></a>Filtreler ve özelliklerini listeleme

Bu koddan CoreImage\SampleCode.cs yerleşik filtreleri tam listesi ve bunların parametrelerini çıkarır.

```csharp
var filters = CIFilter.FilterNamesInCategories(new string[0]);
foreach (var filter in filters){
   display.Text += filter +"\n";
   var f = CIFilter.FromName (filter);
   foreach (var key in f.InputKeys){
     var attributes = (NSDictionary)f.Attributes[new NSString(key)];
     var attributeClass = attributes[new NSString("CIAttributeClass")];
     display.Text += "   " + key;
     display.Text += "   " + attributeClass + "\n";
   }
}
```

[CIFilter sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) 50 yerleşik filtreleri ve özelliklerini açıklar. Yukarıdaki kodu kullanarak parametrelerinin varsayılan değerleri ve (girişler, Filtre uygulanmadan önce doğrulamak için kullanılabilir) maksimum ve minimum izin verilen değerleri dahil olmak üzere filtre sınıfları sorgulayabilirsiniz.

Kategorileri listeleme çıkış simulator'da şöyle görünür: tüm filtreleri ve parametreleri görmek için listede gezinebilirsiniz.

 [![](introduction-to-coreimage-images/coreimage05.png "Kategorileri listeleme çıktı simulator'da şu şekilde görünür")](introduction-to-coreimage-images/coreimage05.png#lightbox)

Xamarin.iOS.CoreImage API bütünleştirilmiş kod tarayıcı veya otomatik tamamlama Mac için Visual Studio veya Visual Studio kullanarak da keşfedebilirsiniz için listelenen her bir filtrenin Xamarin.iOS, bir sınıfta olarak gösterilen. 

## <a name="summary"></a>Özet

Bu makalede, yüz algılama ve görüntünün filtreler uygulayarak gibi yeni iOS 5 temel görüntü framework özelliklerinden bazıları kullanmayı göstermiştir. Framework'te kullanabilmeniz için kullanılabilen farklı resmi filtreleri onlarca vardır.

## <a name="related-links"></a>İlgili bağlantılar

- [Temel görüntü (örnek)](https://developer.xamarin.com/samples/CoreImage/)
- [Sözleşme ve görüntü tarif parlaklığını ayarla](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [Temel görüntü filtrelerini kullanma](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [CIFilter sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
