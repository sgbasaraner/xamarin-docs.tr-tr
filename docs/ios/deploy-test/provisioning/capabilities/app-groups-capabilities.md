---
title: Xamarin.iOS uygulaması Grup Özellikleri
description: Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz, uygulama grubu özellikleri için gereken kurulum açıklar.
ms.prod: xamarin
ms.assetid: 0A61220B-BBAC-492B-9D3B-578986E64064
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 3bfae755c11437df721943d2c16526d195e4dec7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785625"
---
# <a name="app-group-capabilities-in-xamarinios"></a>Xamarin.iOS uygulaması Grup Özellikleri

_Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz, uygulama grubu özellikleri için gereken kurulum açıklar._

Bir uygulama grubu farklı uygulamaları (veya bir uygulama ve uzantılarını) paylaşılan dosya depolama konumuna erişim sağlar. Uygulama grupları gibi verileri için kullanılabilir:

*   [Apple Watch ayarları](~/ios/watchos/app-fundamentals/settings.md)
*   [Paylaşılan NSUserDefaults](~/ios/app-fundamentals/user-defaults.md)
*   [Paylaşılan dosyaları](~/ios/watchos/app-fundamentals/parent-app.md#files)

## <a name="configure-a-new-app-group"></a>Yeni bir uygulama grubu yapılandırma

Paylaşılan konumda kullanılarak yapılandırılmış bir [uygulama grubu](https://developer.apple.com/library/content/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), içinde yapılandırılmış **tanımlayıcıları & profilleri, sertifikaları** bölümünde [Apple Geliştirici Merkezi](https://developer.apple.com/account/). Bu değer ayrıca her projenin Entitlements.plist başvurulmuş olması gerekir.

Uygulama grubu, genellikle bir grup paket kimliği olan bir tanımlayıcı sahip olur. önek. Örneğin, paket kimliği `com.xamarin.WatchSettings` uygulama grubu olurdu `group.com.xamarin.WatchSettings`.

Yeni bir uygulama grubu oluşturmak için aşağıdakileri yapın:

1.  Apple'nın ziyaret [iOS Geliştirici Merkezi](https://developer.apple.com/account/)açın, **hesap** ve oturum açın.
2.  Seçin **kimlikleri & profilleri, sertifikaları**.
3.  Altında **tanımlayıcıları** seçin **uygulama grupları** tıklatıp **+** yeni bir grup oluşturmak için düğmesi.
4.  Girin bir **adı** ve bir **tanımlayıcısı** tıklatın ve yeni grup için **devam** düğmesi: 
   
    ![Uygulama Grubu Ekle ayrıntıları](app-groups-capabilities-images/image52.png)

5.  Tıklatın **kaydetmek** grubu oluşturmak için düğmesini ve **Bitti** kayıtlı uygulama grupları listesine geri dönmek için.

## <a name="configure-an-app-to-use-app-groups"></a>Uygulama grupları kullanmak için uygulamayı yapılandırma

Uygulamaları kullanabilmesi oluşturulmuş olan uygulama grubu ile uygulama kimliklerini yapılandırın.

Aşağıdakileri yapın:

1.  Apple'nın ziyaret [iOS Geliştirici Merkezi](https://developer.apple.com/account/)ve günlük bir Apple Developer hesapla.
2.  Gelen **Program kaynakları** menüsünde, select **kimlikleri & profilleri, sertifikaları**.
3.  Altında **tanımlayıcıları** seçin **uygulama kimlikleri** tıklatıp **+** yeni bir kimlik oluşturmak için düğmesi
4.  Uygulama kimliği için bir ad girin ve bir açık uygulama kimliği verin
5.  Altında **uygulama hizmetleri** etkinleştirmek **uygulama grupları**, sonra da devam düğmesini tıklatın:

    ![Uygulama grubu uygulama hizmetleri Ekle](app-groups-capabilities-images/image53.png)

6.  Ayarları doğrulayın ve tıklatın **kaydetmek** bir uygulama kimliği oluşturmak için düğmesi
7.  Tıklatın **Bitti** listesine dönmek için düğmesini kayıtlı uygulama kimlikleri.
8.  Listeden yeni oluşturduğunuz uygulama Kimliğini seçin ve'ı tıklatın **Düzenle** düğmesi:

    ![Uygulama Kimliği listeden seçin](app-groups-capabilities-images/image54.png)

9.  Hizmeti altında **uygulama grubu**, tıklatın **Düzenle** düğmesi:

    ![Uygulama Kimliği listeden seçin](app-groups-capabilities-images/image55.png)

10. Yukarıda oluşturduğunuz uygulama grubu seçin ve tıklatın **devam** düğmesi:

    ![Uygulama Grubu Ekle](app-groups-capabilities-images/image56.png)

11. Tıklatın **atamak** düğmesi, sonra **Bitti** listesine dönmek için düğmesini kayıtlı uygulama kimlikleri.
12. Uygulama grubu kullanan tüm uygulamalar (veya uzantıları) için aşağıdaki adımları yineleyin.

## <a name="next-steps"></a>Sonraki Adımlar
 
Aşağıdaki liste yapılması gereken ek adımları açıklar:

* Uygulamanızda framework ad kullanın.
* Gerekli yetkilendirmeler uygulamanıza ekleyin. Gerekli yetkilendirmeler ve bunları ekleme hakkında bilgi ayrıntılı olarak [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.
* Uygulamasının **iOS paket imzalama**, emin **özel yetkilendirmeler** ayarlanır **Entitlements.plist**. Bu _değil_ hata ayıklama ve iOS simülatörü varsayılan ayarı oluşturur.

Uygulama Hizmetleri ile ilgili sorunlarla karşılaşırsanız, başvurmak [sorun giderme](~/ios/deploy-test/provisioning/capabilities/index.md) ana Kılavuzu'nun bölümünde.