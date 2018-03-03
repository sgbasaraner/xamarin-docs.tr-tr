---
title: "Xamarin.Forms sayfaları"
description: "Xamarin.Forms sayfaları platformlar arası mobil uygulama ekranlar temsil eder."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 35822dbbb7d5694e7f1f0a3f35f10df404206af9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms sayfaları

_Xamarin.Forms sayfaları platformlar arası mobil uygulama ekranlar temsil eder._

<style>.tableimg { max-width: none !important;}</style>

## <a name="pages"></a>Sayfaları

[ `Page` ](http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.Page) Çoğu veya tümü ekranın kaplayan ve tek bir alt öğe içeren bir görsel öğe bir sınıftır. A `Xamarin.Forms.Page` bir görünüm denetleyicisi iOS veya Windows Phone sayfayı temsil eder. Android ekran aktivite gibi her bir sayfa kaplar ancak Xamarin.Forms sayfalarıdır *değil* etkinlikler.

 [ ![](pages-images/pages-sml.png "Xamarin.Forms sayfasını")](pages-images/pages.png "türleri Xamarin.Forms sayfası")

<br clear="all" />

Xamarin.Forms destekler:

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <tr>
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
  </thead></tr>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>
    </td>
    <td align="center" valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a> tek bir görüntüler <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a>, genellikle bir kapsayıcı gibi bir <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a> veya <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentPageDemoPage.cs"><img src="pages-images/ContentPage.png" title="ContentPage örneği" class="tableimg">
    </a></td>
  </tr><tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">sayfa</a> bilgilerinin iki bölme yönetir.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/MasterDetailPageDemoPage.cs"><img src="pages-images/MasterDetailPage.png" title="MasterDetailPage örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">sayfa</a> gezinti ve kullanıcı deneyimi diğer sayfaların yığının yönetir.  
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/NavigationPageDemoPage.cs"><img src="pages-images/NavigationPage.png" title="NavigationPage örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">sayfa</a> sayfaları sekmeleri kullanarak, alt öğeleri arasında gezinme sağlar.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TabbedPageDemoPage.cs"><img src="pages-images/TabbedPage.png" title="TabbedPage örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">sayfa</a> tam ekran içeriğini bir denetim şablonu ve için temel sınıfı ile görüntüler <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="pages-images/TemplatedPage.png" title="TemplatedPage örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">sayfa</a> bir galeri gibi alt sayfalar arasında sağdan hareketleri izin verme.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CarouselPageDemoPage.cs"><img src="pages-images/CarouselPage.png" title="CarouselPage örneği" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms Galerisi (örnek)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms örnekleri](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API belgeleri](http://iosapi.xamarin.com/?link=N%3aXamarin.Forms)
