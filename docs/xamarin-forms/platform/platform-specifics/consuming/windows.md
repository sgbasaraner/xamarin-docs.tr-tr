---
title: Windows Platform özellikleri
description: Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar. Bu makalede Windows platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 43a681350035c3e965798bd63f49cd39f472ebfd
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998422"
---
# <a name="windows-platform-specifics"></a>Windows Platform özellikleri

_Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar. Bu makalede Windows platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir._

Xamarin.Forms, Evrensel Windows Platformu (UWP üzerinde), aşağıdaki platform özellikleri içerir:

- Araç çubuğu yerleştirme seçeneklerini ayarlama. Daha fazla bilgi için [sayfası araç çubuğu yerleştirme değiştirme](#toolbar_placement).
- Daraltma [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) gezinti çubuğu. Daha fazla bilgi için [MasterDetailPage gezinti çubuğu daraltma](#collapsable_navigation_bar).
- Etkinleştirme bir [ `WebView` ](xref:Xamarin.Forms.WebView) UWP iletisi iletişim kutusunda JavaScript uyarıları görüntülemek için. Daha fazla bilgi için [JavaScript uyarıları görüntüleme](#webview-javascript-alert).
- Etkinleştirme bir [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) yazım denetimi altyapısıyla etkileşim kurmak için. Daha fazla bilgi için [etkinleştirmek SearchBar yazım denetimi](#searchbar-spellcheck).
- Metin içeriğini okuma düzenini algılama [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), ve [ `Label` ](xref:Xamarin.Forms.Label) örnekleri. Daha fazla bilgi için [algılama okuma düzeni içeriğinden](#inputview-readingorder).
- Desteklenen bir eski renk modunu devre dışı bırakma [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Daha fazla bilgi için [eski renk modunu devre dışı bırakma](#legacy-color-mode).
- Dokunma hareketi desteğini etkinleştirme bir [ `ListView` ](xref:Xamarin.Forms.ListView). Daha fazla bilgi için [etkinleştirme dokunun hareket desteği bir ListView](#listview-selectionmode).
- Görüntülenecek sayfası simgeler etkinleştirme bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) araç çubuğu. Daha fazla bilgi için [simgeleri bir TabbedPage etkinleştirme](#tabbedpage-icons).
- İçin bir erişim anahtarı ayarı bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Daha fazla bilgi için [ayarı VisualElement erişim anahtarlarını](#visualelement-accesskeys).

<a name="toolbar_placement" />

## <a name="changing-the-page-toolbar-placement"></a>Sayfa araç çubuğu yerleştirme değiştirme

Bu platforma özel bir araç çubuğu yerleşimini değiştirmek için kullanılan bir [ `Page` ](xref:Xamarin.Forms.Page)ve XAML içinde ayarlayarak tüketilen [ `Page.ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty) ekli özellik değerine [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) sabit listesi:

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

`Page.On<Windows>` Yöntemini belirtir bu platforma özgü yalnızca Windows üzerinde çalışır. [ `Page.SetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) araç çubuğu yerleştirme kümesi için kullanılan ad alanı, [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) numaralandırma sağlama üç değer: [ `Default` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default), [ `Top` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top), ve [ `Bottom` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom).

Belirtilen araç çubuğu yerleştirme uygulandığı sonucudur [ `Page` ](xref:Xamarin.Forms.Page) örneği:

[![](windows-images/toolbar-placement.png "Araç çubuğu yerleştirme platforma özgü")](windows-images/toolbar-placement-large.png#lightbox "araç çubuğu yerleştirme platforma özgü")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Daraltma MasterDetailPage gezinti çubuğu

Bu platforma özgü gezinti çubuğunun üzerindeki daraltmak için kullanılan bir [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)ve XAML içinde ayarlayarak tüketilen [ `MasterDetailPage.CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) ve [ `MasterDetailPage.CollapsedPaneWidth` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty)iliştirilmiş özellikler:

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

`MasterDetailPage.On<Windows>` Yöntemini belirtir bu platforma özgü yalnızca Windows üzerinde çalışır. [ `Page.SetCollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) ad alanı, Daralt stilini belirtmek için kullanılan [ `CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) iki sağlama numaralandırması değerler: [ `Full` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) ve [ `Partial` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial). [ `MasterDetailPage.CollapsedPaneWidth` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},System.Double)) Yöntemi kısmen daraltılmış gezinti çubuğunun genişliğini belirtmek için kullanılır.

Sonuç belirtilen olan [ `CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) uygulanan [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) örneğiyle aynı zamanda belirtilen genişlik:

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

<a name="tabbedpage-icons" />

## <a name="enabling-icons-on-a-tabbedpage"></a>Simgeleri bir TabbedPage etkinleştirme

Bu platforma özgü görüntülenecek sayfası simgeler sağlar bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) araç ve isteğe bağlı olarak simge boyutunu belirtme olanağı sağlar. XAML içinde ayarlayarak tüketilir [ `TabbedPage.HeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty) özelliğine bağlı `true`ve isteğe bağlı olarak ayarlayarak [ `TabbedPage.HeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty) özelliğine bağlı bir [ `Size` ](xref:Xamarin.Forms.Size) değeri:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" Icon="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" Icon="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" Icon="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
    {
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", Icon = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", Icon = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", Icon = "contacts.png" });
  }
}
```

`TabbedPage.On<Windows>` Yöntemi bu platforma özgü yalnızca evrensel Windows platformu üzerinde çalışmasını belirtir. [ `TabbedPage.SetHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) ad alanı, üstbilgi simgelerini açıp kapatmak için kullanılır. [ `TabbedPage.SetHeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsSize(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},Xamarin.Forms.Size)) Yöntemi üst bilgisi simgesi boyutu ile isteğe bağlı olarak belirtir bir [ `Size` ](xref:Xamarin.Forms.Size) değeri.

Ayrıca, `TabbedPage` sınıfını `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` aynı zamanda ad alanına sahip bir [ `EnableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*) yönteminin üst bilgi simgeleri tanıyan bir [ `DisableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*) üstbilgi simgeler, devre dışı bırakan yöntemi ve bir [ `IsHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*) döndüren yöntem bir `boolean` üst bilgi simgeleri etkin olup olmadığını gösteren değer.

Simgeler görüntülenebilir sayfa sonucu olacak bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) araç çubuğu için istenen boyuta isteğe bağlı olarak ayarlanıyor simgesi boyutu:

![TabbedPage simgeleri etkin platforma özgü](windows-images/tabbedpage-icons.png "TabbedPage simgeler etkin platforma özgü")

<a name="visualelement-accesskeys" />

## <a name="setting-visualelement-access-keys"></a>Ayar VisualElement erişim anahtarları

Erişim anahtarları olan klavye kısayolları, uygulamanın görünür kullanıcı Arabirimi yerine bir klavye touch aracılığıyla aracılığıyla hızla gidin ve etkileşim kullanımı kolay bir yol sağlayarak kullanılabilirliğini ve erişilebilirliğini Evrensel Windows platformu uygulamaları geliştirmek veya Fare. Bunlar, Alt tuşunu ve genellikle sırasıyla basılan bir veya daha fazla alfasayısal anahtarlarınızı birleşimleridir. Klavye kısayolları, tek bir alfasayısal karakter kullanmak için erişim anahtarlarını otomatik olarak desteklenir.

Erişim anahtarı ipuçları, erişim anahtarlarını içeren denetimleri yanında görüntülenen rozetleri kayan. Her erişim anahtar ipucu ilişkili denetim etkinleştirme alfasayısal anahtarlarını içerir. Bir kullanıcı Alt tuşuna bastığında erişim anahtar ipuçları görüntülenir.

Bu platforma özgü bir erişim anahtarı belirtmek için kullanılan bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). XAML içinde ayarlayarak tüketilir [ `VisualElement.AccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) alfasayısal bir değer ve isteğe bağlı olarak ayarlayarak ekli özellik [ `VisualElement.AccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) değerineekliözellik[ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) numaralandırma [ `VisualElement.AccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) özelliğine bağlı bir `double`ve [ `VisualElement.AccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) bir içinekliözellik`double`:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <ContentPage Title="Page 1"
                 windows:VisualElement.AccessKey="1">
        <StackLayout Margin="20">
            ...
            <Switch windows:VisualElement.AccessKey="A" />
            <Entry Placeholder="Enter text here"
                   windows:VisualElement.AccessKey="B" />
            ...
            <Button Text="Access key F, placement top with offsets"
                    Margin="20"
                    Clicked="OnButtonClicked"
                    windows:VisualElement.AccessKey="F"
                    windows:VisualElement.AccessKeyPlacement="Top"
                    windows:VisualElement.AccessKeyHorizontalOffset="20"
                    windows:VisualElement.AccessKeyVerticalOffset="20" />
            ...
        </StackLayout>
    </ContentPage>
    ...
</TabbedPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var page = new ContentPage { Title = "Page 1" };
page.On<Windows>().SetAccessKey("1");

var switchView = new Switch();
switchView.On<Windows>().SetAccessKey("A");
var entry = new Entry { Placeholder = "Enter text here" };
entry.On<Windows>().SetAccessKey("B");
...

var button4 = new Button { Text = "Access key F, placement top with offsets", Margin = new Thickness(20) };
button4.Clicked += OnButtonClicked;
button4.On<Windows>()
    .SetAccessKey("F")
    .SetAccessKeyPlacement(AccessKeyPlacement.Top)
    .SetAccessKeyHorizontalOffset(20)
    .SetAccessKeyVerticalOffset(20);
...
```

`VisualElement.On<Windows>` Yöntemi bu platforma özgü yalnızca evrensel Windows platformu üzerinde çalışmasını belirtir. [ `VisualElement.SetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.String)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) erişim için anahtar değeri ayarlamak için kullanılan ad alanı, `VisualElement`. [ `VisualElement.SetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},Xamarin.Forms.AccessKeyPlacement)) Yöntemi, isteğe bağlı olarak erişim anahtar İpucu ile görüntülemek için kullanılacak konumu belirtir [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) aşağıdaki olası değerler sağlayan bir sabit listesi:

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) – erişim anahtar ipucu yerleştirme işletim sistemi tarafından belirlenir gösterir.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) – erişim anahtar ipucu üst kenarı görüntüleneceğini belirtir `VisualElement`.
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) – erişim anahtar ipucu alt kenarı görüntüleneceğini belirtir `VisualElement`.
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) – erişim anahtar ipucu öğenin sağ kenarı sağa görüntüleneceğini belirtir `VisualElement`.
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) – erişim anahtar ipucu sol kenarı sola görüntüleneceğini belirtir `VisualElement`.
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) – erişim anahtar ipucu merkezinin Kaplanmış görüntüleneceğini belirtir `VisualElement`.

> [!NOTE]
> Genellikle, [ `Auto` ](xref:Xamarin.Forms.AccessKeyPlacement.Auto) anahtar ipucu yerleştirme yeterli, Uyarlamalı kullanıcı arabirimleri için destek içerir.

[ `VisualElement.SetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) Ve [ `VisualElement.SetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) yöntemleri erişim anahtar ipucu konumun daha ayrıntılı bir denetim için kullanılabilir. Bağımsız değişkeni `SetAccessKeyHorizontalOffset` yöntemi gösterir nasıl erişim anahtar ipucu sol ucundaki taşımak ya da sağ ve bağımsız değişkeni `SetAccessKeyVerticalOffset` yöntemi uzak erişim anahtar ipucu yukarı veya aşağı taşımak nasıl gösterir.

>[!NOTE]
> Erişim anahtarı yerleştirme ayarlandığında erişim anahtar ipucu uzaklıkları ayarlanamaz `Auto`.

Ayrıca, [ `GetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), ve [ `GetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) yöntemleri kullanılabilir bir erişim almak için anahtar değeri ya da onun konumu.

Erişim anahtarı ipuçları herhangi yanındaki görüntülenebilir, sonucudur [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) tanımlayan örneklere erişim anahtarları, Alt tuşuna basarak:

![Platforma özgü VisualElement erişim tuşları](windows-images/visualelement-accesskeys.png "platforma özgü VisualElement erişim anahtarları")

Bir kullanıcı tarafından erişim ardından Alt tuşuna basarak bir erişim anahtarı etkinleştirirken anahtar, için varsayılan eylem `VisualElement` yürütülür. Örneğin, ne zaman bir kullanıcı etkinleştirir erişim anahtarı üzerinde bir [ `Switch` ](xref:Xamarin.Forms.Switch), `Switch` yükseğe. Ne zaman bir kullanıcı etkinleştirir erişim anahtarı üzerinde bir [ `Entry` ](xref:Xamarin.Forms.Entry), `Entry` odak kazanır. Ne zaman bir kullanıcı etkinleştirir erişim anahtarı üzerinde bir [ `Button` ](xref:Xamarin.Forms.Button), için olay işleyicisi [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) olay yürütülür.

Erişim anahtarları hakkında daha fazla bilgi için bkz. [erişim anahtarları](/windows/uwp/design/input/access-keys#key-tip-positioning).

## <a name="summary"></a>Özet

Bu makalede Windows platform Xamarin.Forms içinde oluşturulmuş özellikleri kullanma gösterilmektedir. Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [Platform Özellikleri Oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
