---
title: Platform özellikleri
description: Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 1fc00d463ed28960135094d59b22bd33b46f68fa
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38985937"
---
# <a name="platform-specifics"></a>Platform özellikleri

_Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar._

Aşağıdaki platforma özgü işlevi Xamarin.Forms içinde yerleşik olarak bulunur:

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[MasterDetailPage.CollapsedPaneWidth ve MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|[WebView.IsJavaScriptAlertEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#webview-javascript-alert)
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[VisualElement.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|[SearchBar.IsSpellCheckEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#searchbar-spellcheck)
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[Application.SendDisappearingEventOnPause Application.SendAppearingEventOnResume ve Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|[InputView.DetectReadingOrderFromContent, Label.DetectReadingOrderFromContent](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#inputview-readingorder)
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|[WebView.MixedContentMode](~/xamarin-forms/platform/platform-specifics/consuming/android.md#webview-mixed-content)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#legacy-color-mode)|
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|[Entry.ImeOptions](~/xamarin-forms/platform/platform-specifics/consuming/android.md#entry-imeoptions)|[ListView.SelectionMode](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#listview-selectionmode)|
|[Page.PrefersStatusBarHidden ve Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#legacy-color-mode)|[TabbedPage.HeaderIconsEnabled ve TabbedPage.HeaderIconsSize](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#tabbedpage-icons)|
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|[Button.UseDefaultPadding ve Button.UseDefaultShadow](~/xamarin-forms/platform/platform-specifics/consuming/android.md#button-padding-shadow)|[VisualElement.AccessKey, VisualElement.AccessKeyPlacement VisualElement.AccessKeyHorizontalOffset ve VisualElement.AccessKeyVerticalOffset](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys)|
|[ListView.SeparatorStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#listview-separatorstyle)|[TabbedPage.ToolbarPlacement TabbedPage.BarItemColor ve TabbedPage.BarSelectedItemColor](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar)|
|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#legacy-color-mode)|
|[VisualElement.IsShadowEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#drop-shadow)|
|[Application.PanGestureRecognizerShouldRecognizeSimultaneously](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#simultaneous-pan-gesture)|

Bir platforma özgü API fluent kodu veya XAML aracılığıyla kullanma işlemi aşağıdaki gibidir:

1. Ekleme bir `xmlns` bildirimi veya `using` yönergesi [ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) ad alanı.
1. Ekleme bir `xmlns` bildirimi veya `using` yönerge platforma özgü işlevlerini içeren ad uzayı için:
    1. İos'ta Bu, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) ad alanı.
    1. Android'de budur [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) ad alanı. Android AppCompat için budur [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) ad alanı.
    1. Evrensel Windows platformu budur [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) ad alanı.
1. Platforma özgü kod ile veya XAML uygulama `On<T>` fluent API'si. Değerini `T` olabilir [ `iOS` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOS/), [ `Android` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Android/), veya [ `Windows` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Windows/) gelen türleri [ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) ad alanı.

> [!NOTE]
> Platforma özgü kullanılamaz olduğu bir platformda tüketmeye çalışırken bir hatayla sonuçlanır değil olduğunu unutmayın. Bunun yerine, kodu platforma uygulanan özel yürütülür.

Platform özellikleri tüketilen aracılığıyla `On<T>` fluent kod API dönüş [ `IPlatformElementConfiguration` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IPlatformElementConfiguration%3CTPlatform,TElement%3E/) nesneleri. Bu, birden çok platform yöntemi geçişli ile aynı nesne üzerinde çağrılacak özellikleri sağlar.

Platform özellikleri hakkında daha fazla bilgi için bkz: [Platform özelliklerini kullanma](~/xamarin-forms/platform/platform-specifics/consuming/index.md) ve [Platform özellikleri oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Platform Özelliklerini Kullanma](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [Platform Özellikleri Oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)
