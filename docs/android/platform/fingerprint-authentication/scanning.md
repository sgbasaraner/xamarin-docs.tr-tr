---
title: "Parmak izi için tarama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/23/2016
ms.openlocfilehash: 678ceaf122550c6561541533405fe3500d192110
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="scanning-for-fingerprints"></a>Parmak izi için tarama

Parmak izi kimlik doğrulamasını kullanmak için bir Xamarin.Android uygulaması hazırlamak nasıl anlatıldığı, şimdi geri dönüp `FingerprintManager.Authenticate` yöntemi ve Android 6.0 parmak izi kimlik doğrulaması yerinde tartışın. Hızlı bir genel bakış parmak izi kimlik doğrulaması için iş akışının bu listede açıklanmıştır:

1. Çağırma `FingerprintManager.Authenticate`, geçen bir `CryptoObject` ve `FingerprintManager.AuthenticationCallback` örneği. `CryptoObject` Parmak izi kimlik doğrulaması sonucu kurcalanmadığından emin olmak için kullanılır. 
2. Bir alt [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) sınıfı. Bu sınıfın bir örneği için sağlanan `FingerprintManager` zaman kimlik doğrulaması başlatır parmak izi. Parmak izi tarayıcısı bittiğinde, bu sınıfındaki geri arama yöntemleri birini çağırır.
3. Aygıt parmak izi tarayıcısı başlatıldı ve kullanıcı etkileşimi için bekleyen kullanıcı izin vermek için kullanıcı Arabirimi güncelleştirmek için kod yazma. 
4. Parmak izi tarayıcısı işiniz bittiğinde, Android sonuçları uygulamaya üzerinde bir yöntemini çağırarak döndürülecek `FingerprintManager.AuthenticationCallback` önceki adımda sağlanan örneği.
5. Uygulama kullanıcı parmak izi kimlik doğrulama sonuçlarını bildirin ve sonuçları uygun şekilde tepki. 

Aşağıdaki kod parçacığını bir yöntem için parmak izi taramaya başlar bir etkinlikte örneğidir:

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Şimdi bu parametrelerin her biri ele `Authenticate` biraz daha ayrıntılı yöntemi:

* İlk parametre bir _crypto_ parmak izi tarayıcısı parmak izi tarama sonuçlarını tanımasına yardımcı olmak için kullanacağı nesne. Bu nesne olabilir `null`, bu durumda uygulama doğrudan hiçbir şeyin güvenmek parmak izi sonuçlarıyla değiştirilmediğini. Önerilir bir `CryptoObject` örneği ve sağlanan `FingerprintManager` null yerine. [Bir CryptObject oluşturma](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) örneği oluşturmak nasıl ayrıntılı olarak anlatılmıştır bir `CryptoObject` dayalı bir `Cipher`.
* İkinci parametre daima sıfır olur. Android belgeleri Bu bayrak kümesi tanımlar ve büyük olasılıkla gelecekte kullanılmak üzere ayrılmıştır. 
* Üçüncü parametre `cancellationSignal` parmak izi tarayıcısı devre dışı bırakma ve geçerli isteği iptal etmek için kullanılan bir nesne. Bu bir [Android CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)ve .NET framework türünden değil.
* Dördüncü parametre zorunludur ve o alt sınıfların sınıftır `AuthenticationCallback` soyut sınıf. Bu sınıftaki yöntemleri istemcileri için sinyal çağrılacak zaman `FingerprintManager` tamamladı ve sonuçları nelerdir. Uygulama hakkında anlamak için çok fazla olduğundan `AuthenticationCallback`, içinde ele alınacaktır [kendi bölümdür](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
* Beşinci isteğe bağlı bir parametredir `Handler` örneği. Varsa bir `Handler` nesnesi sağlanır, `FingerprintManager` kullanacağı `Looper` parmak izi donanımdan iletilerini işlerken bu nesneden. Genellikle, bir sağlamanız gerekmez bir `Handler`, FingerprintManager kullanacağı `Looper` uygulamadan.

## <a name="cancelling-a-fingerprint-scan"></a>Parmak izi tarama iptal ediliyor

Kullanıcı (veya uygulama için), başlatılan sonra parmak izi tarama iptal etmek gerekli olabilir. Bu durumda, çağırma [ `IsCancelled` ](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) yöntemi [ `CancellationSignal` ](http://developer.android.com/reference/android/os/CancellationSignal.html) için sağlanmadığından `FingerprintManager.Authenticate` zaman onu çağrıldığı parmak izi tarama başlatma.

Anlatıldığı olduğunuza `Authenticate` yöntemi, bazıları daha önemli parametrelerden biri daha ayrıntılı inceleyelim. İlk olarak, inceleyeceğiz [kimlik doğrulama geri çağırmaları yanıt](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), ele alt nasıl [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), tepki bir Android uygulaması etkinleştiriliyor parmak izi tarayıcı tarafından sağlanan sonuçları.




## <a name="related-links"></a>İlgili bağlantılar

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
