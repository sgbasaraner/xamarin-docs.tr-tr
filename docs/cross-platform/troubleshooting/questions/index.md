---
title: "Genel sık sorulan sorular"
ms.topic: article
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 0c01bfb08d525fd336b29f405304efaeaf749f35
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="general-frequently-asked-questions"></a>Genel sık sorulan sorular

## <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 Sürüm Adayı
### <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarinvisualstudio-2017-rcmd"></a>[Xamarin ile Visual Studio 2017 Sürüm Adayı kullanabilir miyim?](visualstudio-2017-rc.md)
Xamarin Visual Studio2017 RC'de yükleme hakkında bilgi için Visual Studio 2017 Sürüm Adayı (RC ile) de Xamarin kullanarak geçerli etkilerini açıklaması için.

## <a name="portable-class-libraries"></a>Taşınabilir sınıf kitaplıkları
### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Nasıl hangi kitaplıkları bir PCL desteklenir görüntüleyebilirsiniz?](pcl-support-libraries.md)
Bu kılavuz, kaynakları ve varolan kitaplığınızın çeşitli PCL Hedef platformlar tarafından desteklenen ya da bir PCL profiline dönüştürülebilir belirlemek için yöntemleri listeler.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL yansıma API](pcl-reflection.md)
Microsoft, taşınabilir sınıf kitaplıkları kullanmak için yeni bir yansıma API geliştirmiştir. Bir PCL taşımak istediğiniz bazı var olan Yansıma kodu varsa, çalışmayabilir.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL örnek olay incelemesi: nasıl ı çözümleyebildiğinden System.Diagnostics.Tracing için Microsoft TPL veri akışı NuGet paketi için ilgili sorunları?](pcl-case-study.md)
Xamarin.iOS ve Xamarin.Android % 100'başvuru olarak izin her PCL profili kullanılmaz. Mac, Visual Studio ve NuGet Paket Yöneticisi için Visual Studio'da pratik kolaylık olması için Xamarin projeleri yalnızca tamamlanmamış uygulamaları birkaç profili kullanılmasına izin verin. Örneğin, Xamarin.iOS ne Xamarin.Android şu anda tam bir uygulama türlerini içerir `System.Diagnostics.Tracing` PCL ad alanı. Bu sorunu taşınabilir net45 olduğu win8 + wp8 + wpa81 başvurmak için uygulama projesi geçerek çözmek TPL veri akışı kitaplığı sürümü.

## <a name="nuget-packages--xamarin-components"></a>NuGet paketlerini & Xamarin bileşenleri
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[NuGet nasıl güncelleştirebilir miyim?](nuget-update.md)
Altında NuGet güncelleştirmeler, uzantıları ve eklentiler bulunabilir **güncelleştirmeleri** sekmesinde **NuGet Paket Yöneticisi**. Mac ve Visual Studio için Visual Studio'da güncelleştirmelerini bulmak için ayrıntılı gezinti bu Kılavuzu'nda bulunur.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Bir NuGet paketi nasıl düşürmek?](nuget-package-downgrade.md)
Mac için Visual Studio & Visual Studio'nun eski sürümlerinin seçme ve bunları otomatik olarak yükleme özellikleri vardır; nasıl çalışır güncelleştirme paketleri benzer.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Nuget paketlerini güncelleştirdikten sonra eksik paketleri hatası](nuget-packages-missing.md)
Bu sorunu çoğunlukla Xamarin.Forms örnek uygulama çözümlerini bildirildi, ancak bu sorunun olası NuGet paketlerini kullanan herhangi bir projede oluşabilir.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Google Play birleştirin bileşenleri ve NuGet Hizmetleri](gps-components-nuget.md)
Var. birkaç Google Play Hizmetleri bileşenleri ve NuGet paketleri olabilir ancak geliştiriciler için işleri kolaylaştırmak için kullanılan, biz şimdi bizim bileşenleri ve NuGet birleşik iki paketlere. Neredeyse her durumda, Google Play Hizmetleri'nin kullanılmalıdır. (Froyo) paketini kullanmak için yalnızca, etkin olarak Froyo hedeflediğiniz varsa nedenidir.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Bileşenleri my makinede saklandığı?](component-storage.md)
Bir uygulama projesine bir Xamarin bileşeni'ni yüklediğinizde bu kılavuzda listelenen iki konumda yer.


## <a name="troubleshooting"></a>Sorun giderme
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[My sürüm bilgileri ve günlükleri nereden bulabilirim?](version-logs.md)
Bu kılavuzda, Xamarin sorununu gidermek için kullanılan çoğu tanılama bilgilerinin nereden ayrıntıları verilmektedir.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Bir hata raporu, ne zaman ve nasıl dosya?](howto-file-bug.md)
Bizim mühendisleri neden (ve tüm olası düzeltmeleri) bir soruna yönelik daha verimli bir şekilde belirlemek mümkün; böylece bu kılavuzda yüksek kaliteli hata raporları dosyalama için ipuçları verilmektedir.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Neden Jenkins Xamarin desteklenmez?](xamarin-jenkins.md)
Jenkins bir açık kaynak CI paketidir; Jenkins tarafından doğrudan neden bu kadar çok sorunları nedeniyle *kendisini* sorunları kodu nereye aldığınız karşı; gibi dosyalama gerekir [ana Jenkins depodaki](https://github.com/jenkinsci/jenkins), ya da depoyu için [ Jenkins.App](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Hata ayıklayıcı proje ayarları gereklidir?](debugger-settings.md)
(İsabet kesme noktaları, görüntü hata ayıklama günlükleri, vb.) beklendiği şekilde çalışması hata ayıklayıcı için sırayla geliştirici araçları ve hata ayıklama bilgileri görünen her ikisi de etkinleştirilmesi gerekir. Bu kılavuzda nasıl bulacağınızı ve bu ayarları etkinleştirin ayrıntıları verilmektedir.

