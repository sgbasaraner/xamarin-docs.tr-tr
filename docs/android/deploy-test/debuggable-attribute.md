---
title: Debuggable özniteliği
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 85b2462605f76e3be8bae589e9fe6cf655741746
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762696"
---
# <a name="debuggable-attribute"></a>Debuggable özniteliği



Hata ayıklama mümkün kılmak için Android Java hata ayıklama kablo Protokolü (JDWP) destekler. Bu bir JVM ile iletişim kurmak için ADB gibi araçlar sağlayan bir teknolojidir. Geliştirme sırasında JDWP önemli olmakla birlikte, bu yayımlanan uygulama önce devre dışı bırakılmalıdır.

JDWP değerini olabilir `android:debuggable` bir Android uygulamasını özniteliği. Xamarin.Android bu öznitelik ayarlamak için aşağıdaki yöntemleri sağlar:

1.  Oluşturulan bir `AndroidManifext.xml` dosya ve ayar `android:debuggable` özniteliği vardır.
2.  Dahil olmak üzere `ApplicationAttribute` içinde bir `.CS` dosya şu şekilde: `[assembly: Application(Debuggable=false)]` .


Her iki `AndroidManifest.xml` ve `ApplicationAttribute` sunmak, içeriğini olan `AndroidManifest.xml` ne tarafından belirtilen üzerinden öncelik alır `ApplicationAttribute`.

Ne `AndroidManifest.xml` veya `ApplicationAttribute` mevcut olduğundan, varsayılan değeri `android:debuggable` özniteliği bağlıdır hata ayıklama simgeleri desteklemediğini oluşturulur. Hata ayıklama simgeleri sonra Xamarin.Android ayarlayacak `android:debuggable` özniteliğini `true`.

Unutmayın değerini `android:debuggable` özniteliği mutlaka bağlı değildir derleme yapılandırmasını temel. İçin sürüm derlemeler için olası `android:debuggable` özniteliği true.


## <a name="related-links"></a>İlgili bağlantılar

- [Android markette debuggable uygulamalar](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
