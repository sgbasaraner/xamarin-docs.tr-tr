---
title: Yerel başvuruları
description: Yerel başvuruları yerel Framework bir Xamarin.iOS veya Xamarin.Mac veya bağlama projesi ekleme olanağı sağlar.
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: cd271ad68e5edb06158f7d595395ddcd0bcfbdce
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="native-references"></a>Yerel başvuruları

_Yerel başvuruları yerel Framework bir Xamarin.iOS veya Xamarin.Mac veya bağlama projesi ekleme olanağı sağlar._


İOS 8.0 bu yana uygulama uzantıları xcode'da ana uygulama arasında kod paylaşmak için katıştırılmış bir çerçeve oluşturmak mümkün olmuştur. Yerel başvurusu özelliğini kullanarak (Xcode ile oluşturulan) Bu katıştırılmış çerçeveler Xamarin.iOS kullanmak mümkün olacaktır.
 
> [!IMPORTANT]
> Herhangi bir Xamarin.iOS ya da Xamarin.Mac projeleri türünden katıştırılmış çerçeveler oluşturmak mümkün olmaz, yerel başvuruları yalnızca var olan yerel (Objective-C) çerçeveleri tüketimi için izin verir.




<a name="Terminology" />

## <a name="terminology"></a>Terminoloji

İOS 8 (ve üstü), içinde **katıştırılmış çerçeveler** olabilir hem de katıştırılmış statik olarak bağlantılı ve dinamik olarak bağlı çerçeveler. Düzgün bir şekilde dağıtmak için tüm dahil "fat" çerçeveleri yapmanız gerekir, _dilimler_ ile uygulamanızı desteklemek istediğiniz her cihaz mimari.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>Statik vs. Dinamik çerçeveler

**Statik çerçeveler** derleme zamanında bağlı olduğu **dinamik çerçeveler** çalışma zamanında bağlı ve yeniden bağlama olmadan therefor değiştirilebilir. İOS 8 önce tüm 3. taraf Framework kullandıysanız, kullanmakta olduğunuz bir **statik Framework** uygulamanıza derlenmiş. Apple'nın bkz [kitaplığı dinamik programlama](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) daha fazla ayrıntı için belgeleri.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>Katıştırılmış vs. Sistem çerçeveler

**Çerçeveler katıştırılmış** , uygulamaları pakete eklenen ve yalnızca belirli uygulamanıza, korumalı alan aracılığıyla erişilebilir. **Sistem çerçeveler** işletim sistemi düzeyinde depolanır ve cihazdaki tüm uygulamalar için kullanılabilir. Şu anda yalnızca Apple işletim sistemi düzeyinde çerçeveleri oluşturma olanağı vardır.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>İnce vs. FAT çerçeveler

**İnce çerçeveler** yalnızca belirli sistem mimarisi için derlenmiş kod içeren nerede **Fat çerçeveler** birden fazla mimari için kod içerir. Bir çerçeve derlenmiş her mimari özgü codebase olarak adlandırılır bir _dilim_. Şu iki iOS simülatörü mimarileri (i386 ve X86_64) derlenen bir çerçeve olsaydı, bu nedenle, örneğin, bu iki dilimler içerecektir.

Uygulamanız bu örnekle Framework dağıtmak denediyseniz, doğru Simulator'da çalıştırmak, ancak Framework bir iOS cihazı için kod özgü dilimleri içermediğinden cihazda başarısız. Bir çerçeve tüm örneklerde çalışacağından emin olmak için bu da arm64, armv7 ve armv7s gibi aygıta özgü dilimleri dahil etmek gerekir.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>Katıştırılmış çerçeveleri ile çalışma

Katıştırılmış çerçeveleri, bir Xamarin.iOS veya Xamarin.Mac uygulamasında çalışmak için tamamlanması gereken iki adımı vardır: Fat Framework oluşturma ve Framework katıştırma.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Fat Framework oluşturma

Yukarıda belirtildiği gibi katıştırılmış bir çerçeve, uygulamanızda kullanmak için bu, uygulamanızın üzerinde çalışacağı cihazlar için sistem yapılarındaki dilimler tümünü içeren bir Fat Framework olmalıdır.

Framework ve alıcı uygulamanın aynı Xcode projede olduğunda, Xcode Framework ve yapı ayarların aynısını kullanarak uygulama oluştururken bu bir sorun değildir. Xamarin uygulamaları katıştırılmış çerçeveleri oluşturulamıyor olduğundan, bu teknik kullanılamaz.

Bu sorunu çözmek için `lipo` komut satırı aracı, iki veya daha fazla çerçeveler bir Fat tüm gerekli dilimler içeren Framework'e birleştirmek için kullanılabilir. İle çalışma hakkında daha fazla bilgi için `lipo` komutu, lütfen bakın bizim [bağlama yerel kitaplıkları](~/ios/platform/native-interop.md) belgeleri.

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>Bir çerçeve katıştırma

İzleme adım yerel başvurular Xamarin.iOS veya Xamarin.Mac projede bir çerçeve katıştırmak için gereklidir:

1. Yeni bir oluşturun veya varolan bir Xamarin.iOS, Xamarin.Mac veya bağlama projesini açın.
2. İçinde **Çözüm Gezgini**, proje adına sağ tıklayın ve seçin **Ekle** > **yerel başvuru Ekle**: 

    [![](native-references-images/ref01.png "Çözüm Gezgini'nde proje adına sağ tıklayın ve yerel başvuru Ekle seçin")](native-references-images/ref01.png#lightbox)
3. Gelen **açık** iletişim kutusunda, eklemek ve istediğiniz yerel Framework adını seçin **açık** düğmesi: 

    [![](native-references-images/ref02.png "Yerel katıştırmak ve Aç düğmesini tıklatın Framework'ün adı seçin")](native-references-images/ref02.png#lightbox)
4. Framework projenin ağacına eklenecek: 

    [![](native-references-images/ref03.png "Framework projeleri ağacına eklenemez")](native-references-images/ref03.png#lightbox)

Projesi derlendiğinde yerel Framework uygulamanın pakette katıştırılır.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>Uygulama uzantıları ve katıştırılmış çerçeveleri

Xamarin.iOS Mono çalışma zamanı bir çerçeve olarak bağlamak için bu özelliğin avantajlarından dahili olarak alabilir (dağıtım hedef olduğunda > iOS 8.0 =), (Mono çalışma zamanı yalnızca bir kez için dahil edilecek beri böylece önemli ölçüde uzantılarına sahip uygulamalar için uygulama boyutunu azaltma tüm uygulama paketi, her bir uzantı için bir kez ve kapsayıcı uygulama için bir kez yerine).

Tüm uzantıları iOS 8.0 gerektirdiğinden uzantıları Mono çalışma zamanı ile bir çerçeve bağlayın.

Uzantılar ve uygulamalar bu hedef iOS olmayan uygulamalar 

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.iOS veya Xamarin.Mac uygulamasına yerel Framework katıştırma ayrıntılı bir bakış sürdü.

