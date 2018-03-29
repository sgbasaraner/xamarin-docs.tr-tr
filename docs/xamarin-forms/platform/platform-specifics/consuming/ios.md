---
title: iOS Platform-Specifics
description: "Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar. Bu makalede, iOS platformu-Xamarin.Forms yerleşik özellikleri kullanmak gösterilmiştir."
ms.topic: article
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/16/2017
ms.openlocfilehash: 798bb2b15534a620acbe76080e171af1a548ac25
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="ios-platform-specifics"></a>iOS Platform-Specifics

_Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar. Bu makalede, iOS platformu-Xamarin.Forms yerleşik özellikleri kullanmak gösterilmiştir._

İos'ta, Xamarin.Forms aşağıdaki platform özellikleri içerir:

- Herhangi bir desteği ölçeklendirilmelidir [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/). Daha fazla bilgi için bkz: [uygulama Ölçeklendirilmelidir](#blur).
- Sayfa başlığı sayfa gezinti çubuğunda büyük bir başlık olarak görüntülenir olup olmadığını denetleme. Daha fazla bilgi için bkz: [görüntüleme büyük başlık](#large_title).
- Bu sayfa içeriği sağlamak, tüm iOS cihazları için güvenlidir ekranın herhangi bir alan konumlandırıldı. Daha fazla bilgi için bkz: [güvenli alan Düzen kılavuzunu etkinleştirmeden](#safe_area_layout).
- Yarı saydam gezinme çubuğu. Daha fazla bilgi için bkz: [gezinti çubuğu saydam hale getirme](#translucent_navigation_bar).
- Denetleme durum çubuğu metni üzerinde renk olup bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) gezinti çubuğunda parlaklığını eşleşecek şekilde ayarlanır. Daha fazla bilgi için bkz: [durum çubuğu metni renk modunu ayarlama](#status_bar_color_mode).
- Girilen metin sağlama uygun içine bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) yazı tipi boyutunu ayarlayarak. Daha fazla bilgi için bkz: [bir giriş yazı tipi boyutunu ayarlama](#adjust_font_size).
- Öğe seçimi içinde oluştuğunda denetleme bir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Daha fazla bilgi için bkz: [denetleme Seçici öğe seçimi](#picker_update_mode).
- Durum çubuğu görünürlük ayarını bir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Daha fazla bilgi için bkz: [bir sayfada durum çubuğu görünürlük ayarlama](#set_status_bar_visibility).
- Denetleme olup bir [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) dokunma hareketi işleyen veya içeriğine geçirir. Daha fazla bilgi için bkz: [geciktirme içerik rötuşları bir ScrollView içinde](#delay_content_touches).

<a name="blur" />

## <a name="applying-blur"></a>Bulanıklaştırma uygulama

Bu platforma özgü altında katmanlı içerik ölçeklendirilmelidir için kullanılır ve ayarlayarak XAML'de tüketilen [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) özellik değerine bağlı [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) numaralandırma:

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

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

`BoxView.On<iOS>` Yöntemi belirtir. bu platforma özgü yalnızca İos'ta çalıştırma. [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) ile Bulanıklaştırma efekti uygulamak için kullanılan ad alanı, [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) dört sağlama numaralandırması değerler: [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None/), [ `ExtraLight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight/), [ `Light` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light/), ve [ `Dark` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark/).

Sonuç belirtilen olan [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) uygulanan [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) örneği, hangi bulanıklıklaştırmalar [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) altında katmanlı:

![](ios-images/blur-effect.png "Etkisi platforma özgü ölçeklendirilmelidir")

<a name="large_title" />

## <a name="displaying-large-titles"></a>Büyük başlık görüntüleme

Bu platforma özgü iOS 11 veya büyük kullanan cihazlar için Gezinti çubuğundaki büyük bir başlık olarak sayfa başlığı görüntülemek için kullanılır. Büyük bir başlık sola hizalı ve daha büyük bir yazı tipi kullanır ve kullanıcı içerik kaydırma başladığında Ekran Gayrimenkul verimli bir şekilde kullanılmasını sağlamak amacıyla standart başlık geçiş. Ancak, yatay yönde başlığı içerik düzeni iyileştirmek için gezinti çubuğunu merkezine döndürür. XAML'de ayarlayarak tüketiliyor `NavigationPage.PrefersLargeTitles` özelliğine bağlı bir `boolean` değeri:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Alternatif olarak, fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` Yöntemi belirtir. bu platforma özgü yalnızca İos'ta çalıştırma. `NavigationPage.SetPrefersLargeTitle` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) ad alanı, büyük başlık etkinleştirilip etkinleştirilmediğini denetler.

Büyük başlık etkinleştirilmiş olması koşuluyla, [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), tüm sayfaları Gezinti yığınında büyük başlıklarını görüntüler. Bu davranış sayfalarında ayarlayarak geçersiz kılınabilir `Page.LargeTitleDisplay` özellik değerine bağlı `LargeTitleDisplayMode` numaralandırma:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Alternatif olarak, sayfa davranışı fluent API kullanarak C# geçersiz kılınabilir:

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

`Page.On<iOS>` Yöntemi belirtir. bu platforma özgü yalnızca İos'ta çalıştırma. `Page.SetLargeTitleDisplay` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) ad alanı, üzerinde büyük başlık davranışını denetleyen [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), ile `LargeTitleDisplayMode` numaralandırması üç olası sağlama değerler:

- `Always` – zorla gezinti çubuğu ve yazı tipi boyutu büyük biçimini kullanın.
- `Automatic` – Gezinti yığınında önceki öğesi olarak aynı stil (büyük veya küçük) kullanın.
- `Never` – Normal, küçük biçim gezinti çubuğu kullanımını zorunlu kılar.

Ayrıca, `SetLargeTitleDisplay` yöntemi çağrılarak numaralandırma değerlerini geçiş yapmak için kullanılabilir `LargeTitleDisplay` geçerli döndürür yöntemi `LargeTitleDisplayMode`:

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

Sonuç belirtilen olan `LargeTitleDisplayMode` uygulanan [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), büyük başlık davranışı denetler:

![](ios-images/large-title.png "Etkisi platforma özgü ölçeklendirilmelidir")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Güvenli alan Düzen kılavuzu etkinleştirme

Bu platforma özgü sayfa içeriği iOS 11 ve büyük kullanan tüm cihazlar için güvenlidir ekranın herhangi bir alan konumlandırılır emin olmak için kullanılır. Özellikle, emin olmak için sağlayacak yuvarlatılmış aygıt köşeleri, ev göstergesi ya da X iPhone algılayıcı muhafazası o içeriğin kırpılıp değil. XAML'de ayarlayarak tüketiliyor `Page.UseSafeArea` özelliğine bağlı bir `boolean` değeri:

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

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>` Yöntemi belirtir. bu platforma özgü yalnızca İos'ta çalıştırma. `Page.SetUseSafeArea` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) ad alanı, güvenli alan Düzen kılavuzu etkin olup olmadığını denetler.

Sayfa içeriği tüm İphone'lar için güvenlidir ekranın herhangi bir alan yerleştirilebilir oluşur:

[![](ios-images/safe-area-layout.png "Güvenli alan Düzen kılavuzu")](ios-images/safe-area-layout-large.png#lightbox "güvenli alan Düzen kılavuzu")

> [!NOTE]
> Apple tarafından tanımlanan güvenli alanı içinde Xamarin.Forms ayarlamak için kullanılan [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) özelliği ve ayarlanan bu özelliğinin önceki değerlerini geçersiz kılar.

Güvenli alan alarak özelleştirilebilir kendi [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) değerini `Page.SafeAreaInsets` yönteminden [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) ad alanı. Olarak daha sonra değiştirilebilir gerekli ve yeniden atanan `Padding` sayfa Oluşturucusu özelliğinde veya [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) geçersiz kıl:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

<a name="translucent_navigation_bar" />

## <a name="making-the-navigation-bar-translucent"></a>Gezinti çubuğu saydam hale getirme

Bu platforma özgü gezinti çubuğunda saydamlığını değiştirmek için kullanılır ve ayarlayarak XAML'de tüketilen [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) özelliğine bağlı bir `boolean` değeri:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>` Yöntemi belirtir. bu platforma özgü yalnızca İos'ta çalıştırma. [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) ad alanı, gezinti çubuğunda Saydam yapmak için kullanılır. Ayrıca, [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) sınıfını `Xamarin.Forms.PlatformConfiguration.iOSSpecific` da ad alanına sahip bir [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) gezinti çubuğu varsayılan durumuna geri yükler yöntemi ve [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) çağırarak gezinti çubuğu saydamlık geçiş yapmak için kullanılan yöntem [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) yöntemi:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Gezinti çubuğu saydamlığını değiştirilebilir oluşur:

![](ios-images/translucent-navigation-bar.png "Yarı saydam gezinti çubuğu platforma özgü")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Durum çubuğu metni renk modunu ayarlama

Bu platforma özgü denetimleri durum çubuğu metni üzerinde renk olup bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) gezinti çubuğunda parlaklığını eşleşecek şekilde ayarlanır. XAML'de ayarlayarak tüketiliyor [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) özellik değerine bağlı [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) numaralandırma:

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

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

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

`NavigationPage.On<iOS>` Yöntemi belirtir. bu platforma özgü yalnızca İos'ta çalıştırma. [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) ad alanı, denetimleri durum çubuğu metni üzerinde renk olup olmadığını [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) eşleşecek şekilde ayarlanır Gezinti çubuğu parlaklığını ile [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) numaralandırma sağlayan iki olası değerler:

- [`DoNotAdjust`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust/) – Durum çubuğu metin rengi değil ayarlanması gereken gösterir.
- [`MatchNavigationBarTextLuminosity`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity/) – Durum çubuğu metin rengi gezinti çubuğunda parlaklığını eşleşmelidir gösterir.

Ayrıca, [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) yöntemi, geçerli değerini almak için kullanılabilir [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) uygulanan numaralandırma [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/).

Durum çubuğu metin rengini sonucu olan bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) gezinti çubuğunda parlaklığını eşleşecek şekilde ayarlanabilir. Bu örnekte, durum çubuğu metni renk değişiklikleri kullanıcı anahtarları arasında [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) ve [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) sayfaları bir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "Durum çubuğu metni renk modu platforma özgü")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Bir giriş yazı tipi boyutunu ayarlama

Bu platforma özgü yazı tipi boyutunu ölçeklendirmek için kullanılan bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) girilen metin denetiminde sığmasını sağlamak için. XAML'de ayarlayarak tüketiliyor [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) özelliğine bağlı bir `boolean` değeri:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
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

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>` Yöntemi belirtir. bu platforma özgü yalnızca İos'ta çalıştırma. [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) uyduğunu olduğundan emin olmak için girilen metin yazı tipi boyutunu ölçeklendirmek için kullanılan ad alanı, [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). Ayrıca, [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) sınıfını `Xamarin.Forms.PlatformConfiguration.iOSSpecific` da ad alanına sahip bir [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) bu platforma devre dışı bırakır yöntemi ve [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) çağırarak ölçeklendirme yazı tipi boyutunu geçiş yapmak için kullanılan yöntem [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) yöntemi:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Yazı tipi boyutunu olan sonucu [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) girilen metin denetiminde uygun emin olmak için ölçeği:

![](ios-images/entry-font-size.png "Giriş yazı tipi boyutu platforma özgü Ayarla")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Seçici öğe seçimi denetleme

Bu platforma özgü denetimleri öğe seçimi içinde oluştuğunda bir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), öğe seçimi denetimindeki öğeleri göz atarken ya da yalnızca bir kez oluştuğunu belirlemek kullanıcının **Bitti** düğmeye basıldığında. XAML'de ayarlayarak tüketiliyor `Picker.UpdateMode` özellik değerine bağlı `UpdateMode` numaralandırma:

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

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` Yöntemi belirtir. bu platforma özgü yalnızca İos'ta çalıştırma. `Picker.SetUpdateMode` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) ad alanı, öğe seçimi oluştuğunda, denetlemek için kullanılan ile `UpdateMode` iki olası değerler sağlama numaralandırma:

- `Immediately` – öğe seçimi oluşur kullanıcı öğelerde gözatar gibi [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Xamarin.Forms varsayılan davranış budur.
- `WhenFinished` – öğe seçimi yalnızca oluşur kullanıcı bastığı sonra **Bitti** düğmesini [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/).

Ayrıca, `SetUpdateMode` yöntemi çağrılarak numaralandırma değerlerini geçiş yapmak için kullanılabilir `UpdateMode` geçerli döndürür yöntemi `UpdateMode`:

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

Sonucu belirtilen olan `UpdateMode` uygulanan [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), öğe seçimi oluştuğunda denetler:

[![](ios-images/picker-updatemode.png "Seçici UpdateMode platforma özgü")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Durum çubuğu ayarı sayfasında görünürlüğü

Bu platforma özgü durum çubuğunun görünürlüğünü ayarlamak için kullanılan bir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), ve durum çubuğu girer veya bırakır nasıl denetleme olanağı içerir `Page`. XAML'de ayarlayarak tüketiliyor `Page.PrefersStatusBarHidden` özellik değerine bağlı `StatusBarHiddenMode` numaralandırma ve isteğe bağlı olarak `Page.PreferredStatusBarUpdateAnimation` özellik değerine bağlı `UIStatusBarAnimation` numaralandırma:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>` Yöntemi belirtir. bu platforma özgü yalnızca İos'ta çalıştırma. `Page.SetPrefersStatusBarHidden` Yöntemi, `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ad alanı, durum çubuğunun görünürlüğünü ayarlamak için kullanılan bir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) birini belirterek `StatusBarHiddenMode` numaralandırma değerlerinin: `Default`, `True` , veya `False`. `StatusBarHiddenMode.True` Ve `StatusBarHiddenMode.False` değerleri durum çubuğu görünürlük cihaz yönlendirmesini bağımsız olarak ayarlayın ve `StatusBarHiddenMode.Default` değeri gizler Durum Çubuğu dikey compact ortamında.

Durum çubuğu üzerinde görünürlük sonucu olan bir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) ayarlanabilir:

![](ios-images/hide-status-bar.png "Durum çubuğu görünürlük platforma özgü")

> [!NOTE]
> Üzerinde bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), belirtilen `StatusBarHiddenMode` numaralandırma değeri ayrıca tüm alt sayfaları durum çubuğunda güncelleştirin. Diğer tüm [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-türetilmiş tür, belirtilen `StatusBarHiddenMode` numaralandırma değeri yalnızca geçerli sayfa durum çubuğunda güncelleştirin.

`Page.SetPreferredStatusBarUpdateAnimation` Yöntemi nasıl durum çubuğu girer veya bırakır ayarlamak için kullanılan [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) birini belirterek `UIStatusBarAnimation` numaralandırma değerlerinin: `None`, `Fade`, veya `Slide`. Varsa `Fade` veya `Slide` numaralandırma değeri belirtilirse, 0,25 animasyon yürütür durum çubuğu girdiğinde veya bırakır gibi ikinci `Page`.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>Bir ScrollView içinde geciktirme içerik rötuşları

Dokunma hareketi başladığında örtük bir zamanlayıcı tetiklenir bir [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) iOS ve `ScrollView` süreölçer aralığı içinde bir kullanıcı eylemi hareketi işlemek ya da içeriğini geçirmek dayalı karar verir. Varsayılan olarak, iOS `ScrollView` gecikmeler içerik rötuşları, ancak bazı durumlarda ile sorunlara neden olabilir `ScrollView` içeriği gerektiğinde hareketi kazanma değil. Bu nedenle, bu platforma özgü denetimleri olup bir `ScrollView` dokunma hareketi işleyen veya içeriğine geçirir. XAML'de ayarlayarak tüketiliyor `ScrollView.ShouldDelayContentTouches` özelliğine bağlı bir `boolean` değeri:

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

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` Yöntemi belirtir. bu platforma özgü yalnızca İos'ta çalıştırma. `ScrollView.SetShouldDelayContentTouches` Yöntemi, [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) ad alanı, kullanılan denetimine olup bir [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) dokunma hareketi işleyen veya içeriğine geçirir. Ayrıca, `SetShouldDelayContentTouches` yöntemi çağrılarak içerik rötuşları geciktirme geçiş yapmak için kullanılabilir `ShouldDelayContentTouches` içerik rötuşları Gecikmeli olup olmadığını döndürülecek yöntemi:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Sonuç bir [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) içerik rötuşları, bu nedenle alma geciktirme devre dışı bırakabilirsiniz, bu senaryoda [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) hareketi alır yerine [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) sayfasında [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "Platforma özgü ScrollView gecikme içerik dokunur")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

## <a name="summary"></a>Özet

Bu makalede, iOS platformu-Xamarin.Forms yerleşik özellikleri kullanma gösterilmektedir. Platform özellikleri yalnızca özel oluşturucu veya efektleri uygulamadan belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Platform Özellikleri Oluşturma](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)