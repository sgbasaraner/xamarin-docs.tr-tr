---
title: iOS Platform özellikleri
description: Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar. Bu makalede, iOS platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/06/2018
ms.openlocfilehash: 98d4ce241c01bd09c68d86c583f12fdc7a11db0f
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39175196"
---
# <a name="ios-platform-specifics"></a>iOS Platform özellikleri

_Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar. Bu makalede, iOS platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir._

## <a name="visualelements"></a>VisualElements

İOS, platforma özgü aşağıdaki işlevleri Xamarin.Forms görünümleri, sayfalar ve düzenleri için sağlanır:

- Herhangi bir desteği bulanıklaştıran [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Daha fazla bilgi için [uygulama bulanıklaştıran](#blur).
- Desteklenen bir eski renk modunu devre dışı bırakma [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Daha fazla bilgi için [eski renk modunu devre dışı bırakma](#legacy-color-mode).
- Gölge etkinleştirme bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Daha fazla bilgi için [bir gölge etkinleştirme](#drop-shadow).

<a name="blur" />

### <a name="applying-blur"></a>Bulanıklaştırma uygulanıyor

Bu platforma özgü altında katmanlı içeriği bulanıklaştıran için kullanılır ve XAML içinde ayarlayarak tüketilen [ `VisualElement.BlurEffect` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty) ekli özellik değerine [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) sabit listesi:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
  ...
  <AbsoluteLayout HorizontalOptions="Center">
      <Image Source="monkeyface.png" />
      <BoxView x:Name="boxView" ios:VisualElement.BlurEffect="ExtraLight" HeightRequest="300" WidthRequest="300" />
  </AbsoluteLayout>
  ...
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

`BoxView.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `VisualElement.UseBlurEffect` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ile Bulanıklaştırma efekti uygulamak için kullanılan ad alanı, [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) dört sağlama numaralandırması değerler: [ `None` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None), [ `ExtraLight` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight), [ `Light` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light), ve [ `Dark` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark).

Sonuç belirtilen olan [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) uygulanan [ `BoxView` ](xref:Xamarin.Forms.BoxView) örneği, hangi Bulanıklaştırma [ `Image` ](xref:Xamarin.Forms.Image) altında katmanlı:

![](ios-images/blur-effect.png "Etkin platforma özgü bulanıklaştıran")

> [!NOTE]
> Bulanıklaştırma efekti eklerken bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), dokunma olayları hala tarafından alınan `VisualElement`.

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>Eski renk modunu devre dışı bırakma

Xamarin.Forms görünümlerinden bazılarını eski renk modu özelliği. Bu modda olduğunda [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) görünümün özelliği `false`, görünümü devre dışı durumu için varsayılan yerel renklerle kullanıcı tarafından ayarlanan renkleri geçersiz kılar. Geriye dönük uyumluluk, bu eski renk modu desteklenen görünümler için varsayılan davranış kalır için.

Hatta görünümü devre dışı bırakıldığında kullanıcı tarafından bir görünüm üzerinde ayarlanan renkleri kalmasını sağlamak bu platforma özgü bu eski renk modu devre dışı bırakır. XAML içinde ayarlayarak tüketilir [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsLegacyColorModeEnabledProperty) özelliğine bağlı `false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                ios:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

_legacyColorModeDisabledButton.On<iOS>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, eski renk modunu devre dışı olup olmadığını denetlemek için kullanılır. Ayrıca, [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) yöntemi, eski renk modunu devre dışı bırakılıp bırakılmadığını döndürmek için kullanılabilir.

Bir görünüm üzerinde kullanıcı tarafından ayarlanan renkleri görünümü devre dışı bırakıldığında bile kalır eski renk modunu devre dışı bırakılabilir olduğunu, oluşur:

![](ios-images/legacy-color-mode-disabled.png "Eski renk modu devre dışı")

> [!NOTE]
> Ayarlarken bir [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) bir görünüm üzerinde eski renk modunu tamamen yoksayılır. Görsel durumlar hakkında daha fazla bilgi için bkz: [Xamarin.Forms görsel durum Yöneticisi](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="drop-shadow" />

### <a name="enabling-a-drop-shadow"></a>Gölge etkinleştirme

Bu platforma özgü gölge etkinleştirmek için kullanılan bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). XAML içinde ayarlayarak tüketilir [ `VisualElement.IsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsShadowEnabledProperty) özelliğine bağlı `true`, gölge denetleyen özellikler birkaç ek birlikte isteğe bağlı:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <BoxView ...
                 ios:VisualElement.IsShadowEnabled="true"
                 ios:VisualElement.ShadowColor="Purple"
                 ios:VisualElement.ShadowOpacity="0.7"
                 ios:VisualElement.ShadowRadius="12">
            <ios:VisualElement.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </ios:VisualElement.ShadowOffset>
         </BoxView>
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var boxView = new BoxView { Color = Color.Aqua, WidthRequest = 100, HeightRequest = 100 };
boxView.On<iOS>()
       .SetIsShadowEnabled(true)
       .SetShadowColor(Color.Purple)
       .SetShadowOffset(new Size(10,10))
       .SetShadowOpacity(0.7)
       .SetShadowRadius(12);
```

`VisualElement.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `VisualElement.SetIsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsShadowEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, bir gölgenin etkin olup olmadığını denetlemek için kullanılan `VisualElement`. Ayrıca, gölge denetlemek için aşağıdaki yöntemlerden çağrılabilir:

- [`SetShadowColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.Color)) – gölge rengini ayarlar. Varsayılan renk [ `Color.Default` ](xref:Xamarin.Forms.Color.Default*).
- [`SetShadowOffset`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.Size)) – Gölge uzaklığı ayarlar. Uzaklık gölge türüne dönüştürülür ve belirtilen yönde değişiklikleri bir [ `Size` ](xref:Xamarin.Forms.Size) değeri. `Size` Yapısı değerleri uzaklık (negatif değer) sola veya sağa (pozitif) olan ilk değerini ve yukarıdaki uzaklığı olan ikinci değeri (negatif) ile ya da (pozitif) aşağıda CİHAZDAN bağımsız birimler cinsinden ifade edilir . Bu özelliğin varsayılan değeri (0.0, 0.0), her tarafındaki gölge olduğu sonuçlanır cast `VisualElement`.
- [`SetShadowOpacity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowOpacity(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Double)) – değeriyle 0.0 aralığında (opak) 1.0 (saydam) olan gölge opaklığını ayarlar. Varsayılan opaklık 0,5 değerdir.
- [`SetShadowRadius`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowRadius(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Double)) – Gölge işlemek için kullanılan Bulanıklaştırma RADIUS ayarlar. Varsayılan RADIUS 10.0 değerdir.

> [!NOTE]
> Gölge durumunu çağırarak sorgulanabilir [ `GetIsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsShadowEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), [ `GetShadowColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), [ `GetShadowOffset` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), [ `GetShadowOpacity` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowOpacity(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), ve [ `GetShadowRadius` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowRadius(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) yöntemleri.

Gölge üzerinde etkinleştirilebilir sonucu olan bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement):

![](ios-images/drop-shadow.png "Etkin gölge")

## <a name="views"></a>Görünümler

İOS, platforma özgü aşağıdaki işlevleri için Xamarin.Forms görünümleri sağlanır:

- Metin girilen sağlayarak en uygun içine bir [ `Entry` ](xref:Xamarin.Forms.Entry) yazı tipi boyutunu ayarlayarak. Daha fazla bilgi için [bir girdinin yazı tipi boyutunu ayarlama](#adjust_font_size).
- İmleç rengi ayarı bir [ `Entry` ](xref:Xamarin.Forms.Entry). Daha fazla bilgi için [giriş imleç rengi ayarlama](#entry-cursorcolor).
- Ayıraç stili ayarını bir [ `ListView` ](xref:Xamarin.Forms.ListView). Daha fazla bilgi için [ayıraç stili ayarı üzerinde bir ListView](#listview-separatorstyle).
- Öğe seçimi, oluştuğunda denetleme bir [ `Picker` ](xref:Xamarin.Forms.Picker). Daha fazla bilgi için [denetleme Seçici öğe seçimi](#picker_update_mode).
- Etkinleştirme [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) dokunarak bir konuma üzerinde ayarlamak için özellik [ `Slider` ](xref:Xamarin.Forms.Slider) çubuğu yerine sürükleyin gerek kalmadan `Slider` thumb. Daha fazla bilgi için [hareket dokunun için bir kaydırıcı tutamacın etkinleştirme](#slider-updateontap).

<a name="adjust_font_size" />

### <a name="adjusting-the-font-size-of-an-entry"></a>Bir girdinin yazı tipi boyutunu ayarlama

Bu platforma özel yazı tipi boyutunu ölçeklendirme için kullanılan bir [ `Entry` ](xref:Xamarin.Forms.Entry) girilen metin denetiminde sığmasını sağlamak için. XAML içinde ayarlayarak tüketilir [ `Entry.AdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty) özelliğine bağlı bir `boolean` değeri:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `Entry.EnableAdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, bunu uygun emin olmak için girilen metnin yazı tipi boyutu ölçeklendirme için kullanılan [ `Entry` ](xref:Xamarin.Forms.Entry). Ayrıca, [ `Entry` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry) sınıfını `Xamarin.Forms.PlatformConfiguration.iOSSpecific` aynı zamanda ad alanına sahip bir [ `DisableAdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) bu platforma devre dışı bırakan yöntemi ve bir [ `SetAdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry},System.Boolean)) çağırarak ölçeklendirme yazı tipi boyutunu değiştirmek için kullanılabilecek yöntemi [ `AdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) yöntemi:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Yazı tipi boyutu olan sonuç [ `Entry` ](xref:Xamarin.Forms.Entry) girilen metin denetiminde sığmasını sağlamak için ölçeği:

![](ios-images/entry-font-size.png "Giriş yazı tipi boyutu platforma ayarlayın")

<a name="entry-cursorcolor" />

### <a name="setting-the-entry-cursor-color"></a>Giriş imleç rengini ayarlama

İmleç rengi bu platforma özel ayarlar bir [ `Entry` ](xref:Xamarin.Forms.Entry) belirtilen bir renk. XAML içinde ayarlayarak tüketilir [ `Entry.CursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.CursorColorProperty) bağlanılabilir özellik için bir [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry ... ios:Entry.CursorColor="LimeGreen" />
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var entry = new Xamarin.Forms.Entry();
entry.On<iOS>().SetCursorColor(Color.LimeGreen);
```

`Entry.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `Entry.SetCursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetCursorColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry},Xamarin.Forms.Color)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, imleç rengi belirtilen bir ayarlar [ `Color` ](xref:Xamarin.Forms.Color). Ayrıca, [ `Entry.GetCursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.GetCursorColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) yöntemi, geçerli imleç rengi almak için kullanılabilir.

İmleç rengi sonucu olan bir [ `Entry` ](xref:Xamarin.Forms.Entry) belirli bir ayarlanabilir [ `Color` ](xref:Xamarin.Forms.Color):

![](ios-images/entry-cursorcolor.png "Giriş imleç rengi")

<a name="listview-separatorstyle" />

### <a name="setting-the-separator-style-on-a-listview"></a>Bir ListView üzerinde ayıraç stili ayarlama

Bu platforma özgü arasında ayıraç olarak hücreleri olup olmadığını denetleyen bir [ `ListView` ](xref:Xamarin.Forms.ListView) tam genişliğini kullanan `ListView`. XAML içinde ayarlayarak tüketilir [ `ListView.SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) ekli özellik değerine [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) sabit listesi:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

`ListView.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı içinde arasındaki ayırıcı hücreleri olup olmadığını denetlemek için kullanılan [ `ListView` ](xref:Xamarin.Forms.ListView) tam kullanır genişliği `ListView`, ile [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) iki olası değerler sağlayan bir sabit listesi:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) – Varsayılan iOS ayırıcı davranış gösterir. Xamarin.Forms varsayılan davranışı budur.
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) – bir kenarından ayırıcılar çizileceğini belirten `ListView` diğer.

Sonuç belirtilen olan [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) değeri uygulanan [ `ListView` ](xref:Xamarin.Forms.ListView), hücre arasında ayıraç genişliği denetler:

![](ios-images/listview-separatorstyle.png "ListView SeparatorStyle platforma özgü")

> [!NOTE]
> Ayıraç stili ayarlandıktan sonra `FullWidth`, geri değiştirilemez `Default` zamanında.

<a name="picker_update_mode" />

### <a name="controlling-picker-item-selection"></a>Denetim Seçici öğe seçimi

Bu platforma özel denetimleri içinde öğe seçimi oluştuğunda bir [ `Picker` ](xref:Xamarin.Forms.Picker), öğe seçimi denetimindeki öğeleri göz atarken ya da yalnızca bir kez gerçekleşir belirtmesini sağlayan **Bitti** düğmeye basıldığında. XAML içinde ayarlayarak tüketilir `Picker.UpdateMode` ekli özellik değerine `UpdateMode` sabit listesi:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. `Picker.SetUpdateMode` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, öğe seçimi oluştuğunda, kontrol etmek için kullanılan ile `UpdateMode` iki olası değerler sağlayan bir sabit listesi:

- `Immediately` – öğe seçimi kullanıcı öğelerinde gider gibi gerçekleşir [ `Picker` ](xref:Xamarin.Forms.Picker). Xamarin.Forms varsayılan davranışı budur.
- `WhenFinished` – öğe seçimi kullanıcı bastığını sonra yalnızca gerçekleşir **Bitti** düğmesine [ `Picker` ](xref:Xamarin.Forms.Picker).

Ayrıca, `SetUpdateMode` yöntemi çağırarak numaralandırma değerlerinden geçiş yapmak için kullanılabilir `UpdateMode` geçerli döndüren yöntemi `UpdateMode`:

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Sonuç belirtilen olan `UpdateMode` uygulanan [ `Picker` ](xref:Xamarin.Forms.Picker), öğe seçimi oluştuğunda denetler:

[![](ios-images/picker-updatemode.png "Seçici UpdateMode platforma özgü")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="slider-updateontap" />

### <a name="enabling-a-slider-thumb-to-move-on-tap"></a>Bir kaydırıcı tutamacın hareket dokunun için etkinleştirme

Bu platforma özgü sağlayan [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) dokunarak bir konuma üzerinde ayarlamak için özellik [ `Slider` ](xref:Xamarin.Forms.Slider) çubuğu yerine sürükleyin gerek kalmadan `Slider` thumb. XAML içinde ayarlayarak tüketilir [ `Slider.UpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) bağlanılabilir özellik için `true`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

`Slider.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `Slider.SetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.SetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, bir Dokunuş açık olup olmadığını denetlemek için kullanılan `Slider` çubuğu olarak ayarlanmadıysa [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) özellik. Ayrıca, [ `Slider.GetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.GetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider})) yöntemi döndürmek için kullanılabilir bir Dokunuş açık olup olmadığını `Slider` çubuğu olarak ayarlanmadıysa `Slider.Value` özelliği.

Sonucu olan bir tap [ `Slider` ](xref:Xamarin.Forms.Slider) çubuğu taşıyabilirsiniz `Slider` ayarlayın ve küçük resim [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) özelliği:

![](ios-images/slider-updateontap.png "Kaydırıcı güncelleştirmesinde etkin dokunun")

## <a name="pages"></a>Sayfaları

İos'ta Xamarin.Forms sayfaları için aşağıdaki platforma özgü işlevleri sağlanır:

- Gezinti çubuğu ayırıcı üzerinde gizleme bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Daha fazla bilgi için [bir NavigationPage gezinti çubuğu ayırıcı gizleme](#navigationpage-hideseparatorbar).
- Gezinti çubuğu saydam olup olmadığını denetliyor. Daha fazla bilgi için [gezinti çubuğu saydam hale](#translucent_navigation_bar).
- Denetleme üzerinde durum çubuğu metni rengi olup olmadığını bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) gezinti çubuğunun parlaklık eşleşecek şekilde ayarlanır. Daha fazla bilgi için [durum çubuğunda metin rengi modunu ayarlama](#status_bar_color_mode).
- Sayfa gezinti çubuğunda büyük bir başlık olarak sayfa başlığının görüntülenip görüntülenmeyeceğini denetleme. Daha fazla bilgi için [görüntüleme büyük başlıklar](#large_title).
- Durum çubuğunun görünürlüğünü ayarını bir [ `Page` ](xref:Xamarin.Forms.Page). Daha fazla bilgi için [durum çubuğunun görünürlüğünü bir sayfada ayarlama](#set_status_bar_visibility).
- Bu sayfa içeriği sağlamak, tüm iOS cihazları için güvenlidir ekran alanı konumlandırıldı. Daha fazla bilgi için [güvenli alan düzeni Kılavuzu etkinleştirme](#safe_area_layout).

<a name="navigationpage-hideseparatorbar" />

### <a name="hiding-the-navigation-bar-separator-on-a-navigationpage"></a>Gezinti çubuğunu gizlemek bir NavigationPage ayırıcı

Ayırıcı çizginin ve gezinti çubuğunun alt kısmında bulunan açıktır gölge bu platforma özgü gizler bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). XAML içinde ayarlayarak tüketilir [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) bağlanılabilir özellik için `false`:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ios:NavigationPage.HideNavigationBarSeparator="true">

</NavigationPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;

public class iOSTitleViewNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public iOSTitleViewNavigationPageCS()
    {
        On<iOS>().SetHideNavigationBarSeparator(true);
    }
}
```

`NavigationPage.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `NavigationPage.SetHideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetHideNavigationBarSeparator(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, gezinti çubuğu ayırıcı gizli olup olmadığını denetlemek için kullanılır. Ayrıca, [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparator(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) yöntemi, gezinti çubuğu ayırıcı gizlenip gizlenmeyeceğini döndürmek için kullanılabilir.

Gezinti çubuğu ayırıcı sonucu olan bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) gizlenmiş olabilir:

![](ios-images/navigationpage-hideseparatorbar.png "Gizli NavigationPage gezinti çubuğu")

<a name="translucent_navigation_bar" />

### <a name="making-the-navigation-bar-translucent"></a>Gezinti çubuğu saydam hale getirme

Bu platforma özgü gezinti çubuğunun saydamlık değiştirmek için kullanılır ve XAML içinde ayarlayarak tüketilen [ `NavigationPage.IsNavigationBarTranslucent` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty) özelliğine bağlı bir `boolean` değeri:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `NavigationPage.EnableTranslucentNavigationBar` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, gezinti çubuğunda Saydam yapmak için kullanılır. Ayrıca, [ `NavigationPage` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage) sınıfını `Xamarin.Forms.PlatformConfiguration.iOSSpecific` aynı zamanda ad alanına sahip bir [ `DisableTranslucentNavigationBar` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) gezinti çubuğu varsayılan durumuna geri yükler yöntemi ve bir [ `SetIsNavigationBarTranslucent` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},System.Boolean)) çağırarak gezinti çubuğu saydamlık geçiş yapmak için kullanılan yöntem [ `IsNavigationBarTranslucent` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) yöntemi:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Gezinti çubuğu saydamlığını değiştirilebilir oluşur:

![](ios-images/translucent-navigation-bar.png "Platforma özgü saydam gezinti çubuğu")

<a name="status_bar_color_mode" />

### <a name="adjusting-the-status-bar-text-color-mode"></a>Durum çubuğunda metin rengi modunu ayarlama

Bu platforma özel denetimler hakkında durum çubuğu metni rengi olup olmadığını bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) gezinti çubuğunun parlaklık eşleşecek şekilde ayarlanır. XAML içinde ayarlayarak tüketilir [ `NavigationPage.StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty) ekli özellik değerine [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) sabit listesi:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

`NavigationPage.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `NavigationPage.SetStatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, denetim üzerinde durum çubuğu metni rengi olmadığını [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) eşleşecek şekilde ayarlandı Gezinti çubuğunda, parlaklığını ile [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) iki olası değerler sağlayan bir sabit listesi:

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) – Durum çubuğunda metin rengi ayarlanamadı gösterir.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) – Durum çubuğunda metin rengi gezinti çubuğunun parlaklık eşleşmelidir gösterir.

Ayrıca, [ `GetStatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) yöntemi, mevcut değerini almak için kullanılabilir [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) uygulanan numaralandırma [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage).

Durum çubuğunda metin rengini sonucu olan bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) gezinti çubuğunun parlaklık eşleşecek şekilde ayarlanabilir. Bu örnekte, durum metin rengi değişir çubuğu kullanıcı anahtarları arasında [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) ve [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) sayfalarının bir [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

![](ios-images/status-bar-text-color-mode.png "Durum çubuğunda metin rengi modu platforma özgü")

<a name="large_title" />

### <a name="displaying-large-titles"></a>Büyük başlıklar görüntüleme

Bu platforma özgü sayfa başlığının gezinti çubuğunda, kullanan iOS 11 veya üstü cihazlar için büyük bir başlık olarak görüntülemek için kullanılır. Büyük bir başlık sola hizalıdır ve daha büyük bir yazı tipi kullanır ve kullanıcı içerik, kaydırma başladığında Ekran gerçek boyutunuzu verimli bir şekilde kullanılmasını sağlamak amacıyla standart bir başlık geçer. Ancak, yatay yönde başlığı içerik düzeni en iyi duruma getirmek için gezinti çubuğunda merkezine döndürür. XAML içinde ayarlayarak tüketilir `NavigationPage.PrefersLargeTitles` özelliğine bağlı bir `boolean` değeri:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Alternatif olarak da fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. `NavigationPage.SetPrefersLargeTitle` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, büyük başlıklar etkin olup olmadığını denetler.

Büyük başlıklar etkinleştirilmiş olması koşuluyla, [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), tüm sayfaları gezinme yığınında büyük başlıklar görüntülenir. Bu davranış ayarlayarak sayfalarında kılınabilir `Page.LargeTitleDisplay` ekli özellik değerine `LargeTitleDisplayMode` sabit listesi:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Alternatif olarak, sayfa davranışı fluent API'sini kullanarak C# geçersiz kılınabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

`Page.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. `Page.SetLargeTitleDisplay` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, üzerinde büyük bir başlık davranışını denetleyen [ `Page` ](xref:Xamarin.Forms.Page), ile `LargeTitleDisplayMode` numaralandırması üç olası sağlama değerler:

- `Always` – zorla gezinti çubuğu ve yazı tipi boyutunun büyük biçimini kullanın.
- `Automatic` – önceki öğeye gezinme yığınında (büyük veya küçük) aynı stili kullanın.
- `Never` – zorla normal, küçük biçimi gezinti çubuğunun kullanın.

Ayrıca, `SetLargeTitleDisplay` yöntemi çağırarak numaralandırma değerlerinden geçiş yapmak için kullanılabilir `LargeTitleDisplay` geçerli döndüren yöntemi `LargeTitleDisplayMode`:

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

Sonuç belirtilen olan `LargeTitleDisplayMode` uygulanan [ `Page` ](xref:Xamarin.Forms.Page), büyük başlık davranışı denetler:

![](ios-images/large-title.png "Etkin platforma özgü bulanıklaştıran")

<a name="set_status_bar_visibility" />

### <a name="setting-the-status-bar-visibility-on-a-page"></a>Durum çubuğu ayar sayfasında görünürlük

Bu platforma özel durum çubuğunun görünürlüğünü ayarlamak için kullanılan bir [ `Page` ](xref:Xamarin.Forms.Page), ve durum çubuğu girer veya bırakır nasıl kontrol etme becerisinin içerir `Page`. XAML içinde ayarlayarak tüketilir `Page.PrefersStatusBarHidden` ekli özellik değerine `StatusBarHiddenMode` numaralandırma ve isteğe bağlı olarak `Page.PreferredStatusBarUpdateAnimation` ekli özellik değerine `UIStatusBarAnimation` sabit listesi:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. `Page.SetPrefersStatusBarHidden` Yöntemi, `Xamarin.Forms.PlatformConfiguration.iOSSpecific` durum çubuğunun görünürlüğünü ayarlamak için kullanılan ad alanı, bir [ `Page` ](xref:Xamarin.Forms.Page) birini belirterek `StatusBarHiddenMode` numaralandırma değerlerinin: `Default`, `True` , veya `False`. `StatusBarHiddenMode.True` Ve `StatusBarHiddenMode.False` değerleri durum çubuğunun görünürlüğünü cihaz yönü, bağımsız olarak ayarlayın ve `StatusBarHiddenMode.Default` değer dikey compact ortam durum çubuğunda gizler.

Durum çubuğunda görünürlüğünü sonucu olan bir [ `Page` ](xref:Xamarin.Forms.Page) ayarlanabilir:

![](ios-images/hide-status-bar.png "Durum çubuğu görünürlük platforma özgü")

> [!NOTE]
> Üzerinde bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), belirtilen `StatusBarHiddenMode` numaralandırma değeri de tüm sayfalar durum çubuğunda güncelleştirme. Diğer tüm [ `Page` ](xref:Xamarin.Forms.Page)-türetilmiş türler, belirtilen `StatusBarHiddenMode` numaralandırma değeri yalnızca geçerli sayfayı durum çubuğunda güncelleştirme.

`Page.SetPreferredStatusBarUpdateAnimation` Yöntemi nasıl durum çubuğu girer veya bırakır ayarlamak için kullanılan [ `Page` ](xref:Xamarin.Forms.Page) birini belirterek `UIStatusBarAnimation` numaralandırma değerlerinin: `None`, `Fade`, veya `Slide`. Varsa `Fade` veya `Slide` numaralandırma değeri belirtilirse, 0,25 ikinci animasyon yürütür durum çubuğu girer veya bırakır `Page`.

<a name="safe_area_layout" />

### <a name="enabling-the-safe-area-layout-guide"></a>Güvenli alan düzeni Kılavuzu etkinleştirme

Bu platforma özel sayfa içeriği iOS 11 ve daha sonraki sürümleri kullanan tüm cihazlar için güvenli ekran alanı konumlandırılmış emin olmak için kullanılır. Özellikle, emin olmak için sağlayacak cihaz yuvarlatılmış köşeler, giriş göstergesi veya algılayıcı muhafazası iPhone X içeriği kırpılmış değil. XAML içinde ayarlayarak tüketilir `Page.UseSafeArea` özelliğine bağlı bir `boolean` değeri:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. `Page.SetUseSafeArea` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, güvenli alan düzeni Kılavuzu etkin olup olmadığını denetler.

Sayfa içeriği tüm İphone'lar için güvenlidir ekran alanı konumlandırılmalıdır oluşur:

[![](ios-images/safe-area-layout.png "Güvenli alan düzeni Kılavuzu")](ios-images/safe-area-layout-large.png#lightbox "güvenli alan düzeni Kılavuzu")

> [!NOTE]
> Apple tarafından tanımlanan güvenli alanı Xamarin.Forms içinde ayarlamak için kullanılan [ `Page.Padding` ](xref:Xamarin.Forms.Page.Padding) özelliği ve ayarlanmış bu özellik, önceki değerlerini geçersiz kılar.

Güvenli alan alarak özelleştirilebilir kendi [ `Thickness` ](xref:Xamarin.Forms.Thickness) değerini `Page.SafeAreaInsets` yönteminden [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı. Olarak daha sonra değiştirilebilir gerekli ve yeniden atandı `Padding` özellik sayfası oluşturucuda veya [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) geçersiz kıl:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

## <a name="layouts"></a>Düzenleri

İOS, platforma özgü aşağıdaki işlevleri için Xamarin.Forms düzenleri sağlanır:

- Denetleme olup olmadığını bir [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) touch hareket işleme veya içeriğine geçirir. Daha fazla bilgi için [geciktirme içerik dokunmalar bir ScrollView içinde](#delay_content_touches).

<a name="delay_content_touches" />

### <a name="delaying-content-touches-in-a-scrollview"></a>İçinde bir ScrollView geciktirme içerik dokunmalar

Bir dokunma hareketi başladığında örtük bir zamanlayıcı tetiklendikten bir [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) ios'ta ve `ScrollView` Zamanlayıcı aralığı içinde bir kullanıcı eylemi hareket işleme veya içeriğine geçirmek göre karar verir. Varsayılan olarak, iOS `ScrollView` gecikmeler içerik değişiklikleri, ancak bazı durumlarda ile sorunlara neden bu can `ScrollView` içeriği gerektiğinde hareket kazanma değil. Bu nedenle, bu platforma özel denetimler olup olmadığını bir `ScrollView` touch hareket işleme veya içeriğine geçirir. XAML içinde ayarlayarak tüketilir `ScrollView.ShouldDelayContentTouches` özelliğine bağlı bir `boolean` değeri:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. `ScrollView.SetShouldDelayContentTouches` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, kullanılan denetimine olup bir [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) touch hareket işleme veya içeriğine geçirir. Ayrıca, `SetShouldDelayContentTouches` yöntemi çağırarak içerik dokunmalar geciktirme geçiş yapmak için kullanılabilir `ShouldDelayContentTouches` içerik dokunmalar gecikiyor döndürülecek yöntemi:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Sonuç bir [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) içerik değişiklikleri, bu nedenle alma geciktirme devre dışı bırakabilirsiniz, bu senaryoda [ `Slider` ](xref:Xamarin.Forms.Slider) hareket alır yerine [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) sayfasının [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

[![](ios-images/scrollview-delay-content-touches.png "ScrollView gecikme içerik platforma özgü geliştirmelere değinmektedir.")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

## <a name="application"></a>Uygulama

İOS, platforma özgü aşağıdaki işlevleri için Xamarin.Forms sağlanan [ `Application` ](xref:Xamarin.Forms.Application) sınıfı:

- Etkinleştirme bir [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) kayan bir görünümündeki yakalamak ve kaydırma hareketi kaydırma görünümü ile paylaşmak için. Daha fazla bilgi için [etkinleştirme eşzamanlı kaydırma hareketi tanıma](#simultaneous-pan-gesture).

<a name="simultaneous-pan-gesture" />

### <a name="enabling-simultaneous-pan-gesture-recognition"></a>Eşzamanlı kaydırma hareketi tanıma özelliğini etkinleştirme

Olduğunda bir [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) bir kaydırma görünümü, tüm hareketleri tarafından yakalanır pan görünüme iliştirilmiş `PanGestureRecognizer` ve kaydırma görünüme iletilen değildir. Bu nedenle, kaydırma görünümü artık kayar.

Bu platforma özgü sağlayan bir `PanGestureRecognizer` kayan bir görünümündeki yakalamak ve kaydırma hareketi kaydırma görünümü ile paylaşmak için. XAML içinde ayarlayarak tüketilir [ `Application.PanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) özelliğine bağlı `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

`Application.On<iOS>` Yöntemi bu platforma özgü yalnızca İos'ta çalıştırma belirtir. [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.SetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı, kayan bir görünümündeki bir yatay kaydırma hareket tanıyıcı kaydırma hareketi yakalamak veya yakalama ve pan paylaşma olup olmadığını denetlemek için kullanılır kaydırma görünümü ile hareket. Ayrıca, [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.GetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application})) yöntemi, kaydırma hareketi içerir kaydırma görünümü ile paylaşılan olmadığını döndürmek için kullanılabilir [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer).

Bu nedenle, etkin olduğunda bu platforma özel ile bir [ `ListView` ](xref:Xamarin.Forms.ListView) içeren bir [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)hem `ListView` ve `PanGestureRecognizer` kaydırma hareketi alırsınız ve işleyin. Bununla birlikte, ne zaman devre dışı bu platforma özel bir `ListView` içeren bir `PanGestureRecognizer`, `PanGestureRecognizer` ve kaydırma hareketi yakalama, işleme ve `ListView` kaydırma hareketi almazsınız.

## <a name="summary"></a>Özet

Bu makalede, iOS platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir. Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [Platform Özellikleri Oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
