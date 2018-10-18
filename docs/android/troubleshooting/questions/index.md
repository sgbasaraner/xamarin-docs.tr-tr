---
title: Sıkça Sorulan Sorular
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: ad3fc32245880f6603c63d33aac49309fd61b753
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "36935470"
---
# <a name="frequently-asked-questions"></a>Sıkça Sorulan Sorular

## <a name="installation--setup"></a>Yükleme ve Kurulum

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Hangi Android SDK paketlerini yüklemeliyim?](install-android-sdk-packages.md)

Android SDK'sını yükleme geliştirmek için en düşük gerekli tüm paketleri otomatik olarak içermez. Bu kılavuz bireysel Geliştirici farklılık, ancak genellikle Xamarin.Android ile geliştirmek için gerekli olacak paketleri ele alınmaktadır.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Android SDK konumlarımı nereden ayarlayabilirim?](android-sdk-location.md)

Bu kılavuzda, Android için çoğu kurulumları çalışmalıdır SDK ' nın her iki varsayılan ayarları açıklanır; ve gerekirse bu varsayılan Visual Studio Mac için Visual Studio veya değiştirme.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Java Development Kit (JDK) sürümünü nasıl güncelleştirebilirim?](update-jdk.md)

Bu makalede, Windows ve Mac üzerinde Java Development Kit (JDK) sürümünü güncelleştirmek verilmektedir

### <a name="can-i-use-java-development-kit-jdk-version-9-or-laterjdk9-errorsmd"></a>[Java Development Kit (JDK) 9 veya üzeri bir sürümünü kullanabilir miyim?](jdk9-errors.md)

Xamarin.Android, JDK 8 ya da Microsoft Mobil OpenJDK gerektirir. Bu makalede, JDK sürüm denetimi için yönergeleri ile birlikte JDK 9 veya sonraki bir sürümü yüklüyse görebileceği bazı yaygın hata iletilerini listeler.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Xamarin.Android.Support paketlerinin gerektirdiği Android Desteği kitaplıklarını kendim nasıl yükleyebilirim?](install-android-support-library.md)

Bu kılavuz, yüklemek için örnek adımlar sağlar `Xamarin.Android.Support.v4` Windows ve Mac'te destek kitaplığı

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Windows’da Android hatalarını ayıklamak için hangi USB sürücülerine ihtiyacım var?](android-drivers-debug-windows.md)

İçinde Windows geliştirirken bir Android cihazında hata ayıklamak için; uyumlu bir USB sürücü yüklemeniz gerekir. Android SDK Yöneticisi, Nexus cihazlarında kullanılamaz için destek ekler varsayılan olarak "Google USB sürücüsünü" içerir.
Diğer cihazlar, cihaz üreticisi tarafından yayımlanan USB sürücüleri gerektirir. Bu kılavuz, bu sürücüleri de gibi diğer test yöntemleri bulma hakkında bilgi sağlar.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Mac’te çalışan Android Emulator’lara bir Windows sanal makinesinden bağlanılabilir mi?](connect-android-emulator-mac-windows.md)

Bu kılavuz, Android öykünücüsü'nü kullanırken yöntemleri kapsar.

## <a name="general-questions"></a>Genel Sorular

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Android NUnit Test projesini nasıl otomatikleştirebilirim?](automate-android-nunit-test.md)

Bu kılavuzda bir Android NUnit test proje ayarlama adımları ele alınmaktadır _değil_ Xamarin.UITest proje. Xamarin.UITest kılavuzları bulunabilir [burada](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Android .axml dosyalarında Intellisense’i nasıl etkinleştirebilirim?](enable-axml-intellisense.md)

Bu kılavuz, Android .axml dosyaları için Visual Studio IntelliSense etkinleştirmeyi açıklar.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Android yayın derlemem neden İnternet’e bağlanamıyor?](android-internet.md)

Bu sorunun en yaygın nedeni olan **INTERNET** izni hata ayıklama derlemesinde otomatik olarak eklenir, ancak bir yayın yapısı için el ile ayarlamanız gerekir. Bu kılavuz, sürüm derlemeleri izni etkinleştirmek açıklar.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[Akıllı Xamarin Android Desteği v4 / v13 NuGet Paketleri](android-support-v4v13-libraries.md)

`Support-v4` ve `Support-v13` kullanılamaz birlikte aynı uygulamada, diğer bir deyişle, bunlar birbirini dışlar. Bunun nedeni, `Support-v13` gerçekten tüm türleri ve uygulamasını içeren `Support-v4`. Deneyin ve başvurusu aynı projede hem de, yinelenen tür hataları karşılaşırsınız.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[PathTooLongException hata nasıl giderebilirim?](path-too-long-exception.md)

Bu makalede çözümlemek açıklanmaktadır bir **PathTooLongException** bir Xamarin.Android projesi oluşturulurken ortaya çıkabilecek hata.



## <a name="deprecated"></a>kullanım dışı

> [!NOTE]
> Xamarin güncel sürümlerinde çözümlenen sorunlar için aşağıdaki makaleleri uygulayın. Ancak, yazılımın en son sürümüne bağımlı gerçekleşirse, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) , tam sürüm oluşturma bilgilerini ve tam derleme günlük çıkışı.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[Hangi Xamarin.Android sürümü, Lollipop desteğini eklemiştir?](xa-lollipop.md)

Bu kılavuz, başlangıçta Android M Önizleme için yazılmıştır. Android M Önizleme desteği ve Xamarin.Android 4.20 Android Lollipop desteğini eklemiştir Xamarin.Android 4.17 eklendi.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat - Verilen attr 'android:actionModeShareDrawable' adıyla eşleşen bir kaynak bulunamadı](missing-action-mode-share-drawable.md)

Bazı gerekli Android SDK paketleri eksikse Xamarin eski sürümlerinde bu hata oluşabilir.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Android Designer için Java bellek parametrelerini ayarlama](android-designer-java-memory.md)

Başlatma sırasında kullanılan varsayılan bellek parametrelerini `java` Android designer bazı sistem yapılandırmaları ile uyumsuz olabilir. işlem. Proje başına temelinde 5.7.2.7 Xamarin Studio ve Xamarin ile Visual Studio 3.9.344 bu ayarlar başlangıç özelleştirilebilir.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Android Resource.designer.cs dosyam güncelleştirilmiyor](resource-designer-wont-update.md)

Bir hatada Xamarin.Studio 5.1 kısmen veya tamamen .csproj dosyasındaki xml kodunu silerek .csproj dosyaları daha önce bozuk. Bu önemli bölümleri Android derleme sistemi (örneğin, Android Resource.designer.cs güncelleştirme) neden başarısız. Kararlı 5.1.4 itibaren 15 Temmuz serbest bırakın, bu hatanın düzeltildiğini; Ancak, çoğu durumda, bu kılavuzda açıklandığı şekilde el ile onarılması proje dosyasında yok.



