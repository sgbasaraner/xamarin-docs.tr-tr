---
title: Birleşik API için kod güncelleştirmek için ipuçları
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b23a84c990eb2418e94b414cc9750b3060c572ad
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>Birleşik API için kod güncelleştirmek için ipuçları

Mac, iOS ve Mac projeleri (Xamarin.iOS.dll veya Xamarin.Mac.dll) birleşik API Başvurusu ve çok sayıda gerekli kod değişiklikleri yapmasını dönüştürecek için Visual Studio otomatik geçiş aracı kullanarak (bkz [Xamarin Studio yayın Notlar bölümünde 'Klasik' Unified API kod geçiş aracı için](http://developer.xamarin.com/releases/studio/xamarin.studio_5.7/xamarin.studio_5.7/) belirli Ayrıntılar için).

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>Film şeridi hata NSInvalidArgumentException bulamadı

Var olan bir [hata](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) Visual Studio projenizi birleşik API'lerine dönüştürmek için otomatik geçiş aracı kullandıktan sonra ortaya çıkabilir Mac için geçerli sürümünde. Formda bir hata iletisi alırsanız, güncelleştirmeden sonra:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...

```

Bu sorunu çözmek için aşağıdaki yapı hedef dosyasını bulun aşağıdakileri yapabilirsiniz:

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

Bu dosyada aşağıdaki hedef bildirimi bulmanız gerekir:

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Ve ekleme `DependsOnTargets="_CollectBundleResources"` özniteliği için. Böyle:

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Dosyayı kaydedin, Mac için Visual Studio'yu yeniden başlatın ve temiz yapın ve projenizi yeniden derleyin. Bu sorun için bir düzeltme Xamarin tarafından kısa süre içinde yayımlanması.

## <a name="useful-tips"></a>Faydalı ipuçları

Geçiş Aracı kullanıldıktan sonra el ile müdahale gerektiren bazı derleyici hataları hala alabilirsiniz.
El ile düzeltilmesi gereken bazı noktalar şunlardır:

* Karşılaştırma `enum`s gerektirebilecek bir `(int)` atama.

* `NSDictionary.IntValue` Şimdi döndürür bir `nint`, var olan bir `Int32Value` kullanılabileceği yerine.

* `nfloat` ve `nint` türleri olamaz işaretlenmelidir `const`;   `static readonly nint` makul bir alternatiftir.

* Doğrudan kullanılan şeyler `MonoTouch.` ad alanı genellikle içinde artık `ObjCRuntime.` ad alanı (örn: `MonoTouch.Constants.Version` artık `ObjCRuntime.Constants.Version`).

* Nesneleri serileştiren kodu bölün serileştirmek çalışırken `nint` ve `nfloat` türleri. Seri hale getirme kodunuzu geçişten sonra beklendiği gibi çalıştığını kontrol ettiğinizden emin olun.

* Otomatik aracı içinde kod bazen isabetsiz okuma `#if #else` koşullu derleyici yönergeleri. Bu durumda düzeltmeler el ile yapmanız gerekir (aşağıdaki ortak hataları bakın).

* El ile dışarı aktarılan yöntemlerini kullanarak `[Export]` otomatik olarak geçiş aracı örneğin el ile güncelleştirmeniz gerekir dönüş türü bu kodu snippert tarafından düzeltilebilir değil `nfloat`:

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

 * Kayıpsız dönüştürme olmadığından Unified API NSDate ve .NET DateTime arasında örtük bir dönüştürme sağlamaz. İlgili hataları önlemek için `DateTimeKind.Unspecified` .NET Dönüştür `DateTime` için yerel veya çevrim önce UTC `NSDate`.

 * Objective-C kategori yöntemleri şimdi Unified API uzantı yöntemleri olarak üretilir. Örneğin, daha önce kullanılan kod `UIView.DrawString` şimdi başvuru `NSString.DrawString` Unified API içinde.

 * AVFoundation sınıflarıyla kullanan kod `VideoSettings` kullanacak şekilde değiştirmelisiniz `WeakVideoSettings` özelliği. Bu gerektiren bir `Dictionary`, hangi kullanılabilir ayarları sınıfları bir özellik olarak örneğin:

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

 * NSObject `.ctor(IntPtr)` Oluşturucusu değiştirildi ortak gelen korumalı ([hatalı kullanımı önlemek için](~/cross-platform/macios/unified/index.md#NSObject_ctor)).

 * `NSAction` bırakıldı [yerini](~/cross-platform/macios/unified/index.md#NSAction) starndard .NET ile `Action`. Bazı basit (tek bir parametre) temsilciler de değiştirilmiştir `Action<T>`.

Son olarak, başvurmak [Klasik v Unified API farklılıkları](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) API'leri değişiklikleri kodunuzda aramak için. Arama [bu sayfayı](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) Klasik API'ları ve ne için güncelleştirilmiş bulmaya yardımcı.

**Not:** `MonoTouch.Dialog` ad alanı aynı geçişten sonra kalır. Kodunuzu kullanıyorsa **MonoTouch.Dialog** işleme devam etmemelisiniz bu ad alanını kullanmak - yapmak *değil* değiştirme `MonoTouch.Dialog` için `Dialog`!

## <a name="common-compiler-errors"></a>Genel derleyici hataları

Sık karşılaşılan diğer örnekleri yanı sıra çözüm aşağıda listelenmiştir:

**Hata CS0012: 'MonoTouch.UIKit.UIView' türü başvurulmayan bir derlemede tanımlanmış.**

Düzeltme: Bu genellikle bir bileşeni ya da birleşik API ile oluşturulmamış NuGet paketini projeye başvuruda bulunan anlamına gelir. Sil ve tüm bileşenleri ve NuGet yeniden eklemeniz gerekir paketler. Bu hata çözmezse, harici bir kitaplığı henüz Unified API desteklemiyor olabilir.

**Hata MT0034: aynı Xamarin.iOS projede - 'monotouch.dll' ve 'Xamarin.iOS.dll' içeremez 'Xamarin.iOS.dll' 'monotouch.dll' tarafından başvurulan sırada açıkça başvurulmaktadır ' Xamarin.Mobile, sürüm 0.6.3.0, Culture = neutral, = PublicKeyToken = null'.**

Düzeltme: Bu hataya neden olan bileşen silin ve yeniden projeye ekleyin.

**Hatası CS0234: 'MonoTouch' ad alanında 'Foundation' türü veya ad alanı adı yok. Bir derleme başvurusu eksik olabilir mi?**

Düzeltme: Visual Studio Mac için otomatik geçiş aracı *gereken* Tümünü Güncelleştir `MonoTouch.Foundation` başvurular `Foundation`, ancak bazı durumlarda bu el ile güncelleştirilmesi gerekir. Benzer hatalar daha önce yer alan diğer ad alanları için görünebilir `MonoTouch`, gibi `UIKit`.

**Hatası CS0266: örtük olarak '' System.float' double' türü dönüştürülemiyor**

Düzeltme: türünü değiştirin ve için cast `nfloat`. Bu hata ayrıca diğer türleri için 64-bit eşdeğerleri ile oluşabilir (gibi `nint`,)

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**Hatası CS0266: örtük olarak türü 'System.Drawing.RectangleF' için ' CoreGraphics.CGRect' türüne dönüştürülemez. Açık bir dönüştürme var (bir cast eksik?)**

Düzeltme: Değiştirme örneklerine `RectangleF` için `CGRect`, `SizeF` için `CGSize`, ve `PointF` için `CGPoint`. Ad alanı `using System.Drawing;` ile değiştirilmelidir `using CoreGraphics;` (zaten mevcut değilse).

**hatası CS1502: en iyi yöntem eşleşmesi için aşırı ' CoreGraphics.CGContext.SetLineDash (System.nfloat, System.nfloat[])' bazı geçersiz bağımsız değişkenlere sahiptir**

Düzeltme: Değiştirmek için dizi türü `nfloat[]` ve açıkça noktaya yayın `Math.PI`.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Hatası CS0115: Bir geçersiz kılma ancak geçersiz kılmak için uygun yöntem bulunamadı 'WordsTableSource.RowsInSection (UIKit.UITableView, int)' işaretlenir**

Düzeltme: dönüş değeri ve parametre türleri değiştirme `nint`. Bu genellikle yöntemi geçersiz kılmaları olanlar gibi üzerinde oluşuyor `UITableViewSource`dahil `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`vb.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Hata CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)'**

Düzeltme: Dönüş türü değiştiği için `nint`, dönüş değerini cast `nint`.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**Hatası CS1061:, 'AddElipseInRect' için bir tanım 'CoreGraphics.CGPath' türünü içermiyor**

Düzeltme: yazım denetimi düzeltmek `AddEllipseInRect`. Diğer ad değişiklikleri içerir:

* 'Color.Black'i' değiştirmek `NSColor.Black`.
* Değiştirme MapKit 'AddAnnotation' `AddAnnotations`.
* Değiştirme AVFoundation 'DataUsingEncoding' `Encode`.
* Change AVFoundation 'AVMetadataObject.TypeQRCode' to `AVMetadataObjectType.QRCode`.
* Değiştirme AVFoundation 'VideoSettings' `WeakVideoSettings`.
* Değiştirmek için PopViewControllerAnimated `PopViewController`.
* Değiştirme CoreGraphics 'CGBitmapContext.SetRGBFillColor' `SetFillColor`.

**Hata CS0546: 'MapKit.MKAnnotation.Coordinate' kılınabilir bir set erişimcisi (CS0546) sahip olmadığından geçersiz kılınamaz**

Özel ek açıklama MKAnnotation sınıflara göre oluştururken koordinat alanı hiçbir ayarlayıcı yalnızca bir alıcı içeriyor.

[Fix](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* Koordinat izlemek için bir alan ekleyin
* Bu alan koordinat özellik alıcısı döndürür
* SetCoordinate yöntemini geçersiz kılın ve alanınızı ayarlama
* Geçirilen koordinat parametresine sahip, ctor SetCoordinate çağırın

Aşağıdakine benzer görünmelidir:

```csharp
class BasicPinAnnotation : MKAnnotation
{
    private CLLocationCoordinate2D _coordinate;

    public override CLLocationCoordinate2D Coordinate
    {
        get
        {
            return _coordinate;
        }
    }

    public override void SetCoordinate(CLLocationCoordinate2D value)
    {
        _coordinate = value;
    }

    public BasicPinAnnotation (CLLocationCoordinate2D coordinate)
    {
        SetCoordinate(coordinate);
    }
}
```

## <a name="related-links"></a>İlgili bağlantılar

- [Uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-apps.md)
- [İOS uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Güncelleştirme bağlamaları](~/cross-platform/macios/unified/update-binding.md)
- [Platformlar Arası Uygulamalarda Yerel Türlerle Çalışma](~/cross-platform/macios/native-types-cross-platform.md)
- [Klasik vs Unified API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
