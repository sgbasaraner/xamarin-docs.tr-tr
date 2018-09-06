---
title: Xamarin.Mac macOS kullanıcı arabirimi denetimleri
description: Xamarin.Mac geliştiricilere sunulan çeşitli kullanıcı arabirimi denetimleri kılavuzları bu belge bağlar. Bağlantılı içeriği bir windows iletişim kutuları, uyarılar, menüler, araç çubukları, tablo görünümleri, anahat görünümleri ve daha fazla inceler.
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: b2392f05a03015f903918f15013919be14b99292
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780592"
---
# <a name="macos-user-interface-controls-in-xamarinmac"></a>Xamarin.Mac macOS kullanıcı arabirimi denetimleri

_Çeşitli macOS kullanıcı Arabirimi denetimleri kılavuzları bu makalede bağlar._

Erişimi bir Xamarin.Mac uygulamasında çalışırken, C# ve .NET sayesinde aynı kullanıcı için çalışan bir geliştirici denetleyen arabirim *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Xcode'un kullanabileceğiniz _arabirim Oluşturucu_ oluşturmak ve korumak, kullanıcı arabirimleri (veya isteğe bağlı olarak bunları doğrudan C# kodu oluşturmak için).

Aşağıda listelenen kılavuzları bir Xamarin.Mac uygulamasını macOS UI öğeleri ile çalışma hakkında ayrıntılı bilgi sağlar. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve her makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) açıklar olarak da belge `Register` ve `Export` Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılan öznitelikler.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

Bu makale, windows ve bir Xamarin.Mac uygulamasını panellerinde çalışmayı kapsar. Oluşturma ve windows ve Xcode ve arabirim Oluşturucu windows yükleniyor, paneller ve .storyboard veya .xib dosyaları panellerinden koruma, windows kullanarak ve C# kodunda windows yanıtlama ele alınmaktadır.

## <a name="dialogsmacuser-interfacedialogmd"></a>[İletişim Kutuları](~/mac/user-interface/dialog.md)

Bu makale, iletişim kutuları ve kalıcı bir Xamarin.Mac uygulamasını Windows'ta çalışmayı kapsar. Oluşturulması ve bakımının yapılması Xcode ve arabirim Oluşturucu standart iletişim kutuları ile çalışma ve görüntüleme ve C# kodunda windows yanıtlama, kalıcı windows ele alınmaktadır.

## <a name="alertsmacuser-interfacealertmd"></a>[Uyarılar](~/mac/user-interface/alert.md)

Bu makale, bir Xamarin.Mac uygulamasındaki uyarılarla kapsar. Oluşturma ve C# kod uyarıları görüntüleme ve uyarılara yanıt verme ele alınmaktadır.

## <a name="menusmacuser-interfacemenumd"></a>[Menüler](~/mac/user-interface/menu.md)

Menüleri, bir Mac uygulamanın kullanıcı arabirimini çeşitli bölümlerini kullanılır; açılır menüler ve bağlam menüleri için ekranın üst uygulamanın ana menüden, herhangi bir pencere içinde görünebilir. Menüler Mac uygulamanın kullanıcı deneyimini ayrılmaz bir parçasıdır. Bu makale, bir Xamarin.Mac uygulamasını Cocoa Menülerle çalışma kapsar.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Standart denetimler](~/mac/user-interface/standard-controls.md)

Düğmeler, etiketler, metin alanları, onay kutularını ve bölümlenmiş denetimler Xamarin.Mac uygulamasında gibi standart olan tüm AppKit denetimleri ile çalışma. Bu kılavuz, bir kullanıcı arabirimi tasarımı Xcode'un arabirimi Oluşturucu ekleme, bunları çıkışlar ve eylemleri aracılığıyla koda gösterme ve C# kodunda olan tüm AppKit denetimleri ile çalışma kapsar.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Araç Çubukları](~/mac/user-interface/toolbar.md)

Bu makale, bir Xamarin.Mac uygulamasını araç çubuklarında ile çalışmayı kapsar. Oluşturulması ve bakımının yapılması Xcode ve arabirim Oluşturucu, araç çubuklarını kapsayan çıkışlar ve eylemleri kullanarak, etkinleştirme ve araç çubuğu öğelerini devre dışı bırakma ve son olarak, C# kodunda araç çubuğu öğeleri için yanıt kodu için araç çubuğu öğelerini nasıl sunacağınızı öğrenin.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Tablo görünümleri](~/mac/user-interface/table-view.md)

Bu makale, bir Xamarin.Mac uygulamasını tablo görünümlerde çalışmak kapsar. Oluşturulması ve bakımının yapılması tablo görünümleri Xcode ve arabirim Oluşturucu kapsayan Tablo görünümü kullanıma sunmak için çıkışlar ve eylemleri kullanarak kod öğelerini nasıl tablo görünümleri doldurma ve tablo C# kod öğelerini görüntüle yanıtlama.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Anahat görünümleri](~/mac/user-interface/outline-view.md)

Bu makale, bir Xamarin.Mac uygulamasını anahat görünümleri ile çalışmayı kapsar. Oluşturulması ve bakımının yapılması Xcode ve arabirim Oluşturucu anahat görünümleri kapsayan anahat görünümü kullanıma sunmak için çıkışlar ve eylemleri kullanarak kod öğelerini nasıl anahat görünümleri doldurma ve C# kod öğelerini görüntüle anahat yanıtlama.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Kaynak listeleri](~/mac/user-interface/source-list.md)

Bu makale, bir Xamarin.Mac uygulamasını kaynak listeleriyle çalışma kapsar. Oluşturulması ve bakımının yapılması kaynak listelerinde Xcode ve arabirim Oluşturucu kapsayan çıkışlar ve eylemleri kullanarak, kaynak listelerini doldurmak ve C# kodunda kaynak liste öğelerine yanıt kodu Kaynak liste öğelerine nasıl sunacağınızı öğrenin.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Koleksiyon görünümleri](~/mac/user-interface/collection-view.md)

Bu makale koleksiyonu görünümlerde bir Xamarin.Mac uygulamasını çalışmak kapsar. Oluşturma ve koleksiyon görünümlerini Xcode ve arabirim Oluşturucu koruma kapsayan koleksiyon görünümü kullanıma sunmak için çıkışlar ve eylemleri kullanarak kod öğelerini nasıl koleksiyon görünümlerini doldurma ve koleksiyon görünümlerini C# kodunda yanıtlama.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[Özel denetimler oluşturma](~/mac/user-interface/custom-controls.md)

Bu makalede, özel kullanıcı arabirimi denetimleri oluşturma yer almaktadır (dan devralan tarafından `NSControl`), özel bir arabirim denetimi çizme ve Xcode'un arabirimi Oluşturucusu ile kullanılabilecek özel eylemler oluşturma.

## <a name="mac-samples-gallery"></a>Mac Örnekler Galerisi

Ayrıca göz alma öneririz [Mac Örnekler Galerisi](https://developer.xamarin.com/samples/mac/all/). Daha zengin bir Xamarin.Mac Proje hızlı bir şekilde buluta taşıyın yardımcı olabilecek kullanıma hazır kod içerir.

## <a name="related-links"></a>İlgili bağlantılar

- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
