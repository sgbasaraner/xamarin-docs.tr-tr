---
title: Xamarin.iOS uygulaması temelleri
description: Bu, olaylar, backgrounding ve iş parçacığı oluşturma uygulama taşıma güvenliği gibi Xamarin.iOS geliştirme için temel kavramları tanımlayan çeşitli kılavuzları belge bağlanır.
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/21/2017
ms.openlocfilehash: cdace50d851b2c99f9241b869f248e58d5b93377
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784503"
---
# <a name="xamarinios-application-fundamentals"></a>Xamarin.iOS uygulaması temelleri

Bu bölümde bazı daha yaygın şeyler görevler veya geliştiricilerin Xamarin.iOS (önceki adıyla MonoTouch) uygulamaları geliştirirken farkında olması gereken kavramlar bir kılavuz sağlar.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[Uygulama Aktarım Güvenliği](~/ios/app-fundamentals/ats.md)

Bu makalede, bir iOS 9 uygulama ve Xamarin.iOS projeleriniz için anlamı bu uygulama taşıma güvenliği zorlar güvenlik değişiklikleri getirir, ATS yapılandırma seçeneklerini kapsar ve gerekirse nasıl ATS çevirin kapsar. ATS varsayılan olarak etkinleştirilmiş olduğundan, güvenli olmayan herhangi bir internet bağlantısı (siz açıkça, izin verilen sürece) bir özel durum iOS 9 uygulamalarda ortaya koyar.


## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Arka Planda İşleme](~/ios/app-fundamentals/backgrounding/index.md)

Arka plan işleme veya backgrounding başka bir uygulama ön planda çalıştığı sırada görevleri arka planda uygulamaları izin vererek işlemidir. Bu kılavuz, arka plan iOS işlemleri giriş olarak görev yapar.


## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[Olaylar, protokolleri ve temsilciler](~/ios/app-fundamentals/delegates-protocols-and-events.md)

Bu makalede geri aramalar almak ve kullanıcı arabirimi denetimlerini veri ile doldurmak için kullanılan anahtar iOS teknolojiler sunar. Bu, olaylar, protokolleri ve temsilciler teknolojilerdir; Bu makalede ne açıklanmaktadır her birini ve nasıl her kullanılan C# ' dan. İOS denetimleri Xamarin.iOS Xamarin.iOS Objective-C Kavramları protokolleri ve temsilciler gibi için destek sağlar. nasıl tanıdık .NET olayları da kullanıma sunmak için nasıl kullanır gösterir (Objective-C temsilciler olmamalıdır C# ile temsilciler kafası). Bu makalede ayrıca nasıl protokolleri hem temel olarak Objective-C Temsilciler ve temsilci olmayan senaryolarda kullanılır Göster örnekler verilmektedir.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[İş parçacığı oluşturma](~/ios/app-fundamentals/threading.md)

Bu makalede, bir Xamarin.iOS uygulaması iş parçacığı oluşturma açıklanır ve hakkında biraz ettiği .NET iş parçacığı havuzu, esnek uygulamaları ve atık toplama.&nbsp;

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[Görüntülerle Çalışma](~/ios/app-fundamentals/images-icons/index.md)

Bu makalede Xamarin.iOS, uygulama destek görüntüsü (örneğin, simgeler, yükleme görüntüleri, vb.) ve (örneğin, denetimlerine uygulanan görüntüleri için) uygulamalar içindeki görüntüleri görüntüleri kullanmak nasıl inceler. Ayrıca Mac için Visual Studio görüntüleri birleştirmek için nasıl kullanılacağı ve bunun yanı sıra kodu görüntülerden etkileşimde nasıl ele alınmaktadır.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[Özellik listeleri ile çalışma](~/ios/app-fundamentals/index.md)

Bu belge Info.plist ve Entitlements.plist ile çalışmak için Visual Studio için Mac'ın grafik ve gelişmiş özellik listesi (.plist) Düzenleyicisi içerir. Ayarı simgeler gösterir ve başlatma görüntüleri iOS uygulaması için ve gelen belirten uygulama yeteneklerinin (yetkilendirme) gösteren Mac için Visual Studio içinde

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[Dosya sistemi ile çalışma](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS aynı System.IO sınıfları dosyaları ve dizinleri herhangi bir .NET uygulamasında kullanacağınız iOS içinde çalışmak için kullanabilirsiniz. Ancak, bilinen sınıflar ve yöntemler rağmen iOS oluşturulan veya erişilen dosyalar üzerinde bazı kısıtlamalar uygular ve ayrıca özel özellikleri dizinleri belirli sağlar. Bu makalede, bu sınırlamaları ve özellikler açıklanmaktadır ve dosya erişimi bir Xamarin.iOS uygulaması nasıl çalıştığını gösterir.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[Kodda iOS uygulamaları oluşturma](~/ios/app-fundamentals/ios-code-only.md)

Bu makalede tamamen Mac için Visual Studio ve Visual Studio kullanarak kod içinde iOS uygulamaları oluşturmak nasıl inceler Bir uygulama ekran bir denetleyicide Uıkit görünümleri hiyerarşisini oluşturarak oluşturmak için boş bir proje şablonu başlangıç kullanmayı gösterir. Ardından, bir denetleyicisine yüklenebilir özel görünümleri oluşturma açıklanmaktadır.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[Kullanıcı varsayılanları ile çalışma](~/ios/app-fundamentals/user-defaults.md)

`NSUserDefaults` Sınıfı iOS uygulamaları ve uzantıları programlı olarak sistem genelinde varsayılan sistemi ile etkileşimde bulunmak için bir yol sağlar. Varsayılan olarak sistem kullanarak, kullanıcı bir uygulamanın davranışı veya (uygulama tasarımını göre) tercihlerini karşılamak için stil oluşturma yapılandırabilirsiniz. Örneğin, ölçüm vs verilerde İngiliz ölçümleri sunan veya belirli bir kullanıcı Arabirimi tema seçin.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[Dokunma](~/ios/app-fundamentals/touch/index.md)

Dokunmatik ekranlar bugünün aygıtların çoğu üzerinde kullanıcıların hızlı ve verimli şekilde doğal ve sezgisel şekilde cihazlarla etkileşime girmesine izin. Bu etkileşimi yalnızca basit dokunma algılama sınırlı değildir – hareketleri de kullanmak da mümkündür. Örneğin, tutarak yakınlaştırma hareketi kullanıcı yakınlaştırmak veya uzaklaştırmak iki parmakları ekran parçası çimdik tarafından bu – çok yaygın bir örneği bulunmaktadır. Bu kılavuz, dokunma ve iOS hareketleri inceler.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[Güvenlik ve gizlilik ile çalışma](~/ios/app-fundamentals/security-privacy.md)

Apple geliştirici uygulamalarını güvenliğini artırmak ve son kullanıcının gizliliği güvence altına yardımcı olacak bazı geliştirmeler güvenlik ve gizlilik iOS 10 (ve büyük) kullanıma sunmuştur. Bu makalede, bir Xamarin.iOS uygulaması bu özellikleri uygulama ele alınacaktır.

##  <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[Yerelleştirme](~/ios/app-fundamentals/localization/index.md)

Bu kılavuz kapsar uluslararası destek için bir Xamarin.iOS uygulamasına Kodlamalar eklenmesi.
