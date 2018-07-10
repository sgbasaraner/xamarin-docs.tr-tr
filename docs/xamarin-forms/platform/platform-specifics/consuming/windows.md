---
title: Windows Platform özellikleri
description: Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar. Bu makalede Windows platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 52895564ef327845940d687a58b007fb1502e62b
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935139"
---
# <a name="windows-platform-specifics"></a>Windows Platform özellikleri

_Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar. Bu makalede Windows platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir._

Xamarin.Forms, Evrensel Windows Platformu (UWP üzerinde), aşağıdaki platform özellikleri içerir:

- Araç çubuğu yerleştirme seçeneklerini ayarlama. Daha fazla bilgi için [araç çubuğu yerleştirme değiştirme](#toolbar_placement).
- Daraltma [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) gezinti çubuğu. Daha fazla bilgi için [MasterDetailPage gezinti çubuğu daraltma](#collapsable_navigation_bar).
- Etkinleştirme bir [ `WebView` ](xref:Xamarin.Forms.WebView) UWP iletisi iletişim kutusunda JavaScript uyarıları görüntülemek için. Daha fazla bilgi için [JavaScript uyarıları görüntüleme](#webview-javascript-alert).
- Etkinleştirme bir [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) yazım denetimi altyapısıyla etkileşim kurmak için. Daha fazla bilgi için [etkinleştirmek SearchBar yazım denetimi](#searchbar-spellcheck).
- Metin içeriğini okuma düzenini algılama [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), ve [ `Label` ](xref:Xamarin.Forms.Label) örnekleri. Daha fazla bilgi için [algılama okuma düzeni içeriğinden](#inputview-readingorder).
- Desteklenen bir eski renk modunu devre dışı bırakma [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Daha fazla bilgi için [eski renk modunu devre dışı bırakma](#legacy-color-mode).
- Dokunma hareketi desteğini etkinleştirme bir [ `ListView` ](xref:Xamarin.Forms.ListView). Daha fazla bilgi için [etkinleştirme dokunun hareket desteği bir ListView](#listview-selectionmode).

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>Araç çubuğu yerleştirme değiştirme

Bu platforma özel bir araç çubuğu yerleşimini değiştirmek için kullanılan bir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)ve XAML içinde ayarlayarak tüketilen [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) ekli özellik değerine [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) sabit listesi:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>` Yöntemini belirtir bu platforma özgü yalnızca Windows üzerinde çalışır. [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) araç çubuğu yerleştirme kümesi için kullanılan ad alanı, [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) numaralandırma sağlama üç değer: [ `Default` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default), [ `Top` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top), ve [ `Bottom` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom).

Belirtilen araç çubuğu yerleştirme uygulandığı sonucudur [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) örneği:

[![](windows-images/toolbar-placement.png "Araç çubuğu yerleştirme platforma özgü")](windows-images/toolbar-placement-large.png#lightbox "araç çubuğu yerleştirme platforma özgü")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Daraltma MasterDetailPage gezinti çubuğu

Bu platforma özgü gezinti çubuğunun üzerindeki daraltmak için kullanılan bir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)ve XAML içinde ayarlayarak tüketilen [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) ve [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)iliştirilmiş özellikler:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>` Yöntemini belirtir bu platforma özgü yalnızca Windows üzerinde çalışır. [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) ad alanı, Daralt stilini belirtmek için kullanılan [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) iki sağlama numaralandırması değerler: [ `Full` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) ve [ `Partial` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial). [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) Yöntemi kısmen daraltılmış gezinti çubuğunun genişliğini belirtmek için kullanılır.

Sonuç belirtilen olan [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) uygulanan [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) örneğiyle aynı zamanda belirtilen genişlik:

[![](windows-images/collapsed-navigation-bar.png "Gezinti çubuğu platforma özgü daraltılmış")](windows-images/collapsed-navigation-bar-large.png#lightbox "daraltılmış gezinti çubuğu platforma özgü")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>JavaScript uyarıları görüntüleme

Bu platforma özgü sağlayan bir [ `WebView` ](xref:Xamarin.Forms.WebView) UWP iletisi iletişim kutusunda JavaScript uyarıları görüntülemek için. XAML içinde ayarlayarak tüketilir [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) özelliğine bağlı bir `boolean` değeri:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

`WebView.On<Windows>` Yöntemi bu platforma özgü yalnızca evrensel Windows platformu üzerinde çalışmasını belirtir. [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) ad alanı, JavaScript uyarılar etkin olup olmadığını denetlemek için kullanılır. Ayrıca, `WebView.SetIsJavaScriptAlertEnabled` yöntemi çağırarak JavaScript uyarılar geçiş yapmak için kullanılabilir [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) bunlar etkinleştirilip etkinleştirilmediğini döndürülecek yöntemi:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

JavaScript uyarılar UWP iletisi iletişim kutusunda görüntülenebilir oluşur:

![WebView JavaScript uyarı platforma özgü](windows-images/webview-javascript-alert.png "WebView JavaScript uyarı platforma özgü")

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>SearchBar yazım denetimi etkinleştirme

Bu platforma özgü sağlayan bir [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) yazım denetimi altyapısıyla etkileşim kurmak için. XAML içinde ayarlayarak tüketilir [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) özelliğine bağlı bir `boolean` değeri:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>` Yöntemi bu platforma özgü yalnızca evrensel Windows platformu üzerinde çalışmasını belirtir. [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) ad alanı, yazım denetleyicisi açıp kapatır. Ayrıca, `SearchBar.SetIsSpellCheckEnabled` yöntemi çağırarak yazım denetleyicisi geçiş yapmak için kullanılabilir [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) yazım denetleyicisi etkinleştirilip etkinleştirilmeyeceğini döndürülecek yöntemi:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Girilen metni sonucudur [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) yazım, kullanıcıya gösterilen hatalı beklemesini ile denetlenebilir:

![SearchBar yazım denetimi platforma özgü](windows-images/searchbar-spellcheck.png "SearchBar yazım denetimi platforma özel")

> [!NOTE]
> `SearchBar` Sınıfını [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) aynı zamanda ad alanına sahip [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) ve [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) etkinleştirip devre dışı bırakmak için kullanılan yöntemler Yazım denetleyicisini `SearchBar`sırasıyla.

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>İçerik okuma düzenini algılama

Çift yönlü metin okuma düzeni (sağdan sola veya soldan sağa) bu platforma özgü sağlayan [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), ve [ `Label` ](xref:Xamarin.Forms.Label) dinamik olarak algılanamayacak kadar örnekleri. XAML içinde ayarlayarak tüketilir [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (için `Entry` ve `Editor` örnekleri) veya [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) özelliğine bağlı bir `boolean` değeri:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>` Yöntemi bu platforma özgü yalnızca evrensel Windows platformu üzerinde çalışmasını belirtir. [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) ad alanı, okuma düzeni içeriğinden algılandığında olup olmadığını denetlemek için kullanılan [ `InputView` ](xref:Xamarin.Forms.InputView). Ayrıca, `InputView.SetDetectReadingOrderFromContent` yöntemi okuma düzeni içerikten çağrılarak algılandı görüntülenmeyeceğini belirlemek için kullanılabilir [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) geçerli değerini döndürmek için yöntemi:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

Sonuç [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), ve [ `Label` ](xref:Xamarin.Forms.Label) örnekleri dinamik olarak algılanan içeriklerinin okuma düzeni sahip olabilir:

[![Platforma özgü içeriğinden okuma düzeni algılama InputView](windows-images/editor-readingorder.png "okuma düzeni platforma özgü içeriğinden algılama InputView")](windows-images/editor-readingorder-large.png#lightbox "InputView okuma sırasından algılama platforma özgü içerik")

> [!NOTE]
> Ayar aksine [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği, kendi metin içeriğinden okuma düzeni görünümünde bulunan metin hizalamasını etkilemez algılamak görünümler için mantığı. Bunun yerine, çift yönlü metin bloklarını düzenlenmiştir sırasını ayarlar.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Eski renk modunu devre dışı bırakma

Xamarin.Forms görünümlerinden bazılarını eski renk modu özelliği. Bu modda olduğunda [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) görünümün özelliği `false`, görünümü devre dışı durumu için varsayılan yerel renklerle kullanıcı tarafından ayarlanan renkleri geçersiz kılar. Geriye dönük uyumluluk, bu eski renk modu desteklenen görünümler için varsayılan davranış kalır için.

Hatta görünümü devre dışı bırakıldığında kullanıcı tarafından bir görünüm üzerinde ayarlanan renkleri kalmasını sağlamak bu platforma özgü bu eski renk modu devre dışı bırakır. XAML içinde ayarlayarak tüketilir [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) özelliğine bağlı `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>` Yöntemini belirtir bu platforma özgü yalnızca Windows üzerinde çalışır. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) ad alanı, eski renk modunu devre dışı olup olmadığını denetlemek için kullanılır. Ayrıca, [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) yöntemi, eski renk modunu devre dışı bırakılıp bırakılmadığını döndürmek için kullanılabilir.

Bir görünüm üzerinde kullanıcı tarafından ayarlanan renkleri görünümü devre dışı bırakıldığında bile kalır eski renk modunu devre dışı bırakılabilir olduğunu, oluşur:

![](windows-images/legacy-color-mode-disabled.png "Eski renk modu devre dışı")

> [!NOTE]
> Ayarlarken bir [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) bir görünüm üzerinde eski renk modunu tamamen yoksayılır. Görsel durumlar hakkında daha fazla bilgi için bkz: [Xamarin.Forms görsel durum Yöneticisi](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>Bir ListView içinde dokunma hareketi desteğini etkinleştirme

Xamarin.Forms varsayılan Evrensel Windows platformu üzerinde [ `ListView` ](xref:Xamarin.Forms.ListView) yerel kullanan `ItemClick` yerel yerine etkileşim yanıt olay `Tapped` olay. Bu erişilebilirlik işlevlerini, Windows okuyucu ve klavye ile etkileşimde bulunmasını sağlar `ListView`. Ancak, herhangi bir dokunun hareketleri içinde da işler `ListView` çalışamaz.

Bu platforma özel denetimleri olmadığını öğeler bir [ `ListView` ](xref:Xamarin.Forms.ListView) hareketler, dokunma yanıt verebilir ve bu nedenle olup olmadığını yerel `ListView` ateşlenir `ItemClick` veya `Tapped` olay. XAML içinde ayarlayarak tüketilir [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) ekli özellik değerine [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) sabit listesi:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>` Yöntemi bu platforma özgü yalnızca evrensel Windows platformu üzerinde çalışmasını belirtir. [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) ad alanı, kullanılan denetimine olup öğeler bir [ `ListView` ](xref:Xamarin.Forms.ListView) ile hareketler, dokunma yanıt verebilir [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) iki olası değerler sağlayan bir sabit listesi:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – belirten `ListView` yerel ateşlenir `ItemClick` etkileşim işlemek ve bu nedenle erişilebilirlik işlevselliği sağlamak için olay. Bu nedenle, Windows okuyucu ve klavye ile etkileşim kurabilir `ListView`. Ancak, öğeler `ListView` hareketlerini dokunun yanıt veremez. Varsayılan davranışı budur `ListView` Evrensel Windows platformu üzerinde örnekleri.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – belirten `ListView` yerel ateşlenir `Tapped` etkileşim işlemek için olay. Bu nedenle, öğeler `ListView` hareketlerini dokunun vermesini sağlayabilirsiniz. Ancak, Erişilebilirlik işlevi yoktur ve bu nedenle Windows ekran okuyucu ve klavye ile etkileşime giremezler `ListView`.

> [!NOTE]
> `Accessible` Ve `Inaccessible` seçim modları karşılıklı olarak birbirini dışlar ve erişilebilir bir arasında seçmeniz gerekecektir [ `ListView` ](xref:Xamarin.Forms.ListView) veya `ListView` dokunma hareketlerini yanıtlayabilir.

Ayrıca, [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) yöntemi, geçerli döndürmek için kullanılabilir [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Sonuç belirtilen olan [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) uygulanan [ `ListView` ](xref:Xamarin.Forms.ListView), hangi denetimlerin olmadığını öğeler `ListView` hareketler, dokunma yanıt verebilir ve bu nedenle olup olmadığını yerel `ListView` ateşlenir `ItemClick` veya `Tapped` olay.

## <a name="summary"></a>Özet

Bu makalede Windows platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir. Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [Platform Özellikleri Oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
