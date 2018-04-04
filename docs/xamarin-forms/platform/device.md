---
title: Aygıt sınıfı
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 7ba3808e7b8d948d502be3f80b8830e1aaf3b52f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="device-class"></a>Aygıt sınıfı

[ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Sınıfı, özellikleri ve yöntemleri düzeni ve platform başına temelinde işlevselliği özelleştirme geliştiricilere yardımcı olmak için çeşitli içerir.

Yöntemleri ve belirli donanım türleri ve boyutları, hedef kodu özelliklerine ek olarak `Device` sınıfı içerir [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) gelen kullanıcı Arabirimi ile etkileşim denetimleri bağlandığınızda kullanılmalıdır yöntemi arka plan iş parçacıkları.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>Platforma özgü değerler sağlama

Xamarin.Forms 2.3.4 önce uygulama üzerinde çalışıyordu platform inceleyerek elde edilemedi [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) özelliği ve kendisine karşılaştırma [ `TargetPlatform.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/), [ `TargetPlatform.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/), [ `TargetPlatform.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), ve [ `TargetPlatform.Windows` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) numaralandırma değerleri. Benzer şekilde, aşağıdakilerden birini [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) aşırı bir denetime platforma özgü değerlerini sağlamak için kullanılabilir.

Ancak, 2.3.4 Xamarin.Forms itibaren bu API'leri kullanım dışı değiştirildi ve. [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Sınıfı şimdi platformları – tanımlamak ortak dize sabitleri içerir [ `Device.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), [ `Device.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), [ `Device.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/), [ `Device.WinRT` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinRT/), [ `Device.UWP` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), ve [ `Device.macOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.macOS/). Benzer şekilde, [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) aşırı değiştirilir ile [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) ve [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) API'leri.

C# ' ta platforma özgü değerleri oluşturarak sağlanabilir bir `switch` on deyimi [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) özelliği ve ardından sağlama `case` deyimleri gerekli platformlar için:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.WinPhone:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Ve [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) sınıfları XAML'de aynı işlevselliği sağlar:

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, WinPhone, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Sınıfı genel bir sınıftır ve ile böylece örneğinin oluşturulması gerekir bir `x:TypeArguments` hedef türüyle eşleşen özniteliği. İçinde [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) sınıfı, [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) özniteliği tek bir kabul edebileceği `string` değeri veya virgülle ayrılmış birden çok `string` değerleri.

> [!IMPORTANT]
> Yanlış bir sağlama `Platform` öznitelik değerinde `On` sınıfı bir hata neden. Bunun yerine, kod uygulanmakta platforma özgü değeri olmadan çalıştırır.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom` Düzenleri veya işlevselliği üzerinde uygulamayı çalıştırdığınız aygıt bağlı olarak değiştirmek için kullanılabilir. [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/) Numaralandırma aşağıdaki değerleri içerir:

-  **Telefon** – iPhone, iPod touch, Windows Phone, Android cihazları 600 dıps dar ^
-  **Tablet** – iPad, Windows 8.1 bilgisayarlar, Android cihazları 600 dıps geniş ^
-  **Masaüstü** – döndürdü yalnızca [UWP uygulamaları](~/xamarin-forms/platform/windows/installation/universal.md) Windows 10 masaüstü bilgisayarlara (döndürür `Phone` Continuum senaryolarda dahil olmak üzere mobil cihazlarda Windows,)
-  **TV** – Tizen TV cihazları
-  **Desteklenmeyen** – kullanılmayan

*^ dıps değildir fiziksel piksel sayısı*

`Idiom` Bu gibi daha büyük ekranlar yararlanmak düzenler oluşturmak için özellikle yararlı olur:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

[ `Styles` Özelliği](~/xamarin-forms/user-interface/styles/index.md) bazı denetimler için uygulanabilir yerleşik stil tanımlarını içeren (gibi `Label`) `Style` özelliği. Kullanılabilir stiller şunlardır:

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` ayarlarken kullanılan [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) C# kod:

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

`OpenUri` Yöntemi, bir URL yerel web tarayıcısında Aç gibi temel bir platform üzerinde işlemler tetiklemek için kullanılabilir (**Safari** iOS veya **Internet** android'de).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[WebView örnek](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) kullanarak bir örnek içeren `OpenUri` URL'leri açmasına ve ayrıca telefon aramaları tetikler.

[Haritalar örneği](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) de kullanır `Device.OpenUri` eşlemeleri ve yerel kullanma yönergeleri görüntülemek için **eşlemeleri** iOS ve Android uygulamalar.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

`Device` Sınıfı ayrıca sahip bir `StartTimer` Xamarin.Forms ortak kodun (PCLs dahil) içinde çalıştığı zamana bağımlı görevler tetiklemek için basit bir yol sağlayan yöntemi. Geçirmek bir `TimeSpan` aralığını ayarlayın ve dönmek için `true` süreölçer tutmak için veya `false` sonra geçerli çağırma durdurmak için.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Zamanlayıcı içinde kod kullanıcı arabirimiyle etkileşim varsa (metnini ayarlama gibi bir `Label` veya bir uyarı görüntüleme) içinde yapılmalıdır bir `BeginInvokeOnMainThread` ifade (aşağıya bakın).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

Kullanıcı arabirimi öğeleri, bir süreölçer ya da web istekleri gibi zaman uyumsuz işlemleri tamamlama işleyicisi çalışan kodu gibi arka plan iş parçacıkları tarafından hiçbir zaman erişilmelidir. Kullanıcı arabirimini güncelleştirmek için gereken herhangi bir arka plan kod içine alınmalı [ `BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/). Bu eşdeğerdir `InvokeOnMainThread` iOS, `RunOnUiThread` android'de ve `Dispatcher.BeginInvoke` Windows Phone üzerinde.

Xamarin.Forms kodu verilmiştir:

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Not Bu yöntemlerini kullanarak `async/await` kullanmasına gerek kalmamasını `BeginInvokeOnMainThread` ana kullanıcı Arabirimi iş parçacığından çalıştırıyorsanız.

## <a name="summary"></a>Özet

Xamarin.Forms `Device` sınıfı platformu başına temelinde işlevselliği ve düzenleri üzerinde ayrıntılı denetim sağlar - ortak kodu (PCL veya paylaşılan projeleri) bile.


## <a name="related-links"></a>İlgili bağlantılar

- [Aygıt örneği](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Örnek stilleri](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Cihazı](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)
