---
title: "Xamarin çalışma kitaplarını Düzenleyicisi klavye kısayolları"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: cbbf070a9c94221d98dedcd1df884a897ff7df3e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin çalışma kitaplarını Düzenleyicisi klavye kısayolları

## <a name="the-return-key-and-its-nuances"></a>Dönüş anahtarı ve onun küçük farklar

Aşağıdaki tabloda, kod yürütmek ve markdown yazma için çeşitli anahtar bağlama açıklar. Size tanıdık ve sıvı duyarlı ve tutarlı anahtar bağlama seçmek için çok dikkatli gerçekleştirmişsiniz.

<table>
  <tr>
    <th>Anahtar bağlama</th>
    <th>Kod hücre</th>
    <th>Markdown hücre</th>
  </tr>
  <tr>
    <td><kbd>Döndür</kbd></td>
    <td>
      <p>Şapka hücre arabellek sonunda ise ve hücre başarıyla ayrıştırılabilir, yürütülecek olan sonuçlar arabellek görüntülenir ve yeni bir kod hücre eklenir ve hücre yürütülen hücreden sonra odaklanır.</p>
      
      <p>Ayrıştırma yürütmeye değilse, yeni bir satır arabelleğe eklenir. Ayrıştırma başarılı olmazsa derleyici tanılama üretilen değil.</p>
    </td>
    <td>
      <p>
        <kbd>Dönüş</kbd> şapka konumundaki Markdown bağlamda bağlı olarak farklı bir davranışı sergiler.
      </p>
      <ul>
        <li>
Düzeltme işareti bir Markdown kod bloğunda ise, değişmez değer yeni bir satır eklenir.
        </li>
        <li>
Düzeltme işareti bir Markdown listesi bloğunda ise, yeni bir liste öğesi oluşturun veya geçerli liste öğesinin bölün.
        </li>
        <li>
Şapka başka bir Markdown blok türünde ise, yeni bir paragraf blok oluşturun veya geçerli blok bölün.
        </li>
    </td>
  </tr>
  <tr>
    <td>
      <dl>
        <dt>Mac</dt>
        <dd><kbd>Command‑Return</kbd></dd>
        <dt>Win</dt>
        <dd><kbd>Control‑Return</kbd></dd>
      </dl>
    </td>
    <td>
      <p>Hücre içeriğinin ayrıştıracak ve her zaman çalışır. Derleme başarılı olursa, arabellek (yürütme özel durumlar dahil) sonucu görüntülenmez ve hiçbir sonraki hücre varsa, yeni bir tane oluşturulan odaklanır ve.</p>
      
      <p>Derleme hataları varsa, tanılama görüntülenir ve arabellek değişmeden şapka konumu ile odaklanmış kalır.</p>
    </td>
    <td>
Ekler ve yeni bir kod hücresini geçerli markdown hücreden sonra odaklanır.
    </td>
  </tr>
  <tr>
    <td>
      <dl>
        <dt>Mac</dt>
        <dd><kbd>Command‑Shift‑Return</kbd></dd>
        <dt>Win</dt>
        <dd><kbd>Control‑Shift‑Return</kbd></dd>
      </dl>
    </td>
    <td>
Ekler ve sonra geçerli hücreyi yeni bir markdown hücre odaklanır.
    </td>
    <td>
Aynı davranışı <kbd>Döndür</kbd>
    </td>
  </tr>
  <tr>
    <td><kbd>Shift‑Return</kbd></td>
    <td>
Her zaman düzeltme işareti konumunu veya içeriğini bağımsız olarak yeni bir satır ekler.
    </td>
    <td>
Geçerli Markdown bloğu içinde bir sabit satır sonu ekler.
    </td>
  </tr>
</table>
