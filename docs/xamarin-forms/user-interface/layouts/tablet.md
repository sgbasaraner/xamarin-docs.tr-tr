---
title: Tablet ve Masaüstü uygulamaları için Düzen
description: Bu makalede Xamarin.Forms uygulaması düzenleri aksine telefonlar, tabletler için en iyi duruma getirme açıklanmaktadır.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: b98d1fcf0917b9e25d774a92d56bf90bdd291978
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998642"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Tablet ve Masaüstü uygulamaları için Düzen

Telefonlar ek olarak, uygulamalar üzerinde de çalışabilir için Xamarin.Forms desteklenen platformlarda kullanılabilir olan tüm cihaz türlerini destekler:

* iPad,
* Android tabletler
* Windows tabletler ve masaüstü bilgisayar (Windows 10 çalıştıran).

Bu sayfayı kısaca ele alınmaktadır:

* desteklenen [cihaz türlerini](#Device_Types), ve
* nasıl yapılır [en iyi duruma getirme](#optimize) telefonlar ve tabletler için düzenler.

<a name="Device_Types" />

## <a name="device-types"></a>Cihaz türleri

Tüm Xamarin.Forms tarafından desteklenen platformlar için daha büyük ekran cihazları kullanılabilir.

### <a name="ipads-ios"></a>iPad (iOS)

Otomatik olarak Xamarin.Forms şablon yapılandırarak iPad desteği içeren **Info.plist > cihazlar** ayarını **Evrensel** (yani iPhone ve iPad desteklenir).

Bir eğlenceli başlangıç deneyiminin sunulabilmesi ve tüm cihazlarda tam ekran çözünürlüğü kullanılır emin olmak için olun bir [iPad özgü başlatma ekranı](~/ios/app-fundamentals/images-icons/launch-screens.md) (görsel taslak kullanarak) sağlanır. Bu uygulamayı doğru şekilde iPad mini, iPad ve iPad Pro cihazlarda işlenen sağlar.

Cihazda tam ekranı yukarı tüm uygulamaları iOS 9 önce geçen, ancak bazı İpad'ler artık gerçekleştirebilirsiniz [ekran çok görevli bölme](~/ios/platform/multitasking.md).
Başka bir deyişle, uygulamanızı yalnızca ince bir sütun genişliğini ekran veya ekranın tamamını yüzdesi 50 ekranın kenarındaki ayarlama ele geçirebilir.

[![](tablet-images/ipad-sml.png "iPad bölünmüş ekran örnek")](tablet-images/ipad.png#lightbox "iPad bölünmüş ekran örneği")

Bölünmüş ekran işlevi uygulamanızı geniş veya olarak 1366 piksel genişliğinde olduğunca 320 piksel ile iyi çalışacak şekilde tasarlamanız gerekir anlamına gelir.

### <a name="android-tablets"></a>Android tabletler

Büyük tabletler kadar küçük telefonlardan desteklenen ekran boyutları'nın Android ekosistemi vardır. Xamarin.Forms tüm ekran boyutları destekler, ancak olarak diğer platformlarla büyük cihazlar için kullanıcı arabiriminiz ayarlamak isteyebilirsiniz.

Birçok farklı ekran çözünürlükleri desteklerken, yerel görüntü kaynaklarınızı kullanıcı deneyimini iyileştirmek için farklı boyutlarda sağlayabilirsiniz.
Gözden geçirme [Android kaynakları](~/android/app-fundamentals/resources-in-android/index.md) belgeleri (ve özellikle [çeşitli ekran boyutları için kaynaklar oluşturma](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) Android uygulamanıza dosya adlarını ve klasörleri yapı hakkında daha fazla bilgi için uygulamanızda en iyi duruma getirilmiş resim kaynakları eklemek için proje.

### <a name="windows-tablets-and-desktops"></a>Windows tabletler ve masaüstü bilgisayarlar

Tablet ve Windows çalıştıran masaüstü bilgisayarları desteklemek için kullanmanız gerekecektir [Windows UWP Destek](~/xamarin-forms/platform/windows/installation/index.md), Windows 10'da çalışan Evrensel uygulamalar oluşturur.

Windows tabletler ve masaüstü bilgisayarlar üzerinde çalışan uygulamalar rastgele boyutlarla çalışan tam ekran için ek olarak boyutlandırılabilir.

[![](tablet-images/splitscreen-sml.png "Windows bölünmüş ekran örnek")](tablet-images/splitscreen.png#lightbox "Windows bölünmüş ekran örneği")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Tablet ve Masaüstü için en iyi duruma getirme

Xamarin.Forms kullanıcı Arabiriminizin bağlı olarak telefon olup olmadığını ayarlayabilir veya tablet/Masaüstü cihaz kullanılır. Başka bir deyişle, Tablet ve masaüstü bilgisayarlar gibi büyük ekranlı cihazlarda kullanıcı deneyimini iyileştirebilirsiniz.


### <a name="deviceidiom"></a>Device.Idiom

Kullanabileceğiniz [ `Device` ](~/xamarin-forms/platform/device.md) uygulama ya da Kullanıcı Arabiriminizin davranışını değiştirmek için sınıf. Kullanarak `Device.Idiom` yapabilecekleriniz numaralandırması

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Bu yaklaşım, tek tek sayfa düzenleri için önemli değişiklik ya da daha büyük ekranlarda tamamen farklı sayfaları işlemek için genişletilebilir.

### <a name="leveraging-masterdetailpage"></a>MasterDetailPage yararlanarak

[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) Özellikle nerede kullanan ipad'de büyük ekranlar için ideal olan [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/) yerel iOS deneyimi sağlamak için.

Gözden geçirme [Xamarin bu blog gönderisini](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) nasıl telefonlar bir düzen kullanır ve büyük ekranlar başka kullanabilir, kullanıcı arabirimi uyarlayabilirsiniz görmek için (ile `MasterDetailPage`).



## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin blogu](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe örnek](https://github.com/jamesmontemagno/myshoppe)
