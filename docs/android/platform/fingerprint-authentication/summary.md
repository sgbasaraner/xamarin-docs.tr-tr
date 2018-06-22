---
title: Parmak izi kimlik doğrulaması Kılavuzu
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2b66c3660f6d8af9217089a7615784957fcc6ed7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763312"
---
# <a name="fingerprint-authentication-guidance"></a>Parmak izi kimlik doğrulaması Kılavuzu

## <a name="fingerprint-authentication-guidance"></a>Parmak izi kimlik doğrulaması Kılavuzu

Şimdi biz kavramları gördünüz ve kimlik doğrulaması API'leri Android 6.0 çevreleyen parmak izi göre parmak izi API'ları kullanmak için bazı genel öneriler ele alınmıştır.

1. **Android destek kitaplığı v4 uyumluluk API'lerini kullanan** &ndash; bu koddan API onay kaldırarak uygulama kodu basitleştirmek ve bir uygulamanın en aygıtları olası hedef izin.
2. **Parmak izi kimlik doğrulaması alternatifleri sağlamak** &ndash; parmak izi kimlik doğrulaması, bir kullanıcının kimliğini doğrulamak bir uygulama için harika, hızlı bir yoludur, ancak bunu görüntülenecektir her zaman çalışması veya kullanılabilir olduğunu kabul edemez. Parmak izi tarayıcısı başarısız olabilir, Mercek belki kirli, kullanıcı aygıt parmak izi kimlik doğrulaması kullanacak şekilde yapılandırılmamış olduğu veya parmak izi beri eksik ilerlemiş olduğunu mümkündür. Kullanıcı, uygulamanızla birlikte parmak izi kimlik doğrulaması kullanacak şekilde istemeyen mümkündür. Bu nedenlerle, bir Android uygulaması kullanıcı adı ve parola gibi diğer kimlik doğrulama işlemi sağlamalıdır.
3. **Google parmak izi simgesini kullanın** &ndash; tüm uygulamalar, Google tarafından sağlanan aynı parmak izi simgesi kullanmalıdır. Standart bir simge kullanımını uygulamalarında parmak izi kimlik doğrulama kullanıldığı tanımak Android kullanıcıları kolaylaştırır: 
    
    ![Android parmak izi simgesi](summary-images/ic-fp-40px.png)
    
4. **Kullanıcıya bildir** &ndash; parmak izi tarayıcı etkin olduğunu söyleyen bir bildirim kullanıcı için bir tür bir uygulama görüntülemelidir ve dokunma veya sağdan bekleniyor. 

## <a name="summary"></a>Özet

Parmak izi kimlik doğrulaması, kullanıcıların, uygulama içi satın alımlar gibi hassas özellikleri ile etkileşim kullanıcılar için daha kolay hale hızlı bir şekilde doğrulamak bir Xamarin.Android uygulaması izin vermek için harika bir yoludur. Bu kılavuzda ele alınan kavramlar ve Xamarin.Android uygulamanıza API kullanıcının Android 6.0 parmak izi kavramak için gereken kodu.

Önce bu sizi API kullanıcının kendileri parmak izi ele alınan `FingerprintManager` (ve `FingerprintManagerCompat`). Biz incelenmesi nasıl `FingerprintManager.AuthenticationCallbacks` Özet sınıf bir uygulama tarafından genişletilmiş ve parmak izi donanım ve uygulama arasında bir aracı olarak kullanılır. Biz parmak izi tarayıcı sonuçları bir Java kullanarak bütünlüğünü doğrulamak nasıl incelenmesi sonra `Cipher` nesnesi. Son olarak, biz biraz bir cihazda parmak izi nasıl açıklayan ve kullanılarak test üzerinde işlemdeki **adb** parmak izi manyetik bir öykünücü üzerinde benzetimini yapmak için. 

Zaten yapmadıysanız, gözden geçirmeniz gereken [örnek uygulama](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) bu kılavuzu eşlik. [Parmak izi iletişim örnek](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/) Java'dan Xamarin.Android için bağlantı noktalı ve parmak izi kimlik doğrulaması bir Android uygulama eklemek başka bir örnek sağlar.



## <a name="related-links"></a>İlgili bağlantılar

- [Parmak izi Kılavuzu örnek uygulaması](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Parmak izi iletişim kutusu örneği](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Fingerprint Icon](https://developer.android.comhttps://developer.xamarin.com/samples/FingerprintDialog/res/drawable-hdpi/ic_fp_40px.html)
