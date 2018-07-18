---
title: Ücretsiz xamarin iOS uygulamaları için hazırlama
description: Bu belgede, Apple'nın Ücretli Geliştirici programına kaydolun gerek kalmadan Xamarin.iOS geliştiriciler uygulamalarını fiziksel bir cihazda nasıl sınayabilirsiniz açıklanmaktadır.
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/16/2018
ms.openlocfilehash: 22ac17e211562eccbc49cc213e06079e77dd08c0
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111163"
---
# <a name="free-provisioning-for-xamarinios-apps"></a>Ücretsiz xamarin iOS uygulamaları için hazırlama

Ücretsiz sağlanmasına olanak tanır dağıtma ve iOS cihazlarında uygulamalarını test etmek bir Xamarin.iOS geliştiriciler **olmadan** parçası **Apple Developer Program**.
Simülatör test değerli ve kullanışlı olsa da, ayrıca bunlar gerçek bellek, depolama ve ağ bağlantısı kısıtlamaları altında düzgün olduğunu doğrulamak için fiziksel bir iOS cihazlarındaki uygulamaların test etmek için gereklidir.

Uygulama bir aygıta dağıtmak için ücretsiz sağlama kullanmak için:

- Xcode gerekli oluşturulacağı *imzalama kimliği* (Geliştirici sertifikasını ve özel anahtarı) ve *sağlama profili* (bir açık uygulama kimliği ve bağlı bir iOS cihazın UDID'si içeren).
- İmzalama kimliğini ve sağlama profili Xcode Mac ya da Visual Studio 2017 için Visual Studio tarafından oluşturulan Xamarin.iOS uygulamanızı dağıtmak için kullanın.

> [!IMPORTANT]
> [Otomatik sağlama](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) Mac veya Geliştirici testleri için bir cihaz otomatik olarak ayarlamak için Visual Studio 2017 için Visual Studio sağlar. Ancak, otomatik sağlama ücretsiz sağlama ile uyumlu değil. Otomatik sağlamayı kullanmak için ücretli bir Apple Developer Program hesabınız olması gerekir.

## <a name="requirements"></a>Gereksinimler

Ücretsiz sağlama ile bir cihaz, Xamarin.iOS uygulama dağıtmak için:

- Kullanılan Apple Kimliğini Apple Geliştirici programına bağlı olmamalıdır.
- Xamarin.iOS uygulamanızı açık uygulama kimliği, joker değil uygulama kimliğini kullanmanız gerekir
- Xamarin.iOS uygulamanızda kullanılan paket grubu tanımlayıcısı, benzersiz olmalıdır ve başka bir uygulamada daha önce kullanılmış olamaz. Ücretsiz sağlama ile kullanılan herhangi bir paket grubu tanımlayıcısı **olamaz** yeniden kullanılabilir.
- Zaten dağıtılmış bir uygulama, ücretsiz sağlama ile bu uygulama dağıtamazsınız.
- Uygulama Hizmetleri uygulamanıza kullanıyorsa, bir sağlama profili içinde ayrıntılı olarak oluşturmanız gerekecektir [cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md#appservices) Kılavuzu. 

Bir göz atın [sınırlamaları](#limitations) ilişkili sınırlamaları hakkında daha fazla bilgi için bu belgenin bölüm sağlama ücretsiz ve başvurduğu [uygulama dağıtım kılavuzları](~/ios/deploy-test/app-distribution/index.md) hakkında daha fazla bilgi iOS uygulamaları dağıtma.

## <a name="testing-on-device-with-free-provisioning"></a>Ücretsiz sağlama ile cihaz üzerinde test etme

Ücretsiz sağlama ile Xamarin.iOS uygulamanızı test etmek için aşağıdaki adımları izleyin.

### <a name="use-xcode-to-create-a-signing-identity-and-provisioning-profile"></a>Xcode imzalama kimliğini ve sağlama profili oluşturmak için kullanın

1. Bir Apple kimliği yoksa [oluşturmak](https://appleid.apple.com).
2. Xcode açın ve gidin **Xcode > Tercihler**.
3. Altında **hesapları**, kullanın **+** mevcut Apple kimliğinizi eklemek için Aşağıdaki ekran görüntüsüne benzer görünmelidir:

    ![Xcode tercihleri – hesapları](free-provisioning-images/launchapp1.png "Xcode tercihleri – hesaplar")

4. Xcode tercihleri kapatın.
5. Uygulamanızı dağıtmak istediğiniz iOS cihaz takın.
6. Xcode'da, yeni bir proje oluşturun. Seçin **Dosya > Yeni > Proje** seçip **tek görünüm uygulaması**.
7. Yeni Proje iletişim kutusunda ayarlamak **takım** eklediğiniz Apple kimliği. Aşağı açılan listeden, benzer şekilde görünmelidir **uygulamanızın adı (Kişisel ekibi)**:

    ![Yeni Uygulama Oluştur](free-provisioning-images/launchapp2.png "yeni uygulama oluştur")

8. Yeni Proje oluşturulduktan sonra iOS Cihazınızı (bir benzetici yerine) hedefleyen bir Xcode derleme düzeni seçin.

    ![Bir Xcode derleme düzenini seçin](free-provisioning-images/xcodescheme.png "bir Xcode derleme düzeni seçin")

9. Xcode'un içinde kendi üst düzey düğüm seçerek uygulamanızın proje ayarlarını açın **Proje Gezgini**.
10. Altında **genel > kimlik**, emin **paket grubu tanımlayıcısı** _tam olarak eşleşen_ Xamarin.iOS uygulamanızın paket grubu tanımlayıcısı.

    ![Bir paket grubu tanımlayıcısı ayarlamak](free-provisioning-images/launchapp5.png "paket grubu tanımlayıcısı ayarlayın")

    > [!IMPORTANT]
    > Xcode yalnızca bir açık uygulama kimliği için bir sağlama profili oluşturur ve Xamarin.iOS uygulama için uygulama kimliği aynı olmalıdır.
    > Bunlar farklıysa, Xamarin.iOS uygulamanızı dağıtmak için ücretsiz sağlama kullanmanız mümkün olmayacaktır.

11. Altında **dağıtım bilgisi**, dağıtım hedefi eşleşen veya iOS bağlı iOS Cihazınızda yüklü sürümden daha küçük olduğundan emin olun.
12. Altında **imzalama**seçin **otomatik olarak imzalanmasını yönetmek** ve takımınızın aşağı açılan listeden seçin:

    ![Otomatik olarak imzalanmasını yönetmek](free-provisioning-images/launchapp6.png "otomatik olarak imzalanmasını yönetmek")

    Xcode otomatik imzalama kimliği ve sağlama profiliyle sizin için oluşturur. Bu, sağlama profili yanındaki bilgi simgesine tıklayarak görüntüleyebilirsiniz:

    ![Sağlama profilini görüntülemek](free-provisioning-images/launchapp7.png "sağlama profili görüntüle")

    > [!TIP]
    > Bir hata varsa bir sağlama profili oluşturmak Xcode girişiminde bulunduğunda bu Xcode'un seçili derleme düzeni simülatör yerine, bağlı iOS cihazına hedefleyen emin olun.

13. Xcode içindeki test etmek için çalıştırma düğmesine tıklayarak cihazınızın boş uygulamayı dağıtın.

### <a name="deploy-your-xamarinios-app"></a>Xamarin.iOS uygulamanızı dağıtma

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. İOS Cihazınızı Mac derleme konağı USB aracılığıyla bağlayın veya [kablosuz](~/ios/deploy-test/wireless-deployment.md).
2. Mac için Visual Studio **çözüm bölmesi**, çift tıklayarak **Info.plist**.
3. İçinde **imzalama**seçin **el ile sağlama**.
4. Tıklayın **iOS paket grubu imzalama...** düğmesi.
5. İçin **yapılandırma**seçin **hata ayıklama**.
6. İçin **Platform**seçin **iPhone**.
7. Seçin **imzalama kimliği** Xcode tarafından oluşturuldu.
8. Seçin **sağlama profili** Xcode tarafından oluşturuldu.

    ![İmzalama kimliği ve sağlama profili](free-provisioning-images/launchapp8.png "imzalama kimliğini ve sağlama profili Ayarla")

    > [!TIP]
    > İmzalama kimliğinizi veya doğru sağlama profilini göremiyorsanız, Mac için Visual Studio'yu yeniden başlatmanız gerekebilir

9. Tıklayın **Tamam** kaydedip kapatmak için **proje seçenekleri**.
10. İOS Cihazınızı seçin ve uygulamayı çalıştırın.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio 2017 aktarıldığından emin emin [eşleştirilmiş bir Mac derleme konağı](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. İOS Cihazınızı Mac derleme konağı USB aracılığıyla bağlayın veya [kablosuz](~/ios/deploy-test/wireless-deployment.md).
3. Visual Studio 2017'de **Çözüm Gezgini**, Xamarin.iOS projenize sağ tıklayıp **özellikleri**.
4. Gidin **iOS paket grubu imzalama**.
5. İçin **yapılandırma**seçin **hata ayıklama**.
6. İçin **Platform**seçin **iPhone**.
7. Seçin **el ile sağlama**.
8. Seçin **imzalama kimliği** Xcode tarafından oluşturuldu.
9. Seçin **sağlama profili** Xcode tarafından oluşturuldu.
    
    ![İmzalama kimliği ve sağlama profili](free-provisioning-images/setprofile-w157.png "imzalama kimliğini ve sağlama profili Ayarla")

    > [!TIP]
    > Xcode, bu imzalama kimliğini ve sağlama profili oluşturulur ve bunları, Mac derleme konağı üzerinde depolanır. Edildikten sonra Visual Studio 2017'ye erişilebilir olduklarını [eşleştirilmiş](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Mac derleme konağı için. Bunlar listelenmiyorsa, Visual Studio 2017'yi yeniden başlatmanız gerekebilir.

10. Kaydet ve Proje Özellikleri'ni kapatın.
11. İOS Cihazınızı seçin ve uygulamayı çalıştırın.

-----

## <a name="limitations"></a>Sınırlamalar

Apple, sınırlamalar, ne zaman ve nasıl ücretsiz sağlama yalnızca dağıtabileceğiniz olmasını sağlayarak bir iOS cihazında uygulamanızı çalıştırmak için kullanabileceğiniz bir dizi uygulanan *,* cihaz:

- Geliştiricilerin uygulamalarına serbestçe sağlama erişim iTunes Connect sınırlıdır ve bu nedenle, App Store ve TestFlight yayımlama gibi hizmetler için kullanılamaz. Geçici ve şirket içi bir yol dağıtmak için bir Apple Geliştirici hesabı (Kurumsal veya kişisel) gereklidir.
- Ücretsiz sağlama ile oluşturulan sağlama profilleri, bir hafta sonra sona erer ve imzalama kimlikleri, bir yıl sonra sona erecek. 
- Xcode yalnızca açık uygulama kimlikleri için sağlama profillerini oluşturacak bu yana takip etmeleri gerekir [yukarıdaki yönergeleri](#testing-on-device-with-free-provisioning) yüklemek istediğiniz her uygulama için.
- Çoğu uygulama hizmetleri için sağlama ücretsiz sağlamayı mümkün değildir. Bu, Apple Pay, Game Center, iCloud, uygulama içi satın alma, anında iletme bildirimleri ve Cüzdan içerir. Apple özelliklerin tam bir liste sağlar [desteklenen yetenekler (iOS)](https://help.apple.com/developer-account/#/dev21218dfd6) Kılavuzu. Uygulama Hizmetleri ile kullanmak için uygulamanızı sağlamak için ziyaret [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzlar.

## <a name="summary"></a>Özet

Bu kılavuz, avantajları ve bir iOS cihazında uygulamaları yüklemek için ücretsiz sağlama kullanmanın sınırlamaları incelediniz. Bu ücretsiz sağlama bir Xamarin.iOS uygulaması yüklemek için nasıl kullanılacağını gösteren adım adım kılavuz sağlanır.

## <a name="related-links"></a>İlgili bağlantılar

- [Cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md)
- [Uygulama hizmetleri için hazırlama](~/ios/get-started/installation/device-provisioning/index.md#appservices)
