---
title: "Java tümleştirmesine genel bakış"
description: "Java ekosistemi bileşenlerini farklı ve büyük bir koleksiyonunu içerir. Bu bileşenlerin çoğu bir Android uygulama geliştirmek için gereken süreyi azaltmak için kullanılabilir. Bu belge getirir ve geliştiriciler bu varolan Java bileşenleri kendi Xamarin.Android uygulaması geliştirme deneyimini geliştirmek için kullanabilir yollardan bazılarını üst düzey bir bakış sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/18/2017
ms.openlocfilehash: 88e8c66d36956649f0a996046f038d89a7267cf5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="java-integration-overview"></a>Java tümleştirmesine genel bakış

_Java ekosistemi bileşenlerini farklı ve büyük bir koleksiyonunu içerir. Bu bileşenlerin çoğu bir Android uygulama geliştirmek için gereken süreyi azaltmak için kullanılabilir. Bu belge getirir ve geliştiriciler bu varolan Java bileşenleri kendi Xamarin.Android uygulaması geliştirme deneyimini geliştirmek için kullanabilir yollardan bazılarını üst düzey bir bakış sağlar._

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

Java ekosistemi kapsamını göz önüne alındığında, bir Xamarin.Android uygulaması için gerekli herhangi bir belirli işlevsellik Java'da zaten kodlanmış olduğunu olasılığı yüksektir. Bu nedenle, bir Xamarin.Android uygulaması oluştururken bu varolan kitaplıklar yeniden denemek için çekici. 

Bir Xamarin.Android uygulaması Java kitaplıklarında yeniden kullanmak için üç olası yolu vardır: 

-   **Java bağlamaları kitaplığı oluşturma** &ndash; Bu teknik ile bir Xamarin.Android projesi C# sarmalayıcıları Java türleri geçici oluşturmak için kullanılır. Bir Xamarin.Android uygulaması bu projesi tarafından oluşturulan C# sarmalayıcıları başvuru ve ardından `.jar` dosya. 

-   **Java yerel arabirimi** &ndash; *Java yerel* *arabirimi* (JNI) arayın veya JVM içinde çalışan Java kodu tarafından çağrılması için Java olmayan kod (örneğin, C++ veya C#) sağlayan bir çerçevedir . 

-   **Kod bağlantı noktası** &ndash; Java kaynak kodunu alma ve sonra C# dönüştürmeden bu yöntemi içerir. Bu, el ile veya keskinleştirme gibi otomatikleştirilmiş bir aracı kullanılarak yapılabilir. 

İlk iki teknikleri çekirdeği *Java yerel arabirimi* (JNI). JNI Java'da yazılmış olmayan uygulamaların bir Java sanal makinede çalışan Java kod ile etkileşim sağlayan bir çerçevedir. Xamarin.Android oluşturmak için JNI kullanan *bağlamaları* C# kodu. 

İlk teknik Java kitaplıkları bağlama için daha fazla otomatik ve bildirim temelli bir yaklaşımdır. Mac veya Xamarin.Android tarafından sağlanan bir Visual Studio Proje türü için ya da Visual Studio kullanarak içerir &ndash; Java bağlamaları kitaplığı. Bu bağlamaları başarıyla oluşturmak için bir Java bağlamaları kitaplık hala saf JNI yaklaşım el ile yapılan bazı değişiklikler, ancak olarak kadar fazla olur gerektirebilir. Bkz: [bir Java kitaplığı bağlama](~/android/platform/binding-java-library/index.md) Java bağlama kitaplıklar hakkında daha fazla bilgi için. 

JNI, kullanarak ikinci bir teknik daha düşük bir düzeyde, çalışır ancak için daha hassas bir denetim sağlamak ve normal bir Java bağlama kitaplığı erişilebilir olmaz Java yöntemleri erişim. 

Üçüncü teknik önceki iki tüketicisinin farklıdır: Java'dan C# kod bağlantı noktası oluşturma. Kod bir dilden diğerine taşıma zahmetli bir işlem olabilir, ancak bir aracı Yardım çabayla adlı küçültmek mümkündür *keskinleştirme*. Keskinleştirme, bir Java bir açık kaynak aracıdır-için-C# dönüştürücü. 


<a name="Summary" />

## <a name="summary"></a>Özet

Bu belge kitaplıkları Java'dan bir Xamarin.Android uygulaması kullanılabilme farklı yollardan bazılarını üst düzey bir özeti sağlanır. Bağlamaları kavramlarını sunulan ve aranabilir sarmalayıcılar yönetilen ve C# Java kod bağlantı noktası oluşturma seçenekleri ele alınan. 


## <a name="related-links"></a>İlgili bağlantılar

- [Mimari](~/android/internals/architecture.md)
- [Java kitaplığı bağlama](~/android/platform/binding-java-library/index.md)
- [JNI ile çalışma](~/android/platform/java-integration/working-with-jni.md)
- [Sharpen](https://github.com/slluis/sharpen)
- [Java yerel arabirimi](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
