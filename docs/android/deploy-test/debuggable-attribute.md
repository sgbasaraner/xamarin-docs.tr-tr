---
title: "Debuggable özniteliği"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: db09ebe29b6c404bac892fd76389faf0b9e03d5b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="debuggable-attribute"></a>Debuggable özniteliği

<a name="Overview" />


Hata ayıklama mümkün kılmak için Android Java hata ayıklama kablo Protokolü (JDWP) destekler. Bu bir JVM ile iletişim kurmak için ADB gibi araçlar sağlayan bir teknolojidir. Geliştirme sırasında JDWP önemli olmakla birlikte, bu yayımlanan uygulama önce devre dışı bırakılmalıdır.

JDWP değerini olabilir `android:debuggable` bir Android uygulamasını özniteliği. Xamarin.Android bu öznitelik ayarlamak için aşağıdaki yöntemleri sağlar:

1.  Oluşturulan bir `AndroidManifext.xml` dosya ve ayar `android:debuggable` özniteliği vardır.
1.  Dahil olmak üzere `ApplicationAttribute` içinde bir `.CS` dosya şu şekilde: `[assembly: Application(Debuggable=false)]` .


Her iki `AndroidManifest.xml` ve `ApplicationAttribute` sunmak, içeriğini olan `AndroidManifest.xml` ne tarafından belirtilen üzerinden öncelik alır `ApplicationAttribute`.

Ne `AndroidManifest.xml` ve `ApplicationAttribute`, ardından varsayılan değerini `android:debuggable` özniteliği bağlıdır hata ayıklama simgeleri desteklemediğini oluşturulur. Hata ayıklama simgeleri sonra Xamarin.Android ayarlayacak `android:debuggable` özniteliğini `true`.

Unutmayın değerini `android:debuggable` özniteliği mutlaka bağlı değildir derleme yapılandırmasını temel. İçin sürüm derlemeler için olası `android:debuggable` özniteliği true.


## <a name="related-links"></a>İlgili bağlantılar

- [Android markette debuggable uygulamalar](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
