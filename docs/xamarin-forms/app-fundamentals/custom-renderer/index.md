---
title: "Özel oluşturucu"
description: "Xamarin.Forms kullanıcı arabirimleri, her platform için uygun görünüm ve kullanımında korumak Xamarin.Forms uygulamaların izin verecek şekilde hedef platformu yerel denetimleri kullanarak işlenir. Özel oluşturucu görünümünü ve davranışını her platformda Xamarin.Forms denetimlerin özelleştirmek için bu işlemi geçersiz kılma geliştiriciler olanak tanır."
ms.topic: article
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a92da0320addf1569c25ed05873aa11a198b1daa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="custom-renderers"></a>Özel oluşturucu

_Xamarin.Forms kullanıcı arabirimleri, her platform için uygun görünüm ve kullanımında korumak Xamarin.Forms uygulamaların izin verecek şekilde hedef platformu yerel denetimleri kullanarak işlenir. Özel oluşturucu görünümünü ve davranışını her platformda Xamarin.Forms denetimlerin özelleştirmek için bu işlemi geçersiz kılma geliştiriciler olanak tanır._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Özel oluşturucu giriş](introduction.md)

Özel oluşturucu görünümünü ve Xamarin.Forms denetimleri davranışını özelleştirmek için güçlü bir yaklaşım sağlayın. Bunlar küçük stil değişiklikleri veya karmaşık platforma özgü düzeni ve davranışını özelleştirme için kullanılabilir. Bu makalede özel Oluşturucu bir giriş sağlar ve özel Oluşturucu oluşturma süreci özetlenmektedir.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Oluşturucu temel sınıflar ve yerel denetimleri](renderers.md)

Yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu her Xamarin.Forms denetleyebilir. Bu makalede oluşturucuyu ve her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulamak yerel denetim sınıfları listeler.

## <a name="customizing-an-entryentrymd"></a>[Bir giriş özelleştirme](entry.md)

Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) tek satırlık metin düzenlenmesine izin verir. Bu makalede için özel Oluşturucu Oluşturma gösterilir `Entry` denetim, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işlemeyle geçersiz kılmasına etkinleştirme.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Bir ContentPage özelleştirme](contentpage.md)

A [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) tek bir görünüm görüntüler ve ekranın en kapladığı görsel bir öğedir. Bu makalede için özel Oluşturucu Oluşturma gösterilir `ContentPage` sayfasında, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işlemeyle geçersiz kılmasına etkinleştirme.

## <a name="customizing-a-mapmapindexmd"></a>[Bir harita özelleştirme](map/index.md)

Xamarin.Forms.Maps kullanıcılar için hızlı ve bilinen bir eşlemesi sağlamak için API'ler her platformda deneyimi yerel eşlemesi kullanmak eşlemeleri görüntüleme için platformlar arası Özet sağlar. Bu konu için özel Oluşturucu Oluşturma gösterir `Map` denetim, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işlemeyle geçersiz kılmasına etkinleştirme.

## <a name="customizing-a-listviewlistviewmd"></a>[ListView özelleştirme](listview.md)

Bir Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) dikey bir liste olarak verilerinin bir koleksiyonunu görüntüleyen bir görünüm değil. Bu makalede, yerel liste denetimi performansı hakkında daha fazla denetime izin vererek platforma özgü liste denetimleri ve yerel hücre düzenleri yalıtan özel Oluşturucu Oluşturma gösterilir.

## <a name="customizing-a-viewcellviewcellmd"></a>[Bir ViewCell özelleştirme](viewcell.md)

Bir Xamarin.Forms [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) eklenebilir bir hücre bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) veya [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/), geliştirici tarafından tanımlanan bir görünümü içerir. Bu makale için özel Oluşturucu Oluşturma gösteren bir `ViewCell` Xamarin.Forms içinde barındırılan `ListView` denetim. Bu art arda sırasında çağrılan gelen Xamarin.Forms düzeni hesaplamalar durdurur `ListView` kaydırma.

## <a name="implementing-a-viewviewmd"></a>[Bir görünümü uygulama](view.md)

Xamarin.Forms özel kullanıcı arabirimi denetimlerini türetilen [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) düzenleri ve denetimleri ekranında yerleştirmek için kullanılan sınıf. Bu makalede Önizleme video akışına cihazın kameradan görüntülemek için kullanılan bir Xamarin.Forms özel denetim için özel Oluşturucu Oluşturma gösterilir.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Bir HybridWebView uygulama](hybridwebview.md)

Bu makale için özel Oluşturucu Oluşturma gösteren bir `HybridWebView` çağrılacak C# kodu JavaScript'ten izin vermek için platforma özgü web denetimleri geliştirmek nasıl gösteren özel denetim.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Bir video oynatıcı uygulama](video-player/index.md)

Bu makalede özel bir uygulama Oluşturucu yazma gösterilmektedir `VideoPlayer` videolar web, uygulama kaynakları katıştırılmış videolar ya da, kullanıcının cihaz üzerindeki video kitaplığında depolanan videoları yürütebilirsiniz denetim. Yöntemleri ve salt okunur bağlanabilir özelliklerini uygulama da dahil olmak üzere çeşitli teknikler gösterilmiştir. 


## <a name="related-links"></a>İlgili bağlantılar

- [Etkileri](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Özel oluşturucu (Xamarin University Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Özel oluşturucu (Xamarin University Video) örnek](http://bit.ly/xf-customrenderer)
