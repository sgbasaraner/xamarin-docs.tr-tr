---
title: "MacOS Sierra giriş"
description: "Bu makalede Xamarin.Mac geliştiriciler için tüm yeni ve değiştirilen API'ler ve macOS Sierra kullanılabilir özellikler sunar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 161a5be643ccf5f96b04413cec5956264af6ce60
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-macos-sierra"></a>MacOS Sierra giriş

Yeni macOS ile Sierra, geliştirici son kullanıcının kendi uygulamaları ve Web siteleri önceden kullanılamayan şekilde etkileşime olanak tanıyan yeni API'lerin yararlanabilir. Örneğin, Apple artık güvenli bir şekilde Apple Pay ve Metal framework artırma geliştirmeler aracılığıyla bir uygulamanın grafik ödeme ve bilgi işlem seçeneğinin müşteriler vermek Web siteleri olası izin verir. 

Apple'nın macOS Sierra hakkında daha fazla bilgi için lütfen bkz [macOS + uygulamaları](https://developer.apple.com/macos/) belgeleri.

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>MacOS Sierra yeni nedir

Apple, macOS Sierra dahil olmak üzere var olan özellikleri birçok geliştirmelerin yanı sıra, birkaç yeni API'ler ve Hizmetleri ekledi:

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Apple dosya sistemi

MacOS Sierra Apple, iOS, macOS, tvOS ve watchOS için modern dosya sistemi olarak yeni Apple dosya sistemi yayımladı. Apple dosya sistemi Flash ve SSD depolama için iyi duruma getirilmiş ve aşağıdaki özellikleri sağlar: güçlü şifreleme, yazarken kopyalama meta verileri, paylaşım, dosyaları ve dizinleri, anlık görüntüler, hızlı dizin boyutlandırma ve atomik güvenli Kaydet temelleri için kopyalama boşluk.

Daha fazla bilgi için lütfen Apple'nın bkz [Apple dosya sistemi Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999).

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Apple Pay geliştirmeleri

Apple bazı geliştirmeler Apple Pay için güvenli ödemelerini sitelerinden kullanıcıya izin macOS Sierra içinde kullanıma sunmuştur.

MacOS Sierra birkaç yeni API macOS Sierra, iOS ve watchOS dinamik ödeme ağlar ve yeni bir korumalı alan test ortamı desteklemek için birlikte çalışan eklendi.

macOS Sierra Geliştirici Apple Pay doğrudan iOS ve macOS Safari tabanlı Web eklemenizi sağlayan yeni ApplePay Javascript çerçevesini içerir. Apple Pay destekleyen Web siteleri için kullanıcının kendi iPhone veya Apple Watch kullanarak ödeme yetki verebilir.

Daha fazla bilgi için lütfen Apple'nın bkz [ApplePay JS Framework](https://developer.apple.com/reference/applepayjs) başvuru.

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>Modern macOS uygulamaları oluşturma

Apple Safari web tarayıcısı, sayfaları sözcük işlemci ve birleşik, içeriğe duyarlı kullanıcı taşınabilir paneller gibi geleneksel kullanıcı Arabirimi öğeleri yerine yaptığı arabirimi sunmak için birçok yeni teknolojileri ve birden çok açın numaraları yayılan sayfası kullanım gibi modern macOS uygulamalar Windows.

[![Sekmeli Mac penceresi örneği](images/content08.png)](images/content08.png#lightbox)

Bizim [yapı Modern macOS uygulamaları](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) kılavuz kapsar birkaç ipuçları, özellikleri ve teknikler Geliştirici Xamarin.Mac modern macOS uygulamada oluşturmak için kullanabilirsiniz.

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>CloudKit veri paylaşımı

CloudKit framework macOS Sierra hızlı ve kolay bir şekilde kaydeder veya kendi özel iCloud veritabanlarını kayıt kümelerinden paylaşmak kullanıcı izin verecek şekilde genişletilmiştir.

Gönderme ve paylaşılan kayıt davetleri kabul etme CloudKit tam bir kullanıcı Arabirimi sağlar ve kullanıcı kayıtları erişimi kişileri tam okuma/yazma denetime sahiptir.

Daha fazla bilgi için lütfen Apple'nın bkz [CloudKit Framework başvurusu](https://developer.apple.com/reference/clockkit) ve [CloudKit JS Framework başvurusu](https://developer.apple.com/reference/cloudkitjs).

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Safari uygulama uzantıları desteği

Safari uygulama uzantıları Sierra macOS ile sıkı bir şekilde tümleştirilmiş sırasında Safari web tarayıcısı davranışını genişletmek uygulama izin verin. MacOS Safari uygulama uzantıları iOS Safari uygulama uzantıları benzer iş olduğundan, bunlar için bağlantı noktası başka bir bir sistemden kolaydır.

Daha fazla bilgi için lütfen Apple'nın bkz [Safari uygulama uzantısı Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319).

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>Güvenlik ve gizlilik geliştirmeleri

Apple, güvenlik ve gizlilik macOS uygulama güvenliğini artırmak ve aşağıdakiler de dahil olmak üzere son kullanıcının gizliliği güvence altına uygulaması yardımcı olacak Sierra bazı geliştirmeler yaptı:

- Yeni `NSAllowsArbitraryLoadsInWebContent` anahtar uygulamanın ekleme yapabilir `Info.plist` dosya ve Apple Aktarım güvenlik (ATS) koruma uygulama geri kalanı için hala etkin durumdayken doğru bir şekilde yüklemek için web sayfaları izin verir.
- Ortak veri güvenlik mimarisi (CDSA) API, kullanım dışı bırakıldı ve asimetrik anahtarları oluşturmak için SecKey API'si ile değiştirilmelidir.
- Tüm SSL/TLS bağlantılarını RC4 simetrik şifre artık varsayılan olarak devre dışıdır. Ayrıca, güvenli aktarım API artık SSLv3 destekler ve mümkün olan en kısa sürede SHA-1 ve 3DES şifreleme kullanarak uygulamayı durdurun önerilir.
- Yeni Pano iOS 10 ve macOS Sierra cihazlar arasında kopyalayıp kullanıcıya izin verdiğinden, API için belirli bir aygıt sınırlı ve belirli bir anda otomatik olarak temizlenecek zaman damgalı bir Pano izin verecek şekilde genişletilmiştir. Ayrıca, adlandırılmış pasteboards artık kalıcı ve paylaşılan çalışma alanı kapsayıcılarını ile değiştirilmelidir.
- Uygulama (örneğin, kullanıcının takvim), korunan verilerin erişirse, _gerekir_ doğru amacı dize değeri anahtarı ile bu amacı bildirme kendi `Info.plist` dosyası (`NSCalendarUsageDescription` Takvim söz konusu olduğunda).
- Mac App Store teslim edilmedi Geliştirici imzalandı uygulamaları CloudKit, iCloud Anahtarlık, iCloud sürücü, uzak anında iletme bildirimleri, MapKit ve VPN yetkilendirmeler şimdi avantajından yararlanabilirsiniz.
- macOS Sierra artık harici kod ya da veri kod imzalayan uygulama zip arşivini veya imzasız disk görüntüsü ile birlikte çalışma zamanı yolun önce çalışma zamanı bilinmiyor olarak teslim destekler.

Ayrıca, Sierra macOS üzerinde çalışan uygulamalar (veya üstü) bir veya daha fazla gizlilik belirli anahtarlarında girerek belirli özellikler veya kullanıcı bilgilerini erişmek için kendi hedefi statik olarak bildirilmelidir kendi `Info.plist` neden uygulamanın istediği kazanmak için kullanıcıya açıklayan dosyaları erişim.

MacOS Sierra bu değişiklikler ile iOS 10 paylaşımları olduğundan, lütfen bizim iOS 10 bakın [güvenlik ve gizlilik geliştirmeler](~/ios/app-fundamentals/security-privacy.md) daha fazla bilgi için Kılavuzu.

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>Akıllı kart sürücüsü uzantısı desteği

MacOS Sierra uygulaması oluşturabilirsiniz `NSExtension` içeriği belirli akıllı kartları türlerinden salt okunur erişim sağlayan bir akıllı kart sürücüsünü tabanlı. Bu bilgiler daha sonra (kullanım dışı ortak veri güvenlik mimarisi yöntemi yerine) sistem Anahtarlık içinde sunulur.

Daha fazla bilgi için Pleas bakın Apple's [CryptoTokenKit Framework başvurusu](https://developer.apple.com/reference/cryptotokenkit).

<a name="Unified-Logging" />

### <a name="unified-logging"></a>Birleşik günlüğe kaydetme

Birleşik Günlük sistem tüm düzeyleri arasında verimli ileti için tek bir API uygulamasıyla sağlar. Birleşik günlüğü ile uygulama gizlilik denetimleri ve kolay hata ayıklama için izleme etkinliği içeren birden çok günlük tutma düzeyini hassas denetime sahiptir. 

İzleme ve günlüğe kaydetme etkinlik birlikte kullanıldığında günlük otomatik ileti bağıntı sağlar.

macOS Sierra bağlı aygıtlar dahil olmak üzere birden fazla kaynaktan günlük verilerini görüntülemek için yeni bir konsol uygulaması (içindeki uygulamalar/Utilities) içerir. Ayrıca, parçalanmış ve kaydedilmiş aramaları destekler ve birden çok işlemler arasında ilgili iletiler arasındaki bağlantıları görüntüler.

Ayrıca, günlük iletilerini görüntülenebilir ve komut satırı araçlarını kullanarak saklanır.

Daha fazla bilgi için lütfen Apple'nın bkz [günlüğe kaydetme başvurusu](https://developer.apple.com/reference/os/1891852-logging).

<a name="Wide-Color" />

### <a name="wide-color"></a>Geniş rengi

macOS Sierra genişletilmiş aralığı piksel biçimleri ve çekirdek grafikleri, çekirdek görüntü, Metal ve AVFoundation gibi çerçeveleri dahil olmak üzere Sistem genelindeki wide gam renk alanları desteği genişletir. Geniş renk görüntüler ile cihazları için destek Bu davranış tüm grafik yığını boyunca sağlayarak daha fazla hızının.

Ayrıca, `AppKit` değiştirildi yeni çalışmak için Genişletilmiş **sRGB** renk uzayı, geniş renk gamutları birbirinden önemli performans kaybı olmadan renkleri karışık kolaylaştırır.

Apple geniş renklerle çalışırken aşağıdaki en iyi yöntemleri sunar:

- `NSColor` sRGB Renk aralığı ve artık kullanır değerlere clamp artık `0.0` için `1.0` aralık. Uygulamanın önceki clamp davranışı dayalıysa, Sierra macOS için değiştirilmesi gerekir.
- Görüntü işleme sağlamak için çekirdek grafikleri veya Metal gibi düşük düzey API'si kullanılarak uygulama 16 bit kayan nokta değerlerine destekleyen bir genişletilmiş aralık renk alanını ve piksel biçimi kullanmalıdır. Gerektiğinde, uygulamayı el ile renk bileşeni değerleri clamp gerekecektir.
- Çekirdek grafikleri, çekirdek görüntü ve Metal performans gölgelendiriciler tüm iki renk alanları arasında dönüştürme için yeni yöntemler sağlar.

Daha fazla bilgi için lütfen bkz bizim [geniş renk giriş](~/ios/platform/wide-color.md) Kılavuzu.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Ek Framework değişiklikleri

Ana framework değişiklikler ve eklemeler yukarıda listelenen ek olarak, Apple macOS Sierra birçok ek küçük framework değişiklikler yaptı.

Daha fazla bilgi için lütfen bkz bizim [ek Framework değişiklikler](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) Kılavuzu.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Kullanım dışı API'leri

Aşağıdaki API'leri macOS Sierra kullanım dışı bırakıldı:

- HFS standart dosya sistemi artık desteklenmiyor.

Apple'nın bkz [OS X v10.12 API Diffs](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html) tam listesi için belgelere deprecations ve değişiklikleri.

## <a name="related-links"></a>İlgili bağlantılar

- [Mac Örnekleri](https://developer.xamarin.com/samples/mac/)
- [OS X 10,12 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
