---
title: Dinamik stilleri
description: "Stilleri değil özelliği değişikliklere yanıt verir ve bir uygulama süresi değişmeden kalır. Örneğin, bir stil ayarlayıcı örneklerden birini, kaldırılan değiştirilirse bir görsel öğe ya da eklenen yeni bir ayarlayıcı örnek atadıktan sonra değişiklikler görsel öğe için uygulanmaz. Ancak, uygulamalar çalışma zamanında dinamik olarak stil değişiklikleri dinamik kaynaklar kullanarak yanıt verebilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 2088ae055fb18b3b00712f063d2178f759021088
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="dynamic-styles"></a>Dinamik stilleri

_Stilleri değil özelliği değişikliklere yanıt verir ve bir uygulama süresi değişmeden kalır. Örneğin, bir stil ayarlayıcı örneklerden birini, kaldırılan değiştirilirse bir görsel öğe ya da eklenen yeni bir ayarlayıcı örnek atadıktan sonra değişiklikler görsel öğe için uygulanmaz. Ancak, uygulamalar çalışma zamanında dinamik olarak stil değişiklikleri dinamik kaynaklar kullanarak yanıt verebilir._

`DynamicResource` Biçimlendirme uzantısı benzer `StaticResource` her ikisi arasında bir değer almak için bir sözlük anahtarı kullanmak biçimlendirme uzantısı'nda bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Ancak, `StaticResource` tek sözlük araması gerçekleştirir `DynamicResource` sözlük anahtarı bağlantısını korur. Bu nedenle, anahtar ile ilişkili sözlük girdisi değiştirirse, değişiklik görsel öğe uygulanır. Bu uygulamada yapılması çalışma zamanı stil değişiklikleri sağlar.

Aşağıdaki kod örneğinde gösterilmektedir *dinamik* XAML sayfası stillerde:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle"
                   TargetType="SearchBar"
                   BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle"
                   TargetType="SearchBar">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Placeholder="These SearchBar controls"
                       Style="{DynamicResource searchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Kullanım örnekleri `DynamicResource` başvurmak için işaretleme uzantısı bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) adlı `searchBarStyle`, XAML içinde tanımlanmamış. Ancak, çünkü [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özelliklerini `SearchBar` örnekleri kullanılarak ayarlanır bir `DynamicResource`, eksik bir sözlük anahtar oluşturulan bir özel durum oluşturmaz.

Bunun yerine, arka plan kod dosyasına Oluşturucusu oluşturur bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) anahtarla girişi `searchBarStyle`, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public partial class DynamicStylesPage : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPage ()
    {
        InitializeComponent ();
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
    }

    void OnButtonClicked (object sender, EventArgs e)
    {
        if (originalStyle) {
            Resources ["searchBarStyle"] = Resources ["greenSearchBarStyle"];
            originalStyle = false;
        } else {
            Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
            originalStyle = true;
        }
    }
}
```

Zaman `OnButtonClicked` olay işleyicisi yürütüldüğünde, `searchBarStyle` arasında geçiş yapar `blueSearchBarStyle` ve `greenSearchBarStyle`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

[![](dynamic-images/dynamic-style-blue.png "Mavi dinamik stili örnek")](dynamic-images/dynamic-style-blue-large.png "mavi dinamik stili örnek")
[![](dynamic-images/dynamic-style-green.png "yeşil dinamik stili örnek") ] (dynamic-images/dynamic-style-green-large.png "Yeşil dinamik stili örneği")

Aşağıdaki kod örneği, C# eşdeğer sayfaya gösterir:

```csharp
public class DynamicStylesPageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        ...
        var searchBar1 = new SearchBar { Placeholder = "These SearchBar controls" };
        searchBar1.SetDynamicResource (VisualElement.StyleProperty, "searchBarStyle");
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button  }
        };
    }
    ...
}
```

C# ' ta, [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) kullanım örnekleri [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) başvurmak için yöntemi `searchBarStyle`. `OnButtonClicked` Olay işleyici kodu aynıdır ve XAML örneğe çalıştırıldığında, `searchBarStyle` arasında geçiş yapar `blueSearchBarStyle` ve `greenSearchBarStyle`.

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>Dinamik stili devralma

Stil'nden dinamik bir stil türetme yapamazsınız elde edilebilir kullanarak [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) özelliği. Bunun yerine, [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) sınıfı içerir [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) bir sözlük anahtarı değeri ayarlanabilir özelliği dinamik olarak değişebilir.

Aşağıdaki kod örneğinde gösterilmektedir *dinamik* stil devralma XAML sayfası içinde:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle" TargetType="SearchBar" BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle" TargetType="SearchBar">
              ...
            </Style>
            <Style x:Key="tealSearchBarStyle" TargetType="SearchBar" BaseResourceKey="searchBarStyle">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Text="These SearchBar controls" Style="{StaticResource tealSearchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Kullanım örnekleri `StaticResource` başvurmak için işaretleme uzantısı bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) adlı `tealSearchBarStyle`. Bu `Style` kullanır ve bazı ek özellikler ayarlar [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) başvurmak için özellik `searchBarStyle`. `DynamicResource` Biçimlendirme uzantısı gerekli değildir çünkü `tealSearchBarStyle` değiştirmez, dışında `Style` öğesinden türetilen. Bu nedenle, `tealSearchBarStyle` bağlantı tutar `searchBarStyle` ve temel stili değiştiğinde değiştirilemez.

Arka plan kod dosyasına Oluşturucusu oluşturur bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) anahtarla girişi `searchBarStyle`, dinamik stilleri gösterilen önceki örnekte olduğu gibi başına. Zaman `OnButtonClicked` olay işleyicisi yürütüldüğünde, `searchBarStyle` arasında geçiş yapar `blueSearchBarStyle` ve `greenSearchBarStyle`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

[![](dynamic-images/dynamic-style-inheritance-blue.png "Mavi dinamik stili devralma örnek")](dynamic-images/dynamic-style-inheritance-blue-large.png "mavi dinamik stili devralma örnek")
[![](dynamic-images/dynamic-style-inheritance-green.png "yeşil dinamik stili Devralma örnek")](dynamic-images/dynamic-style-inheritance-green-large.png "yeşil dinamik stili devralma örneği")

Aşağıdaki kod örneği, C# eşdeğer sayfaya gösterir:

```csharp
public class DynamicStylesInheritancePageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesInheritancePageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var tealSearchBarStyle = new Style (typeof(SearchBar)) {
            BaseResourceKey = "searchBarStyle",
            ...
        };
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = {
                new SearchBar { Text = "These SearchBar controls", Style = tealSearchBarStyle },
                ...
            }
        };
    }
    ...
}
```

`tealSearchBarStyle` Doğrudan atanan [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özelliği [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) örnekleri. Bu `Style` bazı ek özelliklerini ayarlar ve kullandığı [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) başvurmak için özellik `searchBarStyle`. [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) Yöntemi değil gerekli burada çünkü `tealSearchBarStyle` değiştirmez, dışında `Style` öğesinden türetilen. Bu nedenle, `tealSearchBarStyle` bağlantı tutar `searchBarStyle` ve temel stili değiştiğinde değiştirilemez.

## <a name="summary"></a>Özet

Stilleri değil özelliği değişikliklere yanıt verir ve bir uygulama süresi değişmeden kalır. Ancak, uygulamalar çalışma zamanında dinamik olarak stil değişiklikleri dinamik kaynaklar kullanarak yanıt verebilir. Ayrıca, *dinamik* stilleri türetilen ile [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) özelliği.



## <a name="related-links"></a>İlgili bağlantılar

- [XAML işaretleme uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dinamik stilleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [stili](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
