---
title: "Sıkça Sorulan Sorular"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/19/2018
ms.openlocfilehash: 69a5c905367f9e6dbd38acb664ad21b9dbe63efc
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2018
---
# <a name="frequently-asked-questions"></a>Sıkça Sorulan Sorular

## <a name="installation--setup"></a>Yükleme ve Kurulumu

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Hangi Android SDK paketlerini yüklemeliyim?](install-android-sdk-packages.md)

Android SDK'sını yükleme geliştirmek için tüm minimum gerekli paketleri otomatik olarak içermez. Tek tek Geliştirici farklılık, ancak bu kılavuz, genellikle Xamarin.Android ile geliştirmek için gerekli olacak paketleri açıklanır.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Android SDK konumlarımı nereden ayarlayabilirim?](android-sdk-location.md)

Bu kılavuzda, Android SDK'ın, çoğu kurulumlarını çalışması gerekir, her iki varsayılan ayarları açıklanır; ve Visual Studio'da bu varsayılan Mac veya Visual Studio için gerekirse nasıl değiştirilir.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Java Development Kit (JDK) sürümünü nasıl güncelleştirebilirim?](update-jdk.md)

Bu makalede Windows ve Mac üzerinde Java Geliştirme Seti (JDK) Sürüm güncelleştirme nasıl gösterir

### <a name="can-i-use-java-development-kit-jdk-version-9jdk9-errorsmd"></a>[Java Development Kit (JDK) 9 sürümünü kullanabilir miyim?](jdk9-errors.md)

Xamarin.Android JDK 9 yerine JDK 8 gerektirir. Bu makalede JDK sürüm denetimi için yönergeler birlikte JDK 9 yüklüyse karşılaşabileceğiniz bazı yaygın hata iletilerini listeler.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Xamarin.Android.Support paketlerinin gerektirdiği Android Desteği kitaplıklarını kendim nasıl yükleyebilirim?](install-android-support-library.md)

Bu kılavuz, yüklemek için örnek adımlar sağlar `Xamarin.Android.Support.v4` Windows & Mac destek kitaplığı

### <a name="how-do-i-install-google-play-services-in-an-emulatorinstall-gpsmd"></a>[Bir öykünücüde Google Play Services’i nasıl yükleyebilirim?](install-gps.md)

Visual Studio'nun Android öykünücüsünde Google Play Hizmetleri yükleme hakkında bilgi için bu kılavuzu bağlantılar.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Windows’da Android hatalarını ayıklamak için hangi USB sürücülerine ihtiyacım var?](android-drivers-debug-windows.md)

Android cihazında Windows geliştirirken hata ayıklamak için; uyumlu bir USB sürücü yüklemeniz gerekir. Android SDK Manager Nexus cihazlar için destek ekler varsayılan olarak "Google USB sürücüsü" içerir.
Diğer cihazları özellikle aygıt üreticisi tarafından yayımlanan USB sürücüleri gerektirir. Bu kılavuz, bu sürücüleri de gibi alternatif test yöntemleri bulma hakkında bilgi sağlar.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Mac’te çalışan Android Emulator’lara bir Windows sanal makinesinden bağlanılabilir mi?](connect-android-emulator-mac-windows.md)

Bu kılavuz, Google Android öykünücüsü kullanırken yöntemleri kapsar.

## <a name="general-questions"></a>Genel Sorular

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Android NUnit Test projesini nasıl otomatikleştirebilirim?](automate-android-nunit-test.md)

Bu kılavuz, bir Android NUnit test projesi adımlarını kapsar. _değil_ Xamarin.UITest projesi. Xamarin.UITest kılavuzları bulunabilir [burada](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Android .axml dosyalarında Intellisense’i nasıl etkinleştirebilirim?](enable-axml-intellisense.md)

Bu kılavuz, Android .axml dosyaları için Visual Studio'nun IntelliSense etkinleştirmeyi açıklar.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Android yayın derlemem neden İnternet’e bağlanamıyor?](android-internet.md)

Bu sorunun en yaygın nedeni olan **Internet** izni olan bir hata ayıklama derlemesi otomatik olarak eklenir, ancak yayın derlemesi için el ile ayarlamanız gerekir. Bu kılavuz, sürüm derlemeleri izni etkinleştirmek açıklar.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[Akıllı Xamarin Android Desteği v4 / v13 NuGet Paketleri](android-support-v4v13-libraries.md)

`Support-v4` ve `Support-v13` kullanılamaz birlikte aynı uygulamada, diğer bir deyişle, bunlar karşılıklı olarak birbirini dışlar. Bunun nedeni, `Support-v13` gerçekten tüm türleri ve uyarlamasını içeren `Support-v4`. Çalışırsanız ve her ikisi de aynı projede başvuru yinelenen türü hatalarla karşılaşır.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[PathTooLongException hata nasıl giderebilirim?](path-too-long-exception.md)

Bu makalede nasıl giderileceği açıklanmaktadır bir **PathTooLongException** bir Xamarin.Android projesi oluşturma sırasında ortaya çıkabilecek hata.



## <a name="deprecated"></a>Kullanım dışı

> [!NOTE]
> Aşağıdaki makaleleri Xamarin en güncel sürümlerini çözümlenen sorunlar için geçerlidir. Ancak, en son sürümünü yazılım üzerinde gerçekleşirse, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) yapı bilgi ve tam bir günlük çıktısı, tam sürüm oluşturma.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[Hangi Xamarin.Android sürümü, Lollipop desteğini eklemiştir?](xa-lollipop.md)

Bu kılavuz, başlangıçta Android M Önizleme için yazılmıştır. Xamarin.Android 4.17 Android M Önizleme desteği & Xamarin.Android 4.20 ek Android Lolipop destek eklendi.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat - Verilen attr 'android:actionModeShareDrawable' adıyla eşleşen bir kaynak bulunamadı](missing-action-mode-share-drawable.md)

Gerekli Android SDK pacakages bazıları eksikse, Xamarin eski sürümlerinde bu hata oluşabilir.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Android Designer için Java bellek parametrelerini ayarlama](android-designer-java-memory.md)

Başlatma sırasında kullanılan varsayılan bellek parametreleri `java` işlemek için Android Tasarımcısı bazı sistem yapılandırmasıyla uyumsuz olabilir. Xamarin Studio 5.7.2.7 ve Xamarin ile Visual Studio 3.9.344 için bu ayarları başlangıç projesi başına temelinde özelleştirilebilir.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Android Resource.designer.cs dosyam güncelleştirilmiyor](resource-designer-wont-update.md)

Xamarin.Studio 5.1 hatada kısmen veya tamamen .csproj dosyasındaki xml kodunu silerek .csproj dosyaları daha önce bozuk. Bu önemli Android bölümlerini yapı (Android Resource.designer.cs güncelleştirme gibi) sistem neden başarısız. Kararlı 5.1.4 itibariyle 15 Temmuz sürüm, bu hatanın düzeltildiğini; Ancak birçok durumda proje dosyası bu kılavuzda açıklandığı şekilde el ile onarılması gerekmektedir.



