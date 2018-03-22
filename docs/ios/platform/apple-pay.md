---
title: Apple Pay
description: "Bu kılavuz yemek, eğlence ve uygulamanızı aracılığıyla üyelikleri gibi fiziksel mal için ödeme yapmak için Apple Pay ile kullanılmak üzere Xamarin.iOS ortamını ayarlama araştırır. Gerekli tanımlayıcıları, sertifikalar ve yetkilendirmeler hakkında bilgi içerir."
ms.topic: article
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: af899bb1c5708e3fc0be88db6224d9127f5a5c6d
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="apple-pay"></a>Apple Pay

_Bu kılavuz yemek, eğlence ve uygulamanızı aracılığıyla üyelikleri gibi fiziksel mal için ödeme yapmak için Apple Pay ile kullanılmak üzere Xamarin.iOS ortamını ayarlama araştırır. Gerekli tanımlayıcıları, sertifikalar ve yetkilendirmeler hakkında bilgi içerir._


Apple Pay, 8, iOS yemek, eğlence ve iOS cihazlarını aracılığıyla üyelikleri gibi fiziksel mal ödeme bağlanmalarını sağlayarak sunulmuştur. İPhone 6 ve iPhone 6 kullanılabilir artı ve ayrıca mağaza satın alma işlemleri için Apple Watch ile eşleştirilmek. İPhone üzerinde kullanıldığında, Touch ID onaylayın ve bir kullanıcının kredi veya ATM kartı işlemleri yetkilendirmek için bir yol olarak kullanır.


## <a name="requirements"></a>Gereksinimler

Apple Pay yalnızca yukarıda ve iOS 8 içinde kullanılabilir ve bu nedenle Xcode 6 en az gerektirir.

Aşağıdaki öğeler de Apple Pay uygulamanıza tümleştirmek için gereklidir:

 - Ödeme İşlemci platformu
 - Ticari tanımlayıcısı
 - Bir Apple Pay sertifikası
 - Apple Pay yetkilendirme

Bu belgede bu öğeler daha ayrıntılı ele alacağız.

## <a name="differences-between-apple-pay-and-iap"></a>Apple Pay ve IAP arasındaki farklar

Apple Pay arasındaki birincil fark ve *uygulama içi satın alma* (IAP) sattığı ürünler için geçerlidir. *Fiziksel* mal satılan Apple Pay; yemek, Konaklama ve fiziksel eğlence (örneğin, sinema biletleri) bu tüm örnekler verilmiştir. Buna karşılık, IAP sattığı *sanal* premium veya ek içeriği ve akış hizmet ya da bir oyunda fazladan yaşamlarını ek ay abonelikleri – düşünme gibi mal.

Kullanılan çerçeveler ayrıca en önemli fark, [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) olan Apple Pay için kullanıldığında, while [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) IAP için API çerçevesi sağlar.

Apple Pay, Apple ile [durumları](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) , "kullanıcılar, tüccarların veya geliştiricilerin Apple Pay ödemeler için kullanmak üzere uyguladığınız değil". Buna karşılık, her işlem için bir % 30 ücret IAP sahiptir. Ayrıca, Apple Pay ile işlem Apple hiç geçmez, bunun yerine, bir ödeme platform gider.


## <a name="using-a-payment-processor-platform"></a>Ödeme İşlemci platformu kullanma

Apple Pay temel bölümlerini ödemeler işlenmesini biridir. Bunu yapmak mümkün olsa da, şifreleme önemli bilgi gerektirir
- Apple'nın içinde ayrıntılı olarak [Rehber ödeme işleme](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Ödeme işleme platformlar, diğer yandan, bu işlemler, uygulamanızı oluşturma odaklanmasına olanak tanıyan işleyin.

İki seçenek içerir:

- **Şerit** -oturum açın [Stripe.com](https://stripe.com/) bunların API'lerine erişmek için.

- **JudoPay** -kullanıma kendi [Xamarin örnek kodu github'da](https://github.com/Judopay/Xamarin-Sample-App)ve adresindeki kayıt [JudoPay.com](https://www.judopay.com/).


## <a name="provisioning-for-apple-pay"></a>Apple Pay için sağlama

Apple Pay kullanmak için bir uygulama yapılandırma Kurulum Apple Geliştirici Portalı ve, uygulamanızın içinde gerektirir. Uygulamanız için Apple pay başarılı bir şekilde sağlamak için izlenmesi gereken adımlar vardır:

1. Satıcı kimliği oluşturur:
    - Adımları [burada](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Geçerli ödeme özelliğine sahip bir uygulama kimliği oluşturmanız ve satıcı ekleyin:
    - Adımları [burada](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Satıcı Kimliği için bir sertifika oluşturun:
    - Adımları [burada](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Yeni oluşturulan uygulama kimliği ile bir sağlama profili oluştur:
    - Adımları [burada](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Apple Pay yetkilendirmeler ekleyin:
    - Apple pay yetkilendirme ayrıntılı olarak seçin [burada](~/ios/deploy-test/provisioning/entitlements.md), dosyadan el ile anahtar/değer çifti ekleyin veya [burada](~/ios/deploy-test/provisioning/entitlements.md)


## <a name="working-with-apple-pay"></a>Apple Pay ile çalışma

Apple bazı geliştirmeler Apple Pay için güvenli Web sitelerinde ve Siri ve Haritalar ile etkileşimi aracılığıyla ödemelerini kullanıcıya izin 10 iOS içinde kullanıma sunmuştur.

İOS 10 birkaç yeni API hem iOS hem de dinamik ödeme ağlar ve yeni bir korumalı alan test ortamı desteklemek için watchOS birlikte çalışan eklendi.


### <a name="apple-pay-website-integration"></a>Apple Pay Web tümleştirme

Yeni iOS 10, geliştirici Apple Pay doğrudan kendi Web sitelerini kullanmaya içine dahil edebilirsiniz **ApplePay JS**. Web sitesi Safari ile iOS veya macOS atan kullanıcılar, iPhone veya Apple Watch işlem doğrulayarak Apple Pay ödemeleri yapabilirsiniz. Daha fazla bilgi için lütfen Apple'nın bkz [ApplePay JP Framework başvurusu](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>PassKit Framework geliştirmeleri

Apple Pay dışında desteklemek için PassKit framework genişletilmiştir iOS 10'da, `UIKit` ve uygulamalarını içinde kendi karttan sunmak kart verenler izin vermek için.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Apple Pay Uıkit dışında destekleme

Kullanarak [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) ve [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), bir uygulama tarafından sağlanan işlevsellik destekleyebilir [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) Uıkit kullanmadan. Bu yeni API Apple Pay Apple Watch (ve de belirli hedefleri) desteklemek için gerekli olsa da, diğer durumlarda (örneğin, var olan uygulamalar) isteğe bağlıdır. Ancak, yeni API için tek bir kod tabanıyla Geliştirici uygulamaları tümünün boyunca geniş Apple Pay destek sağlamak için mümkün olan en kısa sürede taşıma Apple önerir. Hedefleri hakkında daha fazla bilgi ve Siri tümleştirmesi, lütfen bakın bizim [SiriKit giriş](~/ios/platform/sirikit/index.md) belgeleri.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Uygulamaların içindeki veren karttan sunma

İOS 10 kendi uygulamaların içindeki kartlarını sunmak kart verenler izin PassKit framework için yeni özellikler eklenmiştir. Geliştirici ekleyebilirsiniz bir `PKPaymentButtonTypeInStore` UIButton bir kart için bir Apple Pay düğme görüntülenir uygulamanın kullanıcı arabirimi.

`PresentPaymentPass` Yöntemi [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) sınıfı ayrıca program aracılığıyla kartını görüntülemek için kullanılabilir.

### <a name="new-payment-network-support"></a>Yeni ödeme ağ desteği

Yeni değiştirmek, uygulamayı derleyin ve uygulama mağazasında yeniden gönderin gerek kalmadan Geliştirici olmadan yayınlandığında 10 iOS için uygulama otomatik olarak yeni bir ödeme ağ destekleyebilir.

Yeni [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) yöntemi `PKPaymentNetwork` sınıfı bir uygulamanın çalışma zamanında kullanıcının cihazda kullanılabilir ağlar keşfedin olanak tanır. Ayrıca, [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) özelliği, ödeme sağlayıcının adını bağımsız değişken olarak almak için genişletilmiştir. Bu yöntemleri kullanarak bir uygulamayı otomatik olarak ödeme sağlayıcının desteklediği herhangi bir ağ destekler.

Daha fazla bilgi için lütfen bkz bizim [Apple ödeme yapılandırma](~/ios/platform/apple-pay.md) ve Apple'nın [Apple ödeme Kılavuzu](https://developer.apple.com/apple-pay/).

### <a name="new-testing-environment"></a>Yeni bir test ortamı

İOS 10 Apple geliştirici bir iOS cihazında doğrudan test ödeme kartı sağlamak izin veren yeni bir test ortamı kullanıma sunuldu. Bu yeni test ortamını şifrelenmiş test ödeme veri ardından uygulamaya döndürür.

Yeni test ortamını etkinleştirmek için aşağıdakileri yapın:

1. Yeni test iCloud hesabı iTunes bağlantı oluşturun.
2. İOS cihazı yeni test hesabı ile oturum açın.
3. Uygulama test etmek istediğiniz bölgeyi ayarlayın.
4. Test ödeme kartları birini kullanın [Apple ödeme Kılavuzu](https://developer.apple.com/apple-pay/) ödeme yapmak için.

> [!IMPORTANT]
> İCloud hesapları geçerek, cihaz otomatik olarak yeni test ortamına geçiş yapar. Ancak, Apple hala **gerektirir** ile gerçek sınanacak uygulama kartları iTunes App Store'da göndermeden önce bir üretim ortamında.

## <a name="summary"></a>Özet

Bu makalede, Apple Pay uygulamanızda kullanmak için gereken farklı öğeler incelediniz. Satıcı Kimliği oluşturma ve içinde nasıl kullanıldığı inceledik **Entitlements.plist**, el ile değiştirilmesi gerekir.


## <a name="related-links"></a>İlgili bağlantılar

- [Uygulama İçi Satın Alma](~/ios/platform/in-app-purchasing/index.md)
- [PassKit giriş](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (örnek)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
