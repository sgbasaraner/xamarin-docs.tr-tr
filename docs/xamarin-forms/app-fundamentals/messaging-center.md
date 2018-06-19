---
title: Xamarin.Forms MessagingCenter
description: Bu makalede Xamarin.Forms MessagingCenter modelleri görüntüle gibi sınıflar arasında bağ azaltmak için ileti gönderme ve alma için nasıl kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 49a7ecdad53c7820594f7ebc047ae6fbc5a9bc56
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209420"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

_Xamarin.Forms ileti gönderme ve alma için basit bir ileti sistemi hizmeti içerir._

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

Xamarin.Forms `MessagingCenter` görünüm modelleri ve diğer bileşenleri birbiriyle ilgili herhangi bir şey yanı sıra basit bir ileti sözleşmesi bilmek zorunda kalmadan ile iletişim kurmak için etkinleştirir.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>MessagingCenter nasıl çalışır?

İki bölümü vardır `MessagingCenter`:

-  **Abone** - belirli bir imza ile iletiler için dinleme ve alındığında bazı eylemler gerçekleştirme. Birden çok aboneye aynı ileti için dinleniyor olması.
-  **Gönderme** -görev alacak dinleyiciler için ileti yayımlama. Hiçbir dinleyicileri aboneliğiniz varsa iletiyi göz ardı edilir.


`MessagingService` İle statik bir sınıftır `Subscribe` ve `Send` çözüm kullanılan yöntemleri.

İletileri sahip bir dize `message` şekilde olarak kullanılan parametre *adresi* iletileri. `Subscribe` Ve `Send` yöntemleri iletilerin nasıl teslim daha fazla denetlemek için genel parametreler kullanın - iki iletileri ile aynı `message` metin ancak farklı genel tür bağımsız değişkenleri teslim edilemiyor aynı abonelik.

API için `MessagingCenter` basittir:

-  Abone&lt;TSender > (nesne abone, dize ileti, eylem&lt;TSender > geri çağırma, TSender kaynak = null)
-  Abone&lt;TSender, TArgs > (nesne abone, dize ileti, eylem&lt;TSender, TArgs > geri çağırma, TSender kaynak = null)
-  Gönderme&lt;TSender > (TSender gönderen, dize ileti)
-  Gönderme&lt;TSender, TArgs > (TSender gönderen, dize ileti TArgs bağımsız değişken)
-  Aboneliği&lt;TSender, TArgs > (nesne abone, dize ileti)
-  Aboneliği&lt;TSender > (nesne abone, dize ileti)


Bu yöntemleri aşağıda açıklanmıştır.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>MessagingCenter kullanma

Kullanıcı-etkileşimi (bir düğmeye tıklayın gibi), bir sistem olayı (gibi denetimleri durumunu değiştirme) veya (zaman uyumsuz yükleme Tamamlanıyor bir gibi) diğer bazı olay sonucunda iletileri gönderilebilir. Kullanıcı arabirimi görünümünü değiştirme, verileri kaydetmek veya başka bir işlem tetiklemek için aboneleri dinleniyor olması.

### <a name="simple-string-message"></a>Basit bir dize iletisi

Bir dize yalnızca basit iletisini içeren `message` parametresi. A `Subscribe` yöntemi, *dinler* basit dize ileti - aşağıda gösterilen için gönderen türünde olması bekleniyor belirtme genel tür fark `MainPage`. Çözümdeki tüm sınıflar bu söz dizimini kullanarak ileti abone olabilirsiniz:

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

İçinde `MainPage` aşağıdaki kodu sınıf *gönderir* ileti. `this` Parametredir örneği `MainPage`.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

Dize değiştirmez - gösterir *ileti türü* ve bildirmek için hangi aboneleri belirlemek için kullanılır. Bu tür bir ileti bazı olay, "tamamlandı karşıya yükleme gibi" oluştu olduğunu göstermek için kullanılan daha fazla bilgi burada gereklidir.

### <a name="passing-an-argument"></a>Bağımsız değişken geçirme

İleti bir bağımsız değişken geçirmek için bağımsız değişken türü belirtin, `Subscribe` genel değişkenleri ve eylem imzası.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

Bağımsız değişken içermeyen ileti göndermek için türünü genel parametresi ve bağımsız değişkeninin değeri içeren `Send` yöntem çağrısı.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

Bu basit bir örnek kullanan bir `string` bağımsız değişken ancak herhangi bir C# nesne geçirilebilir.

### <a name="unsubscribe"></a>Aboneliği Kaldır

Böylece gelecekteki ileti teslim bir nesne bir ileti imzadan aboneliğinizi iptal edebilirsiniz. `Unsubscribe` Yöntem sözdizimi iletinin imzası yansıtması (Bu nedenle ileti bağımsız değişkeni için genel tür parametresi eklemeniz gerekebilir).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>Özet

MessagingCenter özellikle görünüm modelleri arasında bağ azaltmak için basit bir yoludur. Göndermek ve basit iletileri almasına veya bağımsız değişken sınıflar arasında geçirmek için kullanılabilir. Sınıfları artık almak için istedikleri iletilerden aboneliği.


## <a name="related-links"></a>İlgili bağlantılar

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms Örnekleri](https://github.com/xamarin/xamarin-forms-samples)
