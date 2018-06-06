---
title: Xamarin.iOS özellikleri ile çalışma
description: Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz, tüm özellikler için gereken kurulum açıklar.
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: c897b1f5fbdf950e6858d7b73ebed60049f60e8e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785563"
---
# <a name="working-with-capabilities-in-xamarinios"></a>Xamarin.iOS özellikleri ile çalışma

_Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz, tüm özellikler için gereken kurulum açıklar._

Apple sağlar geliştiricilere _yetenekleri_, çoğunlukla bilinen _uygulama hizmetleri_işlevselliği genişletme ve hangi iOS uygulamalarını kapsamını genişletme yolu yapabilirsiniz gibi. Özellikleri daha ayrıntılı tümleştirme platformu özellikleri gibi kendi uygulamaya eklemek için geliştiricilere izin ver: uygulama, Siri gibi ek aygıt hizmetleri ve diğer başlatılan para işlemleri sahip olma yeteneği.
Bu özellikler Xamarin.iOS projeleri ile kullanılabilir. Hizmetlerin tam listesini aşağıda belirtilmiştir:

* Uygulama grupları
* İlişkili Etki Alanları
* Veri Koruma
* Oyun Merkezi
* HealthKit
* HomeKit
* Kablosuz Aksesuar Yapılandırması
* iCloud
* Uygulama içi satın alma
* Uygulamalar Arası Ses
* Apple Pay
* Cüzdan
* Anında iletme bildirimi
* Kişisel VPN
* Siri
* Eşlemeleri
* Arka plan modları
* Anahtarlık paylaşımı
* Ağ Uzantıları
* Etkin nokta yapılandırma
* Çoklu Yol
* NFC etiketinin okuma


Özellikleri ya da Visual Studio aracılığıyla Mac ve Visual Studio 2017 için veya el ile Apple Geliştirici Portalı'nda etkinleştirilebilir. Cüzdan, Apple Pay ve iCloud gibi belirli özelliklerini uygulama kimliklerini ek yapılandırma gerektirir.

Bu kılavuz, bu uygulama hizmetlerin uygulamanızda Visual Studio'da otomatik ve el ile gerekli olabilecek herhangi bir ek ayar dahil olmak üzere Geliştirici Merkezi aracılığıyla her birini etkinleştirmek açıklanmaktadır. 

## <a name="adding-app-services"></a>Uygulama Hizmetleri ekleme

Özellikleri kullanmak için uygulamayı doğru hizmetinin etkinleştirilmiş bir uygulama kimliği içeren geçerli bir sağlama profili olması gerekir. Bu sağlama profili oluşturma ya da otomatik olarak Mac ve Visual Studio 2017 için Visual Studio veya el ile Apple Geliştirici Merkezi'ndeki yapılabilir.

Bu bölümde, Visual Studio'nun otomatik sağlama veya Geliştirici Merkezi çoğu işlevlerini etkinleştirmek için nasıl kullanılacağı açıklanmaktadır. Cüzdan, iCloud, Apple Pay ve ek kurulum gerektirir uygulama grupları gibi bazı özellikler vardır. Bu bitişik kılavuzları ayrıntılı açıklanmıştır.

> [!IMPORTANT]
> Tüm özellikleri, eklenen ve otomatik sağlama ile yönetilir. Aşağıdaki liste, desteklenen özellikler içerir:
>
>* HealthKit 
>* HomeKit 
>* Kişisel VPN 
>* Kablosuz Aksesuar Yapılandırması 
>* Uygulamalar Arası Ses 
>* SiriKit 
>* Etkin Nokta 
>* Ağ Uzantıları 
>* NFC etiketinin okuma
>* Çoklu Yol 
>
>Anında iletme bildirimleri, oyun Merkezi, uygulama içi satın alma, haritalar, Anahtarlık paylaşımı, ilişkili etki alanları ve veri koruma özellikleri şu anda desteklenmiyor. Bu özellikler eklemek için el ile sağlama kullanması ve adımları [Geliştirici Merkezi](#devcenter) bölümü.

## <a name="using-the-ide"></a>IDE kullanma

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Özellikleri eklenir **Entitlements.plist** Mac için Visual Studio'da Özellikleri eklemek için aşağıdaki adımları kullanın:

1. Açık **Info.plist** seçin ve iOS uygulama dosyasının **otomatik olarak sağlama** düzeni ve **takım** açılan kutusundan. Adımları [otomatik sağlamayı](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) yardıma gereksinim duyarsanız Kılavuzu:

    ![İmzalama seçeneği otomatik olarak yönetme](images/manage-signing.png)

2. Açık **Entitlements.plist** dosya ve eklemek istediğiniz yeteneği seçin:

    ![Entitlements.plist dosyasına yetenekleri ekleme](images/image17.png)

    Bir özellik seçerek iki işlemi yapar:
    * Bu özellik, uygulama Kimliğinize ekler
    * Entitlements.plist dosyanızı yetkilendirme anahtar/değer çifti ekler.

    Mac için Visual Studio ne zaman bu görevleri aşağıdaki başarı iletisi görüntüleyerek gerçekleştirildikten önerilerde:

    ![Entitlements.plist dosyasına yetenekleri ekleme](images/image18.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Özellikleri eklenir **Entitlements.plist**. Visual Studio 2017 içinde özellikleri eklemek için aşağıdaki adımları kullanın:

1. Bölümünde açıklandığı gibi bir Mac için Visual Studio 2017 eşleştirin [Mac çiftine](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Kılavuzu.

2. Hazırlama Seçenekleri'ni seçerek açmak **Proje > sağlama özellikleri...**

3. Seçin **otomatik olarak sağlama** düzeni ve **takım** açılan kutusundan. Adımları [otomatik sağlamayı](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) yardıma gereksinim duyarsanız Kılavuzu:

    ![İmzalama seçeneği otomatik olarak yönetme](images/manage-signing-vs.png)

4. Açık **Entitlements.plist** dosya ve eklemek istediğiniz özelliği seçin. Dosyayı kaydedin.

    Kaydetme **Entitlement.plist** iki işlemi yapar:

    * Bu özellik, uygulama Kimliğinize ekler
    * Entitlements.plist dosyanızı yetkilendirme anahtar/değer çifti ekler.

-----


<a name="devcenter" />

## <a name="using-the-developer-center"></a>Geliştirici Merkezi'ni kullanma

Geliştirici Merkezi'ni kullanarak bir uygulama kimliği oluşturarak ve ardından bir sağlama profili oluşturmak için bu uygulama kimliği kullanılarak gerektiren bir iki adım işlemidir. Bu adımlar aşağıda ayrıntılı olarak açıklanmaktadır.

### <a name="creating-an-app-id-with-an-app-service"></a>Bir uygulama hizmeti ile bir uygulama kimliği oluşturma

1.  Gözat [Apple Geliştirici Merkezi](https://developer.apple.com/account) Mac (bir windows makinesine kullanıyorsanız, yapı konak mac) ve oturum açma.
2.  Seçin **sertifikalar, tanımlayıcılarını ve profilleri**:

    ![Apple Geliştirici Merkezi](images/image5.png)

3.  Altında **tanımlayıcıları**seçin **uygulama kimlikleri**:

    ![Uygulama Kimliği seçim Geliştirici Merkezi](images/image6.png)

4.  Tuşuna **+** yeni bir uygulama kimliği oluşturmak için sağ üst köşedeki düğmesi
5.  Bir uygulama kimliği açıklaması girin, açık uygulama kimliği seçin ve bir paket kimliği şu biçimde girin `com.domain.appname`. Bu paket kimliği, uygulamanın paket Kimliğini projenizdeki eşleşmesi gerekir:

    ![Uygulama Kimliği ayrıntıları ekleme](images/image7.png)

6.  Altında **uygulama hizmetleri** hizmeti veya gerekli hizmetleri uygulamanızı seçin:

    ![Uygulama Hizmetleri seçim sayfası](images/image8.png)

7.  Tuşuna **devam**.
8.  Uygulama kimliğinizi doğrulayın Her hizmet şu durumlardan birinde olacaktır: **etkin**, **devre dışı**, veya **yapılandırılabilir**aşağıda gösterildiği gibi. Eğer öyleyse **etkin** sağlama profilinde kullanılmak üzere hazırdır. Eğer öyleyse **yapılandırılabilir**, ek kurulum bu özellik için gereklidir. Aşağıdaki ek adımları sonraki bölümlerde daha ayrıntılı olarak açıklanmıştır.

    ![Uygulama Kimliği onayı](images/image9.png)

9.  Tıklatın **kaydetmek** ve ardından **Bitti**. Yeni oluşturulan uygulama kimliği iOS uygulama kimlikleri listesinde görüntülenmelidir.


<a name="provisioningprofile" />

### <a name="creating-a-provisioning-profile"></a>Bir sağlama profili oluşturma

Şimdi bu uygulama kimliği içeren bir sağlama profili oluşturun Aşağıdaki adımları izleyin:

1.  Apple Geliştirici Merkezi Gözat **sağlama profilleri > tüm**:

    ![Profil bölümü sağlama](images/image10.png)

2.  Tuşuna **+** yeni bir sağlama profili oluşturmak için sağ üst köşedeki düğmesini.
3.  İhtiyaç ve tıklatın sağlama profili türünü seçin **devam**:

    ![Sağlama profili seçimi](images/image11.png)

4.  Yukarıdaki adımlarda oluşturulan uygulama kimliği açılır listeden seçin ve basın **devam**:

    ![Uygulama Kimliği seçimi](images/image12.png)

5.  Tuşuna basın ve uygulamayı imzalamak için kullanılan sertifikaları seçin **devam**:

    ![Sertifika seçimi](images/image13.png)

6.  Bu profili ve tuşuna dahil edilmesini istediğiniz aygıtları seçin **devam**:

    ![Cihazlar için sağlama profili seçin](images/image14.png)

7.  Böylece tanımlanabilir ve basın profili bir ad verin **devam** profili oluşturmak için:

    ![Sağlama profili adı](images/image15.png)

8.  Tuşuna **karşıdan** indirme düğmesine tıklayın ve sağlama profili yüklemek için bir Bulucu dosyasına çift tıklayın.

9. Visual Studio kullanıyorsanız emin **el ile sağlama** seçeneği işaretlidir.

10. Visual Studio for Mac / Visual Studio, Gözat ' **proje Seçenekleri > paket imzalama** ve yeni oluşturduğunuz bir sağlama profili ayarlayın:

    ![Visual Studio için Mac proje seçenekleri](images/image16.png)

> [!IMPORTANT]
> Entitlement.plist dosyasındaki yetkilendirme anahtarlar ve gizlilik anahtarları Info.plist dosyasında ayarlanan gerekebilir. Bu yetkilendirme hakkında daha fazla bilgi sağlanan [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

<a name="nextsteps" />

## <a name="next-steps"></a>Sonraki Adımlar

Sunucu tarafında bir özellik etkinleştirildikten sonra işlevselliği kullanmanıza olanak sağlamak için yapılması gereken hala iş yok. Aşağıdaki liste yapılması gereken ek adımları açıklar:

*   Uygulamanızda framework ad kullanın.
*   Gerekli yetkilendirmeler uygulamanıza ekleyin. Gerekli yetkilendirmeler ve bunları ekleme hakkında bilgi ayrıntılı olarak [yetkilendirmeler giriş](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

<a name="troubleshooting" />

## <a name="troubleshooting-capabilities"></a>Sorun giderme özellikleri

Aşağıdaki liste bazı roadblocks etkin bir uygulama hizmeti ile bir uygulama geliştirirken oluşturabilirsiniz en yaygın sorunların ayrıntıları verilmektedir.

-   Doğru kimliği düzgün oluşturulduğundan ve kayıtlı olduğundan emin olun **kimlikleri & profilleri, sertifikaları** Apple Geliştirici Portalı bölümü.
-   Hizmet Uygulaması'nın (veya bir uzantının) kimliği için eklenmiştir ve hizmet uygulama grubu/satıcı kimliği/yukarıda oluşturduğunuz kapsayıcısını kullanacak şekilde yapılandırıldığından emin olun **kimlikleri & profilleri, sertifikaları** Apple Geliştirici Portalı.
-   Sağlama profilleri ve uygulama kimlikleri yüklendiğinden emin olun ve uygulamanın **Info.plist** (Xamarin projede) yukarıda yapılandırılan uygulama kimlikleri birini kullanma.
-   Uygulamanın emin **Entitlements.plist** dosyasında (Xamarin Proje) sahip doğru hizmetinin etkinleştirilmiş.
-   Uygun gizlilik anahtarları info.plist dosyasında ayarlandığından emin olun
-   Uygulamasının **iOS paket imzalama**, emin **özel yetkilendirmeler** ayarlanır **Entitlements.plist**. Bu _değil_ hata ayıklama ve iOS simülatörü varsayılan ayarı oluşturur.

<a name="summary" />

## <a name="summary"></a>Özet

Bu kılavuzda açıklanan özellikleri veya _uygulama hizmetleri_ve nasıl bunlar Visual Studio ve Apple Geliştirici Merkezi'ndeki etkinleştirilebilir açıklanmıştır. Ayrıca, Cüzdan, iCloud, Apple Pay ve uygulama grupları gibi daha karmaşık Hizmetleri ayarlama konusunda ayrıntılı. Son olarak, sonraki adımlar için ayarlanan ve basit sorun giderme seçenekleri ele.