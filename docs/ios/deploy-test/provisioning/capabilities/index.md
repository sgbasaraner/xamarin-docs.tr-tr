---
title: Özellikleri ile çalışma
description: Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz, tüm özellikler için gereken kurulum açıklar.
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: ff918ac104e7eab4f2e8c0d0be46df240138c97c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-capabilities"></a>Özellikleri ile çalışma

_Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz, tüm özellikler için gereken kurulum açıklar._

Apple sağlar geliştiricilere _yetenekleri_, çoğunlukla bilinen _uygulama hizmetleri_işlevselliği genişletme ve hangi iOS uygulamalarını kapsamını genişletme yolu yapabilirsiniz gibi. Özellikleri daha ayrıntılı tümleştirme platformu özellikleri gibi kendi uygulamaya eklemek için geliştiricilere izin ver: uygulama, Siri gibi ek aygıt hizmetleri ve diğer başlatılan para işlemleri sahip olma yeteneği.
Bu özellikler Xamarin.iOS projeleri ile kullanılabilir. Hizmetlerin tam listesini aşağıda belirtilmiştir:

* Uygulama grupları
* İlişkili etki alanları
* Veri koruma
* Oyun Merkezi
* HealthKit
* HomeKit
* Kablosuz Aksesuar Yapılandırması
* iCloud
* Uygulama içi satın alma
* Uygulamalar Arası Ses
* Apple Pay
* Wallet
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


Özellikleri ya da Visual Studio aracılığıyla Mac için veya el ile Apple Geliştirici Portalı'nda etkinleştirilebilir. Cüzdan, Apple Pay ve iCloud gibi belirli özelliklerini uygulama kimliklerini ek yapılandırma gerektirir.

Bu kılavuzda, uygulamanızda Mac için ve gerekli olabilecek herhangi bir ek ayar dahil olmak üzere Geliştirici Merkezi aracılığıyla el ile hem Visual Studio'da bu uygulama hizmetlerinin her biri etkinleştirmek açıklanmaktadır. 

## <a name="adding-app-services"></a>Uygulama Hizmetleri ekleme

Özellikleri kullanmak için uygulamayı doğru hizmetinin etkinleştirilmiş bir uygulama kimliği içeren geçerli bir sağlama profili olması gerekir. Bu sağlama profili oluşturma ya da otomatik olarak Mac için Visual Studio veya el ile Apple Developer Center'da yapılabilir.

Bu bölümde Mac'ın otomatik sağlama için Visual Studio veya Geliştirici Merkezi çoğu işlevlerini etkinleştirmek için nasıl kullanılacağı açıklanmaktadır. Cüzdan, iCloud, Apple Pay ve ek kurulum gerektirir uygulama grupları gibi bazı özellikler vardır. Bu bitişik kılavuzları ayrıntılı açıklanmıştır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

> [!IMPORTANT]
> Tüm özellikleri eklenebilir ve Mac için Visual Studio'daki yönetilen Aşağıdaki liste, desteklenen özellikler içerir:
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


Özellikleri eklenir **Entitlements.plist** Mac için Visual Studio'da Özellikleri eklemek için aşağıdaki adımları izleyin:

1. Açık **Info.plist** iOS uygulamanızın dosyasını ve olun **imzalama otomatik olarak yönetir** seçilir. Adımları [otomatik sağlamayı](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) yardıma gereksinim duyarsanız Kılavuzu:

    ![İmzalama seçeneği otomatik olarak yönetme](images/manage-signing.png)

2. Açık **Entitlements.plist** dosya ve eklemek istediğiniz yeteneği seçin:

    ![Entitlements.plist dosyasına yetenekleri ekleme](images/image17.png)

    Bir özellik seçerek iki işlemi yapar:
    * Bu özellik, uygulama Kimliğinize ekler
    * Entitlements.plist dosyanızı yetkilendirme anahtar/değer çifti ekler.

    Mac için Visual Studio ne zaman bu görevleri aşağıdaki başarı iletisi görüntüleyerek gerçekleştirildikten önerilerde:

    ![Entitlements.plist dosyasına yetenekleri ekleme](images/image18.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Otomatik Visual Studio 2017 sağlama şu anda hiçbir desteği olduğundan kullanmalısınız [Geliştirici Merkezi](#devcenter) oluşturmak için bir doğru uygulama hizmetleri ile bir uygulama kimliği.

-----

<!--
<a name="xcode" />

## Xcode

Xamarin developers can also use Xcode to quickly create a provisioning profile with a suitable App ID. This process, described below, can be used for any app service in the list:

1.  Open Xcode and create a ‘dummy’ project. Give the dummy project the same name as your Xamarin.iOS project. The bundle identifier should be identical to the bundle identifier of your Xamarin.iOS project:

    ![Xcode Create Project](images/image1.png)

2.  Ensure **Automatically manage signing** is selected:

    ![Automatically manage signing selection](images/image2.png)

3.  Once the app has been created, go to the tab named **Capabilities**:

    ![Xcode Capabilities tab](images/image3.png)

4.  Browse to the capability that you wish to add, and move the switch to the **ON** position.
5.  This will create a provisioning profile with an App ID that contains the capability and adds the entitlement to the profile.
6.  In Visual Studio for Mac / Visual Studio, browse to **Project Options > Bundle Signing** and set the provisioning profile to the one that was just created in Xcode:

    ![Visual Studio for Mac Project Options](images/image4.png)
-->

<a name="devcenter" />

## <a name="developer-center"></a>Geliştirici Merkezi

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

9. Mac sağlamak için Visual Studio kullanıyorsanız **imzalama otomatik olarak yönetir** seçeneği de serbest seçili **Info.plist** dosyası

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