---
title: "Apple Pay watchOS üzerinde"
description: "Bu makalede yenilikleri kapsayan Apple yaptı Apple Pay watchOS 3 ve bunların içinde Xamarin.iOS için Apple Watch nasıl uygulanacağını."
ms.topic: article
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 591f6f53c9e787ee9499b2a1a3cc812f7e72749a
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="apple-pay-on-watchos"></a>Apple Pay watchOS üzerinde

Apple bazı geliştirmeler Apple Pay için uygulama ödemeler için destek ekler 3 watchOS içinde kullanıma sunmuştur. Bu kullanıcının güvenli bir şekilde ödeme sağlar ve fiziksel mal ve hizmetler için doğrudan Apple Watch ödeme bilgilere başvurun olanak sağlar.


## <a name="about-apple-pay-enhancements"></a>Apple Pay geliştirmeleri hakkında

Yukarıdaki Stated Apple bazı geliştirmeler Apple Pay için için güvenli ödeme izin ve fiziksel mal ve hizmetler için doğrudan Apple Watch ödeme bilgilere başvurun watchOS 3 içinde kullanıma sunmuştur. Bu geliştirmeler, değişiklikleri PassKit Framework tarafından sağlanır.

İOS 10 ve watchOS 3 ile hem iOS hem de dinamik ödeme ağlar ve yeni bir korumalı alan test ortamı desteklemek için watchOS birlikte çalışan birkaç yeni API eklenmiştir.

## <a name="passkit-framework-enhancements"></a>PassKit Framework geliştirmeleri

Apple Pay dışında desteklemek için PassKit framework genişletilmiştir iOS 10'da, `UIKit` ve uygulamalarını içinde kartlarını sunmak kart verenler izin vermek için. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Apple Pay Uıkit dışında destekleme

Kullanarak [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) ve [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), bir uygulama tarafından sağlanan işlevsellik destekleyebilir [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) Uıkit kullanmadan. Bu yeni API Apple Pay Apple Watch (ve de belirli hedefleri) desteklemek için gerekli olsa da, diğer durumlarda (örneğin, var olan uygulamalar) isteğe bağlıdır. Ancak, yeni API için tek bir kod tabanıyla Geliştirici uygulamaları tümünün boyunca geniş Apple Pay destek sağlamak için mümkün olan en kısa sürede taşıma Apple önerir. Hedefleri hakkında daha fazla bilgi ve Siri tümleştirmesi, lütfen bakın bizim [SiriKit giriş](~/ios/platform/sirikit/index.md) belgeleri.

### <a name="presenting-issuer-cards-from-within-apps"></a>Uygulamaların içindeki veren karttan sunma

İOS 10 ve watchOS 3 ile kendi uygulamaların içindeki kendi ödeme kartları sunmak kart verenler izin PassKit framework için yeni özellikler eklenmiştir. Geliştirici ekleyebilirsiniz bir `PKPaymentButtonTypeInStore` UIButton bir kart için bir Apple Pay düğme görüntülenir uygulamanın kullanıcı arabirimi.

`PresentPaymentPass` Yöntemi [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) sınıfı ayrıca program aracılığıyla kartını görüntülemek için kullanılabilir.

## <a name="new-payment-network-support"></a>Yeni ödeme ağ desteği

Yeni değiştirmek, uygulamayı derleyin ve uygulama mağazasında yeniden gönderin gerek kalmadan Geliştirici olmadan kullanılabilir olduğunda iOS 10 ve watchOS 3 için bir uygulamayı otomatik olarak yeni bir ödeme ağ destekleyebilir.

Yeni [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) yöntemi `PKPaymentNetwork` sınıfı bir uygulamanın çalışma zamanında kullanıcının cihazda kullanılabilir ağlar keşfedin olanak tanır. Ayrıca, [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) özelliği, ödeme sağlayıcının adını bağımsız değişken olarak almak için genişletilmiştir. Bu yöntemleri kullanarak bir uygulamayı otomatik olarak ödeme sağlayıcının desteklediği herhangi bir ağ destekler.

Daha fazla bilgi için lütfen bkz bizim [Apple ödeme yapılandırma](~/ios/platform/apple-pay.md) ve Apple'nın [Apple ödeme Kılavuzu](https://developer.apple.com/apple-pay/).

## <a name="new-testing-environment"></a>Yeni bir test ortamı

Apple iOS 10 ve watchOS 3 ile bir iOS cihazında doğrudan test ödeme kartı sağlamak Geliştirici sağlayan yeni bir test ortamı kullanıma sunuldu. Bu yeni test ortamını şifrelenmiş test ödeme veri ardından uygulamaya döndürür.

Yeni test ortamını etkinleştirmek için aşağıdakileri yapın:

1. Yeni test iCloud hesabı iTunes bağlantı oluşturun.
2. İOS cihazı yeni test hesabı ile oturum açın.
3. Uygulama test etmek istediğiniz bölgeyi ayarlayın.
4. Test ödeme kartları birini kullanın [Apple ödeme Kılavuzu](https://developer.apple.com/apple-pay/) ödeme yapmak için.

> [!NOTE]
> İCloud hesapları geçerek, cihaz otomatik olarak yeni test ortamına geçiş yapar. Ancak, Apple hala **gerektirir** ile gerçek sınanacak uygulama kartları iTunes App Store'da göndermeden önce bir üretim ortamında.

## <a name="summary"></a>Özet

Bu makalede ele alınan geliştirmeleri Apple yaptı Apple Pay watchOS 3 ve Xamarin.iOS içinde uygulama.
