---
title: "Sağlama profilleri"
description: "Bu kılavuz gerekli sağlama Xamarin.Mac uygulamayı yayımlamak için gerekli profilleri oluşturmada size yol gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: bdff6c32-f7e3-4a97-a093-dbda48be8227
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: dab923f6150bdf005e9468add6d26d4fdb691a93
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="provisioning-profiles"></a>Sağlama profilleri

Sağlama profilleri bir geliştirici Xamarin.Mac uygulamalarını birkaç (eski adıyla Mac OS X) macOS belirli özellikleri (örneğin, iCloud ve anında iletme bildirimleri) eklemenizi sağlar. Bunlar gerekir oluşturmak, indirin ve bu özellikleri kullanan geliştirmekte olduğunuz her uygulama için bir Mac sağlama profili yükleyin.

[![](profiles-images/certif13.png "Apple sağlama portalı")](profiles-images/certif13.png#lightbox)

<a name="Development_Provisioning_Profile" />

## <a name="development-provisioning-profile"></a>Geliştirme sağlama profili

Bir geliştirme sağlama profili profildeki ayarı olmuştur belirli bilgisayarlara sınanacak Mac App Store hedeflenen bir uygulama sağlar. Bu iCloud ve anında iletme bildirimleri gibi macOS özellikleri kullanırken özellikle geçerlidir.

> [!NOTE]
> Bir geliştirme sağlama profili oluşturmadan önce Geliştirici Mac geliştirme sertifikası oluşturmuş olmalısınız. Oluşturulacak üzerinde bu ekran görüntüsünde gösterildiği gibi ayrıntıları tamamlayın bir **geliştirme sağlama profili** yapıları oluşturmak için kullanılabilir. Koyulmalıdır geçerli bir Mac geliştirme sertifikası seçilmek üzere kullanılabilir **sertifika** kutusu ve en az bir sistem test etmek için kayıtlı.

Aşağıdakileri yapın:

1. Sağlama türü profil oluşturmak ve'ı tıklatın, select **devam** düğmesi: 

     [![](profiles-images/certif14.png "Profil türü seçme")](profiles-images/certif14.png#lightbox)
2. İçin profil oluşturma tıklatıp uygulama Kimliğini seçin **devam** düğmesi: 

     [![](profiles-images/certif15.png "Uygulama Kimliği seçme")](profiles-images/certif15.png#lightbox)
3. Profil oturumu ve tıklatın için kullanılan Geliştirici seçin **devam**: 

     [![](profiles-images/certif16.png "Geliştirici kimliği seçme")](profiles-images/certif16.png#lightbox)
4. Bu profili kullanılabilir ve tıklatın bilgisayarları seçin **devam**: 

     [![](profiles-images/certif17.png "İzin verilen bilgisayarlar seçiliyor")](profiles-images/certif17.png#lightbox)
5. Şimdi, girmek bir **profil adı** tıklatıp **Generate** düğmesi: 

     [![](profiles-images/certif18.png "Profil oluşturma")](profiles-images/certif18.png#lightbox)
6. Tıklatın **karşıdan** yeni profilini yüklemek için düğmeyi: 

     [![](profiles-images/certif19.png "Profili indiriliyor")](profiles-images/certif19.png#lightbox)
7. Geliştirme sağlama profilleri Mac'ın profilleri Tercihler bölmesinde yüklü **sistem tercihleri** uygulama: 

     [![](profiles-images/certif20.png "Profili yükleme")](profiles-images/certif20.png#lightbox)
8. Profil Tercihler bölmesinde yüklü tüm profilleri gösterir: 

     [![](profiles-images/image47.png "Yüklü tüm gösteren profilleri")](profiles-images/image47.png#lightbox)
9. Profil de görünür **Geliştirici sertifika yardımcı programını** durumunda yeniden indirilmesi gerekir: 

     [![](profiles-images/image48.png "Geliştirici sertifika yardımcı programını")](profiles-images/image48.png#lightbox)

Yeni bir geliştirme sağlama profili her yeni bir uygulama için veya yeni bir bilgisayar üzerinde test eklendiğinde oluşturulmuş olması gerekir.

<a name="Production_Provisioning_Profile" />

## <a name="production-provisioning-profile"></a>Üretim sağlama profili

Üretim sağlama profilleri göndermelerini Mac App Store için bir paket oluşturmak için gereklidir.

Aşağıdakileri yapın:

1. Oluşturma ve tıklatın profili türünü seçin **devam** düğmesi: 

    [![](profiles-images/certif21.png "Profil türünü seçme")](profiles-images/certif21.png#lightbox)
2. İçin profil oluşturma tıklatıp uygulama Kimliğini seçin **devam** düğmesi: 

    [![](profiles-images/certif15.png "Uygulama Kimliği seçme")](profiles-images/certif15.png#lightbox)
3. Profil oturum tıklatıp şirket kimliği seçin **devam** düğmesi: 

    [![](profiles-images/certif23.png "Şirket Kimliği seçme")](profiles-images/certif23.png#lightbox)
4. Girin bir **profil adı** tıklatıp **Generate** düğmesi: 

    [![](profiles-images/certif24.png "Profil oluşturma")](profiles-images/certif24.png#lightbox)
5. Tıklatın **karşıdan** sağlama profili dosyası almak için (uzantısı `.provisionprofile`): 

    [![](profiles-images/certif25.png "Profili indiriliyor")](profiles-images/certif25.png#lightbox)
6. Buraya sürükleyin **Xcode Düzenleyicisi** veya yüklemek için çift tıklayın. Profil daha sonra Xcode Düzenleyici'yi görünür: 

    [![](profiles-images/image51.png "Profili yükleme")](profiles-images/image51.png#lightbox)
7. Sağlama profili de listede görünür: 

    [![](profiles-images/certif26.png "Yüklü profiller gösterme")](profiles-images/certif26.png#lightbox)


Geliştirici herhangi bir zamanda (ör. bir uygulama kimliği tarafından kullanılan özellikleri değişirse iCloud veya anında iletme bildirimlerini etkinleştirme) bunlar sağlama profilleri, uygulama kimliği için yeniden oluşturmanız

## <a name="related-links"></a>İlgili bağlantılar

- [Yükleme](~//mac/get-started/installation.md)
- [Merhaba, Mac örnek](~//mac/get-started/hello-mac.md)
- [Mac App Store, uygulamaları dağıtma](https://developer.apple.com/devcenter/mac/checklist/)
- [Araçlar Kılavuzu: Kod uygulamanızı imzalama](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Geliştirici kimliği ve ağ geçidi](https://developer.apple.com/resources/developer-id/)
