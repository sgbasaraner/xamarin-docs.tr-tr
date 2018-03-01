---
title: Windows Platform-Specifics
description: "Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar. Bu makalede Windows platform-Xamarin.Forms yerleşik özellikleri kullanma gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: da9279939af8dc4033cd89769a7add60a745ac85
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="windows-platform-specifics"></a>Windows Platform-Specifics

_Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar. Bu makalede Windows platform-Xamarin.Forms yerleşik özellikleri kullanma gösterilmektedir._

Evrensel Windows Platformu (UWP üzerinde), Xamarin.Forms aşağıdaki platform özellikleri içerir:

- Araç çubuğu yerleştirme seçenekleri. Daha fazla bilgi için bkz: [araç çubuğu yerleştirme değiştirme](#toolbar_placement).
- Kısmen daraltılabilir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) gezinti çubuğu. Daha fazla bilgi için bkz: [MasterDetailPage gezinti çubuğu daraltma](#collapsable_navigation_bar).

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

[![](windows-images/toolbar-placement.png "Araç çubuğu yerleştirme platforma özgü")](windows-images/toolbar-placement-large.png "araç çubuğu yerleştirme platforma özgü")

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

[![](windows-images/collapsed-navigation-bar.png "Gezinti çubuğu platforma özgü daraltılmış")](windows-images/collapsed-navigation-bar-large.png "gezinti çubuğu platforma özgü daraltılmış")

## <a name="summary"></a>Özet

Bu makalede Windows platform-Xamarin.Forms yerleşik özellikleri kullanma gösterilmektedir. Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Platform özellikleri oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
