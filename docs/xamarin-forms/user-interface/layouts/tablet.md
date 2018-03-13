---
title: "Tablet ve Masaüstü uygulamaları için düzeni"
ms.topic: article
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: f870cda73625197fb15bf19be1cdabbd675124d6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Tablet ve Masaüstü uygulamaları için düzeni

Xamarin.Forms desteklenen platformlarda kullanılabilir tüm cihaz türlerini destekler, bu nedenle telefonlarının yanı sıra, uygulamalar üzerinde de çalışabilir:

* iPads,
* Android tabletler
* Windows tabletler ve masaüstü bilgisayarlar (Windows 8.1 veya Windows 10 çalıştıran).

Bu sayfa kısaca ele alınmıştır:

* desteklenen [aygıt türleri](#Device_Types), ve
* nasıl yapılır [en iyi duruma getirme](#optimize) telefonlar ve tabletler için düzenler.

<a name="Device_Types" />

## <a name="device-types"></a>Aygıt türleri

Büyük ekran aygıtlarının tüm Xamarin.Forms tarafından desteklenen platformlar için kullanılabilir.

### <a name="ipads-ios"></a>İpad'ler (iOS)

Otomatik olarak Xamarin.Forms şablon yapılandırarak iPad desteği içerir **Info.plist > aygıtları** ayarını **Evrensel** (yani iPhone ve iPad desteklenir).

Bir eğlenceli başlangıç deneyimi sağlar ve tüm aygıtlarda tam ekran çözünürlüğü kullanılan sağlamak için emin olmalısınız bir [iPad özgü başlatma ekranı](~/ios/app-fundamentals/images-icons/launch-screens.md) (film şeridi kullanarak) sağlanır. Bu uygulamayı doğru iPad mini, iPad ve iPad Pro cihazlarda işlenen sağlar.

Tam ekran cihazda yukarı tüm uygulamaları iOS 9 önce sürdü, ancak bazı İpad'ler şimdi gerçekleştirebilirsiniz [ekran görevli bölme](~/ios/platform/multitasking.md).
Başka bir deyişle, uygulamanızı yalnızca ince sütun ekran veya tüm ekranı genişliğini % 50'si ekran yan tarafında yukarı ele geçirebilir.

[![](tablet-images/ipad-sml.png "iPad bölünmüş ekran örnek")](tablet-images/ipad.png#lightbox "iPad bölünmüş ekran örnek")

Bölünmüş ekran işlevselliği geniş veya kadar 1366 piksel genişliğinde iyi olduğunca 320 piksel ile çalışmak için uygulamanızı tasarlamanız gerekir anlamına gelir.

### <a name="android-tablets"></a>Android tabletler

Android ekosistemi desteklenen ekran boyutlarına, büyük tabletler kadar küçük telefonları çok sayıda sahiptir. Xamarin.Forms tüm ekran boyutlarına destekler, ancak olarak ile diğer platformlar, büyük cihazlar için Kullanıcı Arabiriminizin ayarlamak isteyebilirsiniz.

Birçok farklı ekran çözünürlükleri desteklendiğinde, kullanıcı deneyimini iyileştirmek için farklı boyutlarda yerel görüntü kaynaklarınızı sağlayabilirsiniz.
Gözden geçirme [Android kaynakları](~/android/app-fundamentals/resources-in-android/index.md) belgeleri (ve özellikle [ekran boyutlarına değişen için kaynakları oluşturma](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) klasörler ve Android uygulamanızı adlarında yapısı hakkında daha fazla bilgi için Uygulamanızı en iyi duruma getirilmiş görüntü kaynakları eklemek için proje.

### <a name="windows-tablets-and-desktops"></a>Windows tabletler ve masaüstü bilgisayarlar

Tablet ve Windows çalıştıran masaüstü bilgisayarları desteklemek için iki desteklenen proje türleri birini kullanmanız gerekir:

* [Windows 8.1](~/xamarin-forms/platform/windows/installation/tablet.md) -
  özellikle Windows 8.1 tabletler ve masaüstü bilgisayarlar için uygulamalar oluşturur.
* [Windows UWP Destek](~/xamarin-forms/platform/windows/installation/universal.md) -
  hem Windows 10 telefonlar, tabletler ve masaüstü bilgisayarlar üzerinde çalışan Evrensel uygulamalar oluşturur.

Windows tabletler ve masaüstü bilgisayarlar üzerinde çalışan uygulamalar için rastgele boyutları ayrıca çalışan tam ekran için boyutlandırılabilir.

[![](tablet-images/splitscreen-sml.png "Bölünmüş Pencereler ekran örnek")](tablet-images/splitscreen.png#lightbox "ekran örnek Bölünmüş Pencereler")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Tablet ve Masaüstü için en iyi duruma getirme

Xamarin.Forms kullanıcı Arabiriminizin bağlı olarak bir telefon olup olmadığını ayarlayabilir veya tablet/Masaüstü cihaz kullanılır. Başka bir deyişle, tabletler ve masaüstü bilgisayarlar gibi büyük ekran cihazlar için kullanıcı deneyimi en iyi duruma getirebilirsiniz.


### <a name="deviceidiom"></a>Device.Idiom

Kullanabileceğiniz [ `Device` ](~/xamarin-forms/platform/device.md) , uygulama veya kullanıcı arabiriminizi davranışını değiştirmek için sınıf. Kullanarak `Device.Idiom` yapabilecekleriniz numaralandırması

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Bu yaklaşım, tek tek sayfa düzenleri için önemli değişiklikler yapmak veya daha büyük ekranında tamamen farklı sayfaları işlemek için genişletilebilir.

### <a name="leveraging-masterdetailpage"></a>Yararlanmayı MasterDetailPage

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Daha büyük ekranda, özellikle burada kullanan iPad için idealdir [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/) yerel iOS deneyimi sağlamak için.

Gözden geçirme [bu Xamarin blog gönderisine](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) nasıl telefonlar bir düzen kullanır ve büyük ekranlar başka kullanabilir, kullanıcı arabiriminizi uyarlayabilirsiniz görmek için (ile `MasterDetailPage`).



## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin blogu](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe örnek](https://github.com/jamesmontemagno/myshoppe)
