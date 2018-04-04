---
title: Cüzdan özellikleri
description: Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz Cüzdan özellikleri için gereken kurulum açıklar.
ms.prod: xamarin
ms.assetid: BD9475E6-F586-488C-93D4-8A2A1629B99B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: f41ba2b693b37f39ea49f62e322932537014a317
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="wallet-capabilities"></a>Cüzdan özellikleri

_Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz Cüzdan özellikleri için gereken kurulum açıklar._

Cüzdan depolayan ve barkodları ve anahtarları, binme geçişleri ve bunların aygıttan sağ kuponlar görüntülemek kullanıcıların diğer içeriği görüntüleyen bir uygulamadır. Bu bilgiler depolanan bir _geçirmek_. Örneğin, binme geçiş veya tek bir bilet tekil geçişi olacaktır. 

Geliştiriciler, çeşitli şekillerde Cüzdan birlikte çalışabilir:

*   Bir geçiş oluşturun, bir uygulamanın oluşturulması gerekmez. Bir Passfile bazı JSON dosyaları ve isteğe bağlı meta veri dosyaları içeren bir sıkıştırılmış Arşivi ' dir. Bu, hazırlamak için bir [geçirmek türü kimliği](~/ios/platform/passkit.md) ve [geçişi sertifika](~/ios/platform/passkit.md) gereklidir. Bu bilgiler daha sonra bir JSON dosyası bildirildi. Bir Passfile sağlama hakkında daha fazla bilgi bulunabilir [PassKit giriş](~/ios/platform/passkit.md) Kılavuzu.

*   Yardımcı uygulamaları geçişleri dağıtmak için yazılmıştır. Aynı zamanda oluşturmak, düzenlemek ve geçişleri güncelleştirmek için işlevlere sahiptirler ve ardından bunları Cüzdan uygulamaya eklemek için. Bir kullanıcı bir bilet uygulama üzerinden satın aldıktan sonra bu tür bir uygulama için güzel bir örnek anahtarı için Cüzdan doğrudan uygulamadan eklenebilir sinema uygulama – olacaktır. Bir yardımcı uygulamayı kullanmak için sağlama profilinizin aşağıdaki adımları izleyerek ayarlayabilirsiniz Cüzdan özelliklere sahip bir uygulama kimliği içermelidir. Uygulamanızı gerekli yetkilendirmeler de dahil etmelisiniz.

*   Conduit değil işlemek uygulamalar geçirir doğrudan uygulamalardır. İletiyi almasını ve kullanıcı için Cüzdan ekleme seçeneğiniz vermiş ötesinde geçişi minimum etkileşim sahiptirler. Bu uygulamaları herhangi bir özel sağlama veya yetkilendirmeler gerekmez, ancak bazı yöntemler PassKit çerçeveden kullanır.

## <a name="developer-center"></a>Geliştirici Merkezi

Cüzdan ile kullanmak için yeni bir sağlama profili oluşturmak için aşağıdakileri yapın:

1.  Gözat [sertifikalar, tanımlayıcılarını ve profilleri](https://developer.apple.com/account/ios/certificate/) Apple Geliştirici Portalı bölümü.
2.  Altında **tanımlayıcıları**, Gözat **uygulama kimlikleri**: 
    
    ![Uygulama Kimliği seçimi](wallet-capabilities-images/image17.png)

3.  Tıklatın **+** üst simgesine sağ sayfasının.
4.  Yeni bir uygulama kimliği, vererek kaydetmek bir **adı** ve bir paket tanımlayıcısı. (Bu paket tanımlayıcısı projenizdeki paket kimliği eşleşmesi gerektiğini unutmayın):
   
    ![Uygulama Kimliği ayrıntılarını Ekle](wallet-capabilities-images/image18.png)

5.  Seçin **Cüzdan** Hizmetleri listesinden uygulama hizmeti:
    
    ![Hizmet ekran'ı seçin](wallet-capabilities-images/image19.png)

6.  Tuşuna **devam**ve ardından **kaydetmek** bir uygulama kimliği oluşturmak için

Gerekirse, var olan uygulama kimlikleri Cüzdan özelliği eklemek için düzenlenebilir.

Bu uygulama kimliği artık oluşturmak ya da, yeni bir sağlama profili yeniden oluşturmak için açıklandığı gibi kullanılabilir [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu:

![Kullanarak yeni sağlama profili oluşturmak için uygulama kimliği oluşturulmuş](wallet-capabilities-images/image20.png)


Cüzdan kullanma hakkında daha fazla bilgi için aşağıdaki kılavuzlara bakın:

*   [PassKit giriş](~/ios/platform/passkit.md)
 
## <a name="next-steps"></a>Sonraki Adımlar
 
Aşağıdaki liste yapılması gereken ek adımları açıklar:

* Uygulamanızda framework ad kullanın.
* Gerekli yetkilendirmeler uygulamanıza ekleyin. Gerekli yetkilendirmeler ve bunları ekleme hakkında bilgi ayrıntılı olarak [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.
* Uygulamasının **iOS paket imzalama**, emin **özel yetkilendirmeler** ayarlanır **Entitlements.plist**. Bu _değil_ hata ayıklama ve iOS simülatörü varsayılan ayarı oluşturur.

Uygulama Hizmetleri ile ilgili sorunlarla karşılaşırsanız, başvurmak [sorun giderme](~/ios/deploy-test/provisioning/capabilities/index.md) ana Kılavuzu'nun bölümünde.