---
title: Bölüm 6 özeti. Düğme tıklamaları
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 6 özeti. Düğme tıklamaları'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: f06d0b312422889072be634768611ea1cc25088d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997178"
---
# <a name="summary-of-chapter-6-button-clicks"></a>Bölüm 6 özeti. Düğme tıklamaları

[ `Button` ](xref:Xamarin.Forms.Button) Komutu başlatmak kullanıcıya izin veren bir görünümdür. A `Button` metin tarafından tanımlanan (ve isteğe bağlı olarak gösterildiği şekilde görüntü [13. bölüm, bit eşlemler](chapter13.md)). Sonuç olarak, `Button` aynı özelliklerin çoğu tanımlar `Label`:

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

`Button` Ayrıca üç özelliklerini tanımlar kenarlığını görünümünü yönetir, ancak bu özellikler ve bunların karşılıklı bağımsızlığı desteklenmesi platforma bağlıdır:

- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) türü `Color`
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) türü `Double`
- [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) türü `Double`

`Button` Ayrıca tüm özelliklerini devralır `VisualElement` ve `View`de dahil olmak üzere `BackgroundColor`, `HorizontalOptions`, ve `VerticalOptions`.

## <a name="processing-the-click"></a>Tıklayarak işleme

`Button` Sınıfı tanımlayan bir [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) kullanıcı dokunduğunda harekete geçirilen olay `Button`. `Click` İşleyicisidir türünü `EventHandler`. İlk bağımsız değişken `Button` olay oluşturma nesne; ikinci bağımsız değişkeni bir `EventArgs` ek bilgi sağlayan nesne.

[ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) örnek gösterir basit `Clicked` işleme.

## <a name="sharing-button-clicks"></a>Düğmeyi paylaşmayı tıklar

Birden çok `Button` görünümleri aynı paylaşabilir `Clicked` işleyicisi, ancak işleyici genellikle gerektiğini belirleyen `Button` için belirli bir olay sorumludur. Bir yaklaşım ise çeşitli depolamak için `Button` alanları ve hangisinin olay işleyicisinde tetikleme denetimi olarak nesne.

[ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) örnek, bu teknik gösterir. Program ayrıca nasıl ayarlanacağı gösterilmektedir [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) özelliği bir `Button` için `false` basıldığında `Button` artık geçerli değil. Devre dışı bir `Button` oluşturmaz bir `Clicked` olay.

## <a name="anonymous-event-handlers"></a>Anonim olay işleyicileri

Tanımlamak mümkündür `Clicked` lambda anonim işlevler olarak işleyicileri olarak [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) örnek gösterir. Ancak, bazı kullanmak istemiyor yansıma kodu olmadan anonim işleyicileri paylaşılamaz.

## <a name="distinguishing-views-with-ids"></a>Kimlikleri görünümlerle ayırt etme

Birden çok `Button` nesneleri ayrıca ayırt edilebilir ayarlayarak [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) özelliği veya [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) özelliğini bir `string`. Bu özellik tarafından tanımlanan `Element` ancak Xamarin.Forms içinde kullanılmaz. Yalnızca uygulama programları tarafından kullanılmak üzere tasarlanmıştır.

[ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) örnek bir sayısal tuş takımındaki tüm 10 sayı anahtarları için aynı olay işleyicisi kullanır ve bunları ayırt `StyleId` özelliği:

[![Üç ekran görüntüsü basit tuş](images/ch06fg04-small.png "hesaplayıcı")](images/ch06fg04-large.png#lightbox "hesaplayıcı")

## <a name="saving-transient-data"></a>Geçici verileri kaydetme

Birçok uygulama, bir program sonlandırıldığında veri kaydetmek ve programı yeniden başlatıldığında, bu verileri yeniden yüklemek için gerekir. [ `Application` ](xref:Xamarin.Forms.Application) Programınızı kaydedin ve geçici veri geri yükleme yardımcı çeşitli üyeleri sınıf tanımlar:

- [ `Properties` ](xref:Xamarin.Forms.Application.Properties) Özelliği ile bir sözlük `string` anahtarları ve `object` öğeleri. Sözlüğün içeriğini otomatik olarak uygulama, program sonlanması öncesinde yerel depolama alanı kaydedilir ve programı başlatıldığında yeniden.
- `Application` Sınıfı tanımlayan üç korumalı sanal yöntemler program standart `App` sınıfı geçersiz kılmalar: [ `OnStart` ](xref:Xamarin.Forms.Application.OnStart), [ `OnSleep` ](xref:Xamarin.Forms.Application.OnSleep), ve [ `OnResume` ](xref:Xamarin.Forms.Application.OnResume). Bunlar başvurmak *uygulama yaşam döngüsü* olayları.
- [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) Yöntemi sözlüğün içeriğini kaydeder.

Bu çağrı gerekli değildir `SavePropertiesAsync`. Sözlüğün içeriğini otomatik olarak program sonlanması öncesinde kaydedilir ve program başlatma önce alınır. Program çöktüğünde verileri kaydetmek için program test sırasında yararlı olacaktır.

Yararlı olur:

- [`Application.Current`](xref:Xamarin.Forms.Application.Current), geçerli döndüren statik özelliğe `Application` ardından elde etmek için kullanabileceğiniz nesne `Properties` sözlüğü.

İlk adım, program sonlandırıldığında kalıcı hale getirmek istediğiniz sayfasında tüm değişkenleri belirlemektir. Burada bu değişkenleri değiştirme her yerde biliyorsanız, yalnızca kendisine ekleyebileceğiniz `Properties` bu noktada sözlüğü. Sayfanın oluşturucuda değişkenleri ayarlayabilirsiniz `Properties` anahtar varsa sözlüğü.

Daha büyük bir program, büyük olasılıkla ile uygulama yaşam döngüsü olaylarını dağıtılacak gerekir. En önemlisi `OnSleep` yöntemi. Bu yönteme bir çağrı, program ön ayrıldığını gösterir. Belki de kullanıcı bastığını **giriş** cihazda düğme veya görüntülenen tüm uygulamaları ya da telefon kapatılıyor. Bir çağrı `OnSleep` yalnızca bildirim sonlandırılmadan önce bir programı alır. Program emin olmak için bu fırsattan yararlanarak `Properties` sözlüktür güncel.

Bir çağrı `OnResume` program son çağrısından sona ermedi değil olduğunu gösteren `OnSleep` ancak şu anda ön planda yeniden çalışıyor. Program, internet bağlantılarını (örneğin) yenilemek için bu fırsatı kullanabilir.

Bir çağrı `OnStart` program başlatma sırasında gerçekleşir. Erişim için bu yöntemi çağırmak kadar beklemek değil `Properties` sözlük içeriği zaten bırakıldığından geri `App` Oluşturucu çağrılır.

[ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) örnek çok benzer **SimplestKeypad** program kullanması hariç, `OnSleep` geçerli tuş girişi kaydetmek için geçersiz kılın ve verileri geri yüklemek için sayfa Oluşturucusu.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 6 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Bölüm 6 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Bölüm 6 F # örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
