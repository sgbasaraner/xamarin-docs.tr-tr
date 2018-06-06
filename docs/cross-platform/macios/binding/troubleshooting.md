---
title: Sorun giderme bağlama
description: Bu kılavuzda Objective-C Kitaplığı bağlama güçlük çekiyorsanız yapmanız gerekenler açıklanmaktadır. Özellikle, eksik bağlamaları, bağlama ve raporlama hatalar null geçirilirken bağımsız değişken özel durumlarını açıklar.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: b0ce6c3792d6495cabdf8acdab1a644fe75f1219
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780241"
---
# <a name="binding-troubleshooting"></a>Sorun giderme bağlama

Bağlamaları macOS (önceki adıyla OS X olarak bilinir) için sorun giderme için bazı ipuçları Xamarin.Mac API'leri.

## <a name="missing-bindings"></a>Bağlamaları eksik

Bazen Xamarin.Mac Apple API'leri çoğunu kapsarken, bir bağlaması yok bazı Apple API çağrısı gerekebilir henüz. Diğer durumlarda, üçüncü taraf C/Objective-C çağırmanız gerekir. Bu BT Xamarin.Mac bağlamaları kapsamı dışında.

Bir Apple API'si ile çalışıyorsanız, ilk adım, bir bölümü API kapsamı için henüz sahip olmayan devreyi olduğunu bilmeniz Xamarin kurmanızı sağlamaktır. [Dosyalama](#reporting-bugs) eksik API belirtmeye. Raporları müşterilerden biz sonraki iş hangi API'ları önceliklendirmek için kullanırız. Ayrıca, bir iş veya Enterprise lisansı varsa ve bir bağlama bu eksiklik ilerleme durumunuzu engelleme, ayrıca bölümündeki yönergeleri izleyin [Destek](http://xamarin.com/support) bir bilet dosyasına. Bir bağlama etmeyeceğiz olamaz, ancak bazı durumlarda biz iş alabileceğiniz.

Xamarin (varsa), eksik bağlamasının bildiren sonra sonraki adıma kendiniz bağlama ise. Tam bir kılavuz sahibiz [burada](~/cross-platform/macios/binding/overview.md) ve resmi olmayan bazı belgelerde [burada](http://brendanzagaeski.appspot.com/xamarin/0002.html) el ile Objective-C bağlamaları kaydırma için. Bir C API arıyorsanız, C# ' ın P/Invoke mekanizması kullanabilirsiniz, belgeleri [burada](http://www.mono-project.com/docs/advanced/pinvoke/).

İş karar verirseniz bağlamada kendiniz dikkat bağlama yanlışlıklar ilginç çökme (Crash) yerel çalışma zamanında her türlü üretebilir. Özellikle, C# imza bağımsız değişken sayısı ve her bağımsız boyutunu yerel imzada eşleştiğini çok dikkatli olun. Bunun Sağlanamaması, bellek ve/veya yığın bozulmasına neden olabilir ve hemen veya rastgele bir noktada gelecekte kilitlenme veya veri bozulmasına neden.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Bir bağlama null geçirilirken bağımsız değişken özel durumlar

Xamarin yüksek sağlamak için çalışırken kalite ve Apple API'ları için de sınanan bağlamaları bazen hatalar ve notu içindeki hataları. Bir API uygulamasına çalışabilir göre ne kadar yaygın sorun olan atma `ArgumentNullException` geçirdiğiniz zaman null olarak temel API kabul ettiğinde `nil`. API genellikle tanımlama yerel üstbilgi dosyaları API'leri nil kabul etmek ve içinde geçirirseniz, kilitleniyor yeterli bilgi sağlamaz.

Bir durumuna çalıştırırsanız tümleştirilmesidir burada `null` oluşturur bir `ArgumentNullException` ancak düşündüğünüz çalışması, şu adımları izleyin:

1. Apple belgeleri ve/veya kabul ettiği kanıt bulursanız görmek için örnekleri denetleyin `nil`. Objective-C ile kullanabiliyorsanız doğrulamak için bir küçük test programı yazabilirsiniz.
2. [Dosyalama](#reporting-bugs).
3. Hatanın çalışabilir mi? API ile çağırma önleyebilirsiniz varsa `null`, basit bir null denetim çağrıları geçici geçici kolay bir çözüm olabilir.
4. Ancak, bazı API'leri kapatmak veya bazı özellikler devre dışı bırakmak için null bilgilerinde gerektirir. Bu durumlarda, sorunu çözmek derleme tarayıcısını getirerek çalışabilirsiniz (bkz [C# üye için sağlanan Seçici bulma](~/mac/app-fundamentals/mac-apis.md#finding_selector)), bağlama ve null denetimi kaldırma kopyalama. Lütfen kopyalanan bağlama güncelleştirmeler ve düzeltmeler Xamarin.Mac vermiyoruz almazsınız gibi bunu ve bu geçici bir kısa vadeli çözüm düşünülmesi gereken hatayı (2. adım) dosyasına emin olun.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Raporlama hataları

Görüşleriniz bizim için önemlidir. Xamarin.Mac herhangi bir sorun bulursanız:

- Denetleme [Xamarin.Mac forumları](https://forums.xamarin.com/categories/mac)
- Arama [sorunu deposu](https://github.com/xamarin/xamarin-macios/issues) 
- GitHub konulara geçmeden önce Xamarin sorunları üzerinde izlenmekte olan [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Lütfen var. sorunları eşleştirmek için arama yapın.
- Eşleşen bir sorunu bulamazsanız, Lütfen yeni bir sorun dosya [GitHub sorunu depo](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub sorunlar tüm ortak verilmektedir. Yorumlar veya ekleri gizlemek mümkün değildir. 

Lütfen aşağıdaki mümkün olduğunca çok şunları içerir:

- Sorunu yeniden oluşturma, basit bir örnek. Bu **çok** mümkün olduğunda. 
- Kilitlenme tam yığın izleme.
- Kilitlenme çevreleyen C# kod. 
