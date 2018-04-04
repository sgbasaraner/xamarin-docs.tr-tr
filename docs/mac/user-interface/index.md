---
title: macOS kullanıcı arabirimi
description: Bu makale bağlantılar çeşitli macOS açıklamak kılavuzlara UI denetler.
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: d40faa29f2fe278377bf4eae42a032f3dc9086ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="macos-user-interface"></a>macOS kullanıcı arabirimi

_Bu makale bağlantılar çeşitli macOS açıklamak kılavuzlara UI denetler._

Erişimi Xamarin.Mac uygulamada C# ve .NET ile çalışırken, aynı kullanıcı arabirimi, çalışan bir geliştirici denetimleri *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ ve kullanıcı arabirimleri korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

Aşağıda listelenen kılavuzları Xamarin.Mac uygulamasında macOS kullanıcı Arabirimi öğeleri ile çalışma hakkında ayrıntılı bilgi verin. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz her makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) açıklar olarak da, belge `Register` ve `Export` kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılan öznitelikler.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

Bu makalede windows ve paneller Xamarin.Mac uygulamada çalışmaya kapsar. Oluşturma ve windows ve Xcode ve windows yükleme arabirimi Oluşturucusu paneller ve paneller .storyboard veya .xib dosyalarından koruma, windows kullanarak ve C# kodunda windows yanıtlama kapsar.

## <a name="dialogsmacuser-interfacedialogmd"></a>[İletişim Kutuları](~/mac/user-interface/dialog.md)

Bu makalede iletişim kutuları ve Xamarin.Mac uygulamada kalıcı windows ile çalışma kapsar. Oluşturma ve Xcode ve arabirim standart iletişim kutuları ile çalışma ve görüntüleme ve C# kodunda windows yanıtlama Oluşturucusu kalıcı windows koruma kapsar.

## <a name="alertsmacuser-interfacealertmd"></a>[Uyarılar](~/mac/user-interface/alert.md)

Bu makalede Xamarin.Mac uygulamasında uyarılarla çalışma kapsar. Oluşturma ve C# kodundan uyarıları görüntüleme ve uyarılara yanıt verme kapsar.

## <a name="menusmacuser-interfacemenumd"></a>[Menüler](~/mac/user-interface/menu.md)

Menüleri Mac uygulamanın kullanıcı arabiriminde çeşitli kısımlarını kullanılır; açılır menüler ve bağlamsal menüleri ekranın üstünde uygulamanın ana menüden, bir penceresinde herhangi bir yerde görünebilir. Menüleri, bir Mac uygulamasının kullanıcı deneyiminin ayrılmaz bir parçasıdır. Bu makalede, Xamarin.Mac uygulamasında Cocoa Menülerle çalışma yer almaktadır.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Standart denetimler](~/mac/user-interface/standard-controls.md)

Düğmeleri, etiketler, metin alanları, onay kutularını ve Xamarin.Mac uygulama bölümlenmiş denetimlerinde gibi standart AppKit denetimleri ile çalışıyor. Bu kılavuz, bir kullanıcı arabirimi tasarımı Xcode'nın arabirimi Oluşturucu ekleme, bunları çıkışlar ve eylemleri aracılığıyla koda gösterme ve C# kodunda AppKit denetimleriyle çalışma kapsar.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Araç Çubukları](~/mac/user-interface/toolbar.md)

Bu makalede araç çubukları Xamarin.Mac uygulamada çalışmaya kapsar. Oluşturma ve araç çubuklarını Xcode ve arabirim Oluşturucu koruma kapsayan çıkışlar ve eylemleri kullanarak, etkinleştirme ve araç öğelerini devre dışı bırakma ve son olarak C# kodunda araç çubuğu öğeleri için yanıt kodunu araç öğelerine kullanıma sunmak nasıl.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Tablo görünümleri](~/mac/user-interface/table-view.md)

Bu makalede Xamarin.Mac uygulamasında tablo görünümlerle çalışma kapsar. Oluşturma ve Xcode ve arabirim Oluşturucu tablo görünümleri koruma kapsayan Tablo görünümünde kullanıma sunmak için çıkışlar ve eylemleri kullanarak kod öğeleri nasıl tablosu görünümleri doldurma ve C# kodunda Tablo görünümü öğelerine yanıt.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Anahat görünümleri](~/mac/user-interface/outline-view.md)

Bu makalede Xamarin.Mac uygulamasında anahat görünümlerle çalışma kapsar. Oluşturma ve Xcode ve arabirim Oluşturucu anahat görünümleri koruma kapsayan anahat görünümü kullanıma sunmak için çıkışlar ve eylemleri kullanarak kod öğeleri nasıl anahat görünümleri doldurma ve C# kodunda öğeleri görüntüle ana hatlarını belirlemek için yanıt.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Kaynak listeleri](~/mac/user-interface/source-list.md)

Bu makalede Xamarin.Mac uygulamasında kaynak listeleriyle çalışma kapsar. Oluşturma ve kaynak listelerinde Xcode arabirimi Oluşturucu koruma kapsayan çıkışlar ve eylemleri kullanarak, kaynak listeleri doldurma ve C# kodunda kaynak liste öğelerine yanıt kodu Kaynak liste öğelerine kullanıma sunmak nasıl.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Koleksiyon görünümleri](~/mac/user-interface/collection-view.md)

Bu makalede, koleksiyon görünümlerini Xamarin.Mac uygulamada çalışmaya kapsar. Oluşturma ve koleksiyon görünümlerini Xcode ve arabirim Oluşturucu koruma kapsayan koleksiyon görünümü kullanıma sunmak için çıkışlar ve eylemleri kullanarak kod öğeleri nasıl koleksiyon görünümlerini doldurma ve koleksiyon görünümlerini C# kodunda yanıtlama.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[Özel denetimler oluşturma](~/mac/user-interface/custom-controls.md)

Bu makalede ele alınmaktadır özel kullanıcı arabirimi denetimlerini oluşturma (içinden devralma tarafından `NSControl`), özel bir arabirim denetimi için çizim ve Xcode'nın arabirimi Oluşturucu ile kullanılan özel eylemler oluşturma.

## <a name="mac-samples-gallery"></a>Mac Örnekler Galerisi

Ayrıca göz atın alma öneririz [Mac Örnekler Galerisi](https://developer.xamarin.com/samples/mac/all/). Bol Xamarin.Mac proje zemin hızlı bir şekilde elde etmenize yardımcı olabilir kullanıma hazır kod içerir.

## <a name="related-links"></a>İlgili bağlantılar

- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
