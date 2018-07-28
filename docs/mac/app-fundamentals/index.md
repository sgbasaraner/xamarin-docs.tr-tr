---
title: Xamarin.Mac uygulama temelleri
description: Bu belge bağlantıları kılavuzlara Xamarin.Mac uygulamaları geliştirirken anlamak için gerekli olan çeşitli kavramlarını açıklar.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: e085cf33615d216e1ce9963254050ef2b623ae40
ms.sourcegitcommit: d0da5ce4158239abd2b36f67550e9b475055766a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320810"
---
# <a name="xamarinmac-application-fundamentals"></a>Xamarin.Mac uygulama temelleri

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Ortak desenler ve deyimler](~/mac/app-fundamentals/patterns.md)

C# kullanıma sunulan Apple API'leri belirli deyimleri ve desenleri tekrar tekrar gündeme. Xamarin.iOS ile programlama deneyimi varsa, bu tanıdık gelmiş. Bunları düz bir anlayış bulduğunuz belgelerin mantıklı yardımcı olur böylece belgeleri genellikle bu desenler ve deyimler tekrar tekrar başvuracaktır.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Mac API'leri anlama](~/mac/app-fundamentals/mac-apis.md)

Xamarin.Mac ile geliştirirken zamanınızın çoğunu için düşünme, okuma ve temel alınan Objective-C API'leri ile kadar kaygısı olmadan C# dilinde yazın. Ancak, bazen gerektiğinde Apple'dan API belgeleri okumak için sorununuz için Stack Overflow gelen yanıt bir çözüme Çevir veya var olan bir örnek için karşılaştırın.

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[Konsol uygulamaları](~/mac/app-fundamentals/console.md)

Ayrıca, yerel macOS API'leri erişim "gözetimsiz" konsol uygulamaları oluşturabilirsiniz Xamarin.Mac kullanma.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[.Xib dosyaları ile çalışma](~/mac/app-fundamentals/xib.md)

Bu makale, oluşturmak ve korumak için bir Xamarin.Mac uygulamasını kullanıcı arabirimleri için Xcode'un arabirim Oluşturucu oluşturulan .xib dosyaları ile çalışma kapsar.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[kullanıcı arabirimi tasarımı daha az.Storyboard/.xib](~/mac/app-fundamentals/xibless-ui.md)

Bu makalede .storyboard veya .xib dosyaları Xcode'un arabirim Oluşturucu kullanmadan, doğrudan C# kod bir Xamarin.Mac uygulamanın kullanıcı arabirimini oluşturma anlatılmaktadır.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Görüntülerle çalışma](~/mac/app-fundamentals/image.md)

Bu makale, görüntüler ve simgeler Xamarin.Mac uygulamada çalışmaya kapsar. Oluşturma konusunu ele alınmaktadır ve görüntü Bakımı uygulamanızın simgesi ve C# kodu hem de Xcode'un arabirim Oluşturucu görüntüleri kullanarak oluşturmak gerekli.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Veri bağlama ve anahtar-değer kodlaması](~/mac/app-fundamentals/databinding.md)

Bu makale, anahtar-değer kodlaması ve anahtar-değer Xcode'un arabirim Oluşturucu UI öğelerine veri bağlama için izin vermek için gözleme kullanarak kapsar. Bu tekniği kullanarak Xamarin.Mac uygulamanız için yazılması gereken C# kod miktarını önemli ölçüde azaltın. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Veritabanlarıyla çalışma](~/mac/app-fundamentals/databases.md)

Bu makale, anahtar-değer kodlaması ve anahtar-değer veri bağlama Xcode'un arabirimi tasarımcısı kullanıcı Arabirimi öğeleri için SQLite veritabanlarını doğrudan erişim için izin vermek için gözleme kullanarak kapsar. SQLite verilerine erişim sağlaması için SQLite.NET ORM kullanma kapsar.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Kopyala ve Yapıştır ile çalışma](~/mac/app-fundamentals/copy-paste.md)

Bu makale, kopyalama sağlamak ve bir Xamarin.Mac uygulamasında yapıştırmak için çalışma alanı ile çalışmayı kapsar. Nasıl çalışacağınızı göstermektedir standart veri türleriyle birden fazla uygulama ve bir verme uygulama içinde özel veri desteklemek nasıl arasında paylaşılabilir.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Bir Xamarin.Mac uygulamasını korumalı alana alma](~/mac/app-fundamentals/sandboxing.md)

Bu makale App Store yayına yönelik bir Xamarin.Mac uygulamasını korumalı alana alma kapsar. Korumalı alana alma Git tüm öğeleri içerir: kapsayıcı dizinleri, yetkilendirmeleri, kullanıcı tarafından belirlenen izinler, ayrıcalık ayrımı ve çekirdeği zorlama.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[AVAudioPlayer ile ses çalma](~/mac/app-fundamentals/sounds.md)

Bu makalede bir AVAudioPlayer ses kullanmanın kayıttan yürütmeyi denetlemek için bir yardımcı sınıfını kullanmayı gösterir.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Hata Raporlama](~/mac/app-fundamentals/troubleshooting.md)

Bazen hepimiz yükleyememesine istediğimiz şekilde çalışmak için bir API alın veya bir hata iş çalışırken bir proje üzerinde çalışırken takılabilir. Hedefimiz xamarin, mobil ve Masaüstü uygulamalarınızı yazılırken başarılı olması içindir ve yardımcı olacak bazı kaynaklar sağladık.
