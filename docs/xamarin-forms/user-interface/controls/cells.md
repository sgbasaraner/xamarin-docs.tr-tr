---
title: Xamarin.Forms hücreleri
description: Xamarin.Forms hücreleri ListViews ve TableViews eklenebilir. Xamarin.Forms içinde bulunan hücre bu makalede listelenmektedir.
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 3b69aaf0a10468a5950e0ccf5a61ab6ecbbc110f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995926"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms hücreleri

_Xamarin.Forms hücreleri ListViews ve TableViews eklenebilir._

A *hücre* bir tablodaki öğeler için kullanılan özel bir öğe ve listedeki her öğeye nasıl oluşturulacağını açıklar. [ `Cell` ](xref:Xamarin.Forms.Cell) Sınıf türetilir [ `Element` ](xref:Xamarin.Forms.Element), içinden [ `VisualElement` ](xref:Xamarin.Forms.Element) ayrıca türetilir. Bir hücreyi kendisi; bir görsel öğe değildir Bunun yerine bir görsel öğe oluşturmak için bir şablondur.

`Cell` yalnızca ile kullanılan [ `ListView` ](views.md#listView) ve [ `TableView` ](views.md#tableView) kontrol eder. Hücreleri özelleştirme ve nasıl kullanılacağını öğrenmek için bkz [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) ve [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) belgeleri.

## <a name="cells"></a>Hücreleri

Xamarin.Forms aşağıdaki hücresi türlerini destekler:

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](xref:Xamarin.Forms.TextCell) bir veya iki metin dizesini görüntüler. Ayarlama [ `Text` ](xref:Xamarin.Forms.TextCell.Text) özelliği ve isteğe bağlı olarak [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail) özelliğini bu metin dizeleri.<br /><br />[API belgeleri](xref:Xamarin.Forms.TextCell) / [Kılavuzu](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![TextCell örnek](cells-images/TextCell.png "TextCell örnek")](cells-images/TextCell-Large.png#lightbox "TextCell örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](xref:Xamarin.Forms.ImageCell) Aynı bilgiyi görüntüler [ `TextCell` ](#textCell) ancak ile ayarlanmış bir bit eşlem içeren [ `Source` ](xref:Xamarin.Forms.Image.Source) özelliği.<br /><br />[API belgeleri](xref:Xamarin.Forms.ImageCell) / [Kılavuzu](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![ImageCell örnek](cells-images/ImageCell.png "ImageCell örnek")](cells-images/ImageCell-Large.png#lightbox "ImageCell örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](xref:Xamarin.Forms.SwitchCell) Kümesi metni içeren [ `Text`'](xref:Xamarin.Forms.SwitchCell.Text) özelliği ve ve açma/kapama düğmesi ile Boole başlangıçta ayarlamak [ `On` ](xref:Xamarin.Forms.SwitchCell.On) özelliği. Tanıtıcı [ `OnChanged` ](xref:Xamarin.Forms.SwitchCell.OnChanged) olduğunda bildirim almak için olay `On` özellik değişiklikleri.<br /><br />[API belgeleri](xref:Xamarin.Forms.SwitchCell) / [Kılavuzu](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell örnek](cells-images/SwitchCell.png "SwitchCell örnek")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](xref:Xamarin.Forms.EntryCell) Tanımlayan bir [ `Label` ](xref:Xamarin.Forms.EntryCell.Label) hücre ve düzenlenebilir metin olarak tek bir satırı tanımlayan özellik [ `Text` ](xref:Xamarin.Forms.EntryCell.Text) özelliği. Tanıtıcı [ `Completed` ](xref:Xamarin.Forms.EntryCell.Completed) kullanıcı metin girişini tamamlandığında bildirilmesini olay.<br /><br />[API belgeleri](xref:Xamarin.Forms.EntryCell) / [Kılavuzu](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell örnek](cells-images/EntryCell.png "EntryCell örnek")](cells-images/EntryCell-Large.png#lightbox "EntryCell örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms'a giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery örnek](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API belgeleri](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
