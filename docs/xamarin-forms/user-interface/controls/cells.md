---
title: "Xamarin.Forms hücreler"
description: "Xamarin.Forms hücreleri ListViews ve TableViews eklenebilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 0f8886546004702adbdbca7d991c67d5700e453e
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms hücreler

_Xamarin.Forms hücreleri ListViews ve TableViews eklenebilir._

A *hücre* tablodaki öğeler için kullanılan özelleştirilmiş bir öğe ve listedeki her öğeye nasıl işleneceğini açıklar. [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) Sınıfı türer [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), içinden [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) de türetilir. Bir hücrenin kendisi bir görsel öğe; değildir Bunun yerine, bir görsel öğe oluşturmak için bir şablon. 

`Cell` özel olarak ile kullanılan [ `ListView` ](views.md#listView) ve [ `TableView` ](views.md#tableView) kontrol eder. Hücreleri özelleştirmek ve kullanmak öğrenmek için bkz [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) ve [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) belgeleri.

## <a name="cells"></a>Hücreleri

Xamarin.Forms aşağıdaki hücre türlerini destekler:

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) bir veya iki metin dizesini görüntüler. Ayarlama [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) özelliği ve isteğe bağlı olarak [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) metin dizelerinden özelliğine.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) / [Kılavuzu](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![TextCell örnek](cells-images/TextCell.png "TextCell örnek")](cells-images/TextCell-Large.png#lightbox "TextCell örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) Aynı bilgileri görüntüler [ `TextCell` ](#textCell) ancak ile ayarlanmış bir bit eşlemi içerir [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) özelliği.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) / [Kılavuzu](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![ImageCell örnek](cells-images/ImageCell.png "ImageCell örnek")](cells-images/ImageCell-Large.png#lightbox "ImageCell örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) Kümesiyle metni içeren [ `Text`'](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCellText/) özelliği ve ve açık/kapalı başlangıçta ile Boolean ayarlamak anahtar [ `On` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCell.On/) özelliği. İşleme [ `OnChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SwitchCell.OnChanged/) ne zaman bildirim almak için olay `On` özellik değişikliği.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) / [Kılavuzu](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell örnek](cells-images/SwitchCell.png "SwitchCell örnek")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) Tanımlayan bir [ `Label` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Label/) hücre ve düzenlenebilir metin olarak tek bir satırı tanımlayan özellik [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Text/) özelliği. İşleme [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.EntryCell.Completed/) kullanıcı metin girişi tamamlandığında bildirim almak için olay.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) / [Kılavuzu](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell örnek](cells-images/EntryCell.png "EntryCell örnek")](cells-images/EntryCell-Large.png#lightbox "EntryCell örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery örnek](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API belgeleri](https://developer.xamarin.com/api/root/Xamarin.Forms/)
