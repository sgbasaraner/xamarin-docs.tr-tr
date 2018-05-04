---
title: Sınama, en iyi duruma getirme ve Xamarin.Android uygulamaları dağıtma
description: Bu bölüm, bir uygulamayı test, performansı iyileştirmek, sürüm için hazırlama, bir sertifika ile oturum açın ve bir uygulama Mağazası'na yayımlamak açıklanmaktadır kılavuzları içerir.
ms.prod: xamarin
ms.assetid: 568C0B85-EFF3-AF6F-5605-95055193D367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 5b3061f30f6f120cf8edb41ccf5e70ae853aeb9e
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="deployment-and-testing"></a>Dağıtım ve Test Etme

Bu bölüm, bir uygulamayı test, performansı iyileştirmek, sürüm için hazırlama, bir sertifika ile oturum açın ve bir uygulama Mağazası'na yayımlamak açıklanmaktadır kılavuzları içerir.


##  <a name="application-package-sizesapp-package-sizemd"></a>[Uygulama paketi boyutları](app-package-size.md)

Bu makalede, bir xamarin Android uygulama paketi ve hata ayıklama sırasında verimli paket dağıtımı için kullanılan ve geliştirme aşamalarını yayın ilişkili stratejiler bağlı bölümlerini inceler.

##  <a name="building-appsbuilding-appsindexmd"></a>[Uygulamalar Oluşturma](building-apps/index.md)

Bu bölümde, oluşturma işlemi nasıl çalıştığını ve nasıl ABI özgü APKs geliştireceğinizi açıklar açıklanmaktadır.

##  <a name="command-line-emulatorcommand-line-emulatormd"></a>[Komut Satırı Öykünücüsü](command-line-emulator.md)

Bu makalede, komut satırı aracılığıyla öykünücü başlatma kısaca ele alınır.

## <a name="debuggingandroiddeploy-testdebuggingindexmd"></a>[Hata Ayıklama](~/android/deploy-test/debugging/index.md)

Bölümdeki kılavuzları Android öykünücüsünü, gerçek Android cihazlar ve hata ayıklama günlüğünü kullanarak uygulamanızın hatalarını ayıklama yardımcı olur.

##  <a name="setting-the-debuggable-attributeandroiddeploy-testdebuggable-attributemd"></a>[Debuggable özniteliğini ayarlama](~/android/deploy-test/debuggable-attribute.md)

Bu makalede gibi araçları şekilde debuggable özniteliğinin nasıl ayarlanacağı açıklanmaktadır `adb` JVM ile iletişim kurabilir.

##  <a name="environmentenvironmentmd"></a>[Ortam](environment.md)

Bu makalede Xamarin.Android yürütme ortamı ve program yürütme etkilemek Android Sistem özellikleri açıklanmaktadır.

##  <a name="gdbgdbmd"></a>[GDB](gdb.md)

Bu makalede nasıl kullanılacağı açıklanmaktadır `gdb` bir Xamarin.Android uygulaması hata ayıklama için.

##  <a name="installing-a-system-appinstall-system-appmd"></a>[Sistem uygulama yükleme](install-system-app.md)

Bu kılavuz bir Xamarin.Android uygulaması Android cihazında sistem uygulaması olarak veya özel bir ROM bir parçası olarak nasıl yükleneceğini açıklar

##  <a name="linking-on-androidlinkermd"></a>[Android bağlama](linker.md)

Bu makalede, uygulamanın son boyutunu azaltmak için Xamarin.Android tarafından kullanılan bağlama işlemini anlatılmaktadır. Gerçekleştirilebilir ve bazı yönergeler ve sorun giderme önerileri bağlayıcı kullanarak sonuçlanabilir hatalarını azaltmak için sağlayan bağlama çeşitli düzeyinden birini açıklar.

## <a name="xamarinandroid-performanceandroiddeploy-testperformancemd"></a>[Xamarin.Android performans](~/android/deploy-test/performance.md)

Xamarin.Android ile oluşturulan uygulamaların performansını artırmak için birçok tekniği vardır. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir.

## <a name="preparing-an-application-for-releaseandroiddeploy-testrelease-prepindexmd"></a>[Bir uygulama sürüm için hazırlama](~/android/deploy-test/release-prep/index.md)

Bir uygulama kodlanmış ve test sonra paketi dağıtım için hazırlamak üzere gereklidir. Bu paket hazırlama için ilk görev çoğunlukla bazı uygulama özniteliklerini ayarlama kapsar yayın uygulaması oluşturmaktır.

## <a name="signing-the-android-application-packageandroiddeploy-testsigningindexmd"></a>[Android uygulama paketi imzalama](~/android/deploy-test/signing/index.md)

Kimlik imzalama Android oluşturmayı öğrenin, Android uygulamaları için yeni bir imzalama sertifikası oluşturmak ve uygulama imzalama sertifikası ile oturum açın. Ayrıca, bu konuda disk için uygulama verilecek açıklanmaktadır *geçici* dağıtım. Sonuçta elde edilen APK bir uygulama mağazasına giderek olmadan Android cihazları içine dışarıdan yüklenebilir.

## <a name="publishing-an-applicationandroiddeploy-testpublishingindexmd"></a>[Bir uygulama yayımlama](~/android/deploy-test/publishing/index.md)

Bu makale dizisi Xamarin.Android ile oluşturulan bir uygulamanın genel dağıtım için adımları açıklar. Dağıtım e-posta, bir özel web sunucusu, Google Play veya Amazon uygulama mağazası gibi kanallar aracılığıyla Android için gerçekleşebilir.
