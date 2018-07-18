---
title: Xamarin.iOS uygulama temelleri
description: Bu, olaylar, arka planda işleme ve iş parçacığı uygulama taşıma güvenliği gibi Xamarin.iOS geliştirme için temel kavramları açıklayan çeşitli kılavuzları belge bağlanır.
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/21/2017
ms.openlocfilehash: de337291554e81a2434dcc30c163f4789fc832eb
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111218"
---
# <a name="xamarinios-application-fundamentals"></a>Xamarin.iOS uygulama temelleri

Bu bölümde bazı yaygın şeyler görevler veya geliştiriciler Xamarin.iOS (eski adıyla MonoTouch) uygulamaları geliştirirken bilmeniz gereken kavramlar bir kılavuz sağlar.

## <a name="accessibilityiosapp-fundamentalsaccessibilitymd"></a>[Erişilebilirlik](~/ios/app-fundamentals/accessibility.md)

Bu belgede, çeşitli API'ler ve mümkün olduğu kadar kullanıcılar için erişilebilir olan uygulamalar oluşturmanıza yardımcı olmak için kullanılabilecek araçlar açıklanmaktadır.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[Uygulama Aktarım Güvenliği](~/ios/app-fundamentals/ats.md)

Bu makalede, iOS 9 uygulama ve bunun sizin için Xamarin.iOS projeleri anlamı uygulama taşıma güvenliği zorlayan güvenlik değişiklikleri kullanıma sunacak, ATS yapılandırma seçeneklerini kapsar ve gerekirse nasıl çevirme ATS kapsar. ATS varsayılan olarak etkin olduğundan güvenli olmayan herhangi bir internet bağlantısı (siz açıkça izin sürece) iOS 9 apps'te bir özel durum oluşturacak.

## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Arka Planda İşleme](~/ios/app-fundamentals/backgrounding/index.md)

Arka planda işleme veya işlerken arka plan uygulamaları başka bir uygulama ön planda çalışırken görevleri arka planda izin vererek işlemidir. Bu kılavuz, iOS arka plan giriş olarak görev yapar.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[Kod içinde iOS uygulamaları oluşturma](~/ios/app-fundamentals/ios-code-only.md)

Bu makalede tamamen Mac için Visual Studio ve Visual Studio kullanarak kod içinde iOS uygulamaları oluşturmak nasıl inceler. Bu Uıkit görünümleri hiyerarşisini oluşturarak bir denetleyicide bir uygulama ekranı oluşturmak için boş bir proje şablonu başlangıç nasıl gösterir. Ardından, bir denetleyicisi yüklenebilen özel görünümlerini oluşturma işlemini açıklar.

## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[Olaylar, protokoller ve temsilciler](~/ios/app-fundamentals/delegates-protocols-and-events.md)

Bu makalede, geri çağırmalarını almak ve kullanıcı arabirimi denetimlerini veri ile doldurmak için kullanılan anahtar iOS teknolojiler sunar. Bu, olaylar, protokoller ve temsilciler teknolojilerdir; Bu makalede ne açıklanmaktadır. bunların her biri olduğunu ve nasıl her kullanılan C# ' tan. İOS denetimleri Xamarin.iOS Xamarin.iOS, protokoller ve temsilciler gibi Objective-C Kavramları için destek sağlar. nasıl tanıdık .NET olaylar, de oluşturmak için nasıl kullandığını gösterir (Objective-C temsilciler olmamalıdır C# ile temsilciler kafanız). Bu makalede ayrıca protokolleri hem de temel olarak Objective-C Temsilciler ve temsilci olmayan senaryolarda kullanımını gösteren örnekler sağlar.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[Dosya sistemi ile çalışma](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS, dosya ve dizinleri, herhangi bir .NET uygulamasında kullanacağınız iOS ile çalışmak için aynı System.IO sınıflarını kullanabilirsiniz. Ancak tanıdık sınıflar ve yöntemler rağmen iOS oluşturulan veya erişilen dosyalar için bazı sınırlamalar uygular ve bu özelliklere belirli dizinleri sağlar. Bu makalede, bu kısıtlamaları ve özellikler açıklanmaktadır ve dosya erişimi bir Xamarin.iOS uygulamasına nasıl çalıştığını gösterir.

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[Görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md)

Bu makalede, Xamarin.iOS, hem uygulama destek görüntüleri (örneğin simgeler gibi yükleme görüntüleri, vb.) hem de (denetimlere uygulanan görüntüleri için gibi) uygulamaları yansımalar görüntüleri kullanma inceler. Ayrıca kod görüntülerden ile etkileşim kurmayı yanı sıra, görüntüleri birleştirmek için Mac için Visual Studio kullanmayı da kapsar.

## <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[Yerelleştirme](~/ios/app-fundamentals/localization/index.md)

Bu kılavuzda ele alınmaktadır uluslararası destek için bir Xamarin.iOS uygulamasına Kodlamalar eklenmesi.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[Özellik listeleri ile çalışma](~/ios/app-fundamentals/index.md)

Bu belge, Visual Studio Entitlements.plist Info.plist ile çalışmak için Mac grafik ve gelişmiş özellik listesi (.plist) Düzenleyicisi için tanıtır. Ayar simgeleri gösterir ve başlatma görüntüleri için iOS uygulama ve gelen belirten uygulama özellikleri (yetkilendirme) gösterir. Mac için Visual Studio içinde

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[Güvenlik ve gizlilik ile çalışma](~/ios/app-fundamentals/security-privacy.md)

Apple geliştirici uygulamalarını güvenliğini artırmak ve son kullanıcının gizlilik olun yardımcı olacak bazı geliştirmeler güvenlik ve gizlilik iOS 10 (ve büyük) kullanıma sunmuştur. Bu makalede, bir Xamarin.iOS uygulaması bu özelliklerini uygulama ele alınacaktır.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[İş parçacığı oluşturma](~/ios/app-fundamentals/threading.md)

Bu makalede, bir Xamarin.iOS uygulamasına iş parçacığı oluşturma açıklanır ve biraz hakkında konuşuyor .NET iş parçacığı havuzu, yanıt veren uygulamaları ve çöp toplama.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[Dokunma](~/ios/app-fundamentals/touch/index.md)

Günümüzün cihazlarının çoğu dokunmatik ekranlarda kullanıcıların doğal ve sezgisel bir şekilde, hızlı ve verimli bir şekilde cihazlarla etkileşime girmesine izin verin. Bu etkileşimi yalnızca basit touch algılama için sınırlı değildir: hareketlerini de kullanmak da mümkündür. Örneğin, tabletinizde sıkıştırarak yakınlaştırma hareketini bir yaygın bunun – kullanıcının yakınlaştırma veya uzaklaştırma iki parmağınızı ekranla parçası hareketinden tarafından örneğidir. Bu kılavuz, dokunma ve iOS hareketleri inceler.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[Kullanıcı varsayılanları ile çalışma](~/ios/app-fundamentals/user-defaults.md)

`NSUserDefaults` Sınıfı iOS uygulamaları ve uzantıları programlı olarak varsayılan sistem genelinde sistemi ile etkileşim kurmak için bir yol sağlar. Varsayılan olarak sistem kullanarak, kullanıcı bir uygulamanın davranışı veya (tabanlı uygulama tasarımı) tercihlerini karşılamak için stil oluşturma yapılandırabilirsiniz. Örneğin, ölçüm vs'de veriler İngiliz ölçümler sunmak veya belirli bir kullanıcı Arabirimi temasını seçin.
