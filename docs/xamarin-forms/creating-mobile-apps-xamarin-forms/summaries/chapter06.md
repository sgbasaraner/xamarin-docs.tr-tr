---
title: Bölüm 6 özeti. Düğme tıklama
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a45d1f1663b141912744e83763e7d618866159d7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-6-button-clicks"></a>Bölüm 6 özeti. Düğme tıklama

[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Bir komut başlatmak kullanıcıya izin veren bir görünümdür. A `Button` metin tarafından tanımlanır (ve örnekte gösterildiği gibi isteğe bağlı olarak bir resim [Bölüm 13, bit eşlemler](chapter13.md)). Sonuç olarak, `Button` aynı özelliklerin çoğu tanımlar `Label`:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/)
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontFamily/)
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/)
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontAttributes/)
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/)

`Button` Ayrıca üç özelliklerini tanımlar kenarlığını görünümünü belirleyen, ancak bu özellikler ve bunların karşılıklı bağımsızlığı desteklenmesi platforma bağlıdır:

- [`BorderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderColor/) türü `Color`
- [`BorderWidth`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderWidth/) türü `Double`
- [`BorderRadius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/) türü `Double`

`Button` Ayrıca tüm özelliklerini devralır `VisualElement` ve `View`dahil `BackgroundColor`, `HorizontalOptions`, ve `VerticalOptions`.

## <a name="processing-the-click"></a>Öğesini işleme

`Button` Sınıfı tanımlayan bir [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) kullanıcı dokunur zaman tetikleyen olayı `Button`. `Click` İşleyicisidir türü `EventHandler`. İlk bağımsız değişken `Button` ; ikinci bağımsız değişkeni olay oluşturma nesnesinin bir `EventArgs` ek bilgi yok sağlayan nesne.

[ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) örnek gösterilmektedir basit `Clicked` işleme.

## <a name="sharing-button-clicks"></a>Düğme paylaşımı tıklar

Birden çok `Button` görünümleri aynı paylaşabilir `Clicked` işleyici ancak işleyici genellikle gerekiyor belirleyen `Button` için belirli bir olay sorumludur. Bir yaklaşım ise çeşitli depolamak için `Button` nesneleri alanları ve hangisinin işleyicisinde olay tetikleme onay olarak.

[ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) örneği, bu teknik gösterir. Program ayrıca nasıl ayarlanacağını gösterir [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) özelliği bir `Button` için `false` basıldığında `Button` artık geçerli değil. Devre dışı A `Button` oluşturmayacak bir `Clicked` olay.

## <a name="anonymous-event-handlers"></a>Anonim olay işleyicileri

Tanımlamak mümkündür `Clicked` anonim lambda işlev olarak işleyicileri olarak [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) örnek gösterilmektedir. Ancak, bazı düzensiz yansıma kodu olmadan anonim işleyicileri paylaşılamaz.

## <a name="distinguishing-views-with-ids"></a>Kimlikleri ayırt görünümleri

Birden çok `Button` nesneleri de ayırt ayarlayarak [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) özelliği veya [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) özelliğine bir `string`. Bu özellik tarafından tanımlanan `Element` ancak Xamarin.Forms içinde kullanılmaz. Yalnızca uygulama programlar tarafından kullanılmak üzere tasarlanmıştır.

[ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) örnek sayısal tuş takımında tüm 10 sayı anahtarları için aynı olay işleyicisi kullanır ve aralarında ayırt `StyleId` özelliği:

[![Üçlü ekran görüntüsü basit tuş](images/ch06fg04-small.png "hesaplayıcı")](images/ch06fg04-large.png#lightbox "hesaplayıcısı")

## <a name="saving-transient-data"></a>Geçici verileri kaydetme

Birçok uygulama, bir program sonlandırıldığında verileri kaydetmek için ve programı yeniden başlatıldığında bu verileri yeniden yüklemek için gerekir. [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) Sınıfı programınızı kaydetme ve geçici veri geri yükleme yardımcı birkaç üyeleri tanımlar:

- [ `Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) Özelliktir ile bir sözlük `string` anahtarları ve `object` öğeleri. Sözlüğün içeriğini otomatik olarak uygulama yerel depolama program sonlandırma önce kaydedilmiş ve program başlatıldığında yeniden yüklendi.
- `Application` Sınıfı tanımlayan üç korumalı sanal yöntemler program standart `App` sınıfı geçersiz kılmalar: [ `OnStart` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnStart()/), [ `OnSleep` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnSleep()/), ve [ `OnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnResume()/). Bunlar başvurmak *uygulama yaşam döngüsü* olaylar.
- [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) Yöntemi sözlüğün içeriğini kaydeder.

Bu çağrı gerekli değildir `SavePropertiesAsync`. Sözlüğün içeriğini otomatik olarak program sonlandırma önce kaydedilmiş ve program başlatma önce alınır. Program kilitlenirse bu durum verileri kaydetmek için program test sırasında yararlı olacaktır.

Ayrıca yararlıdır:

- [`Application.Current`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Current/), geçerli döndüren statik özelliğe `Application` sonra elde etmek için kullanabileceğiniz nesne `Properties` sözlük.

İlk adım, program sonlandırıldığında kalıcı hale getirmek istediğiniz sayfasında tüm değişkenleri belirlemektir. Değişkenlere buradan tüm yerlerde biliyorsanız, yalnızca kendisine ekleyebileceğiniz `Properties` bu noktada sözlük. Sayfanın oluşturucuda değişkenlerinden ayarlayabilirsiniz `Properties` anahtar varsa sözlük.

Büyük olasılıkla daha büyük bir program, uygulama yaşam döngüsü olayları ile ilgili yapmanız gerekir. En önemlisi `OnSleep` yöntemi. Bu yöntem çağrısı program ön ayrıldığını gösterir. Kullanıcı belki de bastığı **giriş** düğmesini cihazda veya tüm uygulamaların görüntülenen ya da telefon kapatılıyor. Çağrı `OnSleep` yalnızca bildirim sonlanmadan önce bir programı alır. Program emin olmak için bu fırsatı gerçekleştirmesi gereken `Properties` sözlük güncel.

Çağrı `OnResume` program son çağrısından sonlandırılmadı gösterir `OnSleep` ancak ön planda yeniden şu anda çalışıyor. Program (örneğin) Internet bağlantılarını yenilemek için bu fırsatı kullanabilir.

Çağrı `OnStart` program başlatma sırasında oluşur. Bu yöntemi çağırmak için erişim beklemek gerekli değil `Properties` sözlük içeriği zaten silinmiş olduğundan geri `App` Oluşturucusu çağrılır.

[ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) örnek çok benzer **SimplestKeypad** program kullanır ancak bu `OnSleep` geçerli tuş girdiyi kaydetmek için geçersiz kılın ve Bu verileri geri yüklemek için sayfa Oluşturucusu.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 6 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Bölüm 6 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Bölüm 6 F # örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
