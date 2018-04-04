---
title: Uygulama yaşam döngüsü
description: Nasıl yanıt vereceğini uygulama yaşam döngüsü
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 511591482a0e7512be34f6a210c6f44a1826be24
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="app-lifecycle"></a>Uygulama yaşam döngüsü

`Application` Temel sınıf aşağıdaki özellikleri sunmaktadır:

* [Yaşam döngüsü yöntemleri](#Lifecycle_Methods) `OnStart`, `OnSleep`, ve `OnResume`.
* [Kalıcı gezinti olaylarını](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, ve `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Yaşam döngüsü yöntemleri

`Application` Sınıfı yaşam döngüsü yöntemlerini işlemek için geçersiz kılınabilir üç sanal yöntemlerini içerir:

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

<a name="modal" />

## <a name="modal-navigation-events"></a>Kalıcı Gezinti olayları

Dört yeni olaylar vardır `Application` Xamarin.Forms 1.4, her biri kendi olay bağımsız sınıfında:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - `ModalPoppingEventArgs` sınıfı içeren bir `Cancel` özelliği. Zaman `Cancel` ayarlanır `true` kalıcı pop iptal edildi.
* **ModalPopped** - `ModalPoppedEventArgs`

Bu olaylar, uygulama yaşam döngüsü daha iyi yönetmenize yardımcı olur, gösterilen ve kapatıldığında kalıcı sayfalara yanıt vererek.

> [!NOTE]
> Uygulama yaşam döngüsü yöntemleri ve kalıcı Gezinti olayları, tüm ön uygulamak için`Application` Xamarin.Forms uygulaması oluşturma yöntemleri (IE. statik kullanan sürüm 1.2 veya eski yazılan uygulamaları `GetMainPage` yöntemi) oluşturmak için güncelleştirilmiş bir Varsayılan `Application` üst öğesi olarak ayarlanmış `MainPage`.
>
> Bu eski davranış kullanan Xamarin.Forms uygulamalarınız güncelleştirildi, için bir `Application` açıklandığı gibi bir alt [uygulama sınıfı](~/xamarin-forms/app-fundamentals/application-class.md) sayfası.
