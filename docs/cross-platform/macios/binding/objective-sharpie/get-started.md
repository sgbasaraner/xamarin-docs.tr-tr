---
title: Amaç Sharpie ile çalışmaya başlama
description: Bu belgenin amacı Sharpie, C# bağlamaları Objective-C kodunun oluşturulmasını otomatik hale getirmek için kullanılan araç yüksek düzeyli bir genel bakış sağlar.
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: da8c51c4ba4df74afac950bbff867221e7307d6e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854785"
---
# <a name="getting-started-with-objective-sharpie"></a>Amaç Sharpie ile çalışmaya başlama

> [!IMPORTANT]
> Amaç Sharpie Gelişmiş bilgi Objective-c (ve uzantılarının, C) ile deneyimli bir Xamarin geliştiricilerine yönelik bir araçtır. Objective-C kitaplığını bağlama çalışmadan önce komut satırı (ve yerel kitaplığı nasıl çalıştığını iyi anlamış) yerel kitaplık oluşturmak nasıl sağlam bilgiye sahip olmalıdır.

<a name="installing" />

## <a name="installing-objective-sharpie"></a>Yükleme hedefi Sharpie

Amaç Sharpie şu anda bir tek başına komut satırı aracı için Mac OS X 10.10 sürümünü ve ve _değil tam olarak desteklenen bir Xamarin ürün_. Bunu yalnızca ileri düzey geliştiriciler tarafından Objective-C kitaplığını 3 tarafa bağlama proje oluşturma sürecinde yardımcı olmak için kullanılmalıdır.

Amaç Sharpie standart OS X Paket Yükleyici indirilebilir.
Yükleyiciyi çalıştırın ve tüm Yükleme Sihirbazı'ndan ekrandaki ister:

- **Geçerli sürüm: 3.4**
  - [En son sürümünü indirin](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Forum Duyurusu](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> Kullanım `sharpie update` en son sürüme güncelleştirmek için komutu.

## <a name="basic-walkthrough"></a>Temel gözden geçirme

Oluşturmanıza yardımcı olur Xamarin tarafından bir 3 taraf Objective-C kitaplığını C# ' tan bağlamak için tanımları gereken sağlanan hedef Sharpie komut satırı aracıdır.
Hedefi Sharpie, geliştirici kullanırken bile *olacak* araç tarafından otomatik olarak işlenemedi sorunları gidermek için hedefi Sharpie bittikten sonra oluşturulan dosyalar değiştirmeniz gerekir.

Mümkünse, hedefi Sharpie API'leri ile olan bazı şüpheli nasıl düzgün bir şekilde bağlamak açıklama ekleyeceksiniz (yerel kodda birçok yapıları belirsizdir).
Bu ek açıklamaları görünür [ `[Verify]` öznitelikleri](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

Hedefi Sharpie çıktı dosyaları - çiftidir [ `ApiDefinition.cs` ve `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -Xamarin uygulamaları kullanabileceğinizi hangi derlerken bir kitaplığa bir bağlama projesi oluşturmak için kullanılabilir.

> [!IMPORTANT]
> Amaç Sharpie ile birlikte gelen **ana** kuralı için uygun Kullanım: kesinlikle, doğru clang derleyici komut satırı bağımsız değişkenlerini ayrıştırma uygun emin olmak için geçirmeniz gerekir. Yalnızca bir aracı aşaması ayrıştırma hedefi Sharpie olduğundan budur [clang libtooling API karşı uygulanan](http://clang.llvm.org/docs/LibTooling.html).

Bu, hedefi Sharpie Clang (bağlamak yerel kitaplık derleyen C/Objective-C/C++ derleyicisi) ve tüm iç bildiğini bağlama için üst bilgi dosyaları tam gücünden olduğu anlamına gelir.
Ayrıştırılmış çevirme yerine [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) nesne kodu için hedefi Sharpie çeviren bir C# bağlama "iskele" girişi için uygun için AST'yi `bmac` ve `btouch` Xamarin bağlama araçları.

Hedefi Sharpie hataları Ayrıştırma sırasında sırasında hata verme, clang geldiğini varsa nedenini anlamak ihtiyacınız ve ayrıştırma, aşama AST'yi oluşturmak çalışıyor.

**YENİ!** sürüm 3.0 denemeleri Bu karmaşıklığı bazıları, Xcode projeleri doğrudan destekleyerek adresi. Yerel bir kitaplığı geçerli bir Xcode projesi varsa, proje için belirtilen hedef ve gerekli giriş üst bilgi dosyaları ve derleyici bayraklarına çıkarmaya yapılandırma hedefi Sharpie değerlendirebilirsiniz.

Xcode proje varsa, doğru giriş üstbilgi dosyalarında, üstbilgi dosyası arama yollarını ve diğer gerekli derleyici bayraklarına deducing tarafından proje ile daha fazla bilgi sahibi olmanız gerekir. Yerel kitaplık oluşturmak için kullanılan derleyici bayrakları için hedefi Sharpie geçirilmelidir aynı olduğunu bilmeniz önemlidir. Daha fazla el ile ve yerel kod ile Clang araç zinciri komut satırında derleme ile aşinalık biraz gerektiren bir budur.

**YENİ!** sürüm 3.0 da kolayca bağlama için bir araç sunar [CocoaPods](https://cocoapods.org) aracılığıyla `sharpie pod` komutu.
Bir CocoaPod ilgilendiğiniz kitaplığı varsa, (kaynak karşı doğrudan bağlanmaya karşı) CocoaPod hedefi Sharpie ile bağlama deneyerek Başlat öneririz.

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)