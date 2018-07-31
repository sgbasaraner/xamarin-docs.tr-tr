---
title: Xamarin.iOS uygulamaları için şirket içi dağıtım
description: Bu belge şirket içinde Apple Geliştirici Kurumsal programı üyesi uygulamaların dağıtım kısa bir genel bakış sağlar.
ms.prod: xamarin
ms.assetid: 9466E51E-303E-466E-85D7-D0525E16BB37
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 1dff0e614943805930cf7d838110c4a42eee6f48
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353210"
---
# <a name="in-house-distribution-for-xamarinios-apps"></a>Xamarin.iOS uygulamaları için şirket içi dağıtım

_Bu belge şirket içinde Apple Geliştirici Kurumsal programı üyesi uygulamaların dağıtım kısa bir genel bakış sağlar._

Xamarin.iOS uygulamanızı geliştirildikten sonra yazılım geliştirme yaşam döngüsünün bir sonraki adımda uygulamanızı kullanıcılara dağıtmaktır. Özel uygulamaları dağıtılabilir *şirket içi* (daha önce kuruluş olarak da bilinir) aracılığıyla **Apple Geliştirici Kurumsal programı**, aşağıdaki avantajları sunar:

- Uygulamanızı gözden geçirilmek üzere Apple tarafından gönderilen gerekmez.
- Bir uygulama dağıtabilirsiniz cihazları miktarının sınırı yoktur
    - Apple, çok açık şirket içi uygulamalar yalnızca dahili kullanım olan kullandığına dikkat edin önemlidir.

Dikkat etmeniz önemlidir Kurumsal programı:

- İTunes Connect dağıtım veya test etme (TestFlight dahil) için erişim sağlamaz.
- Üyelik 299 yılda maliyetidir.

Tüm uygulamalar, yine de Apple tarafından imzalanması gerekir.

<a name="testing" />

## <a name="testing-your-application"></a>Uygulamanızı test etme

Uygulamanızı test etme geçici dağıtım kullanarak gerçekleştirilir. Test etme hakkında daha fazla bilgi için adımları [geçici dağıtım](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) Kılavuzu. Yalnızca üzerinde en fazla 100 cihaz kadar test edebilirsiniz olduğunu unutmayın.

<a name="setup" />

## <a name="getting-set-up-for-distribution"></a>Dağıtım için ayarlamak

Diğer programlarla Apple geliştirici olarak, Apple Geliştirici Kurumsal programı altında yalnızca takım yöneticileri ve aracıları dağıtım sertifikaları ve sağlama profilleri oluşturabilirsiniz.

Apple Geliştirici Kurumsal programı sertifikaları üç yıl boyunca en son ve sağlama profilleri, bir yıl sonra sona erer.

Süresi dolan sertifikalar yenilenemez ve bunun yerine, süresi dolan sertifikanın ayrıntılı olarak yeni bir tane yerine gerekecek dikkate almak önemlidir [aşağıda](#certificate).

<a name="certificate" />

## <a name="creating-a-distribution-certificate"></a>Bir dağıtım sertifikası oluşturma

1. Gözat *tanımlayıcıları & profilleri, Sertifikalar* Apple Geliştirici üye Merkezi'nde bölümü.
2. Altında *sertifikaları*seçin **üretim**.
3. Tıklayın **+** yeni bir sertifika oluşturmak için.
4. Altında *üretim* başlığı seçin **şirket içi ve geçici**:

   [![](in-house-distribution-images/createcertmanually01.png "Şirket içi ve geçici seçin")](in-house-distribution-images/createcertmanually01.png#lightbox)

5. Devam'ı tıklatın ve bir sertifika imzalama isteği Anahtarlık erişimi aracılığıyla oluşturmak için yönergeleri izleyin:

   [![](in-house-distribution-images/createcertmanually02.png "Sertifika imzalama isteği Anahtarlık erişimi aracılığıyla oluşturma")](in-house-distribution-images/createcertmanually02.png#lightbox)

6. CSR'nizi belirtildiği şekilde oluşturduktan sonra Devam'a tıklayın ve üye Merkezi'nde CSR dosyanız karşıya yükleme:

   [![](in-house-distribution-images/createcertmanually03.png "CSR üye Merkezi'nde karşıya yükleme")](in-house-distribution-images/createcertmanually03.png#lightbox)

7. Sertifikanızı oluşturmak için Oluştur'a tıklayın.
8. Tamamlanan sertifikayı indirmek ve yüklemek için dosyaya çift tıklayın.
9. Bu noktada, sertifika makinede yüklü olması gerekir, ancak Xcode içinde görünür olduğundan emin olmak için profillerinizi yenilemeniz gerekebilir.

Alternatif olarak, xcode'da Tercihler iletişim kutusu aracılığıyla bir sertifika istemeniz mümkündür. Bunu yapmak için aşağıdaki adımları izleyin:

1. Takım seçin ve tıklayın *ayrıntıları*:

    [![](in-house-distribution-images/selectteam.png "Takım seçin")](in-house-distribution-images/selectteam.png#lightbox)

2. Ardından, **Oluştur** düğmesinin yanındaki **iOS dağıtım sertifikası**:

   [![](in-house-distribution-images/selectcert.png "İOS dağıtım sertifikası oluşturma")](in-house-distribution-images/selectcert.png#lightbox)

2.   Ardından, **artı (+)** düğmesini tıklatın ve seçin **iOS App Store**:

   [![](in-house-distribution-images/selectcert.png "İOS App Store seçin")](in-house-distribution-images/selectcert.png#lightbox)

<a name="profile" />

## <a name="creating-a-distribution-provisioning-profile"></a>Bir dağıtım sağlama profili oluşturma

<a name="appid" />

### <a name="creating-an-app-id"></a>Uygulama Kimliği oluşturma

Olarak herhangi diğer sağlama oluşturduğunuz profili ile bir uygulama kimliği, kullanıcının cihazına dağıtma uygulamayı tanımlamak için gerekli olacaktır. Henüz oluşturmadıysanız, oluşturmak için aşağıdaki adımları izleyin:


1. İçinde [Apple Developer Center'da](https://developer.apple.com/account/overview.action) göz atın *sertifika, tanımlayıcılarını ve profillerini* bölümü. Seçin **uygulama kimlikleri** altında **tanımlayıcıları**.
2. Tıklayın **+** düğmesine tıklayın ve sağlayan bir **adı** hangi tanımlar, portalda.
3. Uygulama ön eki, takım kimliği ayarlanması gerekir ve değiştirilemez. Explicit veya joker uygulama kimliği'ni seçin ve bir paket kimliği gibi ters DNS biçiminde girin: **açık**: com [DomainName]. [ AppName] **joker**: com [DomainName]. *
4. Seçin [uygulama hizmetleri](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services) , uygulamanız için gerekli.
5. Tıklayın **devam** düğmesi ve yeni uygulama kimliğini oluşturmak için açık ekran yönergeleri

Gerekli dağıtım profili oluşturmak için gerekli bileşenleri oluşturduktan sonra bunu oluşturmak için aşağıdaki adımları izleyin:

1. Apple sağlama Portalı'na dönün ve seçin **sağlama** > **dağıtım**:

   [![](in-house-distribution-images/distribute01.png "Sağlama seçin > Dağıtım")](in-house-distribution-images/distribute01.png#lightbox)

2. Tıklayın **+** olarak oluşturmak istediğiniz dağıtım profili türünü seçin ve düğme **şirket içi**:

   [![](in-house-distribution-images/distribute02.png "Bir şirket içi dağıtım profili oluşturma")](in-house-distribution-images/distribute02.png#lightbox)

3. Tıklayın **devam** düğmesine tıklayın ve aşağı açılan listeden bir dağıtım profili oluşturmak istediğiniz uygulama kimliği'ni seçin:

   [![](in-house-distribution-images/distribute03.png "Açılır listeden uygulama Kimliğini seçin")](in-house-distribution-images/distribute03.png#lightbox)

4. Tıklayın **devam** düğmesi ve select dağıtım sertifikası gerekli uygulamayı imzalamak için:

   [![](in-house-distribution-images/distribute04.png "Uygulamayı imzalamak için gereken seçim dağıtım sertifikası")](in-house-distribution-images/distribute04.png#lightbox)

6. Tıklayın **devam** düğmesine tıklayın ve girin bir **adı** yeni dağıtım profili için:

   [![](in-house-distribution-images/distribute06.png "Yeni dağıtım profili için bir ad girin")](in-house-distribution-images/distribute06.png#lightbox)

7. Tıklayın **Oluştur** yeni profili oluşturun ve işlemi sonlandırmak için düğme.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 Mac için Visual Studio çıkın ve onun kullanılabilir imzalama kimlikleri ve sağlama profillerinin listesini yenile Xcode olabilir ('ndaki yönergeleri takip ederek [imzalama kimlikleri isteyen](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) bölümü) önce yeni bir Mac için Visual Studio dağıtım profili kullanılabilir

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio çıkın ve kullanılabilir imzalama kimlikleri ve sağlama profilleri listesini yenile Xcode (derleme konağın Mac'te) sahip olabilir ('ndaki yönergeleri takip ederek [imzalama kimlikleri isteyen](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) bölüm) Visual Studio'da yeni bir dağıtım profili kullanılabilir olmadan önce.

-----

<a name="inhouse" />

## <a name="distributing-your-app-in-house"></a>Uygulamanız şirket içinde dağıtma

Apple Geliştirici Kurumsal programı ile lisans uygulaması dağıtma ve uymak için sorumlu kişidir [yönergeleri](http://adcdownload.apple.com/Documentation/License_Agreements__Apple_Developer_Enterprise_Program/Apple_Developer_Program_Enterprise_Agreement_20150608.pdf) Apple tarafından ayarlanmış.

Uygulamanızı farklı yollarla çeşitli gibi kullanarak güvenli bir şekilde dağıtılabilir:

- Yerel olarak iTunes
- MDM sunucusu
- Bir iç, güvenli web sunucusu
- E-posta

Şu adımlardan herhangi birini kullanarak uygulamanızı dağıtmak için önce bir IPA dosyasını sonraki bölümde açıklandığı gibi oluşturmanız gerekir.


### <a name="creating-an-ipa-for-in-house-deployment"></a>Şirket içi dağıtım için bir IPA oluşturma

Sağlanan sonra uygulamaları olarak bilinen bir dosyaya paketlenmesi bir *IPA*. Bu uygulamanın yanı sıra ek meta veriler ve simgeler içeren bir zip dosyasıdır. IPA, böylece doğrudan sağlama profilinde bir cihaza eşitlenebilir iTunes yerel bir uygulamaya eklemek için kullanılır.

IPA bakın oluşturma hakkında daha fazla bilgi için [IPA desteği](~/ios/deploy-test/app-distribution/ipa-support.md) Kılavuzu.


## <a name="summary"></a>Özet

Bu makalede, şirket içinde Xamarin.iOS uygulamalarını dağıtmak için kısa bir genel getirdi.

## <a name="related-links"></a>İlgili bağlantılar

- [App Store Dağıtımı](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Geçici Dağıtım](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist Dosyası](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA Desteği](~/ios/deploy-test/app-distribution/ipa-support.md)
