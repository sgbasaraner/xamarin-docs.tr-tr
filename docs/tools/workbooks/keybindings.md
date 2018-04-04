---
title: Xamarin çalışma kitaplarını Düzenleyicisi klavye kısayolları
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 62cd8fd546667643ca90098f63866ab1ca6c4e99
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin çalışma kitaplarını Düzenleyicisi klavye kısayolları

## <a name="the-return-key-and-its-nuances"></a>Dönüş anahtarı ve onun küçük farklar

Aşağıdaki tabloda, kod yürütmek ve markdown yazma için çeşitli anahtar bağlama açıklar. Size tanıdık ve sıvı duyarlı ve tutarlı anahtar bağlama seçmek için çok dikkatli gerçekleştirmişsiniz.

|Anahtar bağlama|Kod hücre|Markdown hücre|
|--- |--- |--- |
|<kbd>Döndür</kbd>|<p>Şapka hücre arabellek sonunda ise ve hücre başarıyla ayrıştırılabilir, yürütülecek olan sonuçlar arabellek görüntülenir ve yeni bir kod hücre eklenir ve hücre yürütülen hücreden sonra odaklanır.</p><p>Ayrıştırma yürütmeye değilse, yeni bir satır arabelleğe eklenir. Ayrıştırma başarılı olmazsa derleyici tanılama üretilen değil.</p>|<p><kbd>Dönüş</kbd> şapka konumundaki Markdown bağlamda bağlı olarak farklı bir davranışı sergiler.</p><ul><li>Düzeltme işareti bir Markdown kod bloğunda ise, değişmez değer yeni bir satır eklenir.</li><li>Düzeltme işareti bir Markdown listesi bloğunda ise, yeni bir liste öğesi oluşturun veya geçerli liste öğesinin bölün.</li><li>Şapka başka bir Markdown blok türünde ise, yeni bir paragraf blok oluşturun veya geçerli blok bölün.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>Win</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>Hücre içeriğinin ayrıştıracak ve her zaman çalışır. Derleme başarılı olursa, arabellek (yürütme özel durumlar dahil) sonucu görüntülenmez ve hiçbir sonraki hücre varsa, yeni bir tane oluşturulan odaklanır ve.</p><p>Derleme hataları varsa, tanılama görüntülenir ve arabellek değişmeden şapka konumu ile odaklanmış kalır.</p>|Ekler ve yeni bir kod hücresini geçerli markdown hücreden sonra odaklanır.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Win</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|Ekler ve sonra geçerli hücreyi yeni bir markdown hücre odaklanır.|Aynı davranışı <kbd>Döndür</kbd>|
|<kbd>Shift‑Return</kbd>|Her zaman düzeltme işareti konumunu veya içeriğini bağımsız olarak yeni bir satır ekler.|Geçerli Markdown bloğu içinde bir sabit satır sonu ekler.|
