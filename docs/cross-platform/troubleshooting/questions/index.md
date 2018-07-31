---
title: Genel sık sorulan sorular
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 06aa6569301d1bfdbf9f6fd1e7397a38a9beb6f6
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350826"
---
# <a name="general-frequently-asked-questions"></a>Genel sık sorulan sorular

## <a name="portable-class-libraries"></a>Taşınabilir sınıf kitaplıkları

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Bir PCL’de hangi kitaplıkların desteklendiğini görüntüleyebilir miyim?](pcl-support-libraries.md)
Bu kılavuz, kaynaklar ve mevcut kitaplığınızı çeşitli PCL Hedef platformlar tarafından desteklenen ya da PCL profil dönüştürülebilir belirlemek için yöntemleri listeler.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL Yansıma API’si](pcl-reflection.md)
Microsoft, taşınabilir sınıf kitaplıkları kullanmak için yeni bir yansıma API'si geliştirmiştir. Bir PCL için taşımak istediğiniz mevcut yansıma kod varsa, çalışmayabilir.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL örnek olay incelemesi: Microsoft TPL Dataflow NuGet paketi için System.Diagnostics.Tracing ile ilgili sorunları nasıl çözebilirim?](pcl-case-study.md)
Xamarin.iOS ve Xamarin.Android %100 başvuru olarak bunların sağlayan her PCL profilinin kullanılmaz. Visual Studio Mac, Visual Studio ve NuGet Paket Yöneticisi için pratik kolaylık sağlamak için Xamarin projeleri yalnızca tamamlanmamış uygulamalara sahip birkaç profil kullanımını sağlar. Örneğin, Xamarin.iOS ve Xamarin.Android ne şu anda tam bir uygulama türlerini içerir `System.Diagnostics.Tracing` PCL ad alanı. Bu sorunu portable-net45 + win8 + wp8 + wpa81 başvurmak için uygulama projesi geçerek iş TPL veri akışı kitaplığı.

## <a name="nuget-packages--xamarin-components"></a>NuGet paketlerini & Xamarin bileşenleri
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[NuGet’i nasıl güncelleştirebilirim?](nuget-update.md)
NuGet güncelleştirmeleri, uzantıları ve eklentiler bulunabilir **güncelleştirmeleri** sekmesinde **NuGet Paket Yöneticisi**. Mac ve Visual Studio için Visual Studio güncelleştirmeleri bulmak için ayrıntılı gezinti bu Kılavuzu'nda bulunur.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Bir NuGet paketinin eski sürümünü nasıl yükleyebilirim?](nuget-package-downgrade.md)
Mac için Visual Studio ve Visual Studio, paketlerin daha eski sürümlerini seçme ve bunları otomatik olarak yükleme özellikleri sahiptir; nasıl çalıştığını güncelleştirme paketleri benzer.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Nuget paketleri güncelleştirildikten sonra paket eksik hatası](nuget-packages-missing.md)
Bu sorunu Xamarin.Forms örnek uygulama çözümleri temel olarak bildirildi, ancak bu sorunun olası NuGet paketlerini kullanan hiçbir projede oluşabilir.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Google Play Services Bileşenleri ile NuGet’i birleştirme](gps-components-nuget.md)
Birden çok Google Play Services bileşenleri ve NuGet paketleri olarak, ancak geliştiriciler için işleri kolaylaştırmak için kullanılabilir, biz artık bizim bileşenleri ile Nuget'i birleşik iki paketlere. Neredeyse her durumda, Google Play Hizmetleri kullanılmalıdır. (Froyo) paketini kullanmak üzere yalnızca etkin bir şekilde Froyo hedeflediğiniz varsa nedenidir.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Bileşenler makinemizin neresinde depolanır?](component-storage.md)
Bir uygulama projesine Xamarin bileşeni'ni yüklediğinizde bu kılavuzda listelenen iki konum arasında yer.


## <a name="troubleshooting"></a>Sorun giderme
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Sürüm bilgilerimi ve günlüklerimi nereden bulabilirim?](version-logs.md)
Bu kılavuzda, Xamarin sorunları gidermek için kullanılan çoğu tanılama bilgilerinin nerede bulunacağı ayrıntıları verilmektedir.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Ne zaman ve nasıl bir hata raporu bildirmeliyim?](howto-file-bug.md)
Mühendislerimiz nedeni (ve tüm olası düzeltmeleri) için bir sorun daha verimli bir şekilde belirlemek mümkün olacak şekilde bu kılavuz yüksek kaliteli hata raporları dosyalama için ipuçları sağlar.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Jenkins neden Xamarin tarafından desteklenmiyor?](xamarin-jenkins.md)
Jenkins CI açık kaynaklı bir pakettir; Jenkins tarafından doğrudan neden bu kadar çok sorunları nedeniyle *kendisini* sorunları kod burada aldığınız karşı; gibi Dosyalanan gerekecektir [Jenkins ana depodaki](https://github.com/jenkinsci/jenkins), veya depoda [ Jenkins.App](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Hata ayıklayıcı için hangi proje ayarları gereklidir?](debugger-settings.md)
Hata ayıklayıcının (isabet kesme noktaları, görüntü hata ayıklama günlükleri, vb.) beklendiği şekilde çalışması sırayla geliştirici araçları ve hata ayıklama bilgileri görüntüleme her ikisi de etkinleştirilmelidir. Bu kılavuz, bulun ve bu ayarları etkinleştirme işlemi açıklanmaktadır.

