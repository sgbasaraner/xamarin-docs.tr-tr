---
title: "Debuggable özniteliği"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 65037029d01d499421fd825f72347ae1bebd9966
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="debuggable-attribute"></a>Debuggable özniteliği



Hata ayıklama mümkün kılmak için Android Java hata ayıklama kablo Protokolü (JDWP) destekler. Bu bir JVM ile iletişim kurmak için ADB gibi araçlar sağlayan bir teknolojidir. Geliştirme sırasında JDWP önemli olmakla birlikte, bu yayımlanan uygulama önce devre dışı bırakılmalıdır.

JDWP değerini olabilir `android:debuggable` bir Android uygulamasını özniteliği. Xamarin.Android bu öznitelik ayarlamak için aşağıdaki yöntemleri sağlar:

1.  Oluşturulan bir `AndroidManifext.xml` dosya ve ayar `android:debuggable` özniteliği vardır.
1.  Dahil olmak üzere `ApplicationAttribute` içinde bir `.CS` dosya şu şekilde: `[assembly: Application(Debuggable=false)]` .


Her iki `AndroidManifest.xml` ve `ApplicationAttribute` sunmak, içeriğini olan `AndroidManifest.xml` ne tarafından belirtilen üzerinden öncelik alır `ApplicationAttribute`.

Ne `AndroidManifest.xml` ve `ApplicationAttribute`, ardından varsayılan değerini `android:debuggable` özniteliği bağlıdır hata ayıklama simgeleri desteklemediğini oluşturulur. Hata ayıklama simgeleri sonra Xamarin.Android ayarlayacak `android:debuggable` özniteliğini `true`.

Unutmayın değerini `android:debuggable` özniteliği mutlaka bağlı değildir derleme yapılandırmasını temel. İçin sürüm derlemeler için olası `android:debuggable` özniteliği true.


## <a name="related-links"></a>İlgili bağlantılar

- [Android markette debuggable uygulamalar](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
