---
title: "Başlarken"
ms.topic: article
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 53e64acd8e64c9a8151b2c55045db4e308f97531
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="getting-started"></a>Başlarken

> [!IMPORTANT]
> Amaç Sharpie Gelişmiş bilgi Objective-C (ve uzantılarının, C) ile deneyimli Xamarin geliştiriciler için bir araçtır. Objective-C Kitaplığı bağlamak denemeden önce yerel kitaplığı komut satırı (ve yerel kitaplığı nasıl çalıştığını iyi anlamış) oluşturmak düz bilgiye sahip olmalıdır.

<a name="installing" />

## <a name="installing-objective-sharpie"></a>Amaç Sharpie yükleme

Amaç Sharpie şu anda Mac OS X 10.10 ve daha yeni bir tek başına komut satırı aracı ve _değil tam olarak desteklenen bir Xamarin ürün_. Bunu yalnızca Gelişmiş geliştiriciler tarafından Objective-C Kitaplığı bir 3. taraf için bir bağlama proje oluşturmanıza yardımcı için kullanılmalıdır.

Amaç Sharpie standart bir işletim sistemi X paketi yükleyicisi indirilebilir.
Yükleyiciyi çalıştırın ve tüm izleyin ekran Yükleme Sihirbazı'ndan ister:

- **Geçerli sürüm: 3.4**
  - [En son sürümü karşıdan yükle](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Forum Duyurusu](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> Kullanım `sharpie update` en son sürüme güncelleştirmek için komutu.

## <a name="basic-walkthrough"></a>Temel gözden geçirme

Amaç Sharpie oluşturmanıza yardımcı olur Xamarin tanımları 3 taraf Objective-C Kitaplık C# bağlamak için gerekli bir komut satırı aracı sağlanır.
Hedefi Sharpie, geliştirici kullanırken bile *olacak* otomatik olarak aracı tarafından işlenemedi sorunlarını gidermek üzere hedefi Sharpie bittikten sonra oluşturulan dosyalar değiştirmeniz gerekir.

Mümkünse, hedefi Sharpie API'leri ile sahip olduğu bazı şüpheli düzgün bağlama hakkında açıklama ekleyeceksiniz (yerel kodda birçok yapıları belirsiz).
Bu ek açıklamaları olarak görünür [ `[Verify]` öznitelikleri](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

Hedefi Sharpie çıktı dosyaları - çiftidir [ `ApiDefinition.cs` ve `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -hangi derlerken bir kitaplığa Xamarin uygulamaları kullanabilirsiniz bağlama projesi oluşturmak için kullanılabilir.

> [!IMPORTANT]
> Amaç Sharpie biriyle gelen **ana** uygun kullanım için kuralı: kesinlikle doğru clang derleyici komut satırı bağımsız değişkenleri uygun ayrıştırma emin olmak için geçirdiğiniz gerekir. Aşama ayrıştırma hedefi Sharpie yalnızca bir aracı olmasıdır [clang libtooling API karşı uygulanan](http://clang.llvm.org/docs/LibTooling.html).

Bu, hedefi Sharpie Clang (gerçekten bağlamak yerel kitaplığı derler C/Objective-C/C++ derleyicisi) ve tüm iç bildiğini bağlama için üstbilgi dosyaları tam güç olduğu anlamına gelir.
Ayrıştırılmış çevirme yerine [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) nesne kodu hedefi Sharpie çevirir için C# bağlama "iskele" giriş için uygun AST `bmac` ve `btouch` Xamarin bağlama araçları.

Hedefi Sharpie hataları Ayrıştırma sırasında bu hata verdi, çıkışı sırasında bu clang anlamına gelir, kendi ayrıştırma aşama AST oluşturmaya çalışılırken ve nedenini bulmak gerekiyor.

**YENİ!** sürüm 3.0 denemeleri bu karmaşıklık bazıları Xcode projeleri doğrudan destekleyerek adres. Yerel bir kitaplığın geçerli bir Xcode projesi varsa, hedefi Sharpie proje için belirtilen hedef ve gerekli giriş üstbilgi dosyaları ve derleyici bayrakları türetme yapılandırma değerlendirebilirsiniz.

Xcode proje varsa, doğru giriş üstbilgi dosyaları, üstbilgi dosyası arama yolları ve diğer gerekli derleyici bayraklar deducing proje ile daha fazla bilgi sahibi olmanız gerekir. Yerel kitaplığı oluşturmak için kullanılan derleyici bayrakları hedefi Sharpie geçirilmelidir aynı olduğunu bilmeniz önemlidir. Daha fazla el ile ve Clang zincirinin ile komut satırında yerel kodu derleme ile benzerlik biraz gerektiren bir budur.

**YENİ!** sürüm 3.0 ayrıca kolayca bağlama için bir araç sunar [CocoaPods](https://cocoapods.org) aracılığıyla `sharpie pod` komutu.
İlgilendiğiniz kitaplığı CocoaPod kullanılabilir durumdaysa, hedefi Sharpie ile CocoaPod (kaynağına karşı doğrudan bağlama girişimi karşı) bağlamak deneyerek Başlat öneririz.
