---
title: Xamarin.Forms Gezinti
description: Bu kılavuz, gezinti Xamarin.Forms uygulamalarında gerçekleştirmeyi açıklar. Xamarin.Forms, kullanılan sayfa türüne bağlı olarak farklı sayfa gezinti deneyimler sağlar.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 202f044ebd7dd5b110b94d2aa60eeb7151150607
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994734"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms Gezinti

_Xamarin.Forms, kullanılan sayfa türüne bağlı olarak farklı sayfa gezinti deneyimler sağlar._

![](images/page-types.png "Xamarin.Forms sayfa türleri")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Hiyerarşik Gezinme](hierarchical.md)

[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Sınıfı kullanıcının olduğu iletir ve geriye doğru istediğiniz şekilde, sayfada gezinme için bir hiyerarşik Gezinti deneyimi sağlar. Sınıfın uyguladığı son giren ilk çıkar (LIFO) yığını olarak Gezinti [ `Page` ](xref:Xamarin.Forms.Page) nesneleri.

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) her sekmesi ayrıntısı alanına içerik yükleme, sekmeler ve daha büyük bir ayrıntı alanı listesini oluşur.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) galeri gibi içerik sayfalarında gezinmek için kullanıcıların yan yana doğru çekin bir sayfadır.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) iki sayfa ilgili bilgilerin – öğeleri sunan bir ana sayfa ve ana sayfada öğe ayrıntılarını sunan bir ayrıntı sayfası yöneten bir sayfadır.

## <a name="modal-pagesmodalmd"></a>[Kalıcı Sayfalar](modal.md)

Xamarin.Forms, kalıcı sayfalar için de destek sağlar. Kalıcı bir sayfa, görev tamamlandı veya iptal kadar UZAĞINIZDA erişilemeyeceğini kendi içinde bir görevi tamamlamak için kullanıcıların önerir.

## <a name="displaying-pop-upspop-upsmd"></a>[Açılır Pencereleri Görüntüleme](pop-ups.md)

Xamarin.Forms iki pop kaydolma gibi kullanıcı arabirimi öğeleri sağlar: bir uyarıyı ve eylem sayfası. Bu arabirim öğeleri, kullanıcıların basit soru sormak ve görevleri aracılığıyla kullanıcılara kılavuzluk etmesi için kullanılabilir.
