---
title: Platform özellikleri
description: Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
ms.openlocfilehash: 284cce95b4b91d10c6516fa2880a48437340db49
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "38997305"
---
# <a name="platform-specifics"></a>Platform özellikleri

_Platform özellikleri, özel oluşturucu veya efekt uygulama olmadan yalnızca belirli bir platformda mevcut işlevi kullanmasını sağlar._

Aşağıdaki platforma özgü işlevleri Xamarin.Forms görünümleri, sayfalar ve düzenleri için sağlanır:

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[VisualElement.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|[VisualElement.AccessKey, VisualElement.AccessKeyPlacement VisualElement.AccessKeyHorizontalOffset ve VisualElement.AccessKeyVerticalOffset](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys)|
|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#legacy-color-mode)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#legacy-color-mode)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#legacy-color-mode)|
|[VisualElement.IsShadowEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#drop-shadow)|

Aşağıdaki platforma özel işlevler için Xamarin.Forms görünümleri sağlanır:

|iOS|Android|Windows|
|--- |--- |--- |
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|[Button.UseDefaultPadding ve Button.UseDefaultShadow](~/xamarin-forms/platform/platform-specifics/consuming/android.md#button-padding-shadow)|[InputView.DetectReadingOrderFromContent, Label.DetectReadingOrderFromContent](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#inputview-readingorder)|
|[Entry.CursorColor](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#entry-cursorcolor)|[Entry.ImeOptions](~/xamarin-forms/platform/platform-specifics/consuming/android.md#entry-imeoptions)|[ListView.SelectionMode](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#listview-selectionmode)|
|[ListView.SeparatorStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#listview-separatorstyle)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[SearchBar.IsSpellCheckEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#searchbar-spellcheck)|
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|[WebView.MixedContentMode](~/xamarin-forms/platform/platform-specifics/consuming/android.md#webview-mixed-content)|[WebView.IsJavaScriptAlertEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#webview-javascript-alert)|
|[Slider.UpdateOnTap](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#slider-updateontap)|

Xamarin.Forms sayfaları için aşağıdaki platforma özgü işlevleri sağlanır:

|iOS|Android|Windows|
|--- |--- |--- |
|[NavigationPage.HideSeparatorBar](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#navigationpage-hideseparatorbar)|[NavigationPage.BarHeight](~/xamarin-forms/platform/platform-specifics/consuming/android.md#navigationpage-barheight)|[MasterDetailPage.CollapsedPaneWidth ve MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[TabbedPage.IsSmoothScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-transition-animations)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|[TabbedPage.HeaderIconsEnabled ve TabbedPage.HeaderIconsSize](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#tabbedpage-icons)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[TabbedPage.ToolbarPlacement TabbedPage.BarItemColor ve TabbedPage.BarSelectedItemColor](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar)|
|[Page.ModalPresentationStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#modal-page-presentation-style)|
|[Page.PrefersStatusBarHidden ve Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|

Aşağıdaki platforma özel işlevler için Xamarin.Forms düzenleri sağlanır:

|iOS|
|--- |
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|

Aşağıdaki platforma özel işlevler için Xamarin.Forms sağlanan [ `Application` ](xref:Xamarin.Forms.Application) sınıfı:

|iOS|Android|
|--- |--- |
|[Application.PanGestureRecognizerShouldRecognizeSimultaneously](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#simultaneous-pan-gesture)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|
||[Application.SendDisappearingEventOnPause Application.SendAppearingEventOnResume ve Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|

## <a name="consuming-platform-specifics"></a>Platform özelliklerini kullanma

Bir platforma özgü API fluent kodu veya XAML aracılığıyla kullanma işlemi aşağıdaki gibidir:

1. Ekleme bir `xmlns` bildirimi veya `using` yönergesi [ `Xamarin.Forms.PlatformConfiguration` ](xref:Xamarin.Forms.PlatformConfiguration) ad alanı.
1. Ekleme bir `xmlns` bildirimi veya `using` yönerge platforma özgü işlevlerini içeren ad uzayı için:
    1. İos'ta Bu, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ad alanı.
    1. Android'de budur [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ad alanı. Android AppCompat için budur [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) ad alanı.
    1. Evrensel Windows platformu budur [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) ad alanı.
1. Platforma özgü kod ile veya XAML uygulama `On<T>` fluent API'si. Değerini `T` olabilir [ `iOS` ](xref:Xamarin.Forms.PlatformConfiguration.iOS), [ `Android` ](xref:Xamarin.Forms.PlatformConfiguration.Android), veya [ `Windows` ](xref:Xamarin.Forms.PlatformConfiguration.Windows) gelen türleri [ `Xamarin.Forms.PlatformConfiguration` ](xref:Xamarin.Forms.PlatformConfiguration) ad alanı.

> [!NOTE]
> Platforma özgü kullanılamaz olduğu bir platformda tüketmeye çalışırken bir hatayla sonuçlanır değil olduğunu unutmayın. Bunun yerine, kodu platforma uygulanan özel yürütülür.

Platform özellikleri tüketilen aracılığıyla `On<T>` fluent kod API dönüş [ `IPlatformElementConfiguration` ](xref:Xamarin.Forms.IPlatformElementConfiguration`2) nesneleri. Bu, birden çok platform yöntemi geçişli ile aynı nesne üzerinde çağrılacak özellikleri sağlar.

Platform özellikleri hakkında daha fazla bilgi için bkz: [Platform özelliklerini kullanma](~/xamarin-forms/platform/platform-specifics/consuming/index.md) ve [Platform özellikleri oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Platform Özelliklerini Kullanma](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [Platform Özellikleri Oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](xref:Xamarin.Forms.PlatformConfiguration)
