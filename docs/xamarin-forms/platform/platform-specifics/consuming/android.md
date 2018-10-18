---
title: Android Platform özellikleri
description: Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar. Bu makalede, Android platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/03/2018
ms.openlocfilehash: c422b9ac5af9417523f349537fda1bb0c01aa7bc
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39175183"
---
# <a name="android-platform-specifics"></a>Android Platform özellikleri

_Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar. Bu makalede, Android platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir._

## <a name="visualelements"></a>VisualElements

Android, aşağıdaki platforma özgü işlevleri Xamarin.Forms görünümleri, sayfalar ve düzenleri için sağlanır:

- Z-çizim sırasını belirlemek için görsel öğelerin sırasını denetleme. Daha fazla bilgi için [yükseltme görsel öğeleri denetleme](#elevation).
- Desteklenen bir eski renk modunu devre dışı bırakma [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Daha fazla bilgi için [eski renk modunu devre dışı bırakma](#legacy-color-mode).

<a name="elevation" />

### <a name="controlling-the-elevation-of-visual-elements"></a>Görsel öğeler ayrıcalıkların denetleme

Bu platforma özgü büyük veya yükseltme ya da Z düzenini, görsel öğe uygulamaları hedefleyen API 21 denetlemek için kullanılır. Bir görsel öğe ayrıcalıkların görsel öğelerin Z değerleri daha düşük ile görsel öğeleri occluding yüksek Z değerleri ile birlikte çizim kendi sırasını belirler. XAML içinde ayarlayarak tüketilir `VisualElement.Elevation` özelliğine bağlı bir `boolean` değeri:

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

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

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

`Button.On<Android>` Yöntemi bu platforma özgü Android'de yalnızca çalışacağını belirtir. `VisualElement.SetElevation` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı, yükseltme, görsel öğe için bir boş değer atanabilir ayarlamak için kullanılan `float`. Ayrıca, `VisualElement.GetElevation` yöntemi, bir görsel öğe yükseltme değerini almak için kullanılabilir.

Yükseltme görsel öğeler, böylece daha yüksek Z değerleri olan görsel öğelerin Z değerleri daha düşük ile görsel öğeleri occlude denetlenebilir sonucudur. Bu nedenle, bu örnekte ikinci [ `Button` ](xref:Xamarin.Forms.Button) yukarıda işlenen [ `BoxView` ](xref:Xamarin.Forms.BoxView) , daha yüksek bir ayrıcalık değer içerdiğinden:

![](android-images/elevation.png)

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>Eski renk modunu devre dışı bırakma

Xamarin.Forms görünümlerinden bazılarını eski renk modu özelliği. Bu modda olduğunda [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) görünümün özelliği `false`, görünümü devre dışı durumu için varsayılan yerel renklerle kullanıcı tarafından ayarlanan renkleri geçersiz kılar. Geriye dönük uyumluluk, bu eski renk modu desteklenen görünümler için varsayılan davranış kalır için.

Hatta görünümü devre dışı bırakıldığında kullanıcı tarafından bir görünüm üzerinde ayarlanan renkleri kalmasını sağlamak bu platforma özgü bu eski renk modu devre dışı bırakır. XAML içinde ayarlayarak tüketilir [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) özelliğine bağlı `false`:

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

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>` Yöntemi bu platforma özgü Android'de yalnızca çalışacağını belirtir. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı, eski renk modunu devre dışı olup olmadığını denetlemek için kullanılır. Ayrıca, [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) yöntemi, eski renk modunu devre dışı bırakılıp bırakılmadığını döndürmek için kullanılabilir.

Bir görünüm üzerinde kullanıcı tarafından ayarlanan renkleri görünümü devre dışı bırakıldığında bile kalır eski renk modunu devre dışı bırakılabilir olduğunu, oluşur:

![](android-images/legacy-color-mode-disabled.png "Eski renk modu devre dışı")

> [!NOTE]
> Ayarlarken bir [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) bir görünüm üzerinde eski renk modunu tamamen yoksayılır. Görsel durumlar hakkında daha fazla bilgi için bkz: [Xamarin.Forms görsel durum Yöneticisi](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="views"></a>Görünümler

Android, aşağıdaki platforma özel işlevler için Xamarin.Forms görünümleri sağlanır:

- Android düğmeleri gölge değerlerini ve varsayılan doldurma kullanma. Daha fazla bilgi için [Android düğmeleri kullanarak](#button-padding-shadow).
- Giriş Yöntemi Düzenleyicisi ayarları için geçici klavye için bir [ `Entry` ](xref:Xamarin.Forms.Entry). Daha fazla bilgi için [ayarı Giriş Giriş Yöntemi Düzenleyicisi Seçenekleri](#entry-imeoptions).
- Hızlı kaydırma etkinleştirme bir [ `ListView` ](xref:Xamarin.Forms.ListView) daha fazla bilgi için [bir ListView içinde hızlı kaydırma etkinleştirme](#fastscroll).
- Denetleme olup olmadığını bir [ `WebView` ](xref:Xamarin.Forms.WebView) karışık içeriği görüntüleyebilir. Daha fazla bilgi için [etkinleştirme karışık içeriği bir WebView](#webview-mixed-content).

<a name="button-padding-shadow" />

### <a name="using-android-buttons"></a>Android düğmeleri kullanma

Bu platforma özgü Xamarin.Forms düğmeleri Android düğmeleri gölge değerlerini ve varsayılan doldurma kullanıp kullanmadığını denetler. XAML içinde ayarlayarak tüketilir [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) ve [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) iliştirilmiş özellikler için `boolean` değerleri:

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

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>` Yöntemi bu platforma özgü Android'de yalnızca çalışacağını belirtir. [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) Ve[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) yöntemleri, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı, varsayılan Xamarin.Forms düğmelerini olup olmadığını denetlemek için kullanılır doldurma ve Android düğmelerinin gölge değerleri. Ayrıca, [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) ve [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) yöntemleri, bir düğme ve varsayılan gölge değerlerini sırasıyla doldurma varsayılan kullanıp kullanmadığını döndürmek için kullanılabilir.

Android düğmeleri gölge değerlerini ve varsayılan doldurma Xamarin.Forms düğmelerini kullanabilirsiniz oluşur:

![](android-images/button-padding-and-shadow.png "Eski renk modu devre dışı")

Her yukarıdaki ekran görüntüsünde unutmayın [ `Button` ](xref:Xamarin.Forms.Button) hariç aynı tanımlarına sahip sağ `Button` Android düğmeleri gölge değerlerini ve varsayılan doldurma kullanır.

<a name="entry-imeoptions" />

### <a name="setting-entry-input-method-editor-options"></a>Ayar Giriş Giriş Yöntemi Düzenleyicisi Seçenekleri

Bu platforma özel Giriş Yöntemi Düzenleyicisi (IME) için geçici klavye seçeneklerini ayarlar bir [ `Entry` ](xref:Xamarin.Forms.Entry). Bu yazılım klavye ve etkileşim alt köşesinde kullanıcı eylem düğmesi ayarını içerir `Entry`. XAML içinde ayarlayarak tüketilir [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) ekli özellik değerine [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) sabit listesi:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>` Yöntemi bu platforma özgü Android'de yalnızca çalışacağını belirtir. [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) için geçici klavye giriş yöntemi eylem seçeneğini ayarlamak için kullanılan ad alanı, [ `Entry` ](xref:Xamarin.Forms.Entry), ile [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) aşağıdaki değerleri sağlayan bir sabit listesi:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – belirli bir eylem anahtar gereklidir ve temel alınan denetimi halinde üretecektir alabilir ve gösterir. Bu `Next` veya `Done`.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – hiçbir eylem anahtarı kullanılabilir hale getirilir olduğunu gösterir.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – Eylem anahtarı bir "Git" işlemi gerçekleştirir, metnin hedef kullanıcı bunlar yazılan gösterir.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – Eylem anahtarı "arama" işlemi gerçekleştirir, kullanıcıya metin arama sonuçlarını alma bunlar yazmış gösterir.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – Eylem anahtarı metin hedefte sunan bir "gönderme" işlemi gerçekleştireceğini gösterir.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – Eylem anahtarı kullanıcı metin kabul edileceği sonraki alana ayırdığınız bir "İleri" işlemi gerçekleştireceğini gösterir.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – Eylem anahtarı geçici klavye kapatmayı bir "Bitti" işlemini gerçekleştireceğini belirtir.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – Eylem anahtarı "önceki" işlemi, kullanıcının metin kabul edileceği önceki alana ayırdığınız gerçekleştireceğini gösterir.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – Maske eylem seçeneklerini belirleyin.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – Yazım ne kullanıcıdan öğrenin ve hangi kullanıcının daha önce yazdığını üzerinde dayalı düzeltmeler önerir gösterir.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – Kullanıcı Arabiriminin ekran geçmeyecek gösterir.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – ayıklanan metin için kullanıcı Arabirimi gösterileceğini belirtir.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – Kullanıcı Arabirimi için özel eylemler gösterileceğini belirtir.

Sonuç belirtilen olan [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) değeri için geçici klavye uygulanan [ `Entry` ](xref:Xamarin.Forms.Entry), Düzenleyici Seçenekleri giriş yöntemi ayarlar:

[![Giriş Yöntemi Düzenleyicisi platforma özgü giriş](android-images/entry-imeoptions.png "Giriş Giriş Yöntemi Düzenleyicisi platforma özgü")](android-images/entry-imeoptions-large.png#lightbox "Giriş Yöntemi Düzenleyicisi platforma özgü giriş")

<a name="fastscroll" />

### <a name="enabling-fast-scrolling-in-a-listview"></a>Bir ListView içinde hızlı kaydırma etkinleştirme

Bu platforma özgü verileri aracılığıyla hızlı kaydırma etkinleştirmek için kullanılan bir [ `ListView` ](xref:Xamarin.Forms.ListView). XAML içinde ayarlayarak tüketilir `ListView.IsFastScrollEnabled` özelliğine bağlı bir `boolean` değeri:

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

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>` Yöntemi bu platforma özgü Android'de yalnızca çalışacağını belirtir. `ListView.SetIsFastScrollEnabled` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı, veri aracılığıyla hızlı kaydırma etkinleştirmek için kullanılan bir [ `ListView` ](xref:Xamarin.Forms.ListView). Ayrıca, `SetIsFastScrollEnabled` yöntemi çağırarak hızlı kaydırma geçiş yapmak için kullanılabilir `IsFastScrollEnabled` hızlı kaydırma etkin olup olmadığını döndürülecek yöntemi:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Bu hızlı veri aracılığıyla kaydırma sonucu olan bir [ `ListView` ](xref:Xamarin.Forms.ListView) hangi kaydırma parmak boyutunu değişiklikleri etkinleştirilebilir:

[![](android-images/fastscroll.png "ListView FastScroll platforma özgü")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="webview-mixed-content" />

### <a name="enabling-mixed-content-in-a-webview"></a>Bir WebView içerikte etkinleştirme

Bu platforma özel denetimler olup olmadığını bir [ `WebView` ](xref:Xamarin.Forms.WebView) görünen karışık içerik uygulamalarda hedefleyen API 21 veya büyük. Karışık içerik, başlangıçta bir HTTPS bağlantısı üzerinden yüklenir, ancak bir HTTP bağlantısı üzerinden kaynakları (örneğin, görüntüleri, ses, video, stil sayfaları, betikler) yükleyen içeriktir. XAML içinde ayarlayarak tüketilir [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) ekli özellik değerine [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) sabit listesi:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>` Yöntemi bu platforma özgü Android'de yalnızca çalışacağını belirtir. [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı, karışık içerik görüntülenmesini olup olmadığını denetlemek için kullanılan ile [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) numaralandırması üç olası değer sağlama:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – belirten [ `WebView` ](xref:Xamarin.Forms.WebView) bir HTTP kaynaktan alınan içeriğin yüklenmesi için bir HTTPS kaynağa izin verir.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – belirten [ `WebView` ](xref:Xamarin.Forms.WebView) bir HTTP kaynaktan alınan içeriğin yüklenmesi için bir HTTPS kaynağa izin vermez.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – belirten [ `WebView` ](xref:Xamarin.Forms.WebView) son cihazın web tarayıcısının bir yaklaşım ile uyumlu olacak şekilde çalışacaktır. Bazı HTTP İçerik HTTPS kaynağı tarafından yüklenmesi için izin verilmiyor olabilir ve diğer içerik türlerine engellenir. İzin verilen veya engellenen içerik türlerini her işletim sistemi sürümü ile farklı olabilir.

Sonuç belirtilen olan [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) değeri uygulanan [ `WebView` ](xref:Xamarin.Forms.WebView), karışık içerik görüntülenmesini denetler:

[![WebView karışık içerik işleme platforma özgü](android-images/webview-mixedcontent.png "WebView karışık içerik işleme platforma özgü")](android-images/webview-mixedcontent-large.png#lightbox "WebView karışık içerik işleme platforma özgü")

## <a name="pages"></a>Sayfaları

Android'de Xamarin.Forms sayfaları için aşağıdaki platforma özgü işlevleri sağlanır:

- Gezinti çubuğunun yüksekliği ayarını bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Daha fazla bilgi için [üzerinde bir NavigationPage gezinti çubuğunun yüksekliği ayarlama](#navigationpage-barheight).
- Sayfalar arasında çekerek etkinleştirme bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Daha fazla bilgi için [etkinleştirme çekerek arasında sayfalarında bir TabbedPage](#enable_swipe_paging).
- Rengi ve araç çubuğu yerleştirme ayarını bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Daha fazla bilgi için [ayarı TabbedPage araç çubuğu yerleştirme ve renk](#tabbedpage-toolbar).

<a name="navigationpage-barheight" />

### <a name="setting-the-navigation-bar-height-on-a-navigationpage"></a>Gezinti çubuğu ayarı üzerinde bir NavigationPage yükseklik

Gezinti çubuğunun yüksekliği bu platforma özel ayarlar bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). XAML içinde ayarlayarak tüketilir [ `NavigationPage.BarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) bağlanılabilir özellik bir tamsayı değerine:

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

public class AndroidNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public AndroidNavigationPageCS()
    {
        On<Android>().SetBarHeight(450);
    }
}
```

`NavigationPage.On<Android>` Yöntemini belirtir bu platforma özgü yalnızca uygulama uyumluluğu Android üzerinde çalışır. [ `NavigationPage.SetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.SetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage},System.Int32)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) ad alanı, gezinti çubuğunun yüksekliği ayarlamak için kullanılan bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Ayrıca, [ `NavigationPage.GetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.GetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage})) yöntemi, gezinti çubuğunda yüksekliğini döndürmek için kullanılabilir `NavigationPage`.

Gezinti çubuğunda yüksekliğini sonucu olan bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) ayarlanabilir:

![](android-images/navigationpage-barheight.png "NavigationPage gezinti çubuğu yüksekliği")

<a name="enable_swipe_paging" />

### <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>Bir TabbedPage sayfalar arasında çekerek etkinleştirme

Bu platforma özgü bir yatay parmağınızı hareket sayfalar arasında ile geçirmeyi etkinleştirmek için kullanılan bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). XAML içinde ayarlayarak tüketilir [ `TabbedPage.IsSwipePagingEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) özelliğine bağlı bir `boolean` değeri:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>` Yöntemi bu platforma özgü Android'de yalnızca çalışacağını belirtir. [ `TabbedPage.SetIsSwipePagingEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled(Xamarin.Forms.BindableObject,System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı, sayfalar arasında geçirmeyi etkinleştirmek için kullanılan bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Ayrıca, `TabbedPage` sınıfını `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` aynı zamanda ad alanına sahip bir [ `EnableSwipePaging` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) yönteminin bu platforma tanıyan ve [ `DisableSwipePaging` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) devre dışı bırakan yöntemi Bu platforma özel. [ `TabbedPage.OffscreenPageLimit` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty) Ekli özellik, ve [ `SetOffscreenPageLimit` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit(Xamarin.Forms.BindableObject,System.Int32)) yöntemini, geçerli sayfa her iki tarafındaki boşta durumda tutulmalıdır sayfaların sayısını ayarlamak için kullanılır.

Sonuç, manyetik sayfalama tarafından görüntülenen sayfaları aracılığıyla, bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) etkinleştirilir:

![](android-images/tabbedpage-swipe.png)

<a name="tabbedpage-toolbar" />

### <a name="setting-tabbedpage-toolbar-placement-and-color"></a>TabbedPage araç çubuğu yerleştirme ve rengini ayarlama

Bu platform özellikleri yerleştirme ve araç rengini ayarlamak için kullanılan bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Ayarlayarak XAML içinde tüketilir [ `TabbedPage.ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) ekli özellik değerine [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) numaralandırma ve [ `TabbedPage.BarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) ve [ `TabbedPage.BarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) iliştirilmiş özellikler için bir [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

Alternatif olarak, bunlar fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

`TabbedPage.On<Android>` Yöntemi bu platform özellikleri, Android'de yalnızca çalışacağını belirtir. [ `TabbedPage.SetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı, araç çubuğu yerleştirme ayarlamak için kullanılan bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), ile [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) aşağıdaki değerleri sağlayan bir sabit listesi:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default) – araç sayfasında varsayılan konumda eklendiğini gösterir. Sayfanın üst kısmında telefonlarda ve diğer cihaz deyimleri sayfanın alt kısmındaki budur.
- [`Top`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top) – sayfanın en üstündeki araç eklendiğini gösterir.
- [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom) – araç sayfanın alt kısmında eklendiğini gösterir.

Ayrıca, [ `TabbedPage.SetBarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color)) ve [ `TabbedPage.SetBarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color)) yöntemleri, rengi ve araç çubuğu öğelerinin seçili araç çubuğu öğeleri, sırasıyla ayarlamak için kullanılır.

> [!NOTE]
> [ `GetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})), [ `GetBarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})), Ve [ `GetBarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) yöntemleri, yerleştirme ve rengini almak için kullanılabilir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) araç çubuğu.

Üzerinde araç çubuğu yerleştirme, araç çubuğu öğelerinin rengi ve seçili araç çubuğu öğesi rengi ayarlanabilir sonucu olan bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

![](android-images/tabbedpage-toolbar-placement.png)

## <a name="application"></a>Uygulama

Android, aşağıdaki platforma özel işlevler için Xamarin.Forms sağlanan [ `Application` ](xref:Xamarin.Forms.Application) sınıfı:

- Geçici klavye işletim modu ayarı. Daha fazla bilgi için [geçici klavye girişi modunu ayarlama](#soft_input_mode).
- Devre dışı bırakma [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) ve [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) sayfa yaşam döngüsü olaylarını duraklatma ve sırasıyla AppCompat kullanan uygulamalar için sürdürebilirsiniz. Daha fazla bilgi için [Disappearing ve sayfa yaşam döngüsü olaylarını görüntülenmesini devre dışı bırakma](#disable_lifecycle_events).

<a name="soft_input_mode" />

### <a name="setting-the-soft-keyboard-input-mode"></a>Geçici klavye giriş modunu ayarlama

Bu platforma özgü geçici klavye giriş alanı için işletim modu ayarlamak için kullanılır ve XAML içinde ayarlayarak tüketilen [ `Application.WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty) ekli özellik değerine [ `WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) sabit listesi:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>` Yöntemi bu platforma özgü Android'de yalnızca çalışacağını belirtir. [ `Application.UseWindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ile geçici klavye giriş alanını işletim modu ayarlamak için kullanılan ad alanı, [ `WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) iki değer sağlayan bir sabit listesi: [ `Pan` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) ve [ `Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize). `Pan` Değeri kullanan [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) giriş denetimi odağa sahip olduğunda penceresini yeniden boyutlandırdığınızda değil ayarlama seçeneği. Bunun yerine, böylece geçerli odak geçici klavye tarafından engellediği değil penceresinin içeriğini kaydırılan. `Resize` Değeri kullanan [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) bir giriş denetimini yer açmak için geçici klavye odağı varken boyutlandırır ayarlama seçeneği.

Geçici klavye alan bir giriş denetim odağa sahip olduğunda işletim modu ayarlanabilir giriş oluşur:

[![](android-images/pan-resize.png "Geçici klavye modu platforma özgü çalıştırma")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="disable_lifecycle_events" />

### <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Sayfa yaşam döngüsü olayları kayboldu ve görüntülenmesini devre dışı bırakma

Bu platforma özgü devre dışı bırakmak için kullanılan [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) ve [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) uygulama sayfası olaylarına duraklatıp sırasıyla AppCompat kullanan uygulamalar için. Buna ek olarak, denetleme olanağı içerir, geçici klavye işletim modu olarak ayarlanmış olması koşuluyla, duraklatma, zobrazilo geçici klavye devamında görüntülenip görüntülenmeyeceğini [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

> [!NOTE]
> Olayları kullanan uygulamalar için mevcut davranışı korumak için bu olayları varsayılan olarak etkin olan unutmayın. Bu olaylar devre dışı bırakma öncesi AppCompat olay döngünüzle uyuşması AppCompat olay döngüsü sağlar.

Bu platforma özgü ayarlayarak XAML içinde kullanılabilen [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty), [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty), ve [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty) içinekliözellikler`boolean` değerleri:

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

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

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

`Application.Current.On<Android>` Yöntemi bu platforma özgü Android'de yalnızca çalışacağını belirtir. [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) ad alanı, etkinleştirme veya devre dışı Açmadığınızda kullanılır [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) sayfası olayı, uygulama arka plan girer. [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) Yöntemi, etkinleştirme veya devre dışı Açmadığınızda kullanılır [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) arka planından uygulama devam ettiğinde sayfası olayı. [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) Yöntemi kullanılır, duraklatma görüntüleniyorsa devam ederken geçici klavye görüntülenip görüntülenmeyeceğini sağlanan geçici klavye işletim modu ayarlamak denetim [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

Sonuç [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) ve [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) sayfası olayları uygulama duraklatma harekete olmaz ve sırasıyla sürdürme ve geçici klavye olması durumunda olduğu zaman görüntülenen uygulama olan uygulama devam ettiğinde duraklatıldı, bu da görüntülenir:

[![](android-images/keyboard-on-resume.png "Yaşam döngüsü olayları platforma özgü")](android-images/keyboard-on-resume-large.png#lightbox "yaşam döngüsü olayları platforma özgü")

## <a name="summary"></a>Özet

Bu makalede, Android platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir. Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [Platform Özellikleri Oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
