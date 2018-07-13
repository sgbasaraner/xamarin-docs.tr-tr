---
title: Xamarin.Forms özel oluşturucular
description: Özel oluşturucular Xamarin.Forms denetimleri davranışını ve görünümünü özelleştirmek için her platformda yerel denetimleri işleme geçersiz kılma geliştiricilerin olanak sağlar.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: c7ae25688b2f8635a9a89318e0b307e58add7a5a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998754"
---
# <a name="xamarinforms-custom-renderers"></a>Xamarin.Forms özel oluşturucular

_Xamarin.Forms kullanıcı arabirimleri, her platform için uygun görünüm ve yapısını korumak Xamarin.Forms uygulamaları hedef platformda yerel denetimlerini kullanarak işlenir. Özel oluşturucular, geliştiricilere her platformda Xamarin.Forms denetimleri davranışını ve görünümünü özelleştirmek için bu işlemi geçersiz kılma olanak tanır._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Özel oluşturucular giriş](introduction.md)

Özel oluşturucular Xamarin.Forms denetimleri davranışını ve görünümünü özelleştirmek için güçlü bir yaklaşım sağlar. Bunlar küçük stil değişikliklerini veya karmaşık platforma özel düzen ve davranışını özelleştirme için kullanılabilir. Bu makalede, özel Oluşturucu tanıtır ve özel Oluşturucu oluşturma işlemi açıklanmaktadır.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Oluşturucu temel sınıfları ve yerel denetimleri](renderers.md)

Yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici her Xamarin.Forms denetleyebilir. Bu makalede her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulayan yerel denetim sınıfları ve işleyici listeler.

## <a name="customizing-an-entryentrymd"></a>[Bir Girdiyi Özelleştirme](entry.md)

Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) denetim tek satırlık bir metin düzenlenmesine izin verir. Bu makale için özel Oluşturucu oluşturma işlemini gösterir `Entry` geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işleme geçersiz kılmak denetim.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Bir ContentPage’i Özelleştirme](contentpage.md)

A [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) tek bir görünümde görüntüleyen ve ekranın çoğunu kaplayan görsel bir öğedir. Bu makale için özel Oluşturucu oluşturma işlemini gösterir `ContentPage` sayfası, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işleme geçersiz kılmak.

## <a name="customizing-a-mapmapindexmd"></a>[Bir Haritayı Özelleştirme](map/index.md)

Xamarin.Forms.Maps kullanıcılar için deneyimi hızlı ve tanıdık bir eşleme sağlamak için API'ler her platformda yerel eşlemesi kullanmak haritalar görüntülemek için bir çoklu platform soyutlama sağlar. Bu konu için özel Oluşturucu oluşturma işlemini gösterir `Map` geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işleme geçersiz kılmak denetim.

## <a name="customizing-a-listviewlistviewmd"></a>[Bir ListView’i Özelleştirme](listview.md)

Bir Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) dikey bir liste olarak verilerinin bir koleksiyonunu görüntüleyen bir görünüm. Bu makalede, yerel liste denetimi performans hakkında daha fazla denetime izin vererek platforma özel liste denetimleri ve yerel hücre düzenleri kapsülleyen bir özel Oluşturucu oluşturma işlemini gösterir.

## <a name="customizing-a-viewcellviewcellmd"></a>[Bir ViewCell’i Özelleştirme](viewcell.md)

Bir Xamarin.Forms [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) eklenebilir bir hücre bir [ `ListView` ](xref:Xamarin.Forms.ListView) veya [ `TableView` ](xref:Xamarin.Forms.TableView), bir geliştirici tarafından tanımlanan görünüm içerir. Bu makale için özel Oluşturucu oluşturma işlemini gösterir. bir `ViewCell` bir Xamarin.Forms içinde barındırılan `ListView` denetimi. Bu Xamarin.Forms Düzen hesaplama sırasında sürekli çağrılmasını durdurur `ListView` kaydırma.

## <a name="implementing-a-viewviewmd"></a>[Bir Görünümü Uygulama](view.md)

Xamarin.Forms özel kullanıcı arabirimi denetimleri türetilmelidir [ `View` ](xref:Xamarin.Forms.View) düzenleri ve ekrandaki denetimleri yerleştirmek için kullanılan sınıf. Bu makalede, cihazın kamerasını Önizleme video akıştan görüntülemek için kullanılan bir Xamarin.Forms özel denetim için özel Oluşturucu oluşturma işlemini gösterir.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Bir HybridWebView’i Uygulama](hybridwebview.md)

Bu makale için özel Oluşturucu oluşturma işlemini gösterir. bir `HybridWebView` JavaScript'ten çağrılacak C# kod izin vermek için platforma özgü web denetimleri geliştirmek yapmayı gösteren özel bir denetim.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Bir video oynatıcıyı uygulama](video-player/index.md)

Bu makalede, özel bir uygulama Oluşturucu yazma işlemi gösterilmektedir `VideoPlayer` videoları, web, uygulama kaynakları eklenen videolarda ya da kullanıcının cihazında video kitaplığında depolanan video oynatabilen denetimi. Uygulama yöntemleri ve salt okunur bağlanabilir özellikler dahil olmak üzere çeşitli teknikler gösterilmiştir.


## <a name="related-links"></a>İlgili bağlantılar

- [Etkiler](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Özel oluşturucular (Xamarin University Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Özel oluşturucular (Xamarin University Video) örneği](http://bit.ly/xf-customrenderer)
