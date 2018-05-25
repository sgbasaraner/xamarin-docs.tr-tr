---
title: Windows platformu-özellikleri
description: Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar. Bu makalede Windows platform-Xamarin.Forms yerleşik özellikleri kullanma gösterilmektedir.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: d4ddb662bf167a0c80561cce097104a7f5fc8096
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2018
---
# <a name="windows-platform-specifics"></a>Windows platformu-özellikleri

_Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar. Bu makalede Windows platform-Xamarin.Forms yerleşik özellikleri kullanma gösterilmektedir._

Evrensel Windows Platformu (UWP üzerinde), Xamarin.Forms aşağıdaki platform özellikleri içerir:

- Araç çubuğu yerleştirme seçeneklerini ayarlama. Daha fazla bilgi için bkz: [araç çubuğu yerleştirme değiştirme](#toolbar_placement).
- Daraltma [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) gezinti çubuğu. Daha fazla bilgi için bkz: [MasterDetailPage gezinti çubuğu daraltma](#collapsable_navigation_bar).
- Etkinleştirme bir [ `WebView` ](xref:Xamarin.Forms.WebView) bir UWP iletisi iletişim kutusunda JavaScript uyarıları görüntülemek için. Daha fazla bilgi için bkz: [JavaScript uyarıları görüntüleme](#webview-javascript-alert).

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>Araç çubuğu yerleştirme değiştirme

Bu platforma özgü bir araç çubuğu yerleşimini değiştirmek için kullanılan bir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)ve ayarlayarak XAML'de tüketilen [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) özellik değerine bağlı [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) numaralandırma:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>` Yöntemi belirtir. bu platforma özgü yalnızca Windows üzerinde çalışır. [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) ad alanı, araç çubuğu yerleştirme ile ayarlamak için kullanılan [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) numaralandırma sağlama üç değerden: [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default/), [ `Top` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top/), ve [ `Bottom` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom/).

Belirtilen araç çubuğu yerleştirme uygulandığı sonucudur [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) örneği:

[![](windows-images/toolbar-placement.png "Araç çubuğu yerleştirme platforma özgü")](windows-images/toolbar-placement-large.png#lightbox "araç çubuğu yerleştirme platforma özgü")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>MasterDetailPage gezinti çubuğu daraltma

Bu platforma özgü gezinti çubuğunun üzerindeki daraltmak için kullanılan bir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)ve ayarlayarak XAML'de tüketilen [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) ve [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)ekli özellikler:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>` Yöntemi belirtir. bu platforma özgü yalnızca Windows üzerinde çalışır. [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) ad alanı, Daralt stili belirtmek için kullanılan [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) iki sağlama numaralandırması değerler: [ `Full` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full/) ve [ `Partial` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial/). [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) Yöntemi kısmen daraltılmış gezinti çubuğunun genişliğini belirtmek için kullanılır.

Sonuç belirtilen olan [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) uygulanan [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) örneğiyle aynı zamanda belirtilen genişlik:

[![](windows-images/collapsed-navigation-bar.png "Gezinti çubuğu platforma özgü daraltılmış")](windows-images/collapsed-navigation-bar-large.png#lightbox "gezinti çubuğu platforma özgü daraltılmış")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>JavaScript uyarıları görüntüleme

Bu platforma özgü sağlayan bir [ `WebView` ](xref:Xamarin.Forms.WebView) bir UWP iletisi iletişim kutusunda JavaScript uyarıları görüntülemek için. XAML'de ayarlayarak tüketiliyor [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) özelliğine bağlı bir `boolean` değeri:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

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

`WebView.On<Windows>` Yöntemi belirtir. bu platforma özgü yalnızca evrensel Windows platformu üzerinde çalışır. [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) ad alanı, JavaScript uyarıları etkin olup olmadığını denetlemek için kullanılır. Ayrıca, `WebView.SetIsJavaScriptAlertEnabled` yöntemi çağrılarak JavaScript uyarıları geçiş yapmak için kullanılabilir [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) bunlar etkinleştirilip etkinleştirilmediğini döndürülecek yöntemi:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Bir UWP iletisi iletişim kutusunda JavaScript uyarıları görüntülenebilir oluşur:

![Web görünümü JavaScript uyarı platforma özgü](windows-images/webview-javascript-alert.png "WebView JavaScript uyarı platforma özgü")

## <a name="summary"></a>Özet

Bu makalede Windows platform-Xamarin.Forms yerleşik özellikleri kullanma gösterilmektedir. Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [Platform Özellikleri Oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
