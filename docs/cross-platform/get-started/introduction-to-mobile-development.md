---
title: Mobil geliştirmeye giriş
description: Bu belgede mobil geliştirme, Xamarin, nasıl çalıştığını ve bu çıkışları uygulamalarını açıklayan bir giriş sağlar.
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
author: asb3993
ms.author: amburns
ms.date: 03/28/2017
ms.openlocfilehash: f3b1f5c11a02710de8d0ffd09741acb3017f5cb6
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780586"
---
# <a name="introduction-to-mobile-development"></a>Mobil geliştirmeye giriş

Mobil uygulamalar oluşturmak, IDE'yi açılırken, bir şey birlikte oluşturma, test etme ve bir App Store – bir öğleden sonra gerçekleştirilen tüm gönderme hızlı bir bit yapmak kadar kolay olabilir. Veya sıkı Ön tasarım, kullanılabilirlik, cihazları, bir tam beta yaşam döngüsü ve ardından dağıtım binlerce üzerinde bir dizi farklı yolla testi QA sınamaları içeren son derece ilgili bir işlem olabilir.

Bu belge, Xamarin platformunu tanıtıyor için tasarlanmıştır. Hakkında daha fazla bilgi edinmek için *işlem* test etmek için mobil uygulamaları tasarımdan oluşturma, başvurmak [mobil yazılım geliştirme yaşam döngüsü giriş](~/cross-platform/get-started/introduction-to-mobile-sdlc.md) belge.

Lütfen bizim [sistem gereksinimleri](~/cross-platform/get-started/requirements.md#macos-requirements) Xamarin yükleyebilmesi için o onaylamak için.

## <a name="introduction-to-xamarin"></a>Xamarin giriş

İOS ve Android uygulamaları oluşturmak nasıl düşünürken birçok kişi yerel diller, Objective-C, Swift ve Java, tek seçeneğiniz olduğunu düşünün. Ancak, geçtiğimiz birkaç yılda, yeni ekosistemi tüm mobil uygulamalar oluşturmaya yönelik platformlar ortaya.

Xamarin benzersiz olduğundan bu alanda tek bir dili – C# sınıf kitaplığı ve tüm üç mobil platformlar, iOS, Android ve Windows Phone üzerinde çalışan çalışma zamanı sunarak (Windows Phone ' ın yerel dili olan zaten C#), yine de, yerel (derleme sırasında yeterli performansa sahip olan, olmayan-yorumlanır) uygulamaları, oyunlar zorlu için bile.

Bu platformların her biri farklı bir özellik kümesine sahiptir ve her yerel uygulamaları – diğer bir deyişle, yazma yeteneğini yerel kod ve temel alınan Java alt birlikte yerlere söz konusu çalışma derleme uygulamalar farklılık gösterir. Örneğin, bazı platformlarda, yalnızca HTML ve JavaScript, bazı çok düşük düzeyli ve yalnızca C/C++ kodu izin derlenmesi uygulamalara izin. Bazı platformlarda bile yerel Denetim Araç Seti yazılımınız yok.

Xamarin, tüm yerel platformları gücünü bir araya getirir ve kendi dahil olmak üzere güçlü özellikler ekler benzersizdir:

1.   **Temel alınan SDK'ları için bağlama tamamlamak** – bağlamaları neredeyse tüm temel platform SDK'ları hem iOS hem de Android için Xamarin içerir. Ayrıca, bu bağlamaları, türü kesin olarak belirtilmiş, güçlü derleme zamanı türü denetimi ve geliştirme sırasında sağlayın gidin ve kullanmak kullanımı kolay temaların anlamına gelir. Bu, daha az çalışma zamanı hataları ve yüksek kaliteli uygulamalar yol açar.
1.   **Objective-C, Java, C ve C++ birlikte çalışması** : Xamarin doğrudan çağırma Objective-C, Java, C olanakları sağlar ve C++ kitaplıkları, 3. taraf çeşit kullanılacak faaliyetlerine kodu zaten oluşturuldu. Bu, var olan iOS ve Android kitaplıkları Objective-C, Java veya C/C++ ile yazılmış yararlanmanızı sağlar. Ayrıca, Xamarin, bir bildirim temelli söz dizimi kullanılarak yerel Objective-C ve Java kitaplıkları kolayca bağlamanıza olanak tanıyan bağlama projeleri sunar.
1.   **Modern dil yapıları** – Xamarin uygulamaları C# ile yazılan, Objective-C ve Java önemli geliştirmeler gibi içeren bir modern dilde *dinamik dil özellikleri* ,  *İşlevsel yapıları* gibi *Lambdalar* , *LINQ* , *paralel programlama* özellikleri, Gelişmiş *genel türler*  ve daha fazlası.
1.   **Temel sınıf kitaplığı (BCL) şaşırtıcı** – Xamarin uygulamaları .NET BCL kullanın, kapsamlı ve kolaylaştırılmış sınıf büyük koleksiyonu yalnızca çok güçlü XML, veritabanı, serileştirme, GÇ, dize ve ağ desteği gibi özellikleri birkaçıdır. Buna ek olarak, var olan C# kodu zaten BCL içindeki kapsamında olmayan bir şeyler yapmanızı sağlayacak kitaplıkları binlerce üzerine binlerce erişim sağlayan bir uygulamalarda kullanmak için derlenebilir.
1.   **Modern tümleşik geliştirme ortamı (IDE)** – Xamarin Mac OS X üzerinde Mac ve Windows üzerinde Visual Studio için Visual Studio kullanır. Bu kod otomatik tamamlama, Gelişmiş bir proje ve çözüm yönetim sistemi, kapsamlı bir proje şablonu kitaplığı, tümleşik kaynak denetimi ve diğer birçok gibi özellikler dahil her iki modern IDE'ler vardır.
1.   **Mobil platformlar arası destek** – Xamarin iOS, Android ve Windows Phone üç önemli mobil platformlara yönelik gelişmiş platformlar arası destek sunar. Uygulamalar, en fazla %90 kendi kod paylaşmak için yazılabilir ve Xamarin.Mobile kitaplığımızı üç tüm platformlarda ortak kaynaklara erişmek için birleştirilmiş bir API sunar. Bu önemli ölçüde geliştirme maliyetlerini hem zaman pazarlama mobil uygulama geliştiricileri için hedef üç en popüler mobil platformları azaltabilir.


Xamarin'in güçlü ve kapsamlı özellik kümesi nedeniyle bir modern dil ve platformu, platformlar arası mobil uygulamalar geliştirmek için kullanmak istediğiniz uygulama geliştiricileri için bir geçersiz kılma doldurur.


> [!NOTE]
> Bu Başlarken serisinin oluşturmaya başlamak iOS ve Android uygulamaları kurmaya odaklanır. Microsoft hakkında bilgi sunar [Evrensel Windows Platformu (UWP) geliştirme](https://docs.microsoft.com/windows/uwp/develop/) Tablet ve masaüstü bilgisayarlar için. (UWP uygulamaları için Windows dahil) Xamarin ile platformlar arası geliştirme hakkında daha fazla bilgi edinmek için [platformlar arası uygulamalar oluşturma Kılavuzu](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).



## <a name="how-does-xamarin-work"></a>Xamarin iş nasıl yapar?

Xamarin iki ticari ürün sunar: Xamarin.iOS ve Xamarin.Android. Bunlar hem üst kısmındaki tasarlanmaları *Mono*, yayımlanan .NET ECMA standartlarına göre bir açık kaynak sürümü .NET Framework'ün temel. Mono geçici .NET framework kendisi ve Linux, Unix, FreeBSD ve Mac OS X dahil olmak üzere neredeyse tüm hayal platformunda çalışan uzun neredeyse olmuştur.

İOS, Xamarin's *zaman Ahead* ( *AOT*) derleyici Xamarin.iOS uygulamalarını doğrudan yerel ARM bütünleştirilmiş kodu derler. Android, Xamarin derleyici aşağı derler. *Ara dil* ( *IL*), ardından olduğu *Just-ın-Time* ( *JIT*) için derlenmiş uygulama başlatıldığında yerel derleme.

Her iki durumda da, bellek ayırma, çöp toplama, temel alınan platformu birlikte çalışabilirliği, vb. gibi şeyler otomatik olarak işleyen bir çalışma zamanı Xamarin uygulamaları kullanın.



### <a name="xamariniosdll-and-monoandroiddll"></a>Xamarin.iOS.dll ve Mono.Android.dll

Xamarin uygulamaları, .NET Xamarin mobil profili bilinen BCL kümesini karşı oluşturulur. Bu profil mobil uygulamaları için özel olarak oluşturulan ve MonoTouch.dll ve Mono.Android.dll paketlenmiş (iOS ve Android için sırasıyla). Silverlight/Moonlight .NET profili karşı yerleşik Silverlight (ve Moonlight) uygulamaların bu çok gibidir. Aslında, Xamarin mobil profili BCL sınıfları geri eklenen bir sürü ile Silverlight 4.0 profiline eşdeğerdir.

Kullanılabilir derlemeler ve sınıfları tam listesi için bkz [Xamarin.iOS derleme listesi](~/cross-platform/internals/available-assemblies.md) ve [Xamarin.Android derleme listesi](~/cross-platform/internals/available-assemblies.md)

Bu DLL'ler BCL yanı sıra sarmalayıcıları neredeyse tüm iOS için SDK'sı ve çağrılacak Temel SDK API'lerini doğrudan C# ' tan sağlayan Android SDK içerir.



### <a name="application-output"></a>Uygulama çıktısı

Xamarin uygulamaları derlendiğinde bir .app dosyasında iOS veya Android .apk dosyası, bir uygulama paketi sonucudur. Bu dosyalar, platformun varsayılan IDE ile yerleşik uygulama paketleri döndürsün ve tam aynı şekilde dağıtılabilir.



## <a name="getting-started"></a>Başlarken

Artık öğrendiğiniz biraz Xamarin nasıl çalıştığı hakkında kolları sıvayın zamanı geldi!

Bu kılavuzlar birini kullanarak uygulama oluşturmaya başlamak için bir sonraki adım olacaktır:

* [**Hello, iOS**](~/ios/get-started/hello-ios/index.md)

![](introduction-to-mobile-development-images/ios.png "Hello, iOS")


* [**Hello, Android**](~/android/get-started/hello-android/index.md)

![](introduction-to-mobile-development-images/android.png "Hello, Android")


* [**Xamarin.Forms'a giriş**](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)





## <a name="summary"></a>Özet

Bu belgede yalnızca Xamarin platformunu kullanıma sundu. İlk uygulamanızı yukarı süreli aldığınızda gerçek eğlenceli başlatır. Kullanıma [Hello, iOS](~/ios/get-started/hello-ios/index.md), [Hello, Android](~/android/get-started/hello-android/index.md), ve [Xamarin.Forms'a giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) başlamak için Kılavuzlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, Android](~/android/get-started/hello-android/index.md)
