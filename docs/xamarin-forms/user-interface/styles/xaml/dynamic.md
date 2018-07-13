---
title: Xamarin.Forms içinde dinamik stiller
description: Bu makalede, nasıl bir Xamarin.Forms uygulaması stil değişikliklerini zamanında dinamik olarak dinamik kaynaklarını kullanarak yanıt verebileceği açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: cedf9e3daed9a2d5f8bfa0962bf66510748b592a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997152"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Xamarin.Forms içinde dinamik stiller

_Stilleri değil özellik değişikliklerine yanıt ve bir uygulama süresi değişmeden kalır. Örneğin, bir stil ayarlayıcı örneklerinden birinde değiştirilen, kaldırılan, bir görsel öğe ya da eklenen yeni bir ayarlayıcı örneği atadıktan sonra değişiklikleri görsel öğe için uygulanmaz. Ancak, uygulamalar çalışma zamanında dinamik olarak stil değişikliklerini dinamik kaynakları kullanarak yanıt verebilirsiniz._

`DynamicResource` İşaretleme uzantısı benzer `StaticResource` hem de bir değer almak için bir sözlük anahtarı kullanmak işaretleme uzantısı'nda bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Ancak, `StaticResource` bir tek sözlük araması gerçekleştirir `DynamicResource` sözlük anahtarı bağlantısını korur. Bu nedenle, anahtarıyla ilişkilendirilmiş sözlük girişinin değiştirilirse Değiştir görsel öğe için uygulanır. Bu uygulamada yapılacak çalışma zamanı stil değişikliklerini sağlar.

Aşağıdaki kod örneğinde *dinamik* XAML sayfası stilleri:

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

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar) Kullanım örnekleri `DynamicResource` başvurmak için işaretleme uzantısı bir [ `Style` ](xref:Xamarin.Forms.Style) adlı `searchBarStyle`, XAML içinde tanımlanmamış. Ancak, çünkü [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özelliklerini `SearchBar` örnekleri kullanılarak ayarlanır bir `DynamicResource`, eksik sözlük anahtarı oluşturulan bir özel durum oluşturmaz.

Bunun yerine arka plan kod dosyasında, oluşturucu oluşturur bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) anahtarla giriş `searchBarStyle`aşağıdaki kod örneğinde gösterildiği gibi:

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

Zaman `OnButtonClicked` olay işleyicisi yürütülür `searchBarStyle` arasında geçiş `blueSearchBarStyle` ve `greenSearchBarStyle`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

[![](dynamic-images/dynamic-style-blue.png "Mavi dinamik stili örnek")](dynamic-images/dynamic-style-blue-large.png#lightbox "mavi dinamik stili örnek")
[![](dynamic-images/dynamic-style-green.png "yeşil dinamik stili örnek") ] (dynamic-images/dynamic-style-green-large.png#lightbox "Yeşil dinamik stili örneği")

Aşağıdaki kod örneği, C# ' de eşdeğer sayfaya gösterir:

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
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button    }
        };
    }
    ...
}
```

C# ' ta, [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) kullanım örnekleri [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource*) başvurmak için yöntemi `searchBarStyle`. `OnButtonClicked` XAML örneğe ve yürütüldüğünde, aynı olay işleyicisini `searchBarStyle` arasında geçiş `blueSearchBarStyle` ve `greenSearchBarStyle`.

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>Dinamik stil devralımı

Bir style'nden dinamik bir stil türetme olamaz elde edilebilir kullanarak [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) özelliği. Bunun yerine, [ `Style` ](xref:Xamarin.Forms.Style) sınıfı içeren [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) değeri bir sözlük anahtarı için ayarlanabilir özelliği dinamik olarak değişebilir.

Aşağıdaki kod örneğinde *dinamik* devralma de bir XAML sayfası stili:

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

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar) Kullanım örnekleri `StaticResource` başvurmak için işaretleme uzantısı bir [ `Style` ](xref:Xamarin.Forms.Style) adlı `tealSearchBarStyle`. Bu `Style` kullanır ve bazı ek özelliklerini ayarlar [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) başvurmak için özellik `searchBarStyle`. `DynamicResource` İşaretleme uzantısı gerekli değildir çünkü `tealSearchBarStyle` değiştirmez, dışında `Style` türetildiği. Bu nedenle, `tealSearchBarStyle` bağlantısını korur `searchBarStyle` ve temel stili değiştiğinde değiştirilemez.

Arka plan kod dosyasında, oluşturucu oluşturur bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) anahtarla giriş `searchBarStyle`, dinamik stiller gösterilen önceki örnekte olduğu gibi başına. Zaman `OnButtonClicked` olay işleyicisi yürütülür `searchBarStyle` arasında geçiş `blueSearchBarStyle` ve `greenSearchBarStyle`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

[![](dynamic-images/dynamic-style-inheritance-blue.png "Mavi dinamik stili devralma örnek")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "mavi dinamik stili devralma örnek")
[![](dynamic-images/dynamic-style-inheritance-green.png "yeşil dinamik stili Devralma örnek")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "yeşil dinamik stili devralma örneği")

Aşağıdaki kod örneği, C# ' de eşdeğer sayfaya gösterir:

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

`tealSearchBarStyle` Doğrudan atanan [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özelliği [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) örnekleri. Bu `Style` bazı ek özelliklerini ayarlar ve kullandığı [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) başvurmak için özellik `searchBarStyle`. [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource*) Yöntemi gerekli değildir burada çünkü `tealSearchBarStyle` değiştirmez, dışında `Style` türetildiği. Bu nedenle, `tealSearchBarStyle` bağlantısını korur `searchBarStyle` ve temel stili değiştiğinde değiştirilemez.

## <a name="summary"></a>Özet

Stilleri değil özellik değişikliklerine yanıt ve bir uygulama süresi değişmeden kalır. Ancak, uygulamalar çalışma zamanında dinamik olarak stil değişikliklerini dinamik kaynakları kullanarak yanıt verebilirsiniz. Ayrıca, *dinamik* stilleri türetilen ile [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) özelliği.



## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dinamik stiller (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Ayarlayıcı](xref:Xamarin.Forms.Setter)
