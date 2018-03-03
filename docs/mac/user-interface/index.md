---
title: "Kullanıcı arabirimi"
description: "Bu makale bağlantılar çeşitli macOS açıklamak kılavuzlara UI denetler."
ms.topic: article
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: a8cb9488849dafc2cd720ecf59d654009a9ad781
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="user-interface"></a>Kullanıcı arabirimi

_Bu makale bağlantılar çeşitli macOS açıklamak kılavuzlara UI denetler._

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı erişiminiz kullanıcı arabirimi denetimlerini, içinde çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ ve kullanıcı arabirimleri korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için). 

Aşağıda listelenen kılavuzları Xamarin.Mac uygulamasında macOS kullanıcı Arabirimi öğeleri ile çalışma hakkında ayrıntılı bilgi verin. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz her makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

Bu makalede Windows ve paneller çalışmak Xamarin.Mac uygulamasında kapsar. Oluşturma ve Oluşturucusu'nda Windows ve paneller gelen yükleme Xcode ve arabirimi, Windows ve paneller koruyarak kapsayan `.storyboard` veya `.xib` Windows kullanarak ve C# kodunda Windows yanıt dosyaları.

## <a name="dialogsmacuser-interfacedialogmd"></a>[İletişim kutuları](~/mac/user-interface/dialog.md)

Bu makalede iletişim kutuları ve kalıcı Windows çalışmayı Xamarin.Mac uygulamasında kapsar. Oluşturma ve kalıcı Windows Xcode ve arabirim oluşturucusu, standart iletişim kutuları ile çalışma, görüntüleme ve Windows için C# kodunda yanıt korumaya kapsar.

## <a name="alertsmacuser-interfacealertmd"></a>[Uyarıları](~/mac/user-interface/alert.md)

Bu makalede uyarılarla çalışma Xamarin.Mac uygulamada yer almaktadır. Oluşturma ve C# kodundan uyarıları görüntüleme ve uyarılara yanıt verme kapsar.

## <a name="menusmacuser-interfacemenumd"></a>[Menüleri](~/mac/user-interface/menu.md)

Menüleri Mac uygulamanın kullanıcı arabiriminde çeşitli kısımlarını kullanılır; uygulamanın ana menüden bir penceresinde herhangi bir yerde görünebilir açılır ve bağlamsal menülere ekranın üstünde. Menüleri Mac uygulamanın kullanıcı deneyimi ayrılmaz bir parçasıdır. Bu makalede Cocoa Menülerle çalışma Xamarin.Mac uygulamada yer almaktadır.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Standart denetimler](~/mac/user-interface/standard-controls.md)

Düğmeleri, etiketler, metin alanlarına, onay kutularını ve bölümlenmiş denetimleri gibi standart AppKit denetimleri Xamarin.Mac uygulamasında ile çalışıyor. Bu kılavuz, bir kullanıcı arabirimi tasarımı Xcode'nın arabirimi Oluşturucu ekleme, bunları çıkışlar ve eylemleri aracılığıyla koda gösterme ve C# kodunda AppKit denetimleriyle çalışma kapsar.

 
## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Araç Çubukları](~/mac/user-interface/toolbar.md)

Bu makalede Xamarin.Mac uygulama araç çubukları ile çalışma kapsar. Oluşturma ve araç çubuklarını Xcode ve arabirim Oluşturucusu'nda, koruma kapsayan çıkışlar ve eylemleri kullanarak, etkinleştirme ve araç öğelerini devre dışı bırakma ve son olarak araç çubuğu öğeleri için C# kodunda yanıt kodu araç öğelerine kullanıma sunmak nasıl.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Tablo görünümleri](~/mac/user-interface/table-view.md)

Bu makalede Xamarin.Mac uygulama tablo görünümlerle çalışma kapsar. Oluşturma ve tablo görünümleri Xcode ve arabirim Oluşturucusu'nda, koruma kapsayan çıkışlar ve eylemleri kullanarak, tablo görünümleri doldurma ve son olarak C# kodunda Tablo görünümü öğelerine yanıt kodu Tablo görünümü öğelerini göstermek nasıl.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Anahat görünümleri](~/mac/user-interface/outline-view.md)

Bu makalede Xamarin.Mac uygulama anahat görünümlerle çalışma kapsar. Oluşturma ve anahat görünümleri Xcode ve arabirim Oluşturucusu'nda, koruma kapsayan çıkışlar ve eylemleri kullanarak, Anahat öğelerini doldurma ve son olarak C# kodunda anahat görünümü öğelerine yanıt kodu anahat görünümü öğelerine kullanıma sunmak nasıl.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Kaynak listeleri](~/mac/user-interface/source-list.md)

Bu makalede Xamarin.Mac uygulama kaynak listeleri ile çalışma kapsar. Oluşturma ve kaynak listeler Xcode ve arabirim Oluşturucusu'nda, koruma kapsayan kaynak listeler öğelerine çıkışlar ve eylemleri kullanarak, kaynak liste öğeleri doldurma ve son olarak C# kodunda kaynak liste öğelerine yanıt kodu kullanıma sunmak nasıl.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Koleksiyon görünümleri](~/mac/user-interface/collection-view.md)

Bu makale koleksiyonu görünümlerle çalışma Xamarin.Mac uygulamasında kapsar. Oluşturma ve koleksiyon görünümlerini Xcode ve arabirim Oluşturucusu'nda, koruma kapsayan çıkışlar ve eylemleri kullanarak, koleksiyon görünümlerini doldurma ve son koleksiyon görünümleri için C# kodunda yanıt kodu koleksiyon görünümü öğelerine kullanıma sunmak nasıl.

## <a name="creating-custom-user-controlsmacuser-interfacecustom-controlsmd"></a>[Özel kullanıcı denetimleri oluşturma](~/mac/user-interface/custom-controls.md)

Bu makalede ele alınmaktadır özel kullanıcı arabirimi denetimlerini oluşturma (içinden devralma tarafından `NSControl`), özel bir arabirim denetimi için çizim ve Xcode'nın arabirimi Oluşturucu ile kullanılan özel eylemler oluşturma.

## <a name="mac-samples-gallery"></a>Mac Örnekler Galerisi

Ayrıca göz atın alma öneririz [Mac Örnekler Galerisi](http://developer.xamarin.com/samples/mac/all/), bol Xamarin.Mac proje zemin hızlı bir şekilde elde etmenize yardımcı olabilir kullanıma hazır kod içerir.

## <a name="related-links"></a>İlgili bağlantılar

- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
