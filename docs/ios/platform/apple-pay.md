---
title: Xamarin.iOS, Apple Pay'i
description: Bu kılavuz, uygulamanız aracılığıyla üyeliklerini Gıda ve eğlence gibi fiziksel mal karşılığında Ödeme yapmalarını sağlayan, Apple Pay ile kullanılmak üzere Xamarin.iOS ortamını ayarlama keşfediyor. Bu, gerekli tanımlayıcıları, sertifikalar ve yetkilendirmeler hakkındaki bilgileri içerir.
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: f2a38a4305aa11c78f3e4e35265a86dc71642777
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351671"
---
# <a name="apple-pay-in-xamarinios"></a>Xamarin.iOS, Apple Pay'i

_Bu kılavuz, uygulamanız aracılığıyla üyeliklerini Gıda ve eğlence gibi fiziksel mal karşılığında Ödeme yapmalarını sağlayan, Apple Pay ile kullanılmak üzere Xamarin.iOS ortamını ayarlama keşfediyor. Bu, gerekli tanımlayıcıları, sertifikalar ve yetkilendirmeler hakkındaki bilgileri içerir._

Apple Pay, 8, iOS yanı sıra kullanıcıların iOS cihazlarını aracılığıyla üyeliklerini Gıda ve eğlence gibi fiziksel mal karşılığında Ödeme yapmalarını sağlayan sunulmuştur. İPhone 6 ve iPhone 6 kullanılabilir artı ve ayrıca mağaza içi satın alma işlemleri için Apple Watch ile eşleştirilmek. İphone'da kullanıldığında, onaylayın ve bir kullanıcının kredi veya banka kartı işlemleri yetkilendirmek için bir yol olarak Touch ID kullanır.

## <a name="requirements"></a>Gereksinimler

Apple Pay yalnızca içinde iOS 8 ve üzerinde kullanılabilir ve bu nedenle Xcode 6 en az gerektirir.

Aşağıdaki öğeler de Apple Pay uygulamanızla tümleştirmek için gereklidir:

 - Ödeme İşlemci platformu
 - Ticari tanımlayıcısı
 - Bir Apple Pay sertifikası
 - Apple Pay yetkilendirme

Bu belge, bu öğeleri daha ayrıntılı bir şekilde bakar.

## <a name="differences-between-apple-pay-and-iap"></a>Apple Pay'i IAP arasındaki farklar

Apple Pay arasındaki birincil fark ve *uygulama içi satın alma* (IAP), bunlar satan ürünleri ilgilidir. *Fiziksel* ürünlerin satılan Apple Pay; Gıda, Konaklama ve fiziksel eğlence (örneğin, sinema biletler) bu tüm örnekleri mevcuttur. Buna karşılık, IAP sattığı *sanal* premium veya ek içeriği ve akış hizmeti ya da ek canlı oyun ek ay abonelikleri – düşünme gibi ürünleri.

Kullanılan çerçeveler, ayrıca önemli bir fark, [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) olan Apple Pay için kullanıldığında, while [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) IAP için framework API sağlar.

Apple Pay, Apple ile [durumları](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) , "Apple Pay'i ödemeleri için kullanılacak kullanıcı, tüccarların veya geliştiricilerin uyguladığınız değil". Buna karşılık, her işlem için % 30 ücret IAP sahiptir. Ayrıca, Apple Pay ile işlem Apple hiç geçmez, bunun yerine, bu ödeme platformu üzerinden gider.

## <a name="using-a-payment-processor-platform"></a>Ödeme İşlemci platformu kullanma

Apple Pay temel bölümlerini ödemeler işlenmesini biridir. Kendi başınıza yapmanız mümkün olsa da, şifreleme önemli bilgi gerektirir.
- Apple'nın ayrıntılı [ödeme işleme Kılavuzu](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Ödeme işleme platformlar, diğer yandan, bu işlemler için uygulamanızı oluşturmaya odaklanmasına olanak tanıyan işleyin.

İki seçenek içerir:

- **Stripe** -adresinde kaydolun [Stripe.com](https://stripe.com/) kendi API'lerine erişmek için.

- **JudoPay** -kullanıma kendi [Xamarin örnek kodu github'da](https://github.com/Judopay/Xamarin-Sample-App)ve en kaydetme [JudoPay.com](https://www.judopay.com/).

## <a name="provisioning-for-apple-pay"></a>Apple Pay'i için sağlama

Apple Pay kullanmak için uygulamaları yapılandırma Apple Developer Portal'a ve uygulamanız içinde kurulum gerektirir. Uygulamanız Apple pay'i için başarılı bir şekilde sağlamak için izlenmesi gereken adımlar vardır:

1. Ticari kimliği oluşturun:
    - Adımları [burada](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Geçerli ödeme özelliğine sahip bir uygulama kimliği oluşturun ve satıcı ekleyin:
    - Adımları [burada](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Satıcı Kimliği için bir sertifika oluşturun:
    - Adımları [burada](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Yeni oluşturulan uygulama Kimliğine sahip bir sağlama profili oluşturun:
    - Adımları [burada](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Apple Pay yetkilendirmelerini ekleyin:
    - Apple ödeme yetkilendirmesini ayrıntılı olarak seçin [burada](~/ios/deploy-test/provisioning/entitlements.md), dosyadan el ile anahtar/değer çifti ekleyin veya [burada](~/ios/deploy-test/provisioning/entitlements.md)

## <a name="working-with-apple-pay"></a>Apple Pay'i ile çalışma

Apple bazı geliştirmeler Apple Pay için Web sitelerinden ve Siri ve haritaları ile etkileşim aracılığıyla güvenli ödeme yapmak kullanıcının olanak tanıyan iOS 10 de yapılmıştır.

İOS 10 ile hem iOS hem de dinamik ödeme ağları ve yeni bir korumalı alan test ortamı desteklemek için watchOS birlikte çalışan çeşitli yeni API'ler eklenmiştir.

### <a name="apple-pay-website-integration"></a>Apple ödeme Web tümleştirme

Yeni iOS 10, geliştirici Apple Pay doğrudan kendi Web sitelerini kullanarak birleştirebilirsiniz **ApplePay JS**. Web sitesi Safari ile iOS veya macOS atan kullanıcılar, iPhone veya Apple Watch işlem doğrulayarak Apple Pay ödemeleri yapabilirsiniz. Daha fazla bilgi için lütfen Apple'nın bakın [ApplePay JP Framework başvurusu](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>PassKit Framework geliştirmeleri

İOS 10'da, PassKit framework Apple Pay dışında destekleyecek şekilde genişletildi `UIKit` ve uygulamalarını kendi kartları sunmak kart verenler izin vermek için.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Apple Pay'i Uıkit dışında destekleme

Kullanarak [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) ve [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), bir uygulama tarafından sağlanan işlevsellik destekleyebilir [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) Uıkit kullanmadan. Bu yeni API'yi Apple Pay Apple Watch (ve de belirli hedefleri) desteklemek için gerekli olsa da, diğer durumlarda (örneğin, var olan uygulamaları) isteğe bağlıdır. Ancak, Apple ile tek bir kod tabanının tüm geliştirici uygulamaları boyunca geniş Apple Pay destek sağlamak için yeni API'ye yönelik olabildiğince çabuk taşıma önerir. Hedefleri hakkında daha fazla bilgi ve Siri tümleştirmesi, lütfen bizim [SiriKit giriş](~/ios/platform/sirikit/index.md) belgeleri.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Veren kartları uygulamaları sunma

İOS 10 ile kendi uygulamaları içinde kartlarını sunmak kart verenler izin PassKit Framework yeni özellikler eklendi. Geliştirici ekleyebilirsiniz bir `PKPaymentButtonTypeInStore` UIButton bir kart için bir Apple Pay düğme görüntüler uygulama kullanıcı arabirimi.

`PresentPaymentPass` Yöntemi [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) sınıfı da kullanılabilir kart programlı bir şekilde görüntülenecek.

### <a name="new-payment-network-support"></a>Yeni ödeme ağ desteği

Yeni Geliştirici değiştirmek için uygulamayı derleyin ve App Store için yeniden gerek olmadan kullanılabilir olduğunda iOS 10, bir uygulamayı otomatik olarak yeni bir ödeme ağ destekleyebilir.

Yeni [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) yöntemi `PKPaymentNetwork` sınıfı, bir uygulama çalışma zamanında kullanıcının cihazında kullanılabilen ağlar bulunacak olanak tanır. Ayrıca, [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) özelliği, bağımsız değişken olarak ödeme sağlayıcının adını almak için genişletilmiştir. Bu yöntemleri kullanarak bir uygulamayı otomatik olarak ödeme sağlayıcının desteklediği herhangi bir ağa destekleyebilir.

Daha fazla bilgi için lütfen bkz. bizim [Apple ödeme yapılandırma](~/ios/platform/apple-pay.md) ve Apple'nın [Apple ödeme Kılavuzu](https://developer.apple.com/apple-pay/).

### <a name="new-testing-environment"></a>Yeni test ortamı

İOS 10 ile Apple Geliştirici doğrudan bir iOS cihazında test ödeme kartı sağlamak izin veren yeni bir test ortamını kullanıma sunmuştu. Bu yeni test ortamı, şifrelenmiş test ödeme verilerini ardından uygulamaya döndürür.

Yeni test ortamı sağlamak için aşağıdakileri yapın:

1. İTunes CONNECT'te yeni test iCloud hesabı oluşturun.
2. İOS cihazı yeni test hesabı ile oturum açın.
3. Uygulamayı test etmek istediğiniz bölgeyi ayarlayın.
4. Test ödeme kartlardan birini [Apple ödeme Kılavuzu](https://developer.apple.com/apple-pay/) ödeme yapma.

> [!IMPORTANT]
> İCloud hesapları geçerek, cihaz otomatik olarak yeni test ortamına geçiş yapacağız. Ancak, Apple hala **gerektirir** kartlar ile gerçek test edilecek uygulamanın iTunes App Store için göndermeden önce bir üretim ortamında.

## <a name="summary"></a>Özet

Bu makalede, biz Apple Pay, uygulamanızda kullanmak için gereken farklı öğeler incelediniz. Satıcı Kimliği oluşturma ve içinde nasıl kullanılacağını inceledik **Entitlements.plist**, el ile değiştirilmesi gerekir.

## <a name="related-links"></a>İlgili bağlantılar

- [Uygulama İçi Satın Alma](~/ios/platform/in-app-purchasing/index.md)
- [PassKit giriş](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (örnek)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
