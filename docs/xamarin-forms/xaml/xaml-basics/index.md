---
title: Xamarin.Forms XAML temelleri
description: Bu kılavuz, mobil cihazlar için platformlar arası XAML kullanmaya başlamak açıklanmaktadır. XAML, geliştiricilerin kod yerine biçimlendirme kullanarak Xamarin.Forms uygulamalarında kullanıcı arabirimlerini tanımlamanızı sağlar.
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 901174eb9510eaab670564655f9f6b4bff940bd7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995446"
---
# <a name="xamarinforms-xaml-basics"></a>Xamarin.Forms XAML temelleri

XAML (eXtensible Application Markup Language), geliştiricilerin kod yerine biçimlendirme kullanarak Xamarin.Forms uygulamalarında kullanıcı arabirimlerini tanımlamasına olanak sağlar. XAML hiçbir zaman bir Xamarin.Forms programında gerekli, ancak genellikle daha yararlıdır birleştiren ve eşdeğer kod daha tutarlı ve potansiyel olarak oluşturulabildiğinden. XAML popüler MVVM (Model-View-ViewModel) uygulama mimarisi ile kullanım için uygun özellikle: XAML ViewModel kodla XAML tabanlı veri bağlamaları ile bağlantılı görünüm tanımlar.

## <a name="xaml-basics-contents"></a>XAML temelleri içeriği

* [Genel bakış](#Overview)
* [Bölüm 1. XAML Kullanmaya Başlarken](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [Bölüm 2. Temel XAML Sözdizimi](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [Bölüm 3. XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [Bölüm 4. Temel Veri Bağlama Bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [Bölüm 5. Verileri bağlama için MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

Bu XAML temelleri makaleler ek olarak bölümlerde kitabın indirebilirsiniz [Xamarin.Forms ile Mobile Apps oluşturma](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "Kitap kapsar")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

XAML konuları kitabın birçok bölümlerde daha ayrıntılı olarak ele alınmıştır dahil olmak üzere:

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>Bölüm 7. XAML vs. Kod</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">PDF'yi indirin</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">Özet</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Bölüm 8. Kod ve XAML uyum içinde</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">PDF'yi indirin</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">Özet</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Bölüm 10. XAML biçimlendirme uzantıları</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">PDF'yi indirin</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">Özet</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Bölüm 18. MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">PDF'yi indirin</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">Özet</a></td></tr>
</table>

Bu bölüm olabilir [ücretsiz karşıdan](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

XAML örnekleme nesneleri başlatma ve üst-alt öğe hiyerarşileri içindeki nesnelerin düzenlemek için programlama koduna alternatif olarak Microsoft tarafından oluşturulan bir XML tabanlı dilidir. XAML içinde .NET framework çeşitli teknolojileri için uyarlanmış, ancak kendi büyük yardımcı programını Windows Presentation Foundation (WPF), Silverlight, Windows çalışma zamanı ve evrensel Windows kullanıcı arabirimleri düzenini tanımlamakta buldu Platformu (UWP).

XAML, ayrıca Xamarin.Forms, platformlar arası yerel olarak tabanlı programlama arabirimi iOS, Android ve UWP için parçası mobil cihazlar. XAML dosyası içinde kullanıcı arabirimleri özel sınıflar tüm Xamarin.Forms görünümleri, düzenler ve sayfaları kullanarak Xamarin.Forms Geliştirici tanımlayabilirsiniz. XAML dosyası ya da derlenmiş veya çalıştırılabilir dosyasına katıştırılabilen. Her iki durumda da, adlandırılmış nesnelerin bulmak için derleme zamanında ve yeniden çalışma zamanında oluşturmak ve nesneleri ve bu nesneleri ve programlama kod arasında bağlantı kurmak için XAML bilgileri ayrıştırılır.

XAML eşdeğer kod üzerinde çeşitli avantajları vardır:

-  XAML olan genellikle daha fazlasını birleştiren ve eşdeğer kod daha okunabilir.
-  XML'de devralınan üst-alt hiyerarşisini üst-alt hiyerarşisini kullanıcı arabirimi nesneleri ile daha visual NET taklit etmek XAML sağlar.
-  XAML kolayca elle programcılar tarafından yazılmış olabilir, ancak ayrıca oluşturulabildiğinden ve görsel Tasarım araçları tarafından oluşturulan kendisine uygundur.

Elbette, ayrıca vardır dezavantajları, çoğunlukla biçimlendirme diller için iç sınırlamalar ilgili:

-  XAML kodu içeremez. Tüm olay işleyicilerine kod dosyasında tanımlanmış olması gerekir.
-  XAML yinelenen işleme for döngüleri içeremez. (Ancak, birkaç Xamarin.Forms görsel nesneler — özellikle [ `ListView` ](xref:Xamarin.Forms.ListView) — birden fazla alt nesneleri temel oluşturabilir, `ItemsSource` koleksiyonu.)
-  XAML (ancak, veri bağlama etkili bir şekilde bazı koşullu işlemesine olanak tanıyan bir kod tabanlı bağlama dönüştürücü başvurabilir.) koşullu işleme içeremez
-  XAML, genellikle parametresiz bir oluşturucu tanımlamazsanız sınıfları örneği oluşturulamıyor. (Ancak, bazen bir yolu var. Bu kısıtlama geçici olarak)
-  XAML genel yöntemler çağrılamaz. (Yeniden, bu kısıtlama bazen üstesinden.)

Yok henüz bir görsel tasarımcı XAML Xamarin.Forms uygulamaları oluşturmak için. Tüm XAML elle yazılmış olması gerekir, ancak var olan bir [XAML Önizleyiciyi](~/xamarin-forms/xaml/xaml-previewer.md). XAML için yeni programcıları sık derleme ve özellikle herhangi bir şeyi sonra açıkça doğru olmayabilir, uygulamalarını çalıştırmak isteyebilirsiniz. XAML deneyiminde çok sayıda bile geliştiricilere deneme ödüllendiriyoruz olduğunu bilirsiniz.

XAML olan temel XML, ancak XAML bazı benzersiz söz dizimi özelliklere sahiptir. En önemli şunlardır:

- Özellik öğeleri
- Ekli özellikler
- Biçimlendirme uzantıları

Bu özellikler *değil* XML uzantıları. XAML tamamen yasal XML'dir. Ancak bu XAML söz dizimi özellikler XML benzersiz şekilde kullanın. Bunlar ayrıntılı aşağıdaki makalelerde, MVVM uygulamak için XAML kullanarak bir giriş sonunda ele alınmıştır.

## <a name="requirements"></a>Gereksinimler

Bu makale, bir Xamarin.Forms ile olunduğunu varsayar. Okuma [xamarin.Forms'a giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) önemle tavsiye edilir.

Bu makalede ayrıca XML, XML ad alanı bildirimi ve koşulları kullanımını anlama da dahil olmak üzere bazı olarak bilindiğini varsayar *öğesi*, *etiketi*, ve *özniteliği*.

Xamarin.Forms ve XML ile ilgili bilgi sahibi olduğunuz, okuma başlattığınızda [bölüm 1. XAML ile çalışmaya başlama](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).



## <a name="related-links"></a>İlgili bağlantılar

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Xamarin.Forms'a giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Mobile Apps defteri oluşturma](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
