---
title: Sorun giderme
description: Bu makalede Xamarin.tvOS uygulamalarda tvOS 10 ile çalışmak için birkaç sorun giderme ipuçları verilmektedir.
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8875e658ead17820655a2401079627875c14958b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Sorun giderme

_Bu makalede Xamarin.tvOS uygulamalarda tvOS 10 ile çalışmak için birkaç sorun giderme ipuçları verilmektedir._

Aşağıdaki bölümlerde tvOS 10 Xamarin.tvOS ve bu sorunların çözümü ile kullanıldığında oluşabilir bazı bilinen sorunlar listelenmektedir:

- [App Store](#App-Store)
- [İkili uyumluluğu](#Binary-Compatibility)
- [CFNetwork HTTP Protokolü](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [CoreImage](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>Uygulama Mağazası

Bilinen Sorunlar:

 - Uygulama içi satın almalara sanal ortamda sınarken, kimlik doğrulama iletişim iki kez görünebilir.
 - İçerik indirme işlemi tamamlanana kadar uygulama ön plana getirilene her zaman korumalı alan ortamıdır barındırılan içerikle uygulama içi satın almalara test edilirken parolası iletişim kutusu görünür.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>İkili uyumluluğu

Bilinen Sorunlar:

 - Çağırma `NSObject.ValueForKey` olacak bir `null` anahtarı, bir özel durum oluşur.
 - Çağrılırken bir yazı tipi adıyla başvuran `UIFont.WithName` çökmeyle neden olur.
 - Her ikisi de `NSURLSession` ve NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' URL'leri.
 - Uygulamalar askıda değerlendirmesi 's geometri her değiştirirseniz `ViewWillLayoutSubviews` veya `LayoutSubviews` yöntemleri.
 - Tüm SSL/TLS bağlantılarını RC4 simetrik şifre artık varsayılan olarak devre dışıdır. Ayrıca, güvenli aktarım API artık SSLv3 destekler ve mümkün olan en kısa sürede SHA-1 ve 3DES şifreleme kullanarak uygulamayı durdurun önerilir.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP Protokolü

`HTTPBodyStream` Özelliği `NSMutableURLRequest` sınıfı ayarlayın, bu yana açılmamış bir akışa `NSURLConnection` ve `NSURLSession` şimdi bu gereksinim kesinlikle zorla.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Uzun süreli işlemler döndürecektir bir _", dosyayı kaydetmek için izniniz yok."_ bir hata oluştu.

<a name="CoreImage" />

## <a name="coreimage"></a>CoreImage

`CIImageProcessor` API artık bir rastgele giriş görüntü sayısı destekler. `CIImageProcessor` TvOS 10 beta 1 eklenmiştir API kaldırılır.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

İletim işleminden sonra `UserInfo` özelliği bir `NSUserActivity` nesnesi boş olabilir. Açıkça çağırın `BecomeCurrent` NSUserActivity' nesnesi geçerli bir geçici çözüm olarak.

<a name="UIKit" />

## <a name="uikit"></a>Uıkit

Bilinen Sorunlar:

 - Değişiklikler için arka plan görünümünü `UINavigationBar`, `UITabBar` veya `UIToolBar` yeni görünüm çözümlemek için bir düzen geçişi neden olabilir. Bu görünümler içinde değiştirmeye bir `LayoutSubviews`, `UpdateConstraints`, `WillLayoutSubviews` veya `DidUpdateSubviews` olay sonsuz düzeni döngüde neden olur.
 - Çağırma tvOS 10 içinde `RemoveGestureRecognizer` yöntemi bir `UIView` nesne açıkça tüm süren hareketi tanıyıcı iptal eder.
 - Sunulan görünüm denetleyicileri şimdi durum çubuğunun görünümünü etkileyebilir.
 - tvOS 10 gerektirir çağırmak Geliştirici `base.AwakeFromNib` sınıflara ayırırken `UIViewController` ve geçersiz kılma `AwakeFromNib` yöntemi.
 - Özel uygulamalarla `UIView` geçersiz kılma alt sınıfların `LayoutSubviews` ve düzenini çağırmadan önce kirli `base.LayoutSubviews` tvOS 10 sonsuz düzeni döngüde tetikleyebilir.
 - Yön özgü veya flippable görüntü varlıklar olan hiçbir atanan zaman çevirme `UIButton` nesneleri.





## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
