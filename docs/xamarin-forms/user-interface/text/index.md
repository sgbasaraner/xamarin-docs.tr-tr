---
title: Metin
description: "Xamarin.Forms girin veya metni görüntülemek için kullanma."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: c276352bc0ef1478c5145089277183b774bd9bff
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="text"></a>Metin

_Xamarin.Forms girin veya metni görüntülemek için kullanma._

Xamarin.Forms metni ile çalışmak için üç birincil görünümler vardır:

- **[Etiket](#Label)**  &mdash; tek veya birden çok satırlı metin sunmak için. Birden çok biçimlendirme seçeneklerini bir metinle aynı satırda gösterebilir.
- **[Giriş](#Entry)**  &mdash; yalnızca tek bir çizgi metni girmek için. Bir parola modu yok.
- **[Düzenleyici](#Editor)**  &mdash; birden fazla satır ele geçirebilir metin girmek için.

Metin görünümü, yerleşik veya özel kullanılarak değiştirilebilir [stilleri](#Styles) ve bazı denetimler özel destek [yazı tiplerini](#Fonts).

## <a name="labellabelmd"></a>[Etiket](label.md)

`Label` Görünüm metni görüntülemek için kullanılır. Birkaç satırlık metin veya tek satırlık metin gösterebilir. `Label` metin satır içinde kullanılan birden fazla biçimlendirme seçenekleri sunabilir. Etiket görünüm sarma veya tek bir satırda sığamıyorsa olduğunda metin kesmek.

![](images/label.png "Etiket örneği")

Bkz: [etiket](label.md) daha ayrıntılı bilgi için makalesi.

Bir etiket kullanılan yazı tipi özelleştirme hakkında daha fazla bilgi için bkz: [yazı tiplerini](fonts.md).

## <a name="entryentrymd"></a>[Giriş](entry.md)

`Entry` tek satırlı metin girişi kabul etmek için kullanılır. `Entry` Teklifler renkleri kontrol ancak yazı tipleri özelleştirilmiş olamaz. `Entry` bir parola modu ve metin girilene kadar yer tutucu metin gösterebilir.

![](images/entry.png "Giriş örneği")

Bkz: [girişi](entry.md) daha fazla bilgi için makalenin.

Farklı Not `Label`, `Entry` özel yazı tipi ayarlarını sahip olamaz.

## <a name="editoreditormd"></a>[Düzenleyicisi](editor.md)

`Editor` çok satırlı metin girişi kabul etmek için kullanılır. `Editor` bir özel bir arka plan rengi, ancak metin rengi olabilir ve yazı tipi değiştirilemez.

![](images/editor.png "Düzenleyici örneği")

Bkz: [Düzenleyicisi](editor.md) daha fazla bilgi için makalenin.

## <a name="fontsfontsmd"></a>[Yazı Tipleri](fonts.md)

`Label` Denetimi, her platform veya uygulamanızla dahil özel yazı tipleri yerleşik yazı tiplerini kullanma farklı yazı tipi ayarlarını destekler. Bkz: [yazı tiplerini](fonts.md) daha ayrıntılı bilgi için makalesi.

## <a name="stylesstylesmd"></a>[Stilleri](styles.md)

Başvurmak [stilleri ile çalışma](~/xamarin-forms/user-interface/styles/index.md) yazı tipi, ayarlama hakkında bilgi edinmek için [renk](~/xamarin-forms/user-interface/colors.md)ve arasında birden çok denetim uygulanan diğer görünen özellikleri.



## <a name="related-links"></a>İlgili bağlantılar

- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
