---
title: "Parmak izi kimlik doğrulaması"
description: "Bu kılavuz, parmak izi kimlik doğrulaması, bir Xamarin.Android uygulaması Android 6.0 ile sunulan ekleme açıklanır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 79f5f81e11f62359c3b951500d4ab5cbd63fb507
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="fingerprint-authentication"></a>Parmak izi kimlik doğrulaması

_Bu kılavuz, parmak izi kimlik doğrulaması, bir Xamarin.Android uygulaması Android 6.0 ile sunulan ekleme açıklanır._


## <a name="fingerprint-authentication-overview"></a>Parmak izi kimlik doğrulamasına genel bakış

Android cihazlarda parmak izi tarayıcılar varış alternatif geleneksel kullanıcı adı/parola yöntemi uygulamalara kullanıcı kimlik doğrulaması sağlar. Bir kullanıcının kimliğini doğrulamak için parmak izi kullanılması, bir kullanıcı adı ve parola daha az müdahale güvenlik birleştirmek bir uygulama için mümkün kılar.

FingerprintManager API'ları parmak izi tarayıcıyla hedef aygıtlar ve API düzeyini 23 (Android 6.0) çalıştırıyorsanız ya da daha yüksek. API'ler bulunan `Android.Hardware.Fingerprints` ad alanı. Android destek kitaplığı v4 parmak izi Android daha eski sürümleri için amacı API sürümleri sağlar. API bulunan Uyumluluk `Android.Support.v4.Hardware.Fingerprint` ad alanı aracılığıyla dağıtılmış [Xamarin.Android.Support.v4 NuGet paketi](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).

[FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (ve destek kitaplığı karşılığı [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) donanım tarama parmak izi kullanmaya yönelik birincil sınıf. Donanım ile etkileşimlerini yönetir sistem düzeyinde hizmeti etrafında bir Android SDK sarmalayıcı sınıftır. Parmak izi tarayıcı başlatmak ve geri bildirim için tarayıcıdan yanıt sorumludur. Bu sınıf, yalnızca üç üyeleriyle oldukça basit bir arabirim vardır:

* **`Authenticate`** &ndash; Bu yöntem, donanım tarayıcı başlatmak ve kullanıcı parmak izini taramak bekleyen arka planda hizmetini başlatın.
* **`EnrolledFingerprints`** &ndash; Bu özellik döndürülecek `true` kullanıcı cihazı ile bir veya daha fazla parmak izi kaydedildiyse.
* **`HardwareDetected`** &ndash; Bu özellik, aygıt parmak izi tarama destekleyip desteklemediğini belirlemek için kullanılır.

`FingerprintManager.Authenticate` Yöntemi, parmak izi tarayıcısı başlatmak için bir Android uygulama tarafından kullanılır. Aşağıdaki kod parçacığında, destek kitaplığı uyumluluk API'lerini kullanarak çağırmak nasıl örneğidir:

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

Bu kılavuzu kullanmak nasıl ele alınacaktır `FingerprintManager` parmak izi kimlik doğrulaması ile bir Android uygulama geliştirmek için API'ler. Örneği ve oluşturmak nasıl ele alınacaktır bir `CryptoObject` parmak izi tarayıcısı sonuçlarından güvenliğinin sağlanmasına yardımcı olur. Biz nasıl uygulamanın alt sınıf gereken inceleyeceğiz `FingerprintManager.AuthenticationCallback` ve geri bildirim için parmak izi tarayıcıdan yanıt. Son olarak, bir Android cihaz veya öykünücü parmak izi kaydetme ve nasıl kullanılacağını göreceğiz **adb** bir parmak izi tarama benzetimini yapmak için.

## <a name="requirements"></a>Gereksinimler

Parmak izi kimlik doğrulaması gerektiren Android 6.0 (API düzeyi 23) veya daha yüksek ve bir parmak izi tarayıcısı aygıtla. 

Cihaz kimliğinin doğrulanması her bir kullanıcı için parmak izi zaten kayıtlı olması gerekir. Bu parola, PIN, manyetik düzeni veya yüz tanıma kullanan bir ekran kilidi ayarlama içerir. Android öykünücüsünde parmak izi kimlik doğrulaması işlevindeki bazıları benzetimini yapmak mümkündür.  Bu iki konular hakkında daha fazla bilgi için lütfen bkz [parmak izi kaydetme](enrolling-fingerprint.md) bölümü. 






## <a name="related-links"></a>İlgili bağlantılar

- [Parmak izi Kılavuzu örnek uygulaması](https://developer.xamarin.com/samples/monodroid/FingerprintGuide/)
- [Parmak izi iletişim kutusu örneği](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Çalışma zamanında izinleri isteyen](http://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](http://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Parmak izi ve ödemeleri API (video)](https://youtu.be/VOn7VrTRlA4)
