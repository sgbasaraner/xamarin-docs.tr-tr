---
title: Xamarin.Forms XAML temelleri
description: Bu kılavuz, mobil cihazlar için platformlar arası XAML kullanmaya başlama açıklanmaktadır. XAML geliştiricilerin kodu yerine biçimlendirme Xamarin.Forms uygulamalarda kullanıcı arabirimleri tanımlamanızı sağlar.
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 627267b95bb2d810a60f84c51e38bf5387fe1f99
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245969"
---
# <a name="xamarinforms-xaml-basics"></a>Xamarin.Forms XAML temelleri

XAML (eXtensible Application Markup Language), geliştiricilerin kod yerine biçimlendirme kullanarak Xamarin.Forms uygulamalarında kullanıcı arabirimlerini tanımlamasına olanak sağlar. XAML bir Xamarin.Forms programında hiçbir zaman gerekli, ancak bunu genellikle daha kısa ve eşdeğer kod görsel olarak daha tutarlı ve potansiyel olarak oluşturulabildiğinden. XAML popüler MVVM (Model-View-ViewModel) uygulama mimarisi ile kullanım için uygun özellikle: XAML XAML tabanlı veri bağlamaları ViewModel koduna bağlı Görünüm tanımlar.

## <a name="xaml-basics-contents"></a>XAML temelleri içeriği

* [Genel bakış](#Overview)
* [Bölüm 1. XAML Kullanmaya Başlarken](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [Bölüm 2. Temel XAML Sözdizimi](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [Bölüm 3. XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [Bölüm 4. Temel Veri Bağlama Bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [Bölüm 5. Verileri için MVVM bağlama](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

Bu XAML temelleri makaleler yanı sıra bölümlerde rehberi indirebilirsiniz [Xamarin.Forms ile Mobile Apps oluşturma](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "Kitap kapak")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

XAML konuları daha kapsamlı rehberi birçok bölümlerde ele alınmıştır dahil olmak üzere:

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>Bölüm 7. XAML vs. Kod</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">PDF indirin</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">Özet</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Bölüm 8. Kod ve XAML uyum içinde</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">PDF indirin</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">Özet</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Bölüm 10. XAML işaretleme uzantıları</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">PDF indirin</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">Özet</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Bölüm 18. MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">PDF indirin</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">Özet</a></td></tr>
</table>

Bu bölümlerde olabilir [ücretsiz olarak karşıdan](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

XAML başlatmasını ve nesneleri başlatma ve bu nesneleri üst-alt hiyerarşileri düzenleme için programlama kodu alternatif olarak Microsoft tarafından oluşturulan bir XML tabanlı bir dildir. .NET framework içinde çeşitli teknolojiler için XAML uyarlanan, ancak kendi büyük yardımcı programı Windows Presentation Foundation (WPF), Silverlight, Windows çalışma zamanı ve evrensel Windows içinde kullanıcı arabirimleri düzenini tanımlama buldu Platformu (UWP).

XAML parçasıdır ayrıca Xamarin.Forms, platformlar arası yerel tabanlı programlama arabirimi iOS, Android ve UWP mobil aygıtlar. XAML dosyası içinde Xamarin.Forms Geliştirici tüm Xamarin.Forms görünümleri, düzenleri ve sayfaları iyi olarak özel sınıfları kullanmaya kullanıcı arabirimleri tanımlayabilirsiniz. XAML dosyası ya da derlenmiş veya yürütülebilir dosya katıştırılmış. Her iki durumda da, adlandırılmış nesneleri bulmak için derleme zamanında ve yeniden çalışma zamanında örneği ve nesneleri başlatma ve bu nesneler ve programlama kodu arasında bağlantılar kurmak için XAML bilgileri ayrıştırılır.

XAML eşdeğer kod çeşitli avantajları vardır:

-  XAML olduğundan genellikle daha kısa ve eşdeğer kodunu daha okunabilir.
-  Üst-alt hiyerarşisi XML'de devralınmış XAML kullanıcı arabirimi nesneleri üst-alt hiyerarşisi ile büyük Görsel Netlik taklit sağlar.
-  XAML kolayca elle programcıları tarafından yazılmış olabilir, ancak aynı zamanda kendisini oluşturulabildiğinden ve görsel Tasarım araçları tarafından oluşturulan kullanabilmek için uygundur.

Elbette, ayrıca vardır dezavantajları, çoğunlukla işaretleme dili için iç sınırlamaları ilgili:

-  XAML kodu içeremez. Tüm olay işleyicileri bir kod dosyasında tanımlanmış olması gerekir.
-  XAML yinelenen işleme döngüler içeremez. (Ancak, çeşitli Xamarin.Forms görsel nesneler — özellikle [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) — birden fazla alt nesneleri temel oluşturabilir, `ItemsSource` koleksiyonu.)
-  XAML koşullu işleme (ancak, veri bağlama bazı koşullu işleme etkili bir şekilde izin veren bir kod tabanlı bağlama dönüştürücü başvurabilir.) içeremez.
-  XAML genellikle parametresiz bir kurucusu tanımlamayın sınıfları örneği oluşturulamıyor. (Ancak, bazen bir yolu yoktur bu kısıtlama geçici.)
-  XAML genellikle yöntemleri çağrılamaz. (Yeniden, bu kısıtlama bazen üstesinden.)

Yok henüz bir görsel tasarımcı XAML Xamarin.Forms uygulamaları oluşturmak için. Tüm XAML elle yazılmış olmalıdır, ancak var olan bir [XAML Önizleyicisi](~/xamarin-forms/xaml/xaml-previewer.md). XAML için yeni programcıları sık oluşturmak ve uygulamalarını, özellikle her şeyi sonra açıkça doğru olmayabilir çalıştırmak isteyebilirsiniz. XAML'de deneyimi çok sayıda bile geliştiricilere deneme çekici olmadığını bildirin.

XAML, aslında XML, ancak XAML bazı benzersiz sözdizimi özelliklere sahiptir. En önemli şunlardır:

- Özellik öğeleri
- Ekli özellikler
- İşaretleme uzantıları

Bu özellikler *değil* XML uzantıları. XAML tamamen yasal XML'dir. Ancak bu XAML sözdizimi özellikleri XML benzersiz şekilde kullanın. Bunlar aşağıdaki makalelerde ayrıntılı hangi MVVM uygulamak için XAML kullanılarak bir giriş ile sonuçlandırmak ele alınmıştır.

## <a name="requirements"></a>Gereksinimler

Bu makalede Xamarin.Forms çalışma bilindiğini varsayar. Okuma [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) önemle önerilir.

Bu makalede ayrıca XML ad alanı bildirimleri ve koşulları kullanımını anlamak da dahil olmak üzere XML aşina varsayar *öğesi*, *etiketi*, ve *özniteliği*.

Xamarin.Forms ve XML ile aşina olduğunuzda okuma Başlat [Kısım 1. XAML ile çalışmaya başlama](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).



## <a name="related-links"></a>İlgili bağlantılar

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Mobile Apps defteri oluşturma](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
