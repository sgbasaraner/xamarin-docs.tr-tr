---
title: 'Xamarin.Essentials: Güvenli Depolama'
description: Bu belgede güvenli bir şekilde basit bir anahtar/değer çiftleri depolar yardımcı olan Xamarin.Essentials SecureStorage sınıfında açıklanmaktadır. Bu sınıf, platform uygulama özellikleri ve sınırlamaları nasıl kullanılacağını açıklar.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: fae5f5f0f15d80e2f3bdce26b8beb5f6fae2f81f
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403448"
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
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

Güvenli depolamadan bir değer almak için:

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
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

[Android KeyStore](https://developer.android.com/training/articles/keystore.html) değeri içine kaydedilmeden önce şifrelemek için kullanılan şifreleme anahtarı depolamak için kullanılan bir [paylaşılan tercihleri](https://developer.android.com/training/data-storage/shared-preferences.html) bir dosya adı ile **[YOUR-APP-PAKETİNİ-ID] .xamarinessentials** .  Paylaşılan tercihleri dosyasında kullanılan anahtarı bir _MD5 karma değeri_ yöntemlere geçirilen anahtarın `SecureStorage` API'nin.

## <a name="api-level-23-and-higher"></a>API düzeyi 23 ve üzeri

Yeni API düzeyleri hakkında bir **AES** anahtar Android KeyStore alınan ve kullanılan bir **AES/GCM/NoPadding** şifre değeri şifrelemenizi paylaşılan tercihleri dosyasında depolanır.

## <a name="api-level-22-and-lower"></a>API düzeyi 22 ve daha düşük

Eski API düzeylerini üzerinde depolama Android KeyStore yalnızca destekler. **RSA** ile kullanılan anahtarlar, bir **ECB/RSA/PKCS1Padding** şifrelemek için şifreleme bir **AES** (rastgele anahtar Çalışma zamanında oluşturulan) ve paylaşılan tercihleri dosyası anahtarı altında depolanan _SecureStorageKey_, biri değil zaten oluşturuldu.

Tüm şifrelenmiş değerler uygulama CİHAZDAN kaldırıldığında kaldırılır.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Anahtar zinciri](https://developer.xamarin.com/api/type/Security.SecKeyChain/) değerleri iOS cihazlarında güvenli bir şekilde depolamak için kullanılır.  `SecRecord` Değeri depolamak için kullanılan sahip bir `Service` değerine **[YOUR-APP-BUNDLE-ID] .xamarinessentials**.

Bazı durumlarda Anahtarlık verileri iCloud ile eşitlenir ve uygulama kaldırma güvenli değerleri iCloud ve kullanıcının diğer cihazlardan kaldıramazsınız.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) UWP cihazlarda güvenli bir şekilde şifrelenen değerleri için kullanılır.

Şifrelenen değerleri depolanır `ApplicationData.Current.LocalSettings`, adıyla bir kapsayıcı içinde **[YOUR-uygulama-kimliği] .xamarinessentials**.

Uygulama kaldırma neden _LocalSettings_ve tüm şifrelenmiş değerler de kaldırılacak.

-----

## <a name="limitations"></a>Sınırlamalar

Bu API, az miktarda metin depolamak için tasarlanmıştır.  Performans, çok miktarda metin depolamak için kullanmayı denerseniz yavaş olabilir.

## <a name="api"></a>API

- [SecureStorage kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage API belgeleri](xref:Xamarin.Essentials.SecureStorage)
