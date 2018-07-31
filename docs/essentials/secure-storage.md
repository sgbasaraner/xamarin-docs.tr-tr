---
title: 'Xamarin.Essentials: Güvenli Depolama'
description: Bu belgede güvenli bir şekilde basit bir anahtar/değer çiftleri depolar yardımcı olan Xamarin.Essentials SecureStorage sınıfında açıklanmaktadır. Bu sınıf, platform uygulama özellikleri ve sınırlamaları nasıl kullanılacağını açıklar.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: 2dfdb7051b269e73c68290a557849b9ae606c165
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353301"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: Güvenli Depolama

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**SecureStorage** sınıfı yardımcı olan güvenli bir şekilde basit bir anahtar/değer çiftleri depolar.

## <a name="getting-started"></a>Başlarken

Erişim için **SecureStorage** işlevselliği, aşağıdaki platforma özgü Kurulum gereklidir:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Ek kurulum gerekli.

# <a name="iostabios"></a>[iOS](#tab/ios)

Mac'teki iOS simülatörünün geliştirirken etkinleştirme **Anahtarlık** yetkilendirmesini ve uygulamanın paket tanımlayıcısı için bir Anahtarlık erişim grubu ekleyin.

Açık **Entitlements.plist** iOS projesi ve Bul **Anahtarlık** yetkilendirmesini ve etkinleştirin. Bu otomatik olarak uygulamanın tanımlayıcısı bir grup olarak ekler.

Proje özelliklerinde altında **iOS paket grubu imzalama** ayarlamak **özel yetkilendirmeler** için **Entitlements.plist**.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ek kurulum gerekli.

-----

## <a name="using-secure-storage"></a>Güvenli Depolama kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Kaydetmek için bir değer için bir verilen _anahtar_ Güvenli Depolama:

```csharp
try
{
  await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

Güvenli depolamadan bir değer almak için:

```csharp
try
{
  var oauthToken = await SecureStorage.GetAsync("oauth_token");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

> [!NOTE]
> İstenen anahtarla ilişkilendirilmiş değer yoksa `GetAsync` döndüreceği `null`.

Belirli bir anahtarın kaldırmak için çağırın:

```csharp
SecureStorage.Remove("oauth_token");
```

Tüm anahtarları kaldırmak için çağırın:

```csharp
SecureStorage.RemoveAll();
```


## <a name="platform-implementation-specifics"></a>Platform uygulaması özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Android KeyStore](https://developer.android.com/training/articles/keystore.html) değeri içine kaydedilmeden önce şifrelemek için kullanılan şifreleme anahtarı depolamak için kullanılan bir [paylaşılan tercihleri](https://developer.android.com/training/data-storage/shared-preferences.html) bir dosya adı ile **[YOUR-APP-PAKETİNİ-ID] .xamarinessentials** .  Paylaşılan tercihleri dosyasında kullanılan anahtarı bir _MD5 karma değeri_ yöntemlere geçirilen anahtarın `SecureStorage` API'leri.

## <a name="api-level-23-and-higher"></a>API düzeyi 23 ve üzeri

Yeni API düzeyleri hakkında bir **AES** anahtar Android KeyStore alınan ve kullanılan bir **AES/GCM/NoPadding** şifre değeri şifrelemenizi paylaşılan tercihleri dosyasında depolanır.

## <a name="api-level-22-and-lower"></a>API düzeyi 22 ve daha düşük

Eski API düzeylerini üzerinde depolama Android KeyStore yalnızca destekler. **RSA** ile kullanılan anahtarlar, bir **ECB/RSA/PKCS1Padding** şifrelemek için şifreleme bir **AES** (rastgele anahtar Çalışma zamanında oluşturulan) ve paylaşılan tercihleri dosyası anahtarı altında depolanan _SecureStorageKey_, biri değil zaten oluşturuldu.

**SecureStorage** kullanan [tercihleri](preferences.md) API ve aynı veri kalıcılığı ana hatlarıyla belirtilen aşağıdaki [tercihleri](preferences.md#persistence) belgeleri. Bir cihaz API düzeyi 22 veya daha düşük API düzeyi 23 ve daha yüksek yükseltir, bu tür bir şifreleme uygulama yüklenmediği sürece kullanılacak devam eder veya **RemoveAll** çağrılır.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Anahtar zinciri](https://developer.xamarin.com/api/type/Security.SecKeyChain/) değerleri iOS cihazlarında güvenli bir şekilde depolamak için kullanılır.  `SecRecord` Değeri depolamak için kullanılan sahip bir `Service` değerine **[YOUR-APP-BUNDLE-ID] .xamarinessentials**.

Bazı durumlarda Anahtarlık verileri iCloud ile eşitlenir ve uygulama kaldırma güvenli değerleri iCloud ve kullanıcının diğer cihazlardan kaldıramazsınız.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) UWP cihazlarda güvenli bir şekilde şifrelenmiş değerler için kullanılır.

Şifrelenmiş değerler depolanır `ApplicationData.Current.LocalSettings`, adıyla bir kapsayıcı içinde **[YOUR-uygulama-kimliği] .xamarinessentials**.

**SecureStorage** kullanan [tercihleri](preferences.md) API ve aynı veri kalıcılığı ana hatlarıyla belirtilen aşağıdaki [tercihleri](preferences.md#persistence) belgeleri.

-----

## <a name="limitations"></a>Sınırlamalar

Bu API, az miktarda metin depolamak için tasarlanmıştır.  Performans, çok miktarda metin depolamak için kullanmayı denerseniz yavaş olabilir.

## <a name="api"></a>API

- [SecureStorage kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage API belgeleri](xref:Xamarin.Essentials.SecureStorage)
