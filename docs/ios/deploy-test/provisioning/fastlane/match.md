---
title: "fastlane iOS için - eşleşmesi"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 435ba4c3799288724625ca06016770b3ecad56a5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="fastlane-for-ios---match"></a>fastlane iOS için - eşleşmesi

Geleneksel olarak, cihaz sağlamayı geliştirme ekibi Xcode aracılığıyla veya Apple Geliştirici portalındaki her bir üyesi tarafından gerçekleştirildi. Birkaç adımdan oluşur:

- Geliştirme sertifikası isteme
- Portalına bir cihaz ekleme
- Bir uygulama kimliği oluşturma
- Bir sağlama profili oluşturma
- Profilleri ve sertifikaları indirme

Geliştirme el ile veya Xcode aracılığıyla bulunabilir için bir aygıtı ayarlamak için gerekli olan adımları hakkında daha fazla bilgi [aygıt Povisioning](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.

Bu kılavuz, Xcode kullanarak alternatif olarak fastlane araçlarını kullanmaya nasıl tanıtır.

## <a name="installation"></a>Yükleme

Giriş fastlane yükleme hakkında daha fazla bilgi için başvurmak [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) Kılavuzu.

<a name="whatismatch" />

## <a name="what-is-match"></a>Eşleşme nedir?

eşleşme mvc'deki oluşturmayı ve kod imzalama sertifikası ve sağlama koruma profilleri ve bir kod tüm geliştiriciler arasında kimlik imzalama paylaşmak bir iOS geliştirme ekibi sağlar.

Bir uygulamanın uygulama mağazası, iletken Beta sürümü test veya uygulama bir cihaza yükleniyor dağıtırken, her bir üyesi geliştirme ekibi imzalama kimliklerini vardır. Bu çakışan kimliklerini neden olabilir, profilleri ve el ile oluşturmak için gereken döndürün ve profilleri ve uygulama kimlikleri yönetebilirsiniz.

Bunun yerine, eşleşen tüm sertifikaları ve profilleri sizin için oluşturarak bulundurur ve bunları özel git deposu içinde depolar. Bu, tüm geliştiriciler erişmek ve bu kimlik bilgilerini kullanmak için bir takım sağlar. Buna karşılık, bu ek güvenlik için sertifikalarınız sağlar: özel git deposunda olmaya ek olarak, bunlar ayrıca ayrıca ile şifrelenmiş bir [parola](#passphrase). Kod imzalama bir havuzda yapıları depolama ekibi aracıları ve yöneticilerin güncelleştirmek ve sertifikalar, daha az zaman her geliştirici yeni sertifikaları dağıtma harcanan anlamına gerektiğinde döndürmek sağlar.

> [!IMPORTANT]
> eşleşme şirket içi Kurumsal profilleri şu anda desteklemiyor.

<a name="initializing" />

## <a name="initializing-your-project-with-match"></a>Eşleşme ile projenize başlatılıyor

Takım Yöneticisi iseniz, özel git deposu oluşturma, tüm ekip üyelerinin depoya katkıda bulunanlar olarak eklediğiniz github.com'u veya bitbucket.com ile olun.

Terminalinizde kullanarak proje dizinine dizini değiştirin ve çalıştırın:

    fastlane match init

İstendiğinde, git deposu URL'sini girin:

 [ ![](match-images/fastlane-image7.png "Git deposu URL'sini girin")](match-images/fastlane-image7.png)

URL bulundu ve tıklayarak kopyalanan **kopyalama veya indirme** aşağıda gösterildiği gibi github.com'u üzerinde düğmesi:

[ ![](match-images/fastlane-image6.png "URL altında github.com'u Kopyala veya indir düğmesi")](match-images/fastlane-image6.png)

Proje başlatma matchfile – ortam değişkenleri eşleşme Aracı'na iletmek için düzenlenebilir bir metin dosyası oluşturur. Matchfile örneği aşağıda gösterilmiştir:

[ ![](match-images/fastlane-image8.png "Bir matchfile örneği")](match-images/fastlane-image8.png)

<a name="running" />

## <a name="running-match"></a>Eşleşme çalıştırma

> [!NOTE]
> eşleşen çalıştırmadan önce ilk olarak fastlane önerir varolan temizlenmesi profilleri ve sertifikaları kullanarak düşünmelisiniz [eşleşen nuke komutu](#using).

Hangi ortamına bağlı olarak yeni bir sertifika ve sağlama profili oluşturun ve sizin yeni git deposuna depolamak için aşağıdaki komutlardan herhangi birini kullanabilirsiniz gerektirir:

    fastlane match appstore

    fastlane match adhoc

    fastlane match development

Yeni sertifikalar ve profilleri oluşturmaya ek olarak, bu komutlardan herhangi birini kullanarak ekler (veya zaten varsa, güncelleştirme) git deposu için aşağıdaki öğeleri:

- Sertifikaları klasörünü
- Profiller klasörü
- Temel yönergelerini içeren Benioku dosyası
- Bir eşleşme sürümü

[ ![](match-images/fastlane-image9.png "Git deposu içinde proje yapısı")](match-images/fastlane-image9.png)

Sağlama profilleri yüklenir `~/Library/MobileDevice/Provisioning Profiles`. Sertifikaları ve özel anahtarları, Anahtarlık doğrudan yüklenir.

<a name="using" />

### <a name="using-the-nuke-command"></a>Kullanarak `nuke` komutu

Untidy sertifikalar varsa kullanabilirsiniz `nuke` sertifikaları ve aşağıdaki komutları kullanarak her ortam için profil iptal etmek için:

    fastlane match nuke

Tüm sertifikaları ve belirli bir ortamın sağlama profilleri iptal etmek için:

    fastlane match nuke development

 veya

    fastlane match nuke distribution

fastlane herhangi bir şey silinmeden önce kaldırılacak dosyaları onaylar.

<a name="passphrase" />

### <a name="passphrase"></a>Parola

Çalıştırırken `match` ilk kez, git deposu için bir parola ayarlama istenir. Farklı bir makineye eşleşme çalıştırdığınızda gerekir gibi parola hatırlaması emin olun. Ek bir güvenlik katmanı budur: her dosya OpenSSL kullanılarak şifrelenir. Sonraki her çalıştırılmasını `match` üzerinde yeni bir makine başlangıçta bu parolayı yerel Anahtarlığa da eklenir girdikten sonra bu parolayı – ister.

Bir ortam değişkeni kullanarak profillerinizi şifresini çözmek için parolayı ayarlamak için kullanın `MATCH_PASSWORD`.

<a name="options" />

## <a name="additional-options"></a>Ek Seçenekler

Ek destek eşleşme kullanırken vermek için aşağıdaki seçenekler kullanılabilir:

- Kullanım `-–help` bayrağı tüm kullanılabilir komutların listesini görmek için:

        fastlane match cert --help

- Kullanım `-–verbose` bayrağı çıkış ayrıntı düzeyini artırmak için:

        fastlane match --development --verbose

- Kullanım `--force_for_new_devices` sağlama zorlamak için bayrağı profilleri Geliştirici portalındaki aygıt sayısı değişirse yenilemek için "

        fastlane match development --force_for_new_devices

## <a name="related-links"></a>İlgili bağlantılar

- [fastlane - eşleşmesi](https://github.com/fastlane/fastlane/blob/master/match/README.md)
