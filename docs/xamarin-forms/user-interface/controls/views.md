---
title: "Xamarin.Forms görünümleri"
description: "Xamarin.Forms görünümleri platformlar arası mobil kullanıcı arabirimleri yapı taşlarıdır."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: df8c8463b2556035c5369c70cb10dbc3dc6b6743
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms görünümleri

_Xamarin.Forms görünümleri platformlar arası mobil kullanıcı arabirimleri yapı taşlarıdır._

<style>.tableimg { max-width: none !important;}</style>

## <a name="views"></a>Görünümler

Xamarin.Forms word kullanan *Görünüm* düğmeleri, etiketler veya pencere öğeleri denetimleri olarak daha yaygın olarak bilinen metin girişi kutuları - gibi görsel nesneler başvurmak için.

Bu kullanıcı Arabirimi öğeleri genellikle olan alt sınıflarının olan [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).

<br clear="right" />

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
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a>
    </td>
    <td valign="top">
Bir şey devam eden olduğunu göstermek için kullanılan bir görsel denetim. Bu denetim bir şey, ilerleme durumunu hakkında bilgisi olmadan gerçekleştiği kullanıcı görsel bir ipucu sağlar.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ActivityIndicatorDemoPage.cs"><img src="views-images/ActivityIndicator.png" title="ActivityIndicator örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> düz renkli dikdörtgen çizmek için kullanılan. BoxView görüntüleri veya özel öğeleri için yararlı bir stand-in ilk prototipi oluşturulurken yaparken var. BoxView 40 x 40 varsayılan boyutu isteği var. Farklı bir boyut gerekirse, Ata <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/">VisualElement.WidthRequest</a> ve <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/">VisualElement.HeightRequest</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/BoxViewDemoPage.cs"><img src="views-images/BoxView.png" title="BoxView örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Düğmesi</a>
    </td>
    <td align="center" valign="top">
Bir düğme <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> , tepki verdiğini dokunma olayları için.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ButtonDemoPage.cs"><img src="views-images/Button.png" title="Düğmesi örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> tarih çekme olanak sağlar. Bir DatePicker görsel gösterimi birine çok benzer <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">girişi</a>, klavye yerine bir tarih çekme için özel bir denetim görünür dışında </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/DatePickerDemoPage.cs"><img src="views-images/DatePicker.png" title="DatePicker örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Düzenleyicisi</a>
    </td>
    <td valign="top">
Birkaç satırlık metin düzenleyebilirsiniz denetim. Tek satırlı girişler için bkz: <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">girişi</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EditorDemoPage.cs"><img src="views-images/Editor.png" title="Düzenleyici örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Giriş</a>
    </td>
    <td valign="top">
Tek satırlık metin düzenleyebilirsiniz denetim. Tek satırlı metin girişi girişidir. En küçük ayrı ayrı kullanıcı adları ve parolalar gibi bilgi toplamak için kullanılır.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="views-images/Entry.png" title="Giriş örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Görüntü</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> bir görüntü içerir.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Görüntü API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/images.md">İmajlarla çalışma</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageDemoPage.cs">Tanıtım kaynak</a>
    </td>
    <td>
    <img src="views-images/Image.png" title="Resim örneği" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Etiket</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> , metin okuma yalnızca biçiminde görüntüler. Bir etiket metin çok satırları bloklarını yanı sıra tek satırlı metin öğelerini görüntülemek için kullanılır.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/LabelDemoPage.cs"><img src="views-images/Label.png" title="Etiket örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a>
    </td>
    <td valign="top">
Bir <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/">ItemView</a> , veri koleksiyonu dikey görüntüler.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/listview/index.md">ListView belgeleri</a>
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ListViewDemoPage.cs"><img src="views-images/ListView.png" title="ListView örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/">OpenGLView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> OpenGL içeriği görüntüler.
    <ul>
      <li>Yalnızca iOS ve Android projeleri (Windows Phone desteği) için çalışır.
      <li>Bir başvuru gerektirir <b>OpenTK 1.0</b> iOS ve Android projeler derleme.</li>
      <li>Paylaşılan projeleri kullanmak için en uygun; bir PCL kullandıysanız sonra bir DependencyService de gerekir.</li>
    </ul>
    </td>
    <td>
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/"><img src="views-images/OpenGL.png" title="OpenGlView örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Picker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> listedeki bir öğe çekme denetimi. Bir seçici görsel gösterimi benzer bir <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">girişi</a>, ancak bir seçici denetim klavye yerine görüntülenir.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/PickerDemoPage.cs"><img src="views-images/Picker.png" title="Seçici örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> bir ilerleme durumunu gösteren denetim.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ProgressBarDemoPage.cs"><img src="views-images/ProgressBar.png" title="ProgressBar örnek sınıf ="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> bir arama kutusu sağlayan denetimi.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SearchBarDemoPage.cs"><img src="views-images/SearchBar.png" title="SearchBar örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Kaydırıcı</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> doğrusal bir değer girdi denetim.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SliderDemoPage.cs"><img src="views-images/Slider.png" title="Kaydırıcı örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Adımlayıcı</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> bir ayrık değeri girdi denetim için bir aralığı kısıtlı.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StepperDemoPage.cs"><img src="views-images/Stepper.png" title="Adımlayıcı örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">geçiş</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> basılı değeri sağlayan denetimi.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchDemoPage.cs"><img src="views-images/Switch.png" title="Anahtar örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">Tablo görünümü</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> satırlarını tutan <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">hücre</a>s.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">Tablo görünümü API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/tableview.md">Tablo görünümü belgeleri</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TableViewFormDemoPage.cs">Tanıtım kaynak</a>
    </td>
    <td>
    <img src="views-images/TableViewNewest.png" title="Tablo görünümü örneği" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> zaman çekme sağlayan denetimi. Bir TimePicker görsel gösterimi birine çok benzer <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">girişi</a>, çekme bir süresi için özel bir denetim yerine bir klavye görünür dışında.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TimePickerDemoPage.cs"><img src="views-images/TimePicker.png" title="TimePicker örneği" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">Web görünümü</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Görünüm</a> HTML içeriğini gösterir.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">Web görünümü API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/webview.md">Web görünümü belgeleri</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/WebViewDemoPage.cs">Tanıtım kaynak</a>
    </td>
    <td>
    <img src="views-images/WebView.png" title="Web görünümü örneği" class="tableimg">
    </td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms Galerisi (örnek)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms örnekleri](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API belgeleri](https://developer.xamarin.com/api/root/Xamarin.Forms/)
