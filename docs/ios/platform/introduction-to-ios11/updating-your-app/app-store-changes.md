---
title: Uygulama mağazası değişiklikleri
description: İOS 11'ın yeni özellikleri keşfetmek
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 9c9355c047d2e7083ca787f473515163d8e6f5f1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="app-store-changes"></a>Uygulama mağazası değişiklikleri

_İOS 11'ın yeni özellikleri keşfetmek_

İOS App Store'da yalnızca kullanıcıların verimli bir şekilde mağazaya gidin izin verir, ancak Ayrıca, uygulamanızı kullanıcılara yükseltmek için bir geliştirici olarak tanır tam bir tasarlanması oluşturdu. Bu promosyon uygulama içi satın almalara güncelleştirmeleri ve ürün sayfası için güncelleştirmeleri içerir. iOS 11 kullanıcılarla iletişim kurma, uygulama simge ekleme ve ortak uygulamanıza serbest bırakma ilgili güncelleştirmeleri de ekler.

![Yeni uygulama mağazası düzeni](app-store-changes-images/image3.jpg)

Yeniden tasarlanan uygulama mağazası aşağıdaki bölümleri içerir:

- **Bugün** – Bu sekme "Düzenleyicisi'nin çekme" veya uygulamayı öne çıkan uygulamaları içerir. Yükseltmek için bilgileri doldurun uygulamanızı buraya'üzerinde [Yükselt](https://developer.apple.com//contact/app-store/promote/) sayfası.
- **Oyunlar** – ayarlanmış olan tüm uygulamaların **oyun** iTunes Bağlan kategorisinde bu sekmesi altında bulunabilir.
- **Uygulamaları** – Bu sekme tüm diğer uygulamaları özellikleri. Kullanıcılar öne çıkan uygulamalar, kategori, üst Ücretli/serbest tarafından göz atabilir.
- **Güncelleştirmeleri** – bu uygulama güncelleştirmeleri uygulamalarınıza görüntüler. Bir uygulama aracılığıyla bıraksanız bile [aşamalı sürüm](#Phased_Release), kullanıcılar, hala bu sekmesine gidin ve en son sürümü yükleyin.
- **Arama** – Bu sekme uygulamanız için arama yapmalarını sağlar.

## <a name="store-icon"></a>Depolama simgesi

Mağaza simgeler (veya pazarlama simgeler) iTunes Connect artık yönetilir ve bunun yerine olarak yer almalıdır bir [varlık Kataloğu](~/ios/app-fundamentals/images-icons/app-icons.md) içinde uygulama ikili dosyanız, uygulamanın simge durumuna benzer. 1024 x 1024 deposu simgesi PNG biçiminde başarılı teslimini 11 iOS uygulamaları için bir varlık kataloğunda eklenmesi gerekir.

Uygulama yoğunluk bu ek varlık Kataloğu uygulama boyutunu artırmaz emin olur.


## <a name="in-app-purchases-promoted-in-the-app-store"></a>Uygulama içi uygulama Mağazası'nda yükseltilmiş satın alma

Apple uygulama içi satın almalara daha bulunabilirlik uygulama Mağazası'nda kullanıma sunmuştur. En çok 20 artık ekleyebilirsiniz _App Store yükseltilmiş_ artık uygulamalar sayfası, uygulamanızın ürün sayfasında ya da arayarak bulunabilir uygulama içi satın almalara.

Uygulama Mağazası'nda görüntülenen, uygulama içi satın almalara sağlamak için aşağıdaki veriler şunları içermelidir:

- **Görüntü** – uygulama içi satın alma yaptığı açıklayan simgesi özel olarak tasarlanmış bir görüntü sağlamanız gerekir. Bu görüntü uygulama simgesinden farklı olması gerekir ve bir ekran görüntüsü olamaz.
- **Ad** – adı yalnızca en çok 30 karakter olabilir.
- **Açıklama** – açıklaması yalnızca en çok 45 karakter olabilir.

Yayımlanabilmesi için tüm uygulama içi satın alma promosyonlar bir app store gözden geçirme tabi olan.

Uygulama içi satın almalara yükseltmek kullanılabilir hale getirmek için uygulamanızı açın ve **Özellikler > uygulama içi satın alımlar**. Git **App Store yükseltme (isteğe bağlı)** bölümünde ve 1024 x 1024 görüntüsü eklemek ve **kaydetmek**aşağıdaki görüntüde gösterildiği gibi:

![Uygulama mağazası yükseltme bölümünde iTune Bağlan](app-store-changes-images/image4.png)

Ayrıca eklemeniz gerekir `ShouldAddStorePayment` yönteme `SKPaymentTransactionObserver` uygulamanızda protokolü.

Uygulama içi satın alma promosyonlar hakkında daha fazla bilgi için Apple'nın bkz [bilgisayarınızı uygulama içi satın yükseltme](https://developer.apple.com/app-store/promoting-in-app-purchases/) sayfa.

## <a name="redesigned-product-page"></a>Yeniden tasarlanan ürün sayfası

Ürün sayfasını aşağıdaki değişiklikler yapılmıştır:

- Başlıklarını şimdi en çok 30 karakter Ayarla
- Bir alt başlık eklendi
    - Başlık compliments kısa bir özeti olmalıdır.
    - Alt başlık en çok 30 karakter olmalıdır
- Uygulama önizlemeleri
    - Artık üç videolar veya ekran görüntüleri olabilir.
    - Bir kullanıcı sayfasını ziyaret ettiğinde videolar otomatik kullan.
    - Daha fazla bilgi için Apple'nın bkz [uygulama Önizleme](https://developer.apple.com/app-store/app-previews/) sayfası.
- Promosyon metni
    - Bu yeni özellik, uygulamanız ile ilgili sık değişen bilgileri tanımlamak izin verme metnin 170 karakterini sağlar.
    - Uygulamasının yeni bir sürümü göndermek zorunda kalmadan herhangi bir zamanda güncelleştirilebilir.

## <a name="customer-communication"></a>Müşteri iletişimi

10.3 içinde Apple incelemeler için yanıt özelliği üzerinden uygulama kullanıcının ile doğrudan iletişim kurmak geliştiriciler için yeni bir yolunu başlattı. Bu yeni işlevselliği iTunes erişebilirsiniz göz atarak bağlanmak **uygulama > etkinlik > derecelendirmelerini ve incelemeler**aşağıdaki görüntüde gösterildiği gibi:

![Açıklamayı Yanıtla girmek için alan gösteren iletişim kutusu](app-store-changes-images/image5.png)

Kullanıcılara yanıtlarken dikkat edilmesi gereken birkaç şey vardır:

- Yalnızca bir kez yanıtlayabilir, ancak istedikleri kadar her iki taraf yorumlarını düzenleyebilirsiniz.
- Açıklama yanıtladığında kullanıcıları bir bildirim alır.
- Rol iTunes oluşturulan yeni bir müşteri desteği bağlayın. Bu rolü veya yönetici rolü kullanıcılarla Yorumlara yanıt verebilir.

Daha fazla bilgi için Apple'nın bkz [incelemeler için yanıt](https://developer.apple.com/app-store/responding-to-reviews/) sayfası.

<a name="Phased_Release"/>

## <a name="phased-release"></a>Aşamalı sürüm

İOS 11 Apple sürümlerini aşamalı güncelleştirmeleri uygulamanıza seçeneği uyguladı. İsteğe bağlı üretim ortamınıza aşırı değil sağlayarak müşteriler için yavaş yavaş yayımlanacak uygulama güncelleştirmenizi izin vermek aşamalı sürümleri etkinleştirebilirsiniz.

Aşamalı sürümleri iTunes Bağlan etkinleştirilir. Kenar uygulamanızda tıklayın, kaydırarak **aşamalı yayın otomatik güncelleştirmeler için** bölümünde kısımda ve seçin **7 günlük dönem kullanarak üzerinden Release güncelleştirmesi aşamalı yayın**:

![Otomatik Güncelleştirmeler için yayın seçeneği gösteren aşamalı](app-store-changes-images/image6.png)

Güncelleştirmeyi hemen uygulama mağazası güncelleştirmeleri sekmesinde indirilebilir. Aşamalı sürümleri yalnızca seçili otomatik yüklemeler sahip kullanıcılar için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Yeni iOS 11 (Apple) nedir](https://developer.apple.com/ios/)
- [Güncelleştirilmiş uygulama mağazası ürün sayfası (Apple)](https://developer.apple.com/app-store/product-page/)
- [Güncelleştirme uygulamanızı iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
