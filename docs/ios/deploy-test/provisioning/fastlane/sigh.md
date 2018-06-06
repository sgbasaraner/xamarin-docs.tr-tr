---
title: iOS – fastlane sigh
description: Bu belgeyi açıklayan oluşturmak için kullanılan fastlane'nın sigh komutu kullanıcının yenileyin ve tüm Xamarin.iOS için onarım sağlama profilleri yapılandırmaları oluşturma.
ms.prod: xamarin
ms.assetid: CD17276F-2C8C-4A46-A54C-DD532EBD5720
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 8eedc86807035887cade48c42868649b362b7cb2
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785508"
---
# <a name="fastlane-for-ios--sigh"></a>iOS – fastlane sigh

> [!IMPORTANT]
> fastlane önerir kullanarak [ `match` ](~/ios/deploy-test/provisioning/fastlane/match.md) oluşturma ve sağlama profilleri sürdürmek için. Yalnızca tam denetim isterseniz ve yeterli kod imzalama hakkında bilmeniz sigh doğrudan kullanın.

## <a name="overview"></a>Genel Bakış

Cihaz sağlama, geliştirme ekibi Xcode aracılığıyla veya Apple Geliştirici portalındaki her bir üyesi tarafından geleneksel olarak gerçekleştirilir. Birkaç adımdan oluşur:

- Geliştirme sertifikası isteme
- Portalına bir cihaz ekleme
- Bir uygulama kimliği oluşturma
- Bir sağlama profili oluşturma
- İndirme profilleri ve sertifikaları

Bu adımların her biri, geliştirmekte olduğunuz uygulama türüne bağlıdır ele alınması gereken değişkenleri içerir. Geliştirme el ile veya Xcode aracılığıyla bulunabilir için bir aygıtı ayarlamak için gerekli olan adımları hakkında daha fazla bilgi [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.

Bu kılavuz, Xcode kullanarak alternatif olarak fastlane araçlar sunar ve aşağıda açıklanmaktadır:

- [Sigh nedir?](#whatissigh)
- [Bir uygulama kimliği oluşturma](#appid)
- [Yeni cihaz ekleme](#newdevices)
- [Sigh kullanma](#using)
- [Ek Seçenekler](#options)

> [!NOTE]
> Paket kimliği, uygulamanızın eşleşen bir uygulama kimliği yoktur ve Cihazınızı Geliştirici Portalı'nda zaten varsa, üzerinde adımları yoksayabilirsiniz [bir uygulama kimliği oluşturma](#appid) ve [aygıt ekleme](#newdevices). Bu durumda, doğrudan Git [kullanma sigh](#using) başlamak için.

## <a name="installation"></a>Yükleme

Giriş fastlane yükleme hakkında daha fazla bilgi için başvurmak [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) Kılavuzu.

<a name="whatissigh" />

## <a name="what-is-sigh"></a>Sigh nedir

sigh oluşturmak ve sağlama profilleri tüm yapılandırmalar için yenilemek izin veren terminal bir arabirim sunar: geliştirme, App Store dağıtım, geçici dağıtım ve kurumsal dağıtım. Bu ek indirmek ve sağlama profilleri onarım basit bir yol sağlar.

<a name="appid" />

## <a name="creating-an-app-id"></a>Bir uygulama kimliği oluşturma

Bir uygulama kimliği aşağıdaki komutla oluşturulabilir:

    fastlane produce -u your@appleid.com -a com.company.appname --skip_itc

Burada `com.company.appname` aşağıda gösterildiği gibi Xamarin.iOS uygulamanızı Info.plist dosyasında bulunan, uygulamanızın paket kimliği:

[![](sigh-images/fastlane-image5.png "Xamarin.iOS uygulamasının Info.plist dosyası")](sigh-images/fastlane-image5.png#lightbox)

Benzersiz uygulama kimliği, bir geriye doğru DNS stili dize olması gerekir. Oluşturulduktan sonra bu kılavuzda sigh kullanırken kullanmak ihtiyaç duyacağınız, Not tutun.

Uygulama üzerinde oluşturulacak gerekiyorsa [iTunes Bağlan](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md), kaldırma `--skip_itc` yukarıdaki komut bayrağı.

<a name="newdevices" />

## <a name="adding-new-devices"></a>Yeni cihaz ekleme

Tek bir cihaz için Geliştirici Portalı'nı komut satırından eklemek için aşağıdaki komutu girin:

```bash
fastlane run register_device name:"Adam iPhone" udid:"abcdeg1234567"
```

Birden fazla cihaz kullanım eklemek için `register_devices` komutu:

```bash
    register_devices(
        devices: {
            "iPhone 6" => "1234567890123456789012345678901234567890",
            "iPad Air 2" => "abcdefghijklmnopqrstvuwxyzabcdefghijklmn"
         }
    )
```

<a name="using" />

## <a name="using-sigh"></a>Sigh kullanma

Sigh yardımcı programı'nı kullanmaya başlamak için terminale aşağıdaki komutu girin:

```bash
fastlane sigh
```

Varsayılan olarak bu oluşturacak bir [App Store dağıtım](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) sağlama profili. Geliştirme için Cihazınızı ayarlamak için geçirmek `--development` bayrağı:

```bash
fastlane sigh --development
```

Fastlane tarafından istendiğinde Apple kimliği kullanıcı adınızı girin. Ayrıca parolasını bu ilk kez kullanıyorsanız fastlane kullanmış istenebilir. Aksi durumda, Anahtarlık parola ortam değişkenini çeker.

Apple Kimliğinizi birden çok ekibin bağlıysa, burada görüntülenir. Kullanmak istediğiniz takım karşılık gelen sayıyı seçin:

[![](sigh-images/fastlane-image2.png "Kullanmak istediğiniz ekibi seçin")](sigh-images/fastlane-image2.png#lightbox)

Takım Kimliği de aşağıdaki şekilde CLI geçirilebilir:

```bash
fastlane sigh -l 2TU993NY9J
```

Girin [uygulama kimliği](#appid)), uygulamanızın. Bu, uygulamanızın Info.plist paket tanımlayıcısını eşleşmelidir unutmayın.

Hesabınıza bağlı tüm aygıtlar için sağlama profilinizin eklenir.

fastlane oluşturmak, indirin ve sağlama profili sizin için yükleyin.

Geliştirici Merkezi göz atarsanız, yeni oluşturulan sağlama profili, aşağıda gösterildiği gibi görüntüleyebilirsiniz:

[![](sigh-images/fastlane-image10.png "Yeni oluşturulan sağlama profili görüntüleyin")](sigh-images/fastlane-image10.png#lightbox)

sigh sağlama profilleri varsayılan olarak geçerli klasörde depolar. Çıktı dizini değiştirmek için Düzenle ' `output_path`, veya aşağıdakileri yapın:

```bash
fastlane sigh -o "~/Library/MobileDevice/Provisioning Profiles"
```

<a name="options" />

## <a name="sigh-additional-options"></a>Ek seçenekler sigh

Ek destek sigh kullanırken vermek için aşağıdaki seçenekler kullanılabilir:

- Karşıdan yüklemek için tüm sağlama profilleri kullanın:

    ```bash
    fastlane sigh download_all
    ```

- Sağlama profili kullanımınız için belirli bir imzalama kimlik kullanmak için:

    ```bash
    fastlane sigh -c "Amy cert"
    ```
    
    Burada `Amy cert` kod imzalama kimliği adıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [fastlane - sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme)
