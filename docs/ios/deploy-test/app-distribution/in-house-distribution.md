---
title: Xamarin.iOS uygulamaları için şirket içi dağıtım
description: Bu belgeyi şirket içinde Apple Enterprise Developer Program üyesi uygulamaların dağıtım kısa bir genel bakış sağlar.
ms.prod: xamarin
ms.assetid: 9466E51E-303E-466E-85D7-D0525E16BB37
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 657370705233e923b482b67fc5afed12631c8187
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785034"
---
# <a name="in-house-distribution-for-xamarinios-apps"></a>Xamarin.iOS uygulamaları için şirket içi dağıtım

_Bu belgeyi şirket içinde Apple Enterprise Developer Program üyesi uygulamaların dağıtım kısa bir genel bakış sağlar._

Xamarin.iOS uygulamanızı geliştirilen sonra yazılım geliştirme yaşam döngüsü sonraki adımda uygulamanızı kullanıcılara dağıtmaktır. Özel uygulamalar dağıtılabilir *şirket içi* (daha önce Kurumsal olarak da adlandırılır) üzerinden **Apple Geliştirici Kurumsal programı**, aşağıdaki avantajları sunar:

- Uygulamanızı gözden geçirilmek üzere Apple tarafından gönderilen gerekmez.
- Bir uygulama dağıtabilirsiniz aygıtları miktarı için sınır yoktur
    - Apple açıkça şirket içi uygulamalar yalnızca dahili kullanım içindir kolaylaştırır olduğunu dikkate almak önemlidir.

Bu dikkate almak önemlidir Kurumsal programı:

- Dağıtım veya (TestFlight dahil) sınama iTunes Bağlan için erişim sağlamaz.
- Üyelik 299 yıllık maliyetidir.

Tüm uygulamalar yine Apple tarafından imzalanması gerekir.

<a name="testing" />

## <a name="testing-your-application"></a>Uygulamanızı test etme

Uygulamanızı test etme geçici dağıtım kullanarak gerçekleştirilir. Test etme hakkında daha fazla bilgi için adımları [geçici dağıtım](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) Kılavuzu. Yalnızca üzerinde en fazla 100 aygıtları kadar test edebilirsiniz olduğunu unutmayın.

<a name="setup" />

## <a name="getting-set-up-for-distribution"></a>Dağıtım için ayarlamak

Diğer programlarla Apple Developer gibi Apple Geliştirici Kurumsal programı altında yalnızca takım yöneticileri ve aracıları dağıtım sertifikalarını ve sağlama profilleri oluşturabilirsiniz.

Apple Geliştirici Kurumsal programı sertifikaları üç yıl boyunca sürer ve sağlama profilleri bir yıl sonra süreleri dolar.

Süresi dolmuş sertifikaları olamaz yenilendi ve bunun yerine, ayrıntılı olarak yeni bir süresi dolan sertifikanın yerine gerekecek dikkate almak önemlidir [aşağıda](#certificate).

<a name="certificate" />

## <a name="creating-a-distribution-certificate"></a>Dağıtım sertifika oluşturma

1. Gözat *tanımlayıcıları & profilleri, sertifikaları* Apple Developer Member Center'da bölümü.
2. Altında *sertifikaları*seçin **üretim**.
3. Tıklatın **+** yeni bir sertifika oluşturmak için düğmeye.
4. Altında *üretim* başlığını seçin **şirket içi ve geçici**:

   [![](in-house-distribution-images/createcertmanually01.png "Şirket içi ve geçici seçin")](in-house-distribution-images/createcertmanually01.png#lightbox)

5. Devam'ı tıklatın ve bir sertifika imzalama isteği Anahtarlık erişim aracılığıyla oluşturmak için yönergeleri izleyin:

   [![](in-house-distribution-images/createcertmanually02.png "Sertifika imzalama isteği Anahtarlık erişim aracılığıyla oluşturma")](in-house-distribution-images/createcertmanually02.png#lightbox)

6. Belirtildiği gibi CSR oluşturduktan sonra devam'ı tıklatın ve üye Merkezi'nde, CSR dosyasını karşıya yükleyin:

   [![](in-house-distribution-images/createcertmanually03.png "Üye Merkezi CSR dosyasını karşıya yükleyin")](in-house-distribution-images/createcertmanually03.png#lightbox)

7. Sertifikanızı oluşturmak için Oluştur'u tıklatın.
8. Tamamlanan sertifikayı indirmek ve yüklemek için dosyaya çift tıklayın.
9. Bu noktada, sertifikanızı makineye yüklenmesi, ancak Xcode'da görünür olduğundan emin olmak için profillerinizi yenilemeniz gerekebilir.

Alternatif olarak, xcode'da Tercihler iletişim kutusu üzerinden sertifika istemek mümkündür. Bunu yapmak için aşağıdaki adımları izleyin:

1. Ekibinizin seçin ve tıklayın *ayrıntıları görüntüle*:

    [![](in-house-distribution-images/selectteam.png "Ekibinizin seçin")](in-house-distribution-images/selectteam.png#lightbox)

2. Bundan sonra öğesini **oluşturma** düğmesine **iOS dağıtım sertifikası**:

   [![](in-house-distribution-images/selectcert.png "İOS dağıtım sertifikası oluşturma")](in-house-distribution-images/selectcert.png#lightbox)

2.   Bundan sonra öğesini **artı (+)** düğmesine tıklayın ve ardından **iOS App Store'da**:

   [![](in-house-distribution-images/selectcert.png "İOS uygulama deposu seçin")](in-house-distribution-images/selectcert.png#lightbox)

<a name="profile" />

## <a name="creating-a-distribution-provisioning-profile"></a>Bir dağıtım sağlama profili oluşturma

<a name="appid" />

### <a name="creating-an-app-id"></a>Bir uygulama kimliği oluşturma

Olarak herhangi diğer sağlama oluşturduğunuz profil ile bir uygulama kimliği kullanıcının cihazına dağıtma uygulamanın tanımlanması için gerekir. Zaten bu oluşturmadıysanız, oluşturmak için aşağıdaki adımları izleyin:


1. İçinde [Apple Geliştirici Merkezi](https://developer.apple.com/account/overview.action) göz *sertifika, tanımlayıcılarını ve profiller* bölümü. Seçin **uygulama kimlikleri** altında **tanımlayıcıları**.
2. Tıklatın **+** düğmesine tıklayın ve sağlayan bir **adı** hangi tanımlar ve bu Portalı'nda.
3. Uygulama önek takım Kimliğiniz ayarlanması gerekir ve değiştirilemez. Explicit veya joker uygulama kimliği seçin ve bir paket kimliği gibi geriye doğru DNS biçimde girin: **Explicit**: com [etkialanıadı]. [ AppName] **joker**: com [etkialanıadı]. *
4. Seçin [uygulama hizmetleri](~/ios/get-started/installation/device-provisioning/index.md#appservices) , uygulamanızı gerektirir.
5. Tıklatın **devam** düğmesi ve yeni bir uygulama kimliği oluşturmak için açık ekran yönergeleri izleyerek

Dağıtım profili oluşturmak için gerekli gerekli bileşenleri olduktan sonra onu oluşturmak için aşağıdaki adımları izleyin:

1. Apple sağlama Portalı'na dönün ve seçin **sağlama** > **dağıtım**:

   [![](in-house-distribution-images/distribute01.png "Sağlama seçin > Dağıtım")](in-house-distribution-images/distribute01.png#lightbox)

2. Tıklatın **+** düğmesini tıklatın ve olarak oluşturmak istediğiniz dağıtım profili türünü seçin **şirket içi**:

   [![](in-house-distribution-images/distribute02.png "Bir şirket içi dağıtım profili oluştur")](in-house-distribution-images/distribute02.png#lightbox)

3. Tıklatın **devam** düğmesini tıklatın ve uygulama kimliği için bir dağıtım profili oluşturmak istediğiniz aşağı açılan listeden seçin:

   [![](in-house-distribution-images/distribute03.png "Uygulama Kimliği açılır listeden seçin")](in-house-distribution-images/distribute03.png#lightbox)

4. Tıklatın **devam** düğmesi ve select dağıtım sertifikası gerekli uygulamayı imzalamak için:

   [![](in-house-distribution-images/distribute04.png "Uygulamayı imzalamak için gereken select dağıtım sertifikası")](in-house-distribution-images/distribute04.png#lightbox)

6. Tıklatın **devam** düğmesini tıklatın ve girin bir **adı** yeni dağıtım profili için:

   [![](in-house-distribution-images/distribute06.png "Yeni dağıtım profili için bir ad girin")](in-house-distribution-images/distribute06.png#lightbox)

7. Tıklatın **Generate** düğmesine yeni profil oluşturmak ve işlemini sonlandırabilir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 Mac için Visual Studio çıkın ve onun kullanılabilir imzalama kimlikleri ve sağlama profilleri listelerini Xcode olması gerekebilir ('ndaki yönergeleri izleyerek [imzalama kimlikleri isteyen](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) bölüm) yeni bir önce Mac için Visual Studio'da dağıtım profili kullanılabilir

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio çıkın ve kullanılabilir oturum kimlikleri ve sağlama profilleri listesini yenile Xcode (üzerinde yapı ana bilgisayarın Mac) sahip olabilir ('ndaki yönergeleri izleyerek [imzalama kimlikleri isteyen](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) bölüm) Visual Studio'da yeni bir dağıtım profili kullanılabilir olmadan önce.

-----

<a name="inhouse" />

## <a name="distributing-your-app-in-house"></a>Uygulamanız şirket içinde dağıtma

Apple Geliştirici Kurumsal programı ile edinmediyseniz sorumlu uygulama dağıtma ve için uymak kişidir [yönergeleri](http://adcdownload.apple.com/Documentation/License_Agreements__Apple_Developer_Enterprise_Program/Apple_Developer_Program_Enterprise_Agreement_20150608.pdf) Apple tarafından ayarlayın.

Uygulamanızı güvenli bir şekilde gibi farklı yollarla çeşitli kullanılarak dağıtılabilir:

- Yerel olarak iTunes
- MDM sunucusu
- Bir iç, güvenli web sunucusu
- E-posta

Bu yollardan biriyle uygulamanızı dağıtmak için önce bir IPA dosyası bir sonraki bölümde açıklandığı gibi oluşturmanız gerekir.


### <a name="creating-an-ipa-for-in-house-deployment"></a>Şirket içi dağıtım için bir IPA oluşturma

Sağlanan sonra uygulamaları olarak bilinen bir dosyaya paketlenmesi bir *IPA*. Ek meta veri ve simgeleri birlikte uygulamayı içeren bir zip dosyası budur. IPA, böylece doğrudan sağlama profilinde bir cihaza eşitlenebilen iTunes yerel bir uygulamaya eklemek için kullanılır.

IPA bakın oluşturma hakkında daha fazla bilgi için [IPA Destek](~/ios/deploy-test/app-distribution/ipa-support.md) Kılavuzu.


## <a name="summary"></a>Özet

Bu makalede Xamarin.iOS uygulamaları şirket içinde dağıtmanın kısa bir genel bakış verdi.

## <a name="related-links"></a>İlgili bağlantılar

- [App Store Dağıtımı](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Geçici Dağıtım](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist Dosyası](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA Desteği](~/ios/deploy-test/app-distribution/ipa-support.md)
