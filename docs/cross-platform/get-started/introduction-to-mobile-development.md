---
title: Mobil Geliştirme giriş
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/28/2017
ms.openlocfilehash: 2f3950509134d3f643f0ea63b6725c1b4fe38409
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-mobile-development"></a>Mobil Geliştirme giriş

Mobil uygulamaları oluşturmak IDE'yi açma, bir şey birlikte atma, sınama ve bir uygulama mağazasının – tüm bir öğleden sonra yapılan gönderme hızlı bir bit yapılması kadar kolay olabilir. Veya ayrıntılı eylemli tasarım, kullanılabilirlik, cihazları, bir tam beta yaşam döngüsü ve ardından dağıtım binlerce üzerinde birkaç farklı yolla sınama QA sınamaları içerir son derece katılan bir işlem olabilir.

Bu belge, Xamarin platform tanıtmak için tasarlanmıştır. Daha fazla bilgi edinmek için *işlem* tasarımından mobil uygulamaları test etme için derleme başvurmak [mobil yazılım geliştirme yaşam döngüsü giriş](~/cross-platform/get-started/introduction-to-mobile-sdlc.md) belge.

Lütfen bizim [sistem gereksinimleri](~/cross-platform/get-started/requirements.md#mac) Xamarin yükleyemeyebilir onaylamak için.

## <a name="introduction-to-xamarin"></a>Xamarin giriş

İOS ve Android uygulamaları oluşturmak nasıl değerlendirirken, birçok kişi yerel diller, Objective-C, Swift ve Java, tek seçeneğiniz olduğunu düşünür. Ancak, son birkaç yıl içinde mobil uygulamaları oluşturmak için platformları tüm yeni ekosistemi ortaya çıktı.

Xamarin benzersiz olduğundan bu alanında tek bir dil – C#, sınıf kitaplığını ve tüm üç mobil platformlarda iOS, Android ve Windows Phone çalışır çalışma zamanı sunarak (Windows Phone ' ın yerel dili olan zaten C#) hala, yerel (derleme sırasında oyunlar yoğun için yeterince kullanıcı olmayan-yorumlanan) uygulamaların bile.

Bu platformların her biri farklı bir özellik kümesinden vardır ve her yerel uygulamaları – başka bir deyişle, yazma yeteneğini yerel kod ve o birlikte çalışma fluently altındaki Java alt sistemi derleme uygulamaları değişir. Örneğin, bazı platformlar yalnızca bazı çok düşük düzeyli ve yalnızca C/C++ kod izin veren ancak HTML ve JavaScript, oluşturulacak uygulamaları izin verir. Bazı platformlar bile yerel Denetim Araç Seti alanını yok.

Xamarin tüm yerel platformları gücünü birleştirir ve kendi başına dahil olmak üzere, güçlü özellikler sayıda ekler benzersizdir:

1.   **Temel alınan SDK'ları için bağlama tamamlamak** – Xamarin neredeyse tüm temel platform SDK iOS ve Android için bağlamaları içerir. Ayrıca, bu bağlamaların, kesin türü belirtilmiş, gidin ve kullanabilir ve sağlam derleme zamanı tür denetimi ve geliştirme sırasında sağlamak kolay anlamına gelir. Bu, daha az sayıda çalışma zamanı hataları ve daha yüksek kaliteli uygulamaları sağlar.
1.   **Objective-C, Java, C ve C++ birlikte çalışması** – Xamarin doğrudan çağırma Objective-C, Java, C olanakları sağlar ve C++ kitaplıkları, 3. taraf geniş bir yelpazede kullanmak için güç vermiş kodu zaten oluşturulmuş. Bu, var olan iOS ve Android kitaplıkları Objective-C, Java veya C/C++ ile yazılmış yararlanmanızı sağlar. Ayrıca, Xamarin kolayca bir bildirim temelli söz dizimini kullanarak yerel Objective-C ve Java kitaplıkları bağlamak izin bağlama projeleri sunar.
1.   **Modern dil yapıları** – Xamarin uygulamaları yazılır C# ' ta gibi Objective-C ve Java önemli geliştirmeler içerir modern bir dil *dinamik dil özellikleri* ,  *İşlev yapıları* gibi *Lambda'lar* , *LINQ* , *paralel programlama* özellikler, Gelişmiş *genel türler*  ve daha fazlası.
1.   **Taban sınıf kitaplığı (BCL) inanılmaz** – Xamarin uygulamaları .NET BCL kullanır, kapsamlı ve kolaylaştırılmış sınıfları yoğun bir koleksiyonu yalnızca çok güçlü XML, veritabanı, seri hale getirme, g/ç, dize ve ağ desteği gibi özellikleri yalnızca birkaçıdır. Ayrıca, var olan C# kodu, binlerce BCL içindeki zaten kapsamında olmayan şeyler sağlayacaktır kitaplıkları binlerce bağlı erişim sağlayan bir uygulamaları kullanmak için derlenebilir.
1.   **Modern tümleşik geliştirme ortamı (IDE)** – Xamarin Mac Mac OS x ve Windows'da Visual Studio için Visual Studio kullanır. Kod otomatik tamamlama, Gelişmiş bir proje ve çözüm yönetimi sistemi, kapsamlı proje Şablon kitaplığı, tümleşik kaynak denetimi ve diğer birçok gibi özellikler dahil her iki modern IDE bunlar.
1.   **Mobil platformlar arası destek** – Xamarin iOS, Android ve Windows Phone üç önemli mobil platformlar için Gelişmiş platformlar arası destek sunar. Uygulamaları % 90 kendi kod paylaşmak için yazılabilir ve bizim Xamarin.Mobile kitaplığı üç tüm platformlarda ortak kaynaklara erişmek için birleştirilmiş bir API sunar. Bu önemli ölçüde geliştirme maliyetleri ve zamanının kısaltırken mobil geliştiriciler için hedef üç en popüler mobil platformları azaltabilir.


Xamarin'ın güçlü ve kapsamlı özellik kümesi nedeniyle void modern dil ve platform platformlar arası mobil uygulamaları geliştirmek için kullanmak istediğiniz uygulama geliştiricileri için doldurur.


> [!NOTE]
> Bu Başlarken serisinin başlatılan yapı iOS ve Android uygulamaları alma odaklanmıştır. Microsoft, Windows Phone geliştirme öğreticileri sunar [burada](http://dev.windowsphone.com/en-us/develop). (UWP uygulamaları için Windows dahil) xamarin'le platformlar arası geliştirme hakkında daha fazla bilgi edinmek için okuma [platformlar arası uygulamalar oluşturma Kılavuzu](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).



## <a name="how-does-xamarin-work"></a>Nasıl yapılır Xamarin iş?

Xamarin iki ticari ürün sunar: Xamarin.iOS ve Xamarin.Android. Bunların her ikisi de üstünde oluşturulmuş *Mono*, açık kaynak sürümü .NET Framework'ün yayımlanan .NET ECMA standartlarını temel. Mono geçici .NET framework kendisi ve Linux, Unix, FreeBSD ve Mac OS X dahil olmak üzere hayal neredeyse her platformda çalışır uzun neredeyse olmuştur.

İOS, Xamarin's *zaman Ahead* ( *Uygulama Nesne AĞACI*) derleyici Xamarin.iOS uygulamaları doğrudan yerel ARM bütünleştirilmiş kodu derler. Android, Xamarin'ın derleyici aşağı derler. *Ara dile* ( *IL*), ardından olduğu *zaman içinde sadece* ( *JIT*) için derlenmiş uygulama başlatıldığında yerel derleme.

Her iki durumda da, Xamarin uygulamaları otomatik olarak bellek ayırma, atık toplama, temel alınan platform birlikte çalışabilirliği vb. gibi şeyler işleyen bir çalışma zamanı kullanın.



### <a name="xamariniosdll-and-monoandroiddll"></a>Xamarin.iOS.dll and Mono.Android.dll

Xamarin uygulamaları bir alt kümesini Xamarin mobil profili bilinen .NET BCL karşı yerleşik olarak bulunur. Bu profili mobil uygulamaları için özel olarak oluşturulan ve MonoTouch.dll ve Mono.Android.dll paketlenmiş (iOS ve Android için sırasıyla). Yol Silverlight (ve Moonlight) uygulamaları karşı Silverlight/Moonlight .NET profili oluşturulur bu çok gibidir. Aslında, Xamarin mobil profili bir grup geri eklenen BCL sınıflar ile Silverlight 4.0 profili eşdeğerdir.

Kullanılabilir derleme ve sınıf tam listesi için bkz: [Xamarin.iOS derleme listesi](~/cross-platform/internals/available-assemblies.md) ve [Xamarin.Android derleme listesi](~/cross-platform/internals/available-assemblies.md)

BCL yanı sıra bu .dll sarmalayıcıları neredeyse tüm iOS için çağrılacak Temel SDK'sı API'lerini doğrudan C# ' dan sağlayan Android SDK ve SDK'sını içerir.



### <a name="application-output"></a>Uygulama çıktısı

Xamarin uygulamaları derlendiğinde bir .app dosyasında iOS veya Android .apk dosyasında bir uygulama paketi sonucudur. Bu dosyalar platformun varsayılan IDE yerleşik uygulama paketlerinden ayırt ve tam aynı şekilde dağıtılabilir.



## <a name="getting-started"></a>Başlarken

Şimdi, öğrenilen biraz Xamarin nasıl çalıştığı hakkında daha yakından inceleyin zamanı geldi!

Bu kılavuzlar birini kullanarak uygulama oluşturmaya başlamak için bir sonraki adım olacaktır:

* [**Merhaba, iOS**](~/ios/get-started/hello-ios/index.md)

![](introduction-to-mobile-development-images/ios.png "Merhaba, iOS")


* [**Hello, Android**](~/android/get-started/hello-android/index.md)

![](introduction-to-mobile-development-images/android.png "Hello, Android")


* [**Xamarin.Forms giriş**](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)





## <a name="summary"></a>Özet

Bu belgede yalnızca Xamarin platform anlatılmıştır. Yukarı ve çalışan ilk uygulamanızı alma gerçek eğlenceli başlar. Kullanıma [Hello, iOS](~/ios/get-started/hello-ios/index.md), [Hello, Android](~/android/get-started/hello-android/index.md), ve [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) başlamak için Kılavuzlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, Android](~/android/get-started/hello-android/index.md)
