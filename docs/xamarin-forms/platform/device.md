---
title: Xamarin.Forms cihaz sınıfı
description: Bu makalede, işlevsellik ve düzenleri üzerinde ayrıntılı denetim platformu başına temelinde Xamarin.Forms cihazı sınıfını kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: ff707cdf73665ae07881d2d17ec837a4cfacaca0
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935377"
---
# <a name="xamarinforms-device-class"></a>Xamarin.Forms cihaz sınıfı

[ `Device` ](xref:Xamarin.Forms.Device) Sınıfı bir sayı düzeninde ve işlevlerinde platform başına temelinde özelleştirme geliştiricilere yardımcı olmak için yöntemleri ve özellikleri içerir.

Yöntemleri ve belirli donanım türleri ve boyutları, hedef kodun özelliklerine ek olarak `Device` sınıfı içeren [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) denetimler kullanıcı Arabirimi ile etkileşim kurduğunuzda kullanılmalıdır yöntemi arka plan iş parçacıkları.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>Platforma özgü değerlerini sağlama

Xamarin.Forms 2.3.4 önce üzerinde uygulamayı çalıştırdığınız olan platform inceleyerek elde edilemedi [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) özelliği ve onu [ `TargetPlatform.iOS` ](xref:Xamarin.Forms.TargetPlatform.iOS), [ `TargetPlatform.Android` ](xref:Xamarin.Forms.TargetPlatform.Android), [ `TargetPlatform.WinPhone` ](xref:Xamarin.Forms.TargetPlatform.WinPhone), ve [ `TargetPlatform.Windows` ](xref:Xamarin.Forms.TargetPlatform.Windows) sabit listesi değerleri. Benzer şekilde, aşağıdakilerden birini [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) aşırı yüklemeler, platforma özgü değerler için bir denetim sağlamak için kullanılabilir.

Ancak, 2.3.4 Xamarin.Forms beri bu API'leri kullanım dışı değiştirildi ve. [ `Device` ](xref:Xamarin.Forms.Device) Sınıfı artık platformları – tanımlayan genel dize sabitleri içerir [ `Device.iOS` ](xref:Xamarin.Forms.Device.iOS), [ `Device.Android` ](xref:Xamarin.Forms.Device.Android), `Device.WinPhone`() kullanım dışı) `Device.WinRT` (kullanım dışı), [ `Device.UWP` ](xref:Xamarin.Forms.Device.UWP), ve [ `Device.macOS` ](xref:Xamarin.Forms.Device.macOS). Benzer şekilde, [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) aşırı yüklemeleri ile değiştirildi [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) ve [ `On` ](xref:Xamarin.Forms.On) API'leri.

C# içinde platforma özgü değerleri oluşturarak sağlanabilir bir `switch` açıklamamızı [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) özellik ve daha sonra sağlama `case` deyimleri gerekli platformlar için:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Ve [ `On` ](xref:Xamarin.Forms.On) sınıfları XAML aynı işlevleri sağlar:

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Sınıfı bir genel sınıftır ve böylece ile örneği oluşturulmalıdır bir `x:TypeArguments` hedef türüyle eşleşen öznitelik. İçinde [ `On` ](xref:Xamarin.Forms.On) sınıfı [ `Platform` ](xref:Xamarin.Forms.On.Platform) özniteliği tek bir kabul edebilir `string` değeri veya virgülle ayrılmış birden çok `string` değerleri.

> [!IMPORTANT]
> Yanlış bir sağlama `Platform` öznitelik değeri `On` sınıf bir hata değil neden olur. Bunun yerine, kodu uygulanmakta platforma özgü değer olmadan yürütülür.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom` Düzenleri veya işlevler üzerinde uygulamayı çalıştırdığınız cihaz bağlı olarak değiştirmek için kullanılabilir. [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom) Numaralandırma aşağıdaki değerleri içerir:

-  **Telefon** – iPhone, iPod touch ve Android cihazlarda 600 dıps dar ^
-  **Tablet** – iPad, Windows cihazları ve Android cihazlarda 600 dıps geniş ^
-  **Masaüstü** – döndürülen yalnızca [UWP uygulamaları](~/xamarin-forms/platform/windows/installation/index.md) Windows 10 masaüstü bilgisayarlarda (döndürür `Phone` Continuum senaryolarda da dahil olmak üzere mobil Windows cihazlarda)
-  **TV** – Tizen TV cihazları
-  **Desteklenmeyen** – kullanılmayan

*^ dıps değildir fiziksel piksel sayısı*

`Idiom` Bu gibi daha büyük ekranlar yararlanan düzenleri oluşturmak için özellikle yararlı olur:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

## <a name="deviceflowdirection"></a>Device.FlowDirection

[ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Değerini alır bir [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) cihaz tarafından kullanılan geçerli akış yönü temsil eden bir numaralandırma değeri. Akış yönü sayfasında kullanıcı Arabirimi öğeleri gözle taranır yönüdür. Sabit listesi değerleri şunlardır:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

XAML içinde [ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) değeri kullanılarak alınabilir `x:Static` işaretleme uzantısı:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

C# ' de eşdeğer kodu verilmiştir:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Akış yönü hakkında daha fazla bilgi için bkz: [sağdan sola yerelleştirme](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

[ `Styles` Özelliği](~/xamarin-forms/user-interface/styles/index.md) bazı denetimler için uygulanabilir yerleşik stil tanımları içerir (gibi `Label`) `Style` özelliği. Kullanılabilir stiller şunlardır:

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` ayarlarken kullanılabilir [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) C# kodunda:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

<a name="Device_OpenUri" />

## <a name="deviceopenuri"></a>Device.OpenUri

`OpenUri` Yöntemi, temel alınan platformda yerel web tarayıcısında açık bir URL gibi işlemleri tetiklemek için kullanılabilir (**Safari** ios'ta veya **Internet** Android).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[WebView örnek](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) kullanmaya bir örnek içeren `OpenUri` URL'leri açmasına ve ayrıca aramaları tetiklersiniz.

[Haritalar örnek](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) de kullanır `Device.OpenUri` eşlemeleri ve yerel kullanma yönergeleri görüntülemek için **eşler** iOS ve Android uygulamaları.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

`Device` Sınıfı de sahip bir `StartTimer` .NET Standard kitaplığı dahil olmak üzere Xamarin.Forms ortak kod içinde çalıştığı zamana bağımlı görevler tetiklemek için basit bir yol sağlayan bir yöntem. Başarılı bir `TimeSpan` aralığını ayarlama ve döndürme için `true` süreölçer tutmak veya `false` sonra mevcut çağırma durdurmak için.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Zamanlayıcı içindeki kod kullanıcı arabirimiyle etkileşim varsa (metnini ayarlamak gibi bir `Label` veya bir uyarı görüntüleme) içinde yapılmalıdır bir `BeginInvokeOnMainThread` ifade (aşağıya bakın).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

Kullanıcı arabirimi öğeleri hiçbir zaman bir zamanlayıcı ya da web istekleri gibi zaman uyumsuz işlemleri tamamlama işleyicisi çalışan kod gibi arka plan iş parçacığı tarafından erişilmelidir. Kullanıcı arabirimini güncelleştirmek için gereken herhangi bir arka plan kod içine alınmalı [ `BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/). Bu eşdeğerdir `InvokeOnMainThread` ios'ta `RunOnUiThread` , Android ve `Dispatcher.RunAsync` Evrensel Windows platformu üzerinde.

Xamarin.Forms kodu verilmiştir:

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Not Bu yöntemleri kullanarak `async/await` kullanmanıza gerek yoktur `BeginInvokeOnMainThread` ana UI iş parçacığından çalıştırıyorsanız.

## <a name="summary"></a>Özet

Xamarin.Forms `Device` ortak kodu (.NET Standard kitaplığı projeleri veya paylaşılan projeler) bile - sınıfı platformu başına temelinde işlevselliği ve düzenleri üzerinde ayrıntılı denetim sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Cihaz örneği](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Örnek stilleri](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Cihaz](xref:Xamarin.Forms.Device)
