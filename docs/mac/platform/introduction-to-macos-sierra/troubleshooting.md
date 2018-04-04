---
title: Sorun giderme
description: Bu makalede, macOS Sierra Xamarin.Mac uygulamalarda ile çalışmak için birkaç sorun giderme ipuçları verilmektedir.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/22/2016
ms.openlocfilehash: 7ea4ec48399b42ce69b0346b1a88a1d9fb9fbf6e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Sorun giderme

_Bu makalede, macOS Sierra Xamarin.Mac uygulamalarda ile çalışmak için birkaç sorun giderme ipuçları verilmektedir._

bölümler aşağıdaki macOS Sierra Xamarin.mac ve bu sorunların çözümü ile kullanıldığında oluşabilir bazı bilinen sorunlar listelenmektedir:

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [İkili uyumluluğu](#Binary-Compatibility)
- [CFNetwork HTTP Protokolü](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [CoreImage](#CoreImage)
- [Bildirimler](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store" />

## <a name="app-store"></a>Uygulama Mağazası

Bilinen Sorunlar:

- Uygulama içi satın almalara sanal ortamda sınarken, kimlik doğrulama iletişim iki kez görünebilir.
- İçerik indirme işlemi tamamlanana kadar uygulama ön plana getirilene her zaman korumalı alan ortamıdır barındırılan içerikle uygulama içi satın almalara test edilirken parolası iletişim kutusu görünür.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Bir hatalı sona erme tarihi veya güvenlik kodunu (FA) yeni bir ödeme kartı Apple Pay eklerken girilirse, sağlama işlemi kartı sonlandırılacak.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>İkili uyumluluğu

Bilinen Sorunlar:

- Çağırma `NSObject.ValueForKey` olacak bir `null` anahtarı, bir özel durum oluşur.
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

`CIImageProcessor` API artık bir rastgele giriş görüntü sayısı destekler. `CIImageProcessor` MacOS Sierra beta 1 eklenmiştir API kaldırılır.

<a name="Notifications" />

## <a name="notifications"></a>Bildirimler

Bildirim içerik uzantıları ile çalışırken, görünüm denetleyicileri değil doğru yayınlanmamaktadır ve uzantı bellek sınırları erişildiğinde bir kilitlenme neden olabilir.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

İletim işleminden sonra `UserInfo` özelliği bir `NSUserActivity` nesnesi boş olabilir. Açıkça çağırın `BecomeCurrent` `NSUserActivity` nesnesi geçerli bir geçici çözüm olarak.

<a name="Safari" />

## <a name="safari"></a>Safari

WebGeolocation güvenli gerektirir (`https://`) iOS 10 hem macOS konum verileri kötü amaçlı kullanılmasını önlemek için Sierra çalışmak için URL.







## <a name="related-links"></a>İlgili bağlantılar

- [Mac Örnekleri](https://developer.xamarin.com/samples/mac/)
- [OS X 10,12 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
