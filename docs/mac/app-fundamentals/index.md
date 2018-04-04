---
title: Uygulama temelleri
description: Bu belge bağlantılar kılavuzlara Xamarin.Mac uygulamaları geliştirirken anlamanız için gereken çeşitli kavramlarını açıklar.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: 807cf0e16cafc00483cecfc578367bc110657672
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="application-fundamentals"></a>Uygulama temelleri

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Ortak desenleri ve deyimleri](~/mac/app-fundamentals/patterns.md)

C# kullanıma sunulan Apple API'leri belirli deyimleri ve desenler tekrar tekrar gündeme. Xamarin.iOS ile programlama deneyimi varsa, bunlar hakkında bilgi görünebilir. Bunları düz bir anlayış sahip bulduğunuz belgelerin anlamlı yardımcı olacak şekilde belgeleri genellikle bu desenleri ve deyimleri tekrar tekrar başvurur.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Mac API'ları anlama](~/mac/app-fundamentals/mac-apis.md)

Xamarin.Mac ile geliştirme zaman çoğunu için düşünün, okuyabilir ve C# ' ta temel Objective-C API'leri ile kadar sorun olmadan yazma. Ancak, bazen gerektiğinde API belgelerine Apple'dan okumak için sorun için çözüm yığın taşması gelen yanıt Çevir veya varolan bir örnek karşılaştırın.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[.Xib dosyalarıyla çalışma](~/mac/app-fundamentals/xib.md)

Bu makalede, Xcode'nın arabirimi Oluşturucu'da oluşturmak ve Xamarin.Mac uygulama için kullanıcı arabirimleri korumak için oluşturulan .xib dosyaları ile çalışma kapsar.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[.Storyboard/.xib daha az kullanıcı arabirimi tasarımı](~/mac/app-fundamentals/xibless-ui.md)

Bu makalede, .storyboard veya .xib dosyalarıyla Xcode'nın arabirimi Oluşturucu kullanmadan doğrudan C# kodundan Xamarin.Mac uygulamanın kullanıcı arabirimi oluşturma yer almaktadır.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[İmajlarla çalışma](~/mac/app-fundamentals/image.md)

Bu makalede, görüntüler ve Xamarin.Mac uygulama simgeleri ile çalışma yer almaktadır. Görüntü Bakımı uygulamanızın simgesi ve C# kodu ve Xcode'nın arabirimi Oluşturucu'da görüntü kullanarak oluşturmak gereken ve oluşturulması ele alınmaktadır.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Veri bağlama ve anahtar-değer kodlama](~/mac/app-fundamentals/databinding.md)

Bu makalede, anahtar-değer kodlama ve anahtar-değer veri bağlama Xcode'nın arabirimi Oluşturucu kullanıcı Arabirimi öğelerine izin vermek üzere Gözlemleme kullanmayı ele alır. Bu teknik kullanılarak, Xamarin.Mac uygulamanız için yazılması gereken C# kod miktarını büyük ölçüde azaltabilir. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Veritabanları ile çalışma](~/mac/app-fundamentals/databases.md)

Bu makalede, anahtar-değer kodlama ve anahtar-değer Xcode'nın arabirimi Oluşturucu kullanıcı Arabirimi öğeleri SQLite veritabanlarına doğrudan erişim ile veri bağlaması izin vermek üzere Gözlemleme kullanmayı ele alır. Ayrıca, SQLite veri erişim sağlamak için SQLite.NET ORM kullanmayı ele alır.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Kopyala ve Yapıştır ile çalışma](~/mac/app-fundamentals/copy-paste.md)

Bu makalede kopyalama sağlamak ve bir Xamarin.Mac uygulamasında yapıştırmak için çalışma alanı'nı çalışmak kapsar. Nasıl çalışacağınız gösterilmektedir standart veri türleriyle birden çok uygulama ve verin uygulamanızın içinde özel veri desteklemek nasıl arasında paylaşılabilir.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Korumalı alan Xamarin.Mac uygulama](~/mac/app-fundamentals/sandboxing.md)

Bu makalede, korumalı alan Xamarin.Mac uygulaması App Store'dan yayın için yer almaktadır. Korumalı alan gidin tüm öğeleri içerir: kapsayıcı dizinleri, yetkilendirmeler, kullanıcı tarafından belirlenen izinler, ayrıcalık ayırma ve çekirdek zorlama.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[İle AVAudioPlayer ses çalma](~/mac/app-fundamentals/sounds.md)

Bu makalede, bir yardımcı sınıfı bir AVAudioPlayer ses kullanımının kayıttan yürütmeyi denetlemek için nasıl kullanılacağı gösterilmektedir.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Raporlama hataları](~/mac/app-fundamentals/troubleshooting.md)

Bazen hepimizin istiyoruz şekilde çalışması için bir API almak için kullanamama veya çalışılırken bir hata olarak çözmek bir proje üzerinde çalışırken takılmış. Mobil ve Masaüstü uygulamalarınızı yazılırken başarılı olması için Xamarin adresindeki Amacımız içindir ve yardımcı olacak bazı kaynaklar sağladık.
