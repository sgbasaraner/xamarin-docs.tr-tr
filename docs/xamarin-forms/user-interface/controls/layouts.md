---
title: "Xamarin.Forms düzenleri"
description: "Xamarin.Forms düzenleri mantıksal yapılara kullanıcı arabirimi denetimlerini oluşturmak için kullanılır."
ms.topic: article
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ecea0f55360fcde7a50c52bb33c45a2c5fff5eeb
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms düzenleri

_Xamarin.Forms düzenleri mantıksal yapılara kullanıcı arabirimi denetimlerini oluşturmak için kullanılır._

<style>.tableimg { max-width: none !important;}</style>

## <a name="layouts"></a>Düzenleri

[`Layout`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) Xamarin.Forms sınıfında türdür bir özel düzenler veya görünümleri bir kapsayıcı gibi diğer davranır görünümünün. Genellikle, alt öğelerinin boyutunu ve konumunu Xamarin.Forms uygulamalarda ayarlamak için mantığını içerir.

 [ ![](layouts-images/layouts-sml.png "Xamarin.Forms yerleşim türleri")](layouts-images/layouts.png "Xamarin.Forms yerleşim türleri")

<br clear="all" />

Xamarin.Forms destekler:

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
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a>
    </td>
    <td valign="top">
Şablonlu görünümleri için Düzen Yöneticisi. İçinde kullanılan bir <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/">ControlTemplate</a> sunulması için içeriği göründüğü işaretlenecek.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/Templates/ControlTemplates/SimpleTheme/SimpleTheme/App.xaml"><img src="layouts-images/ContentPresenter.png" title="ContentPresenter örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a>
    </td>
    <td valign="top">
Tek bir içeriği olan bir öğe. ContentView çok az kullanımını kendi sahiptir. Amacı kullanıcı tanımlı bileşik görünümler için temel sınıf olarak görev yapmaktır.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentViewDemoPage.cs"><img src="layouts-images/ContentView.png" title="ContentView örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Çerçeve</a>
    </td>
    <td valign="top">
Bazı çerçeveleme seçenekleri ile tek bir alt öğe içeren bir öğe. Çerçeve sahip varsayılan bir <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/">Xamarin.Forms.Layout.Padding</a> 20.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/FrameDemoPage.cs"><img src="layouts-images/Frame.png" title="Çerçeve örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>
    </td>
    <td valign="top">
İçerik ise kaydırma yeteneğine sahip bir öğe gerektirir.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ScrollViewDemoPage.cs"><img src="layouts-images/ScrollView.png" title="ScrollView örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a>
    </td>
    <td valign="top">
Bir denetim şablonu ve için temel sınıfı olan içeriği görüntüleyen bir öğe <a href=""/api/type/Xamarin.Forms.ContentView/">ContentView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="layouts-images/TemplatedView.png" title="TemplatedView örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a>
    </td>
    <td valign="top">
Alt öğeler istenen Mutlak Konumlar yerleştirir. Bağlayıcıları ve sınırları atanan kullanıcı konumu ve boyutu denetiminin tanımlar.
    </td>
    <td valign="top">
      <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/AbsoluteLayoutDemoPage.cs"><img src="layouts-images/AbsoluteLayout.png" title="AbsoluteLayout örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Kılavuz</a>
    </td>
    <td valign="top">
Satırları ve sütunları düzenlenmiş görünümleri içeren düzeni.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/GridDemoPage.cs"><img src="layouts-images/Grid.png" title="Kılavuz örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/%601">düzeni</a> kullanan <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/">kısıtlaması</a>s Düzen alt.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/RelativeLayoutDemoPage.cs"><img src="layouts-images/RelativeLayout.png" title="RelativeLayout örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/">düzeni</a> dikey veya yatay olarak yönlendirilebilir tek bir satır içinde alt öğe yerleştirir. Bu düzen alt sınırları otomatik olarak bir düzen döngüsü sırasında ayarlar. Sınırların atanan kullanıcı üzerine yazılır ve bu nedenle bir alt öğe üzerinde kullanıcı tarafından ayarlanmamalıdır.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StackLayoutDemoPage.cs"><img src="layouts-images/StackLayout.png" title="StackLayout örneği" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms Galerisi (örnek)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms örnekleri](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API belgeleri](https://developer.xamarin.com/api/namespace/Xamarin.Forms)
