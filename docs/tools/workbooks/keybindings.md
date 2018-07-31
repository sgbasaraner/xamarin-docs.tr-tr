---
title: Xamarin çalışma kitapları Düzenleyici klavye kısayolları
description: Bu belge, klavye kısayollarını Xamarin Workbooks Düzenleyicisi'nde kullanıma açıklar. Özellikle, dönüş anahtar kullanılır çeşitli yollardan görünüyor.
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: c2b4a8c1bcb8f7b88ab2ae1e2906b1c9c702b76a
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351684"
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin çalışma kitapları Düzenleyici klavye kısayolları

## <a name="the-return-key-and-its-nuances"></a>Dönüş anahtar ve kendi küçük farklar

Aşağıdaki tabloda, kod yürütme ile markdown yazma için çeşitli tuş bağlamaları açıklanmaktadır. Biz, tanıdık ve akıcı, duyarlı ve tutarlı anahtar bağlamalarını seçmek için çok dikkatli yönlendirdik.

|Tuş bağlama|Kodu hücreyi|Markdown hücresi|
|--- |--- |--- |
|<kbd>döndürülecek</kbd>|<p>Giriş işaretini hücre arabellek sonunda ise ve hücre başarıyla ayrıştırılabilir yürütülür ve arabellek sonuçları görüntülenir ve yeni bir kod hücresi eklenir ve yürütülen hücreden sonra hücre odaklanır.</p><p>Ayrıştırma başarılı değilse, yeni bir satır arabelleğe eklenir. Ayrıştırma başarılı değilse, derleyici tanılaması üretilen değil.</p>|<p><kbd>Dönüş</kbd> giriş işaretini Markdown bağlamında bağlı olarak farklı bir davranış gösteriyor.</p><ul><li>Giriş işaretini bir Markdown kod bloğu içinde ise, değişmez değer yeni bir satır eklenir.</li><li>Giriş işaretini bir Markdown listesi bloğu içinde ise, yeni bir liste öğesi oluşturun veya geçerli liste öğesinin bölün.</li><li>Giriş işaretini Markdown blok herhangi başka bir tür içinde ise, yeni bir paragraf bloğu oluşturun veya geçerli bloğu bölün.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>Win</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>Ayrıştırma ve yürütme hücre içeriğini her zaman çalışır. Derleme başarılı olursa, arabellek (yürütme özel durumlar dahil olmak üzere) sonuçları görüntülenir ve sonraki hücre varsa, yeni bir tane oluşturulabilir odaklı ve.</p><p>Herhangi bir derleme hatası varsa, tanılama görüntülenir ve arabellek şapka değişmeden odaklanmış kalır.</p>|Ekler ve yeni bir kod hücresi, geçerli markdown hücreden sonra odaklanır.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Win</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|Ekler ve sonra geçerli hücreyi yeni bir markdown hücresi odaklanır.|Aynı davranışı <kbd>döndürür</kbd>|
|<kbd>Shift‑Return</kbd>|Her zaman giriş işareti konumunu veya içeriğini bağımsız olarak, yeni bir satır ekler.|Geçerli bir Markdown blok içinde bir sabit satır sonu ekler.|
