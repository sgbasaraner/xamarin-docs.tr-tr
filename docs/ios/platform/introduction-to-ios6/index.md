---
title: İOS 6 giriş
description: iOS 6 çeşitli Xamarin.iOS 6 C# geliştiricileri için getirir uygulamaları geliştirmek için yeni teknolojiler içerir.
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 8f3be80ffb8156c24c96b03fda8eac3907ca88bd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-ios-6"></a>İOS 6 giriş

_iOS 6 çeşitli Xamarin.iOS 6 C# geliştiricileri için getirir uygulamaları geliştirmek için yeni teknolojiler içerir._

[ ![](images/ios6-large.jpg "İOS 6 logosu")](images/ios6-large.jpg#lightbox)

İOS 6 ve Xamarin.iOS 6 ile geliştiriciler artık yeteneğinin bol bu hedef iPhone 5 olanları dahil olmak üzere, iOS uygulamaları oluşturmak için kendi elden var.
Bu belgede bazı kullanılabilir daha heyecan verici yeni özellikler ve her konunun makalelerinin bağlantıları listeler. Ayrıca, geliştiricilerin taşıyın iOS 6 ve iPhone 5 yeni çözünürlüğü gibi önemli birkaç değişiklikler dokunur.


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[Koleksiyon görünümlerini giriş](~/ios/user-interface/controls/uicollectionview.md)

Koleksiyon görünümlerini rasgele düzenleri kullanarak görüntülenecek içerik izin verin. Özel düzenler desteklerken kılavuz benzeri düzeni kutudan çıktığında, kolayca oluşturma sağlarlar. Daha fazla bilgi için [koleksiyon görünümlerini giriş](~/ios/user-interface/controls/uicollectionview.md) [](~/ios/user-interface/controls/uicollectionview.md)Kılavuzu.


## <a name="introduction-to-pass-kitiosplatformpasskitmd"></a>[Giriş paketi geçirmek için](~/ios/platform/passkit.md)

Geçirmek Seti framework uygulamalarının Passbook uygulamasında yönetilen dijital geçişleri etkileşime olanak sağlar. Daha fazla bilgi için [geçirmek Seti Kılavuzu giriş](~/ios/platform/passkit.md).


##  <a name="introduction-to-event-kitiosplatformeventkitmd"></a>[Olay Seti Giriş](~/ios/platform/eventkit.md)

EventKit framework takvimleri, takvim olayları ve Takvim veritabanını depolayan anımsatıcıları verilere erişmek için bir yol sağlar. Erişim takvimler ve takvim olayları süredir kullanılabilir 4 iOS itibaren ancak iOS 6 şimdi anımsatıcıları verilere erişim sunar. Daha fazla bilgi için bkz: [ı](~/ios/platform/eventkit.md) [EventKit ntroduction](~/ios/platform/eventkit.md) Kılavuzu.


##  <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[Sosyal Framework'e giriş](~/ios/platform/social-framework.md)

Sosyal Framework Çin'de kullanıcıları için SinaWeibo yanı sıra Twitter ve Facebook dahil olmak üzere sosyal ağlar ile etkileşmek için birleştirilmiş bir API sağlar. Daha fazla bilgi için [sosyal Framework Giriş](~/ios/platform/social-framework.md) Kılavuzu.


##  <a name="changes-to-store-kitchanges-to-storekitmd"></a>[Kit depolamak için değişiklikler](changes-to-storekit.md)

Apple mağazası Seti'ndeki iki yeni özellikler kullanıma sunulan: satın alma ve iTunes veya uygulamanızın uygulama mağazası içeriği indirme ve uygulama içi satın alımlar için içerik dosyalarınızı barındırma!. Daha fazla bilgi için [değişiklikleri deposu pakete](changes-to-storekit.md) Kılavuzu.


## <a name="other-changes"></a>Diğer Değişiklikler


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload ve kullanım dışı ViewDidUnload

`ViewWillUnload` Ve `ViewDidUnload` yöntemlerinin `UIViewController` artık iOS 6 olarak da adlandırılır. Önceki iOS sürümlerinde, bu yöntemleri uygulamalar tarafından bir görünümü bellekten önce durumu ve kodu temizleme sırasıyla kaydetmek için kullanılmış.

Örneğin, Mac için Visual Studio adlı bir yöntem oluşturursunuz `ReleaseDesignerOutlets`altında hangi sonra öğesinden çağrılması gösterilen `ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

Ancak, iOS 6'da, artık çağırmak gerekli değildir `ReleaseDesignerOutlets`.   
   
   
   
Kodu Temizleme için iOS 6 uygulamaları kullanması gereken `DidReceiveMemoryWarning`. Ancak, çağıran kodu `Dispose` olabildiğince az kullanılmalıdır ve yalnızca bellek yoğun gösterildiği gibi altındaki nesneleri için:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

Yeniden çağırma `Dispose` olarak yukarıdaki nadiren gerekli. En uygulamaları yapmanız gereken genel olay işleyicileri kaldırmaktır.

Bu durum durum kaydetme bu uygulamaları gerçekleştirmek `ViewWillDisappear` ve `ViewDidDisappear` yerine `ViewWillUnload`.


### <a name="iphone-5-resolution"></a>iPhone 5 çözümleme

iPhone 5 aygıtların 640 x 1136 çözünürlüğü vardır. İOS önceki sürümlerini hedefleyen uygulama çalıştırıldığında bir iPhone 5, aşağıda gösterildiği gibi letterboxed görünür:

 [![](images/01-letterboxed.png "Önceki iOS sürümlerini hedeflenen uygulamalar iPhone 5 çalıştırdığınızda letterboxed görünür")](images/01-letterboxed.png#lightbox)

Sırada görüntülenmesini uygulama için tam ekran iPhone 5, eklemeniz yeterlidir adlı bir resim `Default-568h@2x.png` 640 x 1136 çözünürlüğü sahip. Aşağıdaki ekran görüntüsü, bu görüntüyü eklenmiştir sonra çalışan uygulama gösterir:

 [![](images/02-fullscreen.png "Bu ekran görüntüsü, bu görüntüyü eklenmiştir sonra çalışan uygulama gösterir.")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>UINavigationBar alt sınıf yapma

İOS 6, `UINavigationBar` sınıflandırma. Bu ek denetimin görünüm ve yapısını sağlar `UINavigationBar`. Örneğin, uygulamaları subviews eklemek, bu görünümler hale getirmeyi ve sınırları değiştirmek için bir alt yapabilir `UINavigationBar`.

Aşağıdaki kod örneği bir altsınıflanmış gösterilmektedir `UINavigationBar` ekleyen bir `UIImageView`:

```csharp
public class CustomNavBar : UINavigationBar
{
    UIImageView iv;
    public CustomNavBar (IntPtr h) : base(h)
    {
        iv = new UIImageView (UIImage.FromFile ("monkey.png"));
        iv.Frame = new CGRect (75, 0, 30, 39);
    }
    public override void Draw (RectangleF rect)
    {
        base.Draw (rect);
        TintColor = UIColor.Purple;
        AddSubview (iv);
    }
}
```

Bir altsınıflanmış eklemek için `UINavigationBar` için bir `UINavigationController`, kullanın `UINavigationController` türü alan oluşturucu `UINavigationBar` ve `UIToolbar`, aşağıda gösterildiği gibi:

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

Bu kullanarak `UINavigationBar` alt sonuçları aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenmesini görüntü görünümünde:

 [![](images/03-navbar.png "Bu ekran görüntüsünde gösterildiği gibi görüntülenmesini görüntü görünümünde bu UINavigationBar alt sonuçları kullanma")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>Yönlendirme arabirimi

6 uygulamaları iOS önce geçersiz kılma `ShouldAutorotateToInterfaceOrientation`, desteklenen belirli denetleyicisi true için tüm yönleriyle döndürüyor. Örneğin, aşağıdaki kod, yalnızca dikey desteklemek için kullanılacak:

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

İOS 6, `ShouldAutorotateToInterfaceOrientation` kullanım dışı bırakılmıştır.
Bunun yerine uygulamaların kılabilirsiniz `GetSupportedInterfaceOrientations` aşağıda gösterildiği gibi kök görünümü denetleyicisinde:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

İPad cihazında bu için tüm dört yönleri, varsayılan olarak `GetSupportedInterfaceOrientation` uygulanmadı. İPhone ve iPod Touch'dışındaki tüm yönleriyle varsayılandır `PortraitUpsideDown`.
