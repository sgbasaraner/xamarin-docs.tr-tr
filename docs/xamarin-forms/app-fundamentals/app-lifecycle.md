---
title: Xamarin.Forms uygulama yaşam döngüsü
description: Bu makalede, yaşam döngüsü yöntemlerini, sayfa gezinti olaylarını ve kalıcı gezinti olaylarını dahil olmak üzere uygulama yaşam döngüsü için yanıt açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: fb651494b63a77ede47dd246ee054b5c67af2a35
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240275"
---
# <a name="xamarinforms-app-lifecycle"></a>Xamarin.Forms uygulama yaşam döngüsü

[ `Application` ](xref:Xamarin.Forms.Application) Temel sınıf aşağıdaki özellikleri sunmaktadır:

* [Yaşam döngüsü yöntemleri](#Lifecycle_Methods) `OnStart`, `OnSleep`, ve `OnResume`.
* [Sayfa gezinti olaylarını](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing), [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing).
* [Kalıcı gezinti olaylarını](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, ve `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Yaşam döngüsü yöntemleri

[ `Application` ](xref:Xamarin.Forms.Application) Sınıfı yaşam döngüsü yöntemlerini işlemek için geçersiz kılınabilir üç sanal yöntemlerini içerir:

* **OnStart** -uygulama başladığında çağrılır.

* **OnSleep** -uygulama arka plana her olduğunda çağrılır.

* **OnResume** -uygulama ettirildiğinde arka plana gönderilen sonra çağrılır.

Unutmayın *hiçbir* yöntemi uygulama sonlandırma için.
Normal koşullar altında (IE. bir kilitlenme) uygulama sonlandırma gelen gerçekleşecek *OnSleep* kodunuz için herhangi bir ek bildirim olmadan durum.

Bu yöntemlerin zaman adlı izlemek için uygulayan bir `WriteLine` her (aşağıda gösterildiği gibi) çağırın ve test her platformda.

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

Güncelleştirirken *eski* Xamarin.Forms uygulamalar (ör.) Xamarin.Forms 1.3 veya eski oluşturma), Android ana etkinlik içerdiğinden emin olmak `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` içinde `[Activity()]` özniteliği. Mevcut değilse gözlemlemek `OnStart` döndürme üzerinde yanı sıra uygulama ilk başladığında yönteminizin çağrıldığından. Bu öznitelik geçerli Xamarin.Forms uygulaması şablonlarında otomatik olarak eklenir.

<a name="page" />

## <a name="page-navigation-events"></a>Sayfa gezintisi olayları

İki olay vardır [ `Application` ](xref:Xamarin.Forms.Application) görünmesini ve kayboluyor sayfaların bildirim sağlamak sınıfı:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) -bir sayfa hakkında ekranda görüntülenen olduğunda oluşturulur.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) -bir sayfa hakkında ekranından kayboluyor olduğunda oluşturulur.

Bu olaylar, ekranda görüntülenen özelliklerdir gibi sayfaları izlemek istediğiniz senaryolarda kullanılabilir.

> [!NOTE]
> [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing) Ve [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing) olayları yükseltilmiş [ `Page` ](xref:Xamarin.Forms.Page) temel sınıfı hemen sonra [ `Page.Appearing` ](xref:Xamarin.Forms.Page.Appearing) ve [ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing) olayları, sırasıyla.

<a name="modal" />

## <a name="modal-navigation-events"></a>Kalıcı Gezinti olayları

Dört olayları vardır [ `Application` ](xref:Xamarin.Forms.Application) sınıfı, her olanak tanıyan kendi olay bağımsız değişkenlerle yanıt gösterilen ve kapatıldığında kalıcı sayfalara:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - `ModalPoppingEventArgs` sınıfı içeren bir `Cancel` özelliği. Zaman `Cancel` ayarlanır `true` kalıcı pop iptal edildi.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> Uygulama yaşam döngüsü yöntemleri ve kalıcı Gezinti olayları, tüm ön uygulamak için`Application` Xamarin.Forms uygulaması oluşturma yöntemleri (IE. statik kullanan sürüm 1.2 veya eski yazılan uygulamaları `GetMainPage` yöntemi) oluşturmak için güncelleştirilmiş bir Varsayılan `Application` üst öğesi olarak ayarlanmış `MainPage`.
>
> Bu eski davranış kullanan Xamarin.Forms uygulamalarınız güncelleştirildi, için bir `Application` açıklandığı gibi bir alt [uygulama sınıfı](~/xamarin-forms/app-fundamentals/application-class.md) sayfası.
