---
title: Xamarin.Forms döngüsü sayfası
description: Xamarin.Forms CarouselPage galeri gibi içerik sayfalarında gezinmek için kullanıcıların yan yana doğru çekin bir sayfadır. Bu makalede sayfaların koleksiyonda gitmek için bir CarouselPage kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: bce3a60f3647a537906cfa11fc1dcfcc6f5cf365
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998620"
---
# <a name="xamarinforms-carousel-page"></a>Xamarin.Forms döngüsü sayfası

_Xamarin.Forms CarouselPage galeri gibi içerik sayfalarında gezinmek için kullanıcıların yan yana doğru çekin bir sayfadır. Bu makalede sayfaların koleksiyonda gitmek için bir CarouselPage kullanmayı gösterir._

## <a name="overview"></a>Genel Bakış

Aşağıdaki ekran görüntüleri Göster bir [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) her platformda:

![](carousel-page-images/thirdpage.png "CarouselPage Thid öğesi")

Düzenini bir [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) her platformda aynıdır. Sayfaları aracılığıyla koleksiyonu üzerinden iletimler gitmek için sağdan sola çekerek ve soldan sağa toplulukta geriye doğru gezinmek için çekerek gezinilebilir. Aşağıdaki ekran görüntüleri ilk sayfasında göster bir [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) örneği:

![](carousel-page-images/firstpage.png "CarouselPage ilk öğe")

Sağdan sola taşır için ikinci sayfasında, aşağıdaki ekran görüntülerinde gösterildiği çekerek:

![](carousel-page-images/secondpage.png "CarouselPage ikinci öğesi")

Soldan sağa doğru çekerek önceki sayfasına döndürür, sağdan sola tekrar geçirmeyi üçüncü sayfasına taşır.

<!--
> [!NOTE]
> The [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](xref:Xamarin.Forms.CarouselView) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Bir CarouselPage oluşturma

İki yaklaşım oluşturmak için kullanılabilir bir [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage):

- [Doldurma](#Populating_a_CarouselPage_with_a_Page_Collection) `CarouselPage` alt koleksiyonuyla [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) örnekleri.
- [Ata](#Populating_a_CarouselPage_with_a_Template) bir koleksiyona [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) özelliği ve ata bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) için [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) döndürüleceközelliği[ `ContentPage` ](xref:Xamarin.Forms.ContentPage) koleksiyonundaki nesneleri için örnekleri.

Her iki yaklaşım ile `CarouselPage` sonra görünen her sayfanın sırayla görüntülenmesi sonraki sayfaya geçme geçirme etkileşim olur.

> [!NOTE]
> A [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) ile yalnızca doldurulabilir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) örnekleri veya `ContentPage` türevleri.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Sayfa koleksiyonu ile bir CarouselPage doldurma

Aşağıdaki XAML kod örnekte gösterildiği bir [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) üç görüntüleyen [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) örnekleri:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <ContentPage>
        <ContentPage.Padding>
          <OnPlatform x:TypeArguments="Thickness">
              <On Platform="iOS, Android" Value="0,40,0,0" />
          </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
            <Label Text="Red" FontSize="Medium" HorizontalOptions="Center" />
            <BoxView Color="Red" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
</CarouselPage>
```

Eşdeğer kullanıcı Arabirimi aşağıdaki kod örneği C# dilinde gösterir:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        var redContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                Children = {
                    new Label {
                        Text = "Red",
                        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                        HorizontalOptions = LayoutOptions.Center
                    },
                    new BoxView {
                        Color = Color.Red,
                        WidthRequest = 200,
                        HeightRequest = 200,
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.CenterAndExpand
                    }
                }
            }
        };
        var greenContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };
        var blueContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };

        Children.Add (redContentPage);
        Children.Add (greenContentPage);
        Children.Add (blueContentPage);
    }
}
```

Her [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) görüntüler yalnızca bir [ `Label` ](xref:Xamarin.Forms.Label) belirli bir renk ve [ `BoxView` ](xref:Xamarin.Forms.BoxView) bu renk.

> [!NOTE]
> [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) Kullanıcı Arabirimi sanallaştırma desteklemez. Bu nedenle, performans etkilenebilir `CarouselPage` çok fazla alt öğe içeriyor.

Varsa bir [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) içine katıştırılmış [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) sayfasının bir [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) özelliği ayarlanmalıdır `false` arasında hareket çakışmalarını önlemek için `CarouselPage` ve `MasterDetailPage`.

Hakkında daha fazla bilgi için [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), bkz: [bölüm 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold'ın Xamarin.Forms rehberi.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Bir şablonu ile bir CarouselPage doldurma

Aşağıdaki XAML kod örnekte gösterildiği bir [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) atayarak oluşturulmuş bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) için [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) sayfaları için döndürülecek özelliği koleksiyondaki nesneleri:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <CarouselPage.ItemTemplate>
        <DataTemplate>
            <ContentPage>
                <ContentPage.Padding>
                  <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS, Android" Value="0,40,0,0" />
                  </OnPlatform>
                </ContentPage.Padding>
                <StackLayout>
                    <Label Text="{Binding Name}" FontSize="Medium" HorizontalOptions="Center" />
                    <BoxView Color="{Binding Color}" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
                </StackLayout>
            </ContentPage>
        </DataTemplate>
    </CarouselPage.ItemTemplate>
</CarouselPage>
```

[ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) Ayarlayarak veri ile doldurulan [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) arka plan kod dosyası için oluşturucu özelliği:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

Aşağıdaki kod örneği, eşdeğer gösterir [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) C# içinde oluşturulan:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        ItemTemplate = new DataTemplate (() => {
            var nameLabel = new Label {
                FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                HorizontalOptions = LayoutOptions.Center
            };
            nameLabel.SetBinding (Label.TextProperty, "Name");

            var colorBoxView = new BoxView {
                WidthRequest = 200,
                HeightRequest = 200,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.CenterAndExpand
            };
            colorBoxView.SetBinding (BoxView.ColorProperty, "Color");

            return new ContentPage {
                Padding = padding,
                Content = new StackLayout {
                    Children = {
                        nameLabel,
                        colorBoxView
                    }
                }
            };
        });

        ItemsSource = ColorsDataModel.All;
    }
}
```

Her [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) görüntüler yalnızca bir [ `Label` ](xref:Xamarin.Forms.Label) belirli bir renk ve [ `BoxView` ](xref:Xamarin.Forms.BoxView) bu renk.

> [!NOTE]
> [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) Kullanıcı Arabirimi sanallaştırma desteklemez. Bu nedenle, performans etkilenebilir `CarouselPage` çok fazla alt öğe içeriyor.

Varsa bir [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) içine katıştırılmış [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) sayfasının bir [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) özelliği ayarlanmalıdır `false` arasında hareket çakışmalarını önlemek için `CarouselPage` ve `MasterDetailPage`.

Hakkında daha fazla bilgi için [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), bkz: [bölüm 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold'ın Xamarin.Forms rehberi.

## <a name="summary"></a>Özet

Bu makalede nasıl kullanılacağı gösterilmiştir bir [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) sayfaların koleksiyonda gidin. `CarouselPage` Galeri gibi çok içerik sayfalarında gezinmek için kullanıcıların yan yana doğru çekin bir sayfadır.


## <a name="related-links"></a>İlgili bağlantılar

- [Sayfa çeşitleri](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)
