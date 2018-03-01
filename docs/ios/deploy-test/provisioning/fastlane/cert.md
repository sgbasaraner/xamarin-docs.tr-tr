---
title: "iOS – fastlane sertifika"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: b98375f8a526cd08f7d11f4ea6bb3498db87009c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="fastlane-for-ios--cert"></a>iOS – fastlane sertifika

> [!IMPORTANT]
> fastlane önerir kullanarak [ `match` ](~/ios/deploy-test/provisioning/fastlane/match.md) oluşturma ve sertifikaları sürdürmek için aracı. Kullanım `cert` doğrudan yalnızca tam denetim istiyorsanız ve kod imzalama hakkında yeterli bilgiye sahipsiniz. En son kod imzalama kimliği indirmek için bu eylemi kullanın.

## <a name="overview"></a>Genel Bakış

Cihaz sağlama, geliştirme ekibi Xcode aracılığıyla veya Apple Geliştirici portalındaki her bir üyesi tarafından geleneksel olarak gerçekleştirilir. Birkaç adımdan oluşur:

- Geliştirme sertifikası isteme
- Portalına bir cihaz ekleme
- Bir uygulama kimliği oluşturma
- Bir sağlama profili oluşturma
- Profilleri ve sertifikaları indirme

Bu adımların her biri, geliştirmekte olduğunuz uygulama türüne bağlıdır ele alınması gereken değişkenleri içerir. Geliştirme el ile veya Xcode aracılığıyla bulunabilir için bir aygıtı ayarlamak için gerekli olan adımları hakkında daha fazla bilgi [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.

Bu kılavuz, Xcode kullanarak alternatif olarak fastlane araçlar sunar ve aşağıda açıklanmaktadır:

- [Sertifika nedir?](#whatiscert)
- [Sertifika kullanma](#using)
- [Ek Seçenekler](#options)

## <a name="installation"></a>Yükleme

Yükleme ve fastlane güncelleştirme hakkında daha fazla bilgi için giriş bakın [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) Kılavuzu.

<a name="whatiscert" />

## <a name="what-is-cert"></a>Sertifika nedir?

sertifika imzalama kimlikleri yeni kod oluşturur terminal bir arabirim sunar (genellikle bir geliştirici olarak bilinen _sertifika_) geliştirme ve dağıtım ortamları için.

<a name="using" />

## <a name="using-cert"></a>Sertifika kullanma

Sertifika yardımcı programını kullanmak için terminal CLI aşağıdaki komutu girin:

    fastlane cert

Varsayılan olarak, bu dağıtım sertifikası oluşturur. Bir geliştirme sertifikası oluşturmak üzere geçirmek `--development` bayrağı:

    fastlane cert --development

sertifika için Apple kimliği ve parolası ister, bu nedenle bu şimdi girin:

[ ![](cert-images/fastlane-image1.png "Sertifika, Apple kimliği ve parola sorar")](cert-images/fastlane-image1.png)

> [!IMPORTANT]
> Parolanızı girilen ilk kez yerel macOS Anahtarlık kaydedilir. Alternatif olarak, ortam değişkenleri, kullanıcı adı ve parola depolamak için kullanılabilir veya kullanabilirsiniz `export fastlane_DONT_STORE_PASSWORD=1` Anahtarlıkta depolanan parolanızı olmasını istemiyorsanız. Fastlane için kullanıcının kimlik bilgileri fastlane ile yönetme ile ilgili daha fazla bilgi için başvurmak [kimlik bilgileri Yöneticisi Kılavuzu](https://github.com/fastlane/fastlane/blob/master/credentials_manager/README.md).

Apple kimliği de aşağıdaki komutu kullanarak bir bağımsız değişken olarak geçirilebilir:

    fastlane cert -u myemailadress@domain.com

Apple Kimliğinizi birden çok ekibin bağlıysa, burada görüntülenir. Kullanmak istediğiniz takım karşılık gelen sayıyı seçin:

[ ![](cert-images/fastlane-image2.png "Kullanmak istediğiniz ekibi seçin")](cert-images/fastlane-image2.png)

Takım Kimliği, ayrıca aşağıdaki bayrağı kullanılarak geçirilebilir:

    fastlane cert -l 2TU993NY9J

fastlane yerel makinenize yüklü herhangi bir kullanılabilir İmzalama sertifikaları ve yoksa onu kullanır kontrol eder.

İmzalama sertifikası yok ise, sertifika olur:

- Yeni özel anahtar ve imza isteği oluşturma
- Oluştur, indirin ve sertifika yükleme
- Anahtarlık özel anahtar ve sertifika alma

İmzalama kimlik, hesap için izin verilen en fazla sayısını erişildiğinde bir özel durum oluşturulur. Yeni bir imza kimliği oluşturmak istiyorsanız, el ile bir Geliştirici Merkezi aracılığıyla var olan sertifikaların iptal etme ve gerekir yeniden deneyin.

> [!NOTE]
> değil zaten, Anahtarlıkta olmaları durumunda fastlane Geliştirici Merkezi'nden varolan imzalama kimlikleri karşıdan yükleyemiyor. Özel anahtarı her zaman sadece bilgisayarınızda veya sertifika exported(*.p12) sürümünde ve hiçbir zaman Geliştirici Merkezi bulunmaktadır olmasıdır.

<a name="options" />

## <a name="additional-options"></a>Ek Seçenekler

Ek destek cert kullanırken vermek için aşağıdaki seçenekler kullanılabilir:

- Kullanım `-–help` bayrağı tüm kullanılabilir komutların listesini görmek için:

        fastlane cert --help

- Kullanım `-–verbose` çıkış ayrıntı düzeyini artırmak için bayrağı

        fastlane cert --development --verbose


## <a name="related-links"></a>İlgili bağlantılar

- [fastlane – sertifika](https://github.com/fastlane/fastlane/blob/master/cert/README.md)
