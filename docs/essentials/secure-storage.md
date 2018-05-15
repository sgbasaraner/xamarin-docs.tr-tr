---
title: Xamarin.Essentials güvenli depolama
description: SecureStorage sınıfı güvenli bir şekilde basit anahtar/değer çiftlerini depolamak yardımcı olur.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: e64160a5579bffa8e9e9820db1a3ba39bdf7304e
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials güvenli depolama

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**SecureStorage** sınıfı yardımcı olan basit bir anahtar/değer çiftleri güvenli bir şekilde saklayın.

## <a name="getting-started"></a>Başlarken

Erişim için **SecureStorage** işlevselliği, aşağıdaki platforma özgü Kurulum gereklidir:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Ek kurulumu gerekmez.

# <a name="iostabios"></a>[iOS](#tab/ios)

İOS simulator'da geliştirirken, etkinleştirme **Anahtarlık** yetkilendirme ve uygulamanın paket tanımlayıcısı için bir Anahtarlık erişim grubu ekleyin.

Açık **Entitlements.plist** iOS projesi ve Bul **Anahtarlık** yetkilendirme ve etkinleştirin. Bu uygulamanın tanımlayıcı bir grup olarak otomatik olarak ekler.

Proje Özellikleri'nde altında **iOS paket imzalama** ayarlamak **özel yetkilendirmeler** için **Entitlements.plist**.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ek kurulumu gerekmez.

-----

## <a name="using-secure-storage"></a>Güvenli Depolama kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

İçin bir değer kaydetmek için bir verilen _anahtar_ Güvenli Depolama:

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

Güvenli depolama biriminden bir değer almak için:

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

## <a name="platform-implementation-specifics"></a>Platform uygulama özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Android anahtar deposu](https://developer.android.com/training/articles/keystore.html) içine kaydedilmeden önce değer şifrelemek için kullanılan şifreleme anahtarı depolamak için kullanılan bir [paylaşılan tercihleri](https://developer.android.com/training/data-storage/shared-preferences.html) bir dosya adı ile **[YOUR-APP-paket-ID] .xamarinessentials** .  Paylaşılan tercihleri dosyasında kullanılan anahtarı bir _MD5 karma değeri_ içine geçirilen anahtarın `SecureStorage` API'nin.

## <a name="api-level-23-and-higher"></a>API düzeyi 23 ve üzeri

Yeni API düzeyde bir **AES** anahtar Android KeyStore alınan ve kullanılan bir **GCM/AES/NoPadding** paylaşılan tercihleri dosyasında depolanan önce değer şifrelemek için şifre.

## <a name="api-level-22-and-lower"></a>API düzeyi 22 ve daha düşük

Depolama yalnızca eski API düzeylerini, Android anahtar deposu destekler **RSA** ile kullanılan anahtarları bir **ECB/RSA/PKCS1Padding** şifrelemek için şifreleme bir **AES** (rastgele anahtar Çalışma zamanında oluşturulan) ve paylaşılan tercihleri dosyanın anahtarı altında depolanan _SecureStorageKey_, bir değil zaten oluşturuldu.

Tüm şifrelenmiş değerler uygulama aygıttan kaldırıldığında kaldırılır.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Anahtarlık](https://developer.xamarin.com/api/type/Android.Security.KeyChain/) değerleri iOS cihazlarda güvenli bir şekilde depolamak için kullanılır.  `SecRecord` Değeri depolamak için kullanılan sahip bir `Service` değerine **[YOUR-APP-paket-ID] .xamarinessentials**.

Bazı durumlarda Anahtarlık veri iCloud ile eşitlenir ve uygulama kaldırma güvenli değerleri iCloud ve kullanıcının diğer cihazlardan kaldıramazsınız.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/en-us/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) UWP aygıtlarda güvenli bir şekilde encryped değerleri için kullanılır.

Encryped değerleri depolanır `ApplicationData.Current.LocalSettings`, adıyla bir kapsayıcı içinde **[YOUR uygulama kimliği] .xamarinessentials**.

Uygulama kaldırma neden olacak _LocalSettings_ve tüm şifreli değerleri de kaldırılacak.

-----

## <a name="limitations"></a>Sınırlamalar

Bu API, küçük miktarda metin depolamak için tasarlanmıştır.  Büyük miktarda metin depolamak için kullanmayı denerseniz performans düşebilir.

## <a name="api"></a>API

- [SecureStorage kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage API belgeleri](xref:Xamarin.Essentials.SecureStorage)
