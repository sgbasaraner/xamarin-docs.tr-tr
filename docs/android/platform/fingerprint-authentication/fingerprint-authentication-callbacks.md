---
title: "Kimlik doğrulama geri çağırmaları yanıt"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: 37d288cd75f232c8674aece085a78a83ce12d720
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="responding-to-authentication-callbacks"></a>Kimlik doğrulama geri çağırmaları yanıt

Parmak izi tarayıcısı kendi iş parçacığında arka planda çalışır ve tamamlandığında, bir yöntemini çağırarak tarama sonuçlarını bildirir `FingerprintManager.AuthenticationCallback` kullanıcı Arabirimi iş parçacığı üzerinde. Bir Android uygulaması, aşağıdaki yöntemlerden uygulama bu Özet sınıf genişleten kendi işleyici sağlamanız gerekir:

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; Kurtarılamaz bir hata olduğunda çağrılır. Bir uygulama veya kullanıcıya büyük olasılıkla yeniden deneyin dışında düzeltmek için yapabileceğiniz daha fazla bir şey yoktur.
* **`OnAuthenticationFailed()`** &ndash; Parmak izi algılandı ancak aygıt tarafından tanınmayan bu yöntem çağrılır.
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; Hızlı tarayıcı geçirilen parmak gibi kurtarılabilir bir hata olduğunda çağrılır.
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; Bu, parmak izi tanınan olduğunda çağrılır.

Varsa bir `CryptoObject` çağrılırken kullanılan `Authenticate`, çağırmak için önerilen `Cipher.DoFinal` içinde `OnAuthenticationSuccessful`.
`DoFinal` Şifre değiştirilmiş veya yanlış başlatılmış bir özel durum oluşturur, parmak izi tarayıcısı sonucunu dışında uygulama oynanmış olduğunu gösteren.


> [!NOTE]
> Geri çağırma sınıfı göreli olarak hafif tutmak ve uygulama belirli mantığını boşaltmak için önerilir. Geri aramalar bir "trafik Kopyala" Android uygulaması ve sonuçları arasında parmak izi tarayıcıdan olarak davranır.

## <a name="a-sample-authentication-callback-handler"></a>Örnek kimlik doğrulama geri çağırma işleyicisi

Aşağıdaki sınıf en az bir örneğidir `FingerprintManager.AuthenticationCallback` uygulama: 

```csharp
class MyAuthCallbackSample : FingerprintManagerCompat.AuthenticationCallback
{
    // Can be any byte array, keep unique to application.
    static readonly byte[] SECRET_BYTES = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // The TAG can be any string, this one is for demonstration.
    static readonly string TAG = "X:" + typeof (SimpleAuthCallbacks).Name;

    public MyAuthCallbackSample()
    {
    }

    public override void OnAuthenticationSucceeded(FingerprintManagerCompat.AuthenticationResult result)
    {
        if (result.CryptoObject.Cipher != null) 
        {
            try
            {
                // Calling DoFinal on the Cipher ensures that the encryption worked.
                byte[] doFinalResult = result.CryptoObject.Cipher.DoFinal(SECRET_BYTES);
    
                // No errors occurred, trust the results.              
            }
            catch (BadPaddingException bpe)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + bpe);
            }
            catch (IllegalBlockSizeException ibse)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + ibse);
            }
        }
        else
        {
            // No cipher used, assume that everything went well and trust the results.
        }
    }

    public override void OnAuthenticationError(int errMsgId, ICharSequence errString)
    {
        // Report the error to the user. Note that if the user canceled the scan,
        // this method will be called and the errMsgId will be FingerprintState.ErrorCanceled.
    }

    public override void OnAuthenticationFailed()
    {
        // Tell the user that the fingerprint was not recognized.
    }

    public override void OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)
    {
        // Notify the user that the scan failed and display the provided hint.
    }
}
```

`OnAuthenticationSucceeded` denetler bir `Cipher` için sağlanan `FingerprintManager` zaman `Authentication` çağrıldı. Bu durumda, `DoFinal` yöntemi, üzerinde şifre çağrılır. Bu kapatır `Cipher`, özgün durumuna geri yükleme. Bir sorun oldu şifre sonra `DoFinal` bir özel durum oluşturur ve kimlik doğrulama girişimi başarısız oldu olarak düşünülmelidir.

`OnAuthenticationError` Ve `OnAuthenticationHelp` geri aramalar her sorunun ne olduğunu belirten bir tamsayı alırsınız. Aşağıdaki bölümde, her olası Yardım veya hata kodları açıklanmaktadır. İki geri çağırmaları benzer amaçlara hizmet &ndash; , parmak izi kimlik doğrulamasının başarısız oldu uygulama bilgilendirmek için. Nasıl bunların farklı önem içinde ' dir. `OnAuthenticationHelp` bir kullanıcı kurtarılabilir çok hızlı parmak izi geçirmeyi gibi bir hatadır; `OnAuthenticationError` daha ciddi bir hata, bozuk parmak izi tarayıcısı gibi değil.

Unutmayın `OnAuthenticationError` parmak izi tarama aracılığıyla iptal edildiğinde çağrılan `CancellationSignal.Cancel()` ileti. `errMsgId` Parametre değeri 5 olacaktır (`FingerprintState.ErrorCanceled`). Uygulaması gereksinimlerine bağlı olarak `AuthenticationCallbacks` bu durum diğer hataları daha farklı davranabilir. 

`OnAuthenticationFailed` parmak izi başarıyla taranan ancak aygıtla kayıtlı tüm parmak izi eşleşmedi çağrılır. 

## <a name="help-codes-and-error-message-ids"></a>Yardım kodları ve hata iletisi kimlikleri 

Bir listesi ve hata kodları ve Yardım kodları açıklaması bulunabilir [Android SDK Belgeleri](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) FingerprintManager sınıfı için. Xamarin.Android temsil eder bu değerlerle `Android.Hardware.Fingerprints.FingerprintState` enum:


-   **`AcquiredGood`** &ndash; (değer 0) Alınan görüntü iyi.


-   **`AcquiredImagerDirty`** &ndash; (değer 3) Parmak izi görüntü algılayıcı şüpheli veya algılanan kirlenme nedeniyle çok gürültülü. Örneğin, bu birden çok sonra dönmek makul `AcquiredInsufficient` veya (takılmış piksel, swaths, vb.) algılayıcı kirlenme gerçek algılanması. Kullanıcı bu döndürüldüğünde algılayıcı temizlemek amacıyla eyleme geçmek için bekleniyor.


-   **`AcquiredInsufficient`** &ndash; (değer 2) Parmak izi görüntü algılanan koşulu (yani kuru kaplama) veya büyük olasılıkla kirli algılayıcı nedeniyle işlemek gürültülü (bkz: `AcquiredImagerDirty`.



-   **`AcquiredPartial`** &ndash; (değer 1) Yalnızca kısmi parmak izi görüntüsü algılandı. Kayıt sırasında kullanıcı ne bu sorun, örn., çözümlemek için olması gerektiğini üzerinde bilgilendirildi &ldquo;sıkıca üzerinde algılayıcı tuşuna basın.&rdquo;



-   **`AcquiredTooFast`** &ndash; (değer 5) Parmak izi görüntü nedeniyle hızlı hareket tamamlanmadı. Parmak alım sırasında taşındıysa çoğunlukla uygun doğrusal dizi algılayıcılar için karşın, bu da meydana gelmiş olabilir. Yavaş parmak taşımak için kullanıcı sorulan (doğrusal) veya daha uzun algılayıcı parmak bırakın.




-   **`AcquiredToSlow`** &ndash; (değer 4) Parmak izi görüntü hareket eksikliği nedeniyle okunamaz. Manyetik hareket gerektiren doğrusal dizi algılayıcılar için en uygun budur.



-   **`ErrorCanceled`** &ndash; (değer 5) Parmak izi algılayıcısı kullanılamadığı için işlem iptal edildi. Örneğin, bu kullanıcının anahtarlanır, cihaz kilitli veya başka bir işlem bekleyen engeller veya devre dışı bırakan çalışıldığında oluşabilir.



-   **`ErrorHwUnavailable`** &ndash; (değer 1) Donanım kullanılamıyor. Daha sonra yeniden deneyin.




-   **`ErrorLockout`** &ndash; (değer 7) Çok fazla sayıda denemesi nedeniyle API kilitli olduğu için işlem iptal edildi.




-   **`ErrorNoSpace`** &ndash; (değer 4) Kayıt gibi işlemleri için döndürülen hata durumunda; işlemi tamamlamak için kalan yeterli depolama alanı olmadığından işlem tamamlanamıyor.



-   **`ErrorTimeout`** &ndash; (değer 3) Geçerli isteğin uzun çalıştığı zaman döndürülen hata durumunda. Bu program için parmak izi algılayıcısı süresiz olarak bekleyen önlemek için tasarlanmıştır. Zaman aşımı platform ve algılayıcı özgü olan, ancak genellikle yaklaşık 30 saniyedir.



-   **`ErrorUnableToProcess`** &ndash; (değer 2) Algılayıcı geçerli görüntü işleyemedi olduğunda döndürülen hata durumunda.



## <a name="related-links"></a>İlgili bağlantılar

- [Şifre](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
