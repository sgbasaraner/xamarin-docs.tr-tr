---
title: Android platformu özellikleri
description: Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar. Bu makalede, Android platformu-Xamarin.Forms yerleşik özellikleri kullanmak gösterilmiştir.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 05f1fc6158e9a20892ab4a4b49b33e4eac6bc5e5
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733067"
---
# <a name="android-platform-specifics"></a>Android platformu özellikleri

_Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar. Bu makalede, Android platformu-Xamarin.Forms yerleşik özellikleri kullanmak gösterilmiştir._

Android, Xamarin.Forms aşağıdaki platform özellikleri içerir:

- Yazılım klavye işletim modu ayarı. Daha fazla bilgi için bkz: [yumuşak klavye girişi modu ayarı](#soft_input_mode).
- Hızlı kaydırmayı etkinleştirme bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) daha fazla bilgi için bkz: [ListView içinde hızlı kaydırmayı etkinleştirme](#fastscroll).
- Sayfaları arasında geçirmeyi etkinleştirme bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Daha fazla bilgi için bkz: [etkinleştirme geçirmeyi arasında sayfalarında bir TabbedPage](#enable_swipe_paging).
- Z-çizim sırayı belirlemek için görsel öğelerin sırasını denetleme. Daha fazla bilgi için bkz: [ayrıcalık biri görsel öğeleri denetleme](#elevation).
- Devre dışı bırakma [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) ve [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) sayfasında Duraklat yaşam döngüsü olayları ve sırasıyla uygulama kullanan uygulamalar için Sürdür. Daha fazla bilgi için bkz: [Disappearing ve sayfa yaşam döngüsü olayları görüntülenmesini devre dışı bırakma](#disable_lifecycle_events).
- Denetleme olup bir [ `WebView` ](xref:Xamarin.Forms.WebView) karışık içerik görüntüleyebilirsiniz. Daha fazla bilgi için bkz: [etkinleştirme karışık içerik bir Web görünümü](#webview-mixed-content).
- Giriş Yöntemi Düzenleyicisi için yazılım klavye için ayar seçenekleri bir [ `Entry` ](xref:Xamarin.Forms.Entry). Daha fazla bilgi için bkz: [ayarı Giriş Giriş Yöntemi Düzenleyicisi Seçenekleri](#entry-imeoptions).
- Desteklenen bir eski renk modunu devre dışı bırakma [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Daha fazla bilgi için bkz: [eski renk modu devre dışı bırakma](#legacy-color-mode).
- Varsayılan doldurma ve gölge değerleri Android düğmelerini kullanma. Daha fazla bilgi için bkz: [kullanarak Android düğmeleri](#button-padding-shadow).

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

Bu platforma özgü büyük veya yükseltme veya Z-sıralamasını, uygulamaların görsel öğeleri hedefleyen API 21 denetlemek için kullanılır. Bir görsel öğe ayrıcalıkların alt Z değerleri olan görsel öğeleri occluding daha yüksek Z değerleri olan görsel öğeleri ile kendi çizim sırası belirler. XAML'de ayarlayarak tüketiliyor `VisualElement.Elevation` özelliğine bağlı bir `boolean` değeri:

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
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
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

`Button.On<Android>` Yöntemi belirtir. bu platforma özgü yalnızca Android üzerinde çalışır. `VisualElement.SetElevation` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) ad alanı, ayrıcalıkların yükseltilmesi görsel öğe için bir boş değer atanabilir ayarlamak için kullanılan `float`. Ayrıca, `VisualElement.GetElevation` yöntemi, bir görsel öğe ayrıcalık değerini almak için kullanılabilir.

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

<a name="webview-mixed-content" />

## <a name="enabling-mixed-content-in-a-webview"></a>Bir Web görünümü karışık içeriği etkinleştirme

Bu platforma özgü denetimleri olup bir [ `WebView` ](xref:Xamarin.Forms.WebView) görüntü karışık içerik uygulamalarında hedefleyen API 21 veya daha büyük. Karışık içerik, başlangıçta bir HTTPS bağlantısı üzerinden yüklendi, ancak bir HTTP bağlantısı üzerinden kaynakları (örneğin, görüntüler, ses, video, stil sayfaları, komut dosyaları) yükleyen içeriktir. XAML'de ayarlayarak tüketiliyor [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) özellik değerine bağlı [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) numaralandırma:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>` Yöntemi belirtir. bu platforma özgü yalnızca Android üzerinde çalışır. [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı, karışık içerik görüntülenmesini olup olmadığını denetlemek için kullanılan ile [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) numaralandırması üç olası değerler sağlama:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – belirten [ `WebView` ](xref:Xamarin.Forms.WebView) bir HTTP kaynaktan içerik yüklemek için bir HTTPS kaynağa izin verir.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – belirten [ `WebView` ](xref:Xamarin.Forms.WebView) bir HTTP kaynaktan içerik yüklemek için bir HTTPS kaynağa izin vermez.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – belirten [ `WebView` ](xref:Xamarin.Forms.WebView) son cihazın web tarayıcısının yaklaşım ile uyumlu olacak şekilde çalışacaktır. Bazı HTTP İçerik HTTPS kaynağa göre yüklenmesi için izin verilmiyor olabilir ve diğer içerik türlerine engellenir. İzin verilen veya engellenen içerik türlerini her işletim sistemi sürümüyle değiştirebilirsiniz.

Sonuç belirtilen olan [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) değeri uygulanan [ `WebView` ](xref:Xamarin.Forms.WebView), karışık içerik görüntülenmesini olup olmadığını kontrol eder:

[![Web görünümü karışık içerik işleme platforma özgü](android-images/webview-mixedcontent.png "WebView karışık içerik işleme platforma özgü")](android-images/webview-mixedcontent-large.png#lightbox "WebView karışık içerik işleme platforma özgü")

<a name="entry-imeoptions" />

## <a name="setting-entry-input-method-editor-options"></a>Ayar girişi Giriş Yöntemi Düzenleyicisi Seçenekleri

Bu platforma özgü Giriş Yöntemi Düzenleyicisi (IME) seçenekleri için yazılım klavye için ayarlar bir [ `Entry` ](xref:Xamarin.Forms.Entry). Bu yazılım klavye ile etkileşim ve alt köşedeki kullanıcı eylem düğmesi ayarı içerir `Entry`. XAML'de ayarlayarak tüketiliyor [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) özellik değerine bağlı [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) numaralandırma:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>` Yöntemi belirtir. bu platforma özgü yalnızca Android üzerinde çalışır. [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) yumuşak klavye için giriş yöntemi eylem seçeneğini ayarlamak için kullanılan ad alanı, [ `Entry` ](xref:Xamarin.Forms.Entry), ile [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) aşağıdaki değerleri sağlayarak numaralandırma:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – belirli bir eylemi anahtar gereklidir ve temel denetim kendiliğinden üretecektir olabilir gösterir. Bu da olacaktır `Next` veya `Done`.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – hiçbir eylem anahtarı kullanımına sunulacak olduğunu gösterir.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – Eylem anahtarı "Git" işlemi gerçekleştirir, hedef metninin kullanıcıya alma, yazılan gösterir.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – Eylem anahtarı "Ara" işlemi gerçekleştirir, metin arama sonuçlarını kullanıcıya alma bunlar yazmış gösterir.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – Eylem anahtarı hedefte metni sunan bir "gönderme" işlemi gerçekleştireceğini belirtir.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – Eylem anahtarı kullanıcı metin kabul eder sonraki alan için ayırdığınız bir "İleri" işlemi gerçekleştireceğini belirtir.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – Eylem anahtarı yumuşak klavye kapatmayı "tamamlandı" bir işlem gerçekleştireceğini belirtir.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – Eylem anahtarı kullanıcı metin kabul eder önceki alan için ayırdığınız "önceki" bir işlem gerçekleştireceğini belirtir.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – maskesi eylem seçenekleri seçin.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – Yazım denetleyicisi ne kullanıcıdan bilgi ne ne kullanıcının daha önce girdiği üzerinde temel düzeltmeleri önermek gösterir.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – UI tam ekran geçmeyecek gösterir.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – Kullanıcı Arabirimi için ayıklanan metin gösterilecek gösterir.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – Özel Eylemler için kullanıcı Arabirimi görüntülenir gösterir.

Sonuç belirtilen olan [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) değer yumuşak klavye için uygulanan [ `Entry` ](xref:Xamarin.Forms.Entry), Düzenleyici Seçenekleri giriş yönteminin ayarlar:

[![Giriş Yöntemi Düzenleyicisi platforma özgü giriş](android-images/entry-imeoptions.png "Giriş Giriş Yöntemi Düzenleyicisi platforma özgü")](android-images/entry-imeoptions-large.png#lightbox "Giriş Yöntemi Düzenleyicisi platforma özgü giriş")

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Eski renk modunu devre dışı bırakma

Xamarin.Forms görünümlerinden bazılarını eski renk modu özelliği. Bu modda olduğunda [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) görünümünün özelliği ayarlanmış `false`, görünümü devre dışı durumunu için varsayılan yerel renkleri ile kullanıcı tarafından ayarlanan renkleri geçersiz kılar. Geriye dönük uyumluluk, bu eski renk modu desteklenen görünümler için varsayılan davranış kalır için.

Görünüm devre dışı olsa bir görünüm üzerinde kullanıcı tarafından ayarlanan renkleri kalmaması bu platforma özgü bu eski renk modu devre dışı bırakır. XAML'de ayarlayarak tüketiliyor [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) özelliğine bağlı `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>` Yöntemi belirtir. bu platforma özgü yalnızca Android üzerinde çalışır. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı, eski renk modunu devre dışı olup olmadığını denetlemek için kullanılır. Ayrıca, [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) yöntemi, eski renk modunu devre dışı bırakılıp bırakılmadığını döndürmek için kullanılabilir.

Böylece bir görünüm üzerinde kullanıcı tarafından ayarlanan renkleri görünümü devre dışı bırakıldığında bile kalır eski renk modunu devre dışı bırakılabilir olduğunu, oluşur:

![](android-images/legacy-color-mode-disabled.png "Eski renk modu devre dışı")

> [!NOTE]
> Ayarlarken bir [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) bir görünüm üzerinde eski renk modunu tamamen göz ardı edilir. Görsel durumları hakkında daha fazla bilgi için bkz: [Xamarin.Forms görsel durum Yöneticisi](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="button-padding-shadow" />

## <a name="using-android-buttons"></a>Android düğmelerini kullanma

Bu platforma özgü Xamarin.Forms düğmeleri varsayılan doldurma ve Android düğmelerinin gölge değerleri kullanıp kullanmadığını denetler. XAML'de ayarlayarak tüketiliyor [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) ve [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) özelliklerine bağlı `boolean` değerler:

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>` Yöntemi belirtir. bu platforma özgü yalnızca Android üzerinde çalışır. [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) Ve[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) yöntemleri, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı, varsayılan Xamarin.Forms düğmelerini olup olmadığını denetlemek için kullanılır doldurma ve Android düğmelerinin gölge değerleri. Ayrıca, [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) ve [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) yöntemleri, bir düğmeyi ve varsayılan gölge değerleri sırasıyla doldurma varsayılan kullanıp kullanmadığını döndürmek için kullanılabilir.

Xamarin.Forms düğmeleri varsayılan doldurma ve Android düğmelerinin gölge değerleri kullanabilirsiniz oluşur:

![](android-images/button-padding-and-shadow.png "Eski renk modu devre dışı")

Her yukarıdaki ekran görüntüsü unutmayın [ `Button` ](xref:Xamarin.Forms.Button) hariç aynı tanımlarına sahip sağ `Button` varsayılan doldurma ve Android düğmelerinin gölge değerleri kullanır.

## <a name="summary"></a>Özet

Bu makalede, Android platformu-Xamarin.Forms yerleşik özellikleri kullanma gösterilmektedir. Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [Platform Özellikleri Oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
