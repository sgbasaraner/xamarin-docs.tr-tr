---
title: Sorun giderme bağlama
description: Bu kılavuzda bir Objective-C kitaplığını bağlama güçlük çekiyorsanız, yapmanız gerekenler açıklanmaktadır. Özellikle, eksik bağlamaları, bağlama ve raporlama hatalarıyla null geçerken bağımsız değişken özel durumlarını açıklar.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: aaceada84b151856506ede66907274e2457c23d4
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854811"
---
# <a name="binding-troubleshooting"></a>Sorun giderme bağlama

MacOS (eski adıyla OS X da bilinir) bağlama sorunlarını giderme için bazı ipuçları Xamarin.Mac API'leri.

## <a name="missing-bindings"></a>Bağlamaları eksik

Bazen Xamarin.Mac Apple API'leri çoğunu kapsar, ancak bir bağlaması yok bazı Apple API çağrısı gerekebilir henüz. Diğer durumlarda, üçüncü taraf C/Objective-C çağırmanız gerekir. böylece Xamarin.Mac bağlamaları kapsamı dışında.

Bir Apple API'si ile uğraşıyorsanız, ilk adım, API'yi bir bölümünü kapsamı için henüz sahip olmayan karşılaştınız demektir olduğunu bilmek Xamarin kurmanızı sağlamaktır. [Hata bildirin](#reporting-bugs) eksik API belirterek. Sonraki sayfada çalıştığımız hangi API'ler önceliğini belirlemek için müşterilerden gelen raporlar kullanın. Ayrıca, Business veya Enterprise lisansınız varsa ve bu bağlama eksikliği ilerlemenizi engelliyor, ayrıca konumundaki yönergeleri [Destek](http://xamarin.com/support) bilet dosya. Bir bağlama etmeyeceğiz olamaz, ancak bazı durumlarda size bir iş alabileceğiniz geçici bir çözüm.

Xamarin (varsa), eksik bağlamasının bildiren sonra kendiniz bağlama dikkate alınması gereken sonraki adım gelir. Tam Kılavuzu sunuyoruz [burada](~/cross-platform/macios/binding/overview.md) ve resmi olmayan bazı belgeler [burada](http://brendanzagaeski.appspot.com/xamarin/0002.html) Objective-C bağlama el ile sarmalama için. Bir C API'sini çağırıyorsanız, C# ' ın P/Invoke mekanizmasıyla kullanabilirsiniz, belgeleri [burada](http://www.mono-project.com/docs/advanced/pinvoke/).

İş karar verirseniz bağlamadaki kendiniz dikkat bağlama hatalar ilginç kilitlenmeleri yerel çalışma zamanında her türlü üretebilir. Özellikle, C# imza bağımsız değişken sayısı ve boyutu her bağımsız değişkeni, yerel imza eşleştiğini çok dikkatli olun. Bunun yapılmaması, bellek ve/veya yığın bozulmasına neden olabilir ve gelecekte hemen veya rastgele bir noktada kilitlenme veya verisi bozuk.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Bağımsız değişken özel durumlarını geçirirken bir bağlama için null

Xamarin yüksek sağlamak için çalışırken kalite Apple API'leri için de test edilmiş bağlamalar bazen hatalar ve irsaliyesi hataları. Bir API, karşılaşabileceğiniz arayla en yaygın sorun özel durum atma `ArgumentNullException` gönderdiğinizde null temel alınan API kabul ettiğinde `nil`. API genellikle tanımlama yerel üst bilgi dosyaları, API nil kabul edin ve içinde geçirirseniz, kilitlenir yeterli bilgi sağlamaz.

Bir durum çalıştırırsanız tümleştirilmesidir burada `null` oluşturur bir `ArgumentNullException` ancak düşünün çalışması, aşağıdaki adımları izleyin:

1. Apple belgeleri ve/veya kabul ettiği kavram bulup bulamayacağınızı öğrenmek için örnekler denetleyin `nil`. Objective-C ile kullanabiliyorsanız bunu doğrulamak için bir küçük test programı yazabilirsiniz.
2. [Hata bildirin](#reporting-bugs).
3. Hataya çalışabilir mi? İle API'yi çağırıp işlemini yapmamak mümkünse `null`, basit bir null kontrolü çağrıları etrafında bir kolayca geçici olabilir.
4. Ancak, bazı API'leri devre dışı bırakmak veya bazı özellikler devre dışı bırakmak için null geçirmeyi gerektirir. Bu gibi durumlarda, derleme tarayıcının taşıyarak bu sorunu geçici olarak çalışabilir (bkz [C# üyesi için belirtilen Seçici bulma](~/mac/app-fundamentals/mac-apis.md#finding_selector)), bağlama ve null denetimi kaldırma kopyalanıyor. (2. adım) hata gibi kopyalanan bağlama, güncelleştirmeler ve Xamarin.Mac'te vermiyoruz düzeltmeleri almazsınız. Bunu yapmak ve bu kısa vadeli bir geçici düşünülmesi gereken dosya emin olun.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Hata Raporlama

Geri bildiriminiz bizim için önemlidir. Xamarin.Mac herhangi bir sorun bulursanız:

- Denetleme [Xamarin.Mac forumları](https://forums.xamarin.com/categories/mac)
- Arama [sorunu depo](https://github.com/xamarin/xamarin-macios/issues) 
- GitHub sorunları için geçmeden önce Xamarin sorunları üzerinde izlenen [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Lütfen sorunları eşleştirmek için burada arayın.
- Eşleşen bir sorun bulamazsanız, Lütfen yeni bir konu dosya [GitHub sorunu deposu](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub sorunları ortaktır. Yorum veya ekler gizlemek mümkün değildir. 

Lütfen aşağıdaki mümkün olduğunca çok şunları içerir:

- Sorunu yeniden oluştururken, basit bir örnek. Bu **her** mümkün olduğunda. 
- Kilitlenme tam yığın izlemesi.
- Kilitlenme çevreleyen C# kod. 

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
