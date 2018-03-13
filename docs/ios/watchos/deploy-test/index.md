---
title: "Dağıtım ve Test Etme"
description: "Aygıtlarda test etme ve uygulama mağazasında karşıya yükleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4c5b9fcbfaabbfc78da1064396dc3fec2d3fde8d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="deployment-and-testing"></a>Dağıtım ve Test Etme

## <a name="deployment-checklist"></a>Dağıtım denetim listesi

Bir test izleme dağıtımı veya uygulama mağazasında karşıya olup olmadığını bu sayfadaki adımları tamamlamanız gerekir:

- İçinde **iOS Geliştirme Merkezi**:
  - [Uygulama kimlikleri](#App_IDs) oluşturulmadı.
  - [Uygulama grupları](#App_Groups) (gerekliyse) yapılandırılmış.
  - Oluşturulan dağıtım sağlama profillerini

- Çözümünüz içinde:

  - Doğrulama [paketi kimliklerini ve proje başvuruları](~/ios/watchos/get-started/installation.md) ayarlanır.
  - Simgeler denetleyin [doğru yapılandırılmış](~/ios/watchos/app-fundamentals/icons.md).
  - Tüm projelerde paketi sürüm numaralarını eşleşiyorsa denetleyin.
  - Yapılandırma **Entitlements.plist** uygulama (gerekliyse) grupları için.

* Ardından yönergeleri izleyin:
  - [Test etmek için bir Apple Watch dağıtmak](~/ios/watchos/deploy-test/device.md), veya
  - [Uygulama deposuna karşıya yükleme](~/ios/watchos/deploy-test/appstore.md).

<a name="App_IDs"/>

## <a name="app-ids"></a>Uygulama kimlikleri

' Da anlatıldığı gibi [kurulum yönergeleri](~/ios/watchos/get-started/installation.md), üç projenin izleme uygulama paketi kimliklerini gibi ilgili:

- Xamarin.iOS Unified project - `com.xamarin.WatchKitCatalog`
- WatchKit uzantı projesi- `com.xamarin.WatchKitCatalog.watchkitextension`
- İzleme uygulaması proje- `com.xamarin.WatchKitCatalog.watchkitapp`

Üç projenin bir eşleşen dağıtım sağlama ya da her uygulama kimlikleri veya joker karakter uygulama kimliği kullanılarak açıkça profili gerektirir

### <a name="explicit-app-ids"></a>Açık uygulama kimlikleri

Oluşturma bir **uygulama kimliği** (hangi Geliştirme Merkezi İos'ta şuna benzeyecektir) her her projenin paket kimliği:

![Paket kimlikleri iOS Geliştirici Merkezi](images/appids-specific-sml.png)

Oluşturma veya uygulama kimlikleri yapılandırma anımsamak belirli özellikleri etkinleştirmek uygulamanızı gerektirir. Bu, anında iletme bildirimleri ve uygulama grupları içerebilir.

Her bir uygulama kimliği için bir dağıtım sağlama profili oluşturmanız gerekir

### <a name="wildcard-app-id"></a>Joker karakter uygulama kimliği

Alternatif olarak, bir joker karakter oluşturabilirsiniz **uygulama kimliği** üç projenin gibi eşleşen `com.xamarin.*`.

Bazı özellikler (örneğin, anında iletme bildirimleri) uygulama kimliği joker karakterle kullanılamaz unutmayın. Uygulamanız bu özellikler gerektiriyorsa, açık uygulama kimlikleri oluşturmanız gerekir.

Dağıtım için yalnızca bir dağıtım sağlama profiliniz joker uygulama kimliği oluşturmanız gerekir

<a name="App_Groups" />

## <a name="app-groups"></a>Uygulama grupları

Veri iOS uygulamanıza ve izleme uzantısı arasında paylaşmak için bir uygulama grubu kullanabilirsiniz. Çözümünüzü sahip olduğundan emin olmanız gerekir:

- Yapılandırılmış **uygulama grubu** Apple Geliştirici Portalı'nda **tanımlayıcıları & profilleri, sertifikaları** bölümü.

- Etkin **uygulama grupları** (ve sağlanan **uygulama grubunun kimliği**) içinde *her ikisi de* iOS uygulaması ve izleme uzantının **uygulama kimliği** ve  **Entitlements.plist**.

### <a name="certificates-identifiers--profiles"></a>Sertifikalar, tanımlayıcılarını ve profilleri

Bir uygulama grubu kullanmak için girdi oluşturma **uygulama grupları** ekran. Aşağıdaki örnekte, uygulama kimlikleri, ancak daha sık kullanılan aynı geriye doğru DNS stiliyle grubun adlandırılma şeklini `group.` (gerekli olan) öneki:

![Tanımlayıcı](images/appgroups-new-sml.png)

Uygulama grubu daha sonra listede görünür:

![Tanımlayıcı listesi](images/appgroups-setup-sml.png)

Grup oluşturulduktan sonra onu olarak başvurulabilir, **uygulama kimliği** yapılandırma. Her iki iOS uygulama ve izleme uzantısı dahil etmeyi unutmayın **uygulama kimlikleri**.

![Kullanılabilir consifurations](images/appgroups-sml.png)

Yapmak **değil** Apple Watch uygulama kodunda uygulama gruplarını etkinleştirmek İzleme üzerinde etkinleştirilmiş olması gerekli değildir.

### <a name="entitlementsplist"></a>Entitlements.plist

Bazı uygulama özelliklerini (ör.) Uygulama grupları), yetkilendirmeler ayarlamanızı gerektirir.
Düzenlemek için çift **Entitlements.plist** bu proje dosyasında:

- iOS uygulama projesi
- Gözcü uzantı projesi

biçimindeki telefon numarasıdır.![Entitlements.plist Düzenleyicisi](images/entitlements-plist-sml.png)

Yapmak **değil** izleme uygulama projesinde yetkilendirmeleri etkinleştirin. İzleme üzerinde etkinleştirilmiş olması gerekli değildir.

## <a name="related-links"></a>İlgili bağlantılar

- [Apple WatchKit gönderme Kılavuzu](https://developer.apple.com/app-store/watch/)
