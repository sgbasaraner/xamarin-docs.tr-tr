---
title: Jenkins neden Xamarin tarafından desteklenmez?
description: Bu belgede, Jenkins CI sistemi Xamarin'in etkileşim yüksek bir düzeyde açıklanmaktadır. Jenkins ile çalışırken gündeme bazı yaygın sorunlar ele alınmaktadır.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: 54947d04d6241120df4b3a0f60947ed5cc2b7ffd
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351165"
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>Jenkins neden Xamarin tarafından desteklenmez?

## <a name="jenkins-support-explanation"></a>Jenkins destek açıklaması

Jenkins CI açık kaynaklı bir pakettir; Jenkins tarafından doğrudan neden bu kadar çok sorunları nedeniyle *kendisini* sorunları kod burada aldığınız karşı; gibi Dosyalanan gerekecektir [Jenkins ana depodaki](https://github.com/jenkinsci/jenkins), veya depoda [ Jenkins.App](https://github.com/stisti/jenkins-app).

Xamarin'in araçlarındaki belirli hatalar yalıtılabilir sorunları bunun özel durumu içindir; Bu durumda olmasını şüpheleniyorsanız kontrol edebilirsiniz, [destek seçenekleri](~/cross-platform/troubleshooting/support-options.md), bir şey hangi Xamarin desteği dışında takım için sorunu yeniden olabilir ancak *doğrudan* yardımcı.

## <a name="setup-jenkins-with-xamarin"></a>Xamarin ile Jenkins ayarlama

Yukarıda da belirtildiği gibi Jenkins sorunları ekibimiz tarafından doğrudan desteklenmeyen sırada; [Xamarin ile Jenkins kullanma](~/tools/ci/jenkins-walkthrough.md) Kılavuzu, Xamarin ile tümleşik bir Jenkins CI sunucusu kurmak için kullanılabilir. 

## <a name="fixes-for-common-issues"></a>Sık karşılaşılan sorunların çözümleri

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins Android SDK bulunamadı

Bu sorun için hata iletisi aşağıdakine benzer şudur:

> hata XA5205: Android SDK dizini bulunamadı. Lütfen /p:AndroidSdkDirectory ayarlayın

Seçeneklerinizi SDK konumu ayarlamak için kullanmakta olduğunuz tam Jenkins Android eklentisi bağlı olarak değişebilir; eklentisi Kılavuzu'nda bu ayarlar hakkında bilgi aramak için iyi bir yerdir. Örneğin; [Android öykünücüsü eklentisi](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) ancak SDK otomatik olarak arar, bulunamıyor; konumu, bu eklenti için Jenkins sistemi yapılandırma sayfası aracılığıyla da ayarlanabilir. 


## <a name="deprecated-errors"></a>Kullanım dışı hataları

> [!IMPORTANT]
> Xamarin güncel sürümlerinde bu sorun giderilmiştir. Ancak, yazılımın en son sürümüne bağımlı gerçekleşirse, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) , tam sürüm oluşturma bilgilerini ve tam derleme günlük çıkışı.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Xamarin lisansı geçersiz Jenkins raporları
Bu sorun için hata iletileri gibi genellikle şunlardır

> XA9008 hata: komut satırından yapı iş lisansı gerektirir

veya

> Hata: Yapı dışında Xamarin Studio, başlangıç sürümü Xamarin.iOS desteklemiyor 

Bu senaryonun en yaygın nedeni Jenkins Xamarin lisansı ile ilişkili olmayan bir kullanıcı hesabıyla oturum açma tarafından kullanılır. Bu, çözümleme en basit yolu, kullanıcı hesabı aracılığıyla doğrudan bir uygulama olarak Jenkins yüklemektir. Bu işlem ve bazı ek hususlar aşağıda açıklanmıştır: [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
