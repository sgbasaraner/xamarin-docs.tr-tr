---
title: Parmak izi kimlik doğrulaması ile çalışmaya başlama
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 70d27ef3d7518619a246c25aac128b2fd1ed70c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-fingerprint-authentication"></a>Parmak izi kimlik doğrulaması ile çalışmaya başlama

Başlamak için şimdi ilk böylece uygulama parmak izi kimlik doğrulaması kullanan bir Xamarin.Android projesi nasıl yapılandırılacağı açıklanmıştır:

1. Güncelleştirme **AndroidManifest.xml** parmak izi API'leri gerektirdiği izinler bildirmek için.
2. Bir başvuru elde `FingerprintManager`.
3. Aygıt parmak izi tarama yeteneğine sahip olup olmadığını denetleyin.

## <a name="requesting-permissions-in-the-application-manifest"></a>İstekte bulunan uygulama izinleri bildirimi

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bir Android uygulamasını istemelidir `USE_FINGERPRINT` bildiriminde izni. Aşağıdaki ekran görüntüsünde, Visual Studio 2015'te uygulama bu izni eklemek gösterilmektedir:

[![Kullanım etkinleştirme\_Android derleme bildirimi ekranında parmak İZİ](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bir Android uygulamasını istemelidir `USE_FINGERPRINT` bildiriminde izni. Aşağıdaki ekran görüntüsünde Mac için Visual Studio'da uygulama bu izni eklemeyi gösterir:

[![Android uygulaması ekranında UseFingerprint etkinleştirme](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>FingerprintManager örneğini alma

Ardından, uygulama örneği almalısınız `FingerprintManager` veya `FingerprintManagerCompat` sınıfı. Android eski sürümleriyle uyumlu olacak şekilde bir Android uygulamasını API Uyumluluk kullanması gereken Android desteği v4 NuGet paketi bulunan. Aşağıdaki kod parçacığında uygun işletim sisteminden nasıl alınacağı gösterilmektedir: 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

Önceki kod parçacığını içinde `context` tüm Android olduğu `Android.Content.Context`. Genellikle bu, kimlik doğrulaması gerçekleştirmeye etkinliktir.

## <a name="checking-for-eligibility"></a>Uygunluk denetleniyor

Bir uygulama, parmak izi kimlik doğrulaması kullanmak mümkün olduğundan emin olmak için birkaç denetimleri gerçekleştirmeniz gerekir. Toplam uygunluk denetlemek için uygulamanın kullandığı beş koşul vardır:  
 

**API düzeyi 23** &ndash; parmak izi API'leri API düzeyi 23 veya üstü gerektirir. `FingerprintManagerCompat` Sınıfı, için API düzey denetimi sarmalamak. Bu nedenle kullanmak için önerilen olan **Android destek kitaplığı v4** ve `FingerprintManagerCompat`; bu denetimleri biri için bu hesabı.

**Donanım** &ndash; uygulamayı ilk kez başlatıldığında parmak izi tarayıcısı varlığını denetlemeniz gerekir:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```
    
**Cihaz güvenli** &ndash; kullanıcının bir ekran kilidi ile güvenliği sağlanan aygıt olması gerekir. Kullanıcı bir ekran kilidi aygıtla güvenlik olmayan ve güvenlik uygulamaya önemli ise, kullanıcı, bir ekran kilidi yapılandırılmalıdır bildirilmesi gerekir. Aşağıdaki kod parçacığını bu öncesi requiste denetleyin gösterilmektedir:

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**Kayıtlı parmak izi** &ndash; kullanıcının en az bir parmak izi işletim sistemi ile kayıtlı olması gerekir. Bu izin onay önce her kimlik doğrulama girişimi oluşabilir:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**İzinleri** &ndash; uygulama izni kullanıcıdan uygulamayı kullanmadan önce istemeniz gerekir. Android 5.0 ve alt için kullanıcı uygulama yükleme koşul olarak izin verir. Android 6.0 çalışma zamanında izinleri denetleyen yeni bir izin modeli sunmuştur. Bu kod parçacığında, Android 6.0 izinlerini denetlemek nasıl örneğidir:

```csharp
// The context is typically a reference to the current activity.
Android.Content.PM.Permission permissionResult = ContextCompat.CheckSelfPermission(context, Manifest.Permission.UseFingerprint);
if (permissionResult == Android.Content.PM.Permission.Granted)
{
    // Permission granted - go ahead and start the fingerprint scanner.
}
else
{
    // No permission. Go and ask for permissions and don't start the scanner. See
    // http://developer.android.com/training/permissions/requesting.html
}
```

Bir uygulama koşullar 3, 4 ve 5 parmak izi kimlik doğrulaması kullanacak şekilde istediği her zaman kontrol etmeniz gerekir. Bir cihaz ve kaydedilen sonuçları uygulamayı ilk kez çalıştırıldığında ilk iki koşul denetlenebilir (Tercihler örneğin paylaşılan).

Android 6.0 izinleri istemek için hakkında daha fazla bilgi için Android kılavuzuna başvurun [isteyen izinleri çalışma zamanında](http://developer.android.com/training/permissions/requesting.html).



## <a name="related-links"></a>İlgili bağlantılar

- [bağlam](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [ContextCompat](https://developer.xamarin.com/api/type/Android.Support.V4.Content.ContextCompat/)
- [KeyguardManager](https://developer.xamarin.com/api/type/Android.App.KeyguardManager/)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [Çalışma zamanında izinleri isteyen](http://developer.android.com/training/permissions/requesting.html)
