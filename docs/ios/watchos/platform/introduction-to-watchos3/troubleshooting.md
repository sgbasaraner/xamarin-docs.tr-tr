---
title: watchOS 3 sorun giderme
description: "Bu makalede, Xamarin Apple Watch uygulamalarda watchOS 3 ile çalışmak için birkaç sorun giderme ipuçları verilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 02ec4c3c9827f7cfac48184eb12491faaa0c92c8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="watchos-3-troubleshooting"></a>watchOS 3 sorun giderme

_Bu makalede, Xamarin Apple Watch uygulamalarda watchOS 3 ile çalışmak için birkaç sorun giderme ipuçları verilmektedir._

Bu sayfada watchOS 3 Xamarin ve bu sorunların çözümü kullanılırken oluşabilir bazı bilinen sorunlar listelenmektedir.

## <a name="activities"></a>Etkinlikler

Doğru çalışması için paylaşımı etkinliği için tüm eşleştirilmiş Apple saatlerde watchOS 3 çalıştırması gerekir.

Bilinen Sorunlar:

- Paylaşımı bir etkinlik bildirimi bazen yanıtlama başarısız olur.
- Bir etkinlik paylaşımı bildirimi iletisiyle birlikte yanıtlarken başarısız olabilir.
- Bir etkinlik paylaşımı bildirim iletisi yukarıda bağlamsal metin yanlış olur.


## <a name="apple-pay"></a>Apple Pay

Bilinen Sorunlar:

- FA kodu veya bir yanlış sona erme tarihi Apple Pay içinde yeni bir ödeme dikkat için basarsa zaman girilirse **sonraki** çalışan işlemi kilitleniyor.
- Bir PIN numarası gerektiren uygulama Apple Pay satın alma işlemleri kilitlenebilir.



## <a name="auto-mac-unlock"></a>Otomatik Mac kilidini aç

İki öğeli kimlik doğrulama kullanıcının iCloud hesabıyla üzerinde etkinleştirilirse watchOS 3 beta 2 (veya daha büyük) ve macOS Sierra beta 2 (veya daha büyük) kullanarak kendi Apple Watch otomatik olarak kullanabileceklerini kendi MAC kilidini aç



## <a name="background-refresh"></a>Arka plan yenilemesi

Sistem kaynakları ihlal watchOS 3 uygulama kilitlenmesi aşağıdaki özel durum kodları ile sonuçlanır:

- **0xc51bad01** -uygulama tüketilen çok fazla CPU süresi.
- **0xc51bad02** -uygulama tüketilen çok fazla duvar saati.
- **0xc51bad03** -uygulama geçerli görevi tamamlamak için yeterli çalışma zamanı sahip değil.



## <a name="clock"></a>Saat

Yeni yüklenen Apple Watch uygulamalardan zorluklar boş gösterebilir. Bu sorunu gidermek için Apple Watch yeniden başlatın.


## <a name="connectivity"></a>Bağlantı

Bilinen Sorunlar:

- watchOS, Apple Watch korumalı kullanıcı verilerine erişim izni kullanıcıdan değil. Veri izleme uygulamada kullanmadan önce bir iPhone uygulamada erişim izni ver.
- Apple Watch burada tüm WatchConnectivity iletimler başarısız bir durumda girebilirsiniz düzeltmek için Apple Watch yeniden başlatın.


## <a name="notifications"></a>Bildirimler

Bir ortam eki çok büyük ise, kullanıcının iPhone ancak bunların Apple Watch sunulur.


## <a name="nsurlconnection"></a>NSURLConnection

Tüm `NSURLConnection` eski TLS protokolleri kullanarak bağlantı başarısız olur. Tüm SSL/TLS bağlantılarını RC4 simetrik şifre artık varsayılan olarak devre dışıdır. Ayrıca, güvenli aktarım API artık SSLv3 destekler ve mümkün olan en kısa sürede SHA-1 ve 3DES şifreleme kullanarak uygulamayı durdurun önerilir.

3 watchOS sürümünden itibaren SSL/TLS bağlantılar güvenlik kesinlikle Apple tarafından zorlanan. Etkilenen hizmetler ve uygulamalar web sunucularını en son TLS protokol sürümleri kullanmak üzere güncelleştirilmiş.


## <a name="nsurlsession"></a>NSURLSession

WatchOS 3 ' ten itibaren `HTTPBodyStream` özelliği `NSMutableURLRequest` sınıfı ayarlayın, bu yana açılmamış bir akışa `NSURLConnection` ve `NSURLSession` şimdi bu gereksinim kesinlikle zorla.


## <a name="privacy"></a>Gizlilik

Bilinen Sorunlar:

İle çalışırken `https://` URL'leri her iki `NSURLSession` ve `NSURLConnection` RC4 şifreleme paketleri TLS anlaşması sırasında artık desteklememektedir. Aşağıdaki hata kodlarından birini oluşturulabilir:

- **-1200 veya-98** - `NSURLErrorSecurityConnectionFailed` ve SecureTransport hataları.
- **[3:-9824]-1200** -http yükleme başarısız oldu.
- **-1200**  -  `NSURLConnection` hata ile tamamlandı.

3 watchOS sürümünden itibaren SSL/TLS bağlantılar güvenlik kesinlikle Apple tarafından zorlanan. Etkilenen hizmetler ve uygulamalar web sunucularını en son TLS protokol sürümleri kullanmak üzere güncelleştirilmiş. Bkz: [NSURLConnection](#NSURLConnection) üzerinde daha fazla bilgi için.


## <a name="snapshots"></a>Anlık görüntüler

Yeni benimseyen değil WatchKit uygulamalar `HandelBackgroundTask` API düzenli güncelleştirmeler artık watchOS 3 alırsınız. 


## <a name="watchkit"></a>WatchKit

Bir uygulama watchOS yerleştirme arka girdiğinde SpriteKit ve SceneKit planda duraklatılır.


## <a name="related-links"></a>İlgili bağlantılar

- [watchOS örnekleri](https://developer.xamarin.com/samples/watchos/all/)
- [WatchOS 3 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
