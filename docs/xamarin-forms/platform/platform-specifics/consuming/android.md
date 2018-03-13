---
title: "Android platformu özellikleri"
description: "Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar. Bu makalede, Android platformu-Xamarin.Forms yerleşik özellikleri kullanmak gösterilmiştir."
ms.topic: article
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: 739ee4ebeb3176d23ab1eb911baaab31a26252c4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="android-platform-specifics"></a>Android platformu özellikleri

_Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar. Bu makalede, Android platformu-Xamarin.Forms yerleşik özellikleri kullanmak gösterilmiştir._

Android, Xamarin.Forms aşağıdaki platform özellikleri içerir:

- Yazılım klavye işletim modu ayarı. Daha fazla bilgi için bkz: [yumuşak klavye girişi modu ayarı](#soft_input_mode).
- Hızlı kaydırmayı etkinleştirme bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) daha fazla bilgi için bkz: [ListView içinde hızlı kaydırmayı etkinleştirme](#fastscroll).
- Sayfaları arasında geçirmeyi etkinleştirme bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Daha fazla bilgi için bkz: [etkinleştirme geçirmeyi arasında sayfalarında bir TabbedPage](#enable_swipe_paging).
- Z-çizim sırayı belirlemek için görsel öğelerin sırasını denetleme. Daha fazla bilgi için bkz: [ayrıcalık biri görsel öğeleri denetleme](#elevation).
- Devre dışı bırakma [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) ve [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) sayfasında Duraklat yaşam döngüsü olayları ve sırasıyla uygulama kullanan uygulamalar için Sürdür. Daha fazla bilgi için bkz: [Disappearing ve sayfa yaşam döngüsü olayları görüntülenmesini devre dışı bırakma](#disable_lifecycle_events).

<a name="soft_input_mode" />

## <a name="setting-the-soft-keyboard-input-mode"></a>Yazılım klavyesini giriş modunu ayarlama

Bu platforma özgü yumuşak klavye giriş alanı için işletim modu ayarlamak için kullanılır ve ayarlayarak XAML'de tüketilen [ `Application.WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty/) özellik değerine bağlı [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) numaralandırma:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>` Yöntemi belirtir. bu platforma özgü yalnızca Android üzerinde çalışır. [ `Application.UseWindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) ile yumuşak klavye giriş alanını işletim modu ayarlamak için kullanılan ad alanı, [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) iki değerleri sağlayarak numaralandırma: [ `Pan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/) ve [ `Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/). `Pan` Değeri kullanan [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) giriş denetimi odağa sahip olduğunda, pencereyi yeniden boyutlandırmak değil ayarlama seçeneği. Geçerli odağa yumuşak klavye tarafından getirilmemeli değil, bunun yerine, pencerenin içeriğini yatay olarak kaydırıldığında. `Resize` Değeri kullanan [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) giriş denetimi yer açmak için yazılım klavye odağını olduğunda yeniden boyutlandırır ayarlama seçeneği.

Yazılım klavyesini giriş denetimi odağa sahip olduğunda işletim modu ayarlanabilir alan giriş oluşur:

[![](android-images/pan-resize.png "Mod platforma özgü işletim yumuşak klavye")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>ListView içinde hızlı kaydırmayı etkinleştirme

Bu platforma özgü verileri aracılığıyla hızlı kaydırma etkinleştirmek için kullanılan bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). XAML'de ayarlayarak tüketiliyor `ListView.IsFastScrollEnabled` özelliğine bağlı bir `boolean` değeri:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>` Yöntemi belirtir. bu platforma özgü yalnızca Android üzerinde çalışır. `ListView.SetIsFastScrollEnabled` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) ad alanı, veri aracılığıyla hızlı kaydırmayı etkinleştirmek için kullanılan bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Ayrıca, `SetIsFastScrollEnabled` yöntemi çağrılarak hızlı kaydırma geçiş yapmak için kullanılabilir `IsFastScrollEnabled` hızlı kaydırmanın etkin olup olmadığını döndürülecek yöntemi:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Bu hızlı veri aracılığıyla kaydırma sonucu olan bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) hangi kaydırın parmak boyutunu değişiklikleri etkinleştirilebilir:

[![](android-images/fastscroll.png "ListView FastScroll platforma özgü")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>Bir TabbedPage sayfaları arasında geçirmeyi etkinleştirme

Bu platforma özgü sayfaları arasında yatay parmak hareketi ile geçirmeyi etkinleştirmek için kullanılan bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). XAML'de ayarlayarak tüketiliyor [ `TabbedPage.IsSwipePagingEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/) özelliğine bağlı bir `boolean` değeri:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>` Yöntemi belirtir. bu platforma özgü yalnızca Android üzerinde çalışır. [ `TabbedPage.SetIsSwipePagingEnabled` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) ad alanı, sayfalar arasında geçirmeyi etkinleştirmek için kullanılan bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Ayrıca, `TabbedPage` sınıfını `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` da ad alanına sahip bir [ `EnableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) bu platforma etkinleştirir yöntemi ve [ `DisableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) devre dışı bırakır yöntemi Bu platforma özgü. [ `TabbedPage.OffscreenPageLimit` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/) Özelliği, eklenmiş ve [ `SetOffscreenPageLimit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/) yöntemi, geçerli sayfanın her iki tarafında boş bir durumda tutulmalıdır sayfa sayısını ayarlamak için kullanılır.

Bu manyetik disk belleği tarafından görüntülenen sayfaları aracılığıyla sonucu olan bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) etkinleştirilir:

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>Görsel öğeleri ayrıcalıkların denetleme

Bu platforma özgü büyük veya yükseltme veya Z-sıralamasını, uygulamaların görsel öğeleri hedefleyen API 21 denetlemek için kullanılır. Bir görsel öğe ayrıcalıkların alt Z değerleri olan görsel öğeleri occluding daha yüksek Z değerleri olan görsel öğeleri ile kendi çizim sırası belirler. XAML'de ayarlayarak tüketiliyor `Elevation.Elevation` özelliğine bağlı bir `boolean` değeri:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:Elevation.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

`Button.On<Android>` Yöntemi belirtir. bu platforma özgü yalnızca Android üzerinde çalışır. `Elevation.SetElevation` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) ad alanı, ayrıcalıkların yükseltilmesi görsel öğe için bir boş değer atanabilir ayarlamak için kullanılan `float`. Ayrıca, `Elevation.GetElevation` yöntemi, bir görsel öğe ayrıcalık değerini almak için kullanılabilir.

Böylece daha yüksek Z değerleri olan görsel öğeleri alt Z değerleri olan görsel öğeleri occlude görsel öğeleri ayrıcalıkların denetlenebilir sonucudur. Bu nedenle, bu örnekte ikinci [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) yukarıda çizilir [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) daha yüksek bir ayrıcalık değerine sahip olduğundan:

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Kayboldu ve görünen sayfa yaşam döngüsü olayları devre dışı bırakma

Bu platforma özgü devre dışı bırakmak için kullanılan [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) ve [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) uygulama sayfası olaylarına duraklatıp sırasıyla uygulama kullanan uygulamalar için. Ayrıca, bu denetleme olanağı içerir, yumuşak klavye işletim modu olarak ayarlanmış olması koşuluyla, onu Duraklat üzerinde görüntülendiyse yumuşak klavye devamında görüntülenip görüntülenmeyeceğini [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

> [!NOTE]
> Olayları kullanan uygulamalar için var olan davranışı korumak için bu olayları varsayılan olarak etkin olan unutmayın. Bu olaylar devre dışı bırakma öncesi uygulama olay döngüsü eşleşen uygulama olay döngüsü getirir.

Bu platforma özgü ayarlayarak XAML'de tüketilebilir [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty/), [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty/), ve [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty/) için Ekliözellikler`boolean` değerler:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

`Application.Current.On<Android>` Yöntemi belirtir. bu platforma özgü yalnızca Android üzerinde çalışır. [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) ad alanı, etkinleştirmek veya tetikleme devre dışı bırakmak için kullanılan [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) sayfa olay, uygulama arka plan girer. [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Yöntemi etkinleştirmek veya tetikleme devre dışı bırakmak için kullanılan [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) uygulama arka planından çıktığında sayfası olayı. [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metodunun kullanıldığından Duraklat üzerinde görüntüleniyorsa yumuşak klavye devamında görüntülenip görüntülenmeyeceğini denetim sağlanan yumuşak klavye işletim modu ayarlamak [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

Sonuç [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) ve [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) sayfası olayları uygulama Duraklat üzerinde harekete olmaz ve sırasıyla sürdürmek ve yazılım klavyesini olması durumunda olduğu zaman görüntülenen uygulama olan uygulama çıktığında duraklatıldı, onu da görüntülenir:

[![](android-images/keyboard-on-resume.png "Yaşam döngüsü olayları platforma özgü")](android-images/keyboard-on-resume-large.png#lightbox "yaşam döngüsü olayları platforma özgü")

## <a name="summary"></a>Özet

Bu makalede, Android platformu-Xamarin.Forms yerleşik özellikleri kullanma gösterilmektedir. Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Platform Özellikleri Oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
