---
title: Bir CryptoObject oluşturma
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: f7a8ab7a43c0a3258cf6e737b0d235cbe7a1c747
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765720"
---
# <a name="creating-a-cryptoobject"></a>Bir CryptoObject oluşturma

Parmak izi kimlik doğrulama sonuçları bütünlüğünü uygulamaya önemlidir &ndash; uygulama kullanıcının kimliğini nasıl bilir olduğu. Teorik olarak kesecek ve parmak izi tarayıcı tarafından döndürülen sonuçlar değiştirmesine, üçüncü taraf kötü amaçlı yazılım mümkündür. Bu bölümde, parmak izi sonuçları geçerliliğini korunması için bir yöntem ele alınacaktır. 

`FingerprintManager.CryptoObject` Tarafından kullanılan ve Java şifreleme API'leri çevresinde bir sarmalayıcı olan `FingerprintManager` kimlik doğrulama isteği bütünlüğü korunacak. Genellikle, bir `Javax.Crypto.Cipher` nesnesi, sistemi, parmak izi tarayıcısı sonuçlarını şifrelemek için mekanizmasıdır. `Cipher` Nesnenin kendisini Android anahtar deposu API'lerini kullanarak uygulama tarafından oluşturulan bir anahtar kullanır.

Tüm bu sınıfların birlikte nasıl çalıştığını anlamak için önce nasıl oluşturulduğunu gösteren aşağıdaki kod bakalım bir `CryptoObject`ve daha ayrıntılı olarak açıklanmaktadır:

```csharp
public class CryptoObjectHelper
{
    // This can be key name you want. Should be unique for the app.
    static readonly string KEY_NAME = "com.xamarin.android.sample.fingerprint_authentication_key";

    // We always use this keystore on Android.
    static readonly string KEYSTORE_NAME = "AndroidKeyStore";

    // Should be no need to change these values.
    static readonly string KEY_ALGORITHM = KeyProperties.KeyAlgorithmAes;
    static readonly string BLOCK_MODE = KeyProperties.BlockModeCbc;
    static readonly string ENCRYPTION_PADDING = KeyProperties.EncryptionPaddingPkcs7;
    static readonly string TRANSFORMATION = KEY_ALGORITHM + "/" +
                                            BLOCK_MODE + "/" +
                                            ENCRYPTION_PADDING;
    readonly KeyStore _keystore;

    public CryptoObjectHelper()
    {
        _keystore = KeyStore.GetInstance(KEYSTORE_NAME);
        _keystore.Load(null);
    }

    public FingerprintManagerCompat.CryptoObject BuildCryptoObject()
    {
        Cipher cipher = CreateCipher();
        return new FingerprintManagerCompat.CryptoObject(cipher);
    }

    Cipher CreateCipher(bool retry = true)
    {
        IKey key = GetKey();
        Cipher cipher = Cipher.GetInstance(TRANSFORMATION);
        try
        {
            cipher.Init(CipherMode.EncryptMode | CipherMode.DecryptMode, key);
        } catch(KeyPermanentlyInvalidatedException e)
        {
            _keystore.DeleteEntry(KEY_NAME);
            if(retry)
            {
                CreateCipher(false);
            } else
            {
                throw new Exception("Could not create the cipher for fingerprint authentication.", e);
            }
        }
        return cipher;
    }

    IKey GetKey()
    {
        IKey secretKey;
        if(!_keystore.IsKeyEntry(KEY_NAME))
        {
            CreateKey();
        }

        secretKey = _keystore.GetKey(KEY_NAME, null);
        return secretKey;
    }

    void CreateKey()
    {
        KeyGenerator keyGen = KeyGenerator.GetInstance(KeyProperties.KeyAlgorithmAes, KEYSTORE_NAME);
        KeyGenParameterSpec keyGenSpec =
            new KeyGenParameterSpec.Builder(KEY_NAME, KeyStorePurpose.Encrypt | KeyStorePurpose.Decrypt)
                .SetBlockModes(BLOCK_MODE)
                .SetEncryptionPaddings(ENCRYPTION_PADDING)
                .SetUserAuthenticationRequired(true)
                .Build();
        keyGen.Init(keyGenSpec);
        keyGen.GenerateKey();
    }
}
```

Örnek kod yeni bir oluşturacak `Cipher` her `CryptoObject`, uygulama tarafından oluşturulan bir anahtar kullanarak. Bu anahtar tarafından tanımlanır `KEY_NAME` başına içinde ayarlandı değişkeni `CryptoObjectHelper` sınıfı. Yöntem `GetKey` deneyin ve Android anahtar deposu API'lerini kullanarak anahtarı alır. Anahtar yoksa, ardından yöntemi `CreateKey` uygulama için yeni bir anahtar oluşturur.

Şifre çağrısıyla örneği `Cipher.GetInstance`, alınması bir _dönüştürme_ (şifre şifrelemek ve verilerin şifresini çözmek nasıl paylaşılacağını açıklayan bir dize değeri). Çağrı `Cipher.Init` şifre başlatma uygulama anahtarından sağlayarak tamamlanır. 

Android anahtarı burada koşullarına bazı durumlar vardır hayata geçirmek önemlidir: 

* Yeni parmak izi aygıtla kaydolmuştur.
* Aygıtla kayıtlı hiçbir parmak izi yok.
* Kullanıcının ekran kilidi devre dışı bıraktı.
* Kullanıcı ekran kilidi (screenlock ya da kullanılan PIN/Düzen türü) değiştirdi.

Bu gerçekleştiğinde, `Cipher.Init` özel durum oluşturacak bir [ `KeyPermanentlyInvalidatedException` ](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html). Yukarıdaki örnek kod, özel durum yakalama, anahtarı silin ve yeni bir tane oluşturun.

Sonraki bölüm anahtarı oluşturun ve cihazda depolamak nasıl ele alınacaktır.

## <a name="creating-a-secret-key"></a>Gizli bir anahtar oluşturma

`CryptoObjectHelper` Sınıfını kullanan Android [ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/) bir anahtar oluşturun ve cihazda depolamak için. `KeyGenerator` Sınıfı oluşturabilir anahtarı ancak ihtiyaçlarını oluşturulacak anahtar türü hakkında bazı meta veriler. Bu bilgiler bir örneği tarafından sağlanan [ `KeyGenParameterSpec` ](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) sınıfı. 

A `KeyGenerator` kullanarak örneği `GetInstance` Üreteç yöntemi. Örnek kod kullanır [ _Gelişmiş Şifreleme Standardı_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) şifreleme algoritması olarak. AES sabit bir boyuta bloklarda verileri bölün ve her bu bloğu şifrelemek.

Ardından, bir `KeyGenParameterSpec` kullanılarak oluşturulan `KeyGenParameterSpec.Builder`. `KeyGenParameterSpec.Builder` Oluşturulacak anahtar hakkında aşağıdaki bilgileri sarmalar:

* Anahtar adı.
* Anahtar hem şifreleme ve şifresini çözmek için geçerli olmalıdır.
* Örnek kodda `BLOCK_MODE` ayarlanır _Şifre blok zincirleme_ (`KeyProperties.BlockModeCbc`), her blok XORed (her bloğu arasındaki bağımlılıkları oluşturma) önceki blok ile; yani. 
* `CryptoObjectHelper` Kullanan [ _ortak anahtar şifrelemesi standart #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) tüm aynı boyutta olduğundan emin olmak için blokları paneli bayt oluşturmak için .
* `SetUserAuthenticationRequired(true)` anahtar kullanılmadan önce kullanıcı kimlik doğrulamasının gerekli olmadığı anlamına gelir.

Bir kez `KeyGenParameterSpec` olan oluşturulan, bunu başlatmak için kullanılan `KeyGenerator`, hangi bir anahtar oluşturmak ve güvenli bir şekilde cihazda depolamak. 

## <a name="using-the-cryptoobjecthelper"></a>CryptoObjectHelper kullanma

Örnek kod oluşturma için mantığı çoğunu kapsüllenmiş olduğunuza göre bir `CryptoWrapper` içine `CryptoObjectHelper` sınıfı, şimdi bu kılavuzun başlangıcı koddan tekrar ziyaret edip kullanmak `CryptoObjectHelper` şifre oluşturmak ve parmak izi tarayıcısı başlatmak için: 

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */
    
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    // Using the Support Library classes for maximum reach
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthCallbacks is a C# class defined elsewhere in code.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Here is where the CryptoObjectHelper builds the CryptoObject. 
    fingerprintManager.Authenticate(cryptohelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Nasıl oluşturulacağını anlatıldığı olduğunuza göre bir `CryptoObject`, görmeye devam sağlar nasıl `FingerprintManager.AuthenticationCallbacks` parmak izi Tarayıcı hizmeti sonuçlarını bir Android uygulamasına aktarmak için kullanılır.



## <a name="related-links"></a>İlgili bağlantılar

- [Şifre](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](http://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
