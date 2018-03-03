---
title: "Xamarin.Forms hücreler"
description: "Xamarin.Forms hücreleri ListViews ve TableViews eklenebilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 509ecc509754bba544115c140e619f634bd64eae
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms hücreler

_Xamarin.Forms hücreleri ListViews ve TableViews eklenebilir._

<style>.tableimg { max-width: none !important;}</style>

## <a name="cells"></a>Hücreleri

Bir hücre bir tablodaki öğeler için kullanılan özelleştirilmiş bir öğe ve listedeki her öğeye nasıl çizileceğini açıklar. Hücre türer [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), VisualElement de türetilen gelen. Hücre bir görsel öğe değil ancak yalnızca bir görsel öğe oluşturmak için bir şablonu açıklar. [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) taban sınıfı ve özellikleri için tüm Xamarin.Forms hücreleri sağlar. Hücreleri öğeleridir eklenmek üzere tasarlanmış [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) veya [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) kontrol eder.

Hücreleri özelleştirmek ve kullanmak öğrenmek için bkz [ListView](~/xamarin-forms/user-interface/listview/index.md) ve [Tablo görünümü](~/xamarin-forms/user-interface/tableview.md) belgeleri.

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Türü</strong>
    </th>
    <th>
      <strong>Açıklama</strong>
    </th>
    <th style="min-width:400px">
      <strong>ekran görüntüsü</strong>
    </th>
  </thead>
  <tbody>
    <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> etiketli ve tek satırlı metin girişi alanı.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="cells-images/EntryCell.png" title="EntryCell örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> etiketli ve açık/kapalı anahtar.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchCellDemoPage.cs"><img src="cells-images/SwitchCell.png" title="SwitchCell örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> birincil ve ikincil metinle.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TextCellDemoPage.cs"><img src="cells-images/TextCell.png" title="TextCell örneği" class="tableimg">
    </a></td>
  </tr>
      <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">metin hücre</a> , ayrıca bir görüntüsünü içerir.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageCellDemoPage.cs"><img src="cells-images/ImageCell.png" title="ImageCell örneği" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms Galerisi (örnek)](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API belgeleri](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
