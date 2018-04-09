---
title: Neden Jenkins Xamarin desteklenmez?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: befcdcbee3114e760cec94a61a84106fddc72cf9
ms.sourcegitcommit: 271d3f7ea4abfcf87734d2c747a68cb8114d743c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2018
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>Neden Jenkins Xamarin desteklenmez?

## <a name="jenkins-support-explanation"></a>Jenkins destek açıklaması

Jenkins bir açık kaynak CI paketidir; Jenkins tarafından doğrudan neden bu kadar çok sorunları nedeniyle *kendisini* sorunları kodu nereye aldığınız karşı; gibi dosyalama gerekir [ana Jenkins depodaki](https://github.com/jenkinsci/jenkins), ya da depoyu için [ Jenkins.App](https://github.com/stisti/jenkins-app).

Bunun özel durumu Xamarin'ın Araçları'nda belirli hataları için yalıtılmış sorunları içindir; Bu durumda olacak şekilde şüpheleniyorsanız kontrol edebilirsiniz, [destek seçenekleri](~/cross-platform/troubleshooting/support-options.md)bir şey hangi Xamarin dışında takım desteklenebilir sorunu yeniden olabilir ancak *doğrudan* yardımcı.

## <a name="setup-jenkins-with-xamarin"></a>Jenkins Xamarin ile Kurulum

Yukarıda da belirtildiği gibi Jenkins sorunları ekibimiz tarafından doğrudan desteklenmeyen sırada; [kullanarak Jenkins xamarin'le](~/tools/ci/jenkins-walkthrough.md) Kılavuzu, Xamarin ile tümleşik bir Jenkins CI sunucusu kurmak için kullanılabilir. 

## <a name="fixes-for-common-issues"></a>Yaygın sorunların çözümleri
### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins Android SDK bulamıyor

Bu sorun için hata iletisi şuna şudur:

> hata XA5205: Android SDK dizini bulunamadı. Lütfen /p:AndroidSdkDirectory ayarlayın

Seçeneklerinizi SDK konumu ayarlamak için kullanmakta olduğunuz tam Jenkins Android eklentisi bağlı olarak değişebilir; eklentisi Kılavuzu'nda bu ayarlar hakkında bilgi aramak için uygun bir yerdir. Örneğin; [Android öykünücüsü eklentisi](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) varsa ancak SDK otomatik olarak arar, bulunamıyor; konumu da bu eklenti için Jenkins sistem yapılandırma sayfası aracılığıyla ayarlanabilir. 


## <a name="deprecated-errors"></a>Kullanım dışı hataları

> [!IMPORTANT]
> Xamarin en son sürümleri bu sorunu Çözümlendi. Ancak, en son sürümünü yazılım üzerinde gerçekleşirse, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) yapı bilgi ve tam bir günlük çıktısı, tam sürüm oluşturma.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Xamarin lisansı geçersiz Jenkins raporları
Bu sorun için hata iletileri genellikle aşağıdakine benzer olan

> XA9008 hata: komut satırından oluşturma iş lisansı gerekir

veya

> Hata: Xamarin Studio dışında yapı, Starter Edition Xamarin.iOS desteklemiyor 

Bu senaryo en yaygın nedenlerini Jenkins Xamarin lisansınız ile ilişkili olmayan bir kullanıcı hesabıyla oturum açarak kullanımıdır. Bu, çözümleme en basit yolu, kullanıcı hesabı aracılığıyla doğrudan bir uygulama olarak Jenkins yüklemektir. Bu işlem ve bazı ek konular burada açıklanmıştır: [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
