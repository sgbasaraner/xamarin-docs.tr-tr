---
title: "Karusel sayfası"
description: "Xamarin.Forms CarouselPage bir galeri gibi içerik sayfaları arasında gezinmek için kullanıcıların yan yana doğru çekin bir sayfadır. Bu makalede bir CarouselPage sayfaları toplulukta gezinmek için nasıl kullanılacağı gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: adb1042b8f44d3f414e2161e20b7eb3dc0e940a2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="carousel-page"></a>Karusel sayfası

_Xamarin.Forms CarouselPage bir galeri gibi içerik sayfaları arasında gezinmek için kullanıcıların yan yana doğru çekin bir sayfadır. Bu makalede bir CarouselPage sayfaları toplulukta gezinmek için nasıl kullanılacağı gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Aşağıdaki ekran görüntüleri Göster bir [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) her platformda:

![](carousel-page-images/thirdpage.png "CarouselPage Thid öğesi")

Düzenini bir [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) her platformda aynıdır. Sayfaları aracılığıyla koleksiyonu aracılığıyla ileten gitmek için sağdan sola geçirmeyi ve soldan sağa toplulukta geriye doğru gidin geçirmeyi gittiğinizde. Aşağıdaki ekran görüntüleri ilk sayfada gösterilecek bir [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) örneği:

![](carousel-page-images/firstpage.png "CarouselPage ilk öğesi")

Sağdan sola taşır için ikinci sayfasında, aşağıdaki ekran görüntülerinde gösterildiği gibi geçirmeyi:

![](carousel-page-images/secondpage.png "CarouselPage ikinci öğesi")

Önceki sayfaya döner soldan sağa geçirmeyi sırada sağdan sola tekrar geçirmeyi üçüncü sayfasına taşır.

<!--
> [!NOTE]
> **Note**: The [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Bir CarouselPage oluşturma

İki yaklaşım, oluşturmak için kullanılabilecek bir [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/):

- [Doldurmak](#Populating_a_CarouselPage_with_a_Page_Collection) `CarouselPage` alt koleksiyonuyla [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örnekleri.
- [Ata](#Populating_a_CarouselPage_with_a_Template) bir koleksiyona [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) özelliği ve ata bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) için [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) döndürüleceközelliği[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) koleksiyonundaki nesneleri için örnek.

Her iki yaklaşımın ile `CarouselPage` sonra görünen her sayfada görüntülenecek sonraki sayfaya taşıma manyetik etkileşim sırayla olur. Bu gezinme deneyimi doğal ve Windows Phone kullanıcıları tanıdık devam edebilir.

> [!NOTE]
> **Not**: A [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) yalnızca ile doldurulabilir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örnekleri veya `ContentPage` türevleri.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>CarouselPage sayfa koleksiyonu ile doldurma

Aşağıdaki XAML kodu örnekte gösterildiği bir [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) üç görüntüleyen [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örnekleri:

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

Aşağıdaki kod örneğinde eşdeğer UI C# dilinde gösterir:

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

Her [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) yalnızca görüntüler bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) belirli bir renk ve [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) bu rengin.

> [!NOTE]
> **Not**: [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) UI sanallaştırma desteklemez. Bu nedenle, performansı etkilenebilir `CarouselPage` çok fazla sayıda alt öğeleri içerir.

Varsa bir [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) içine katıştırılmış [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) sayfasında bir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) özelliği ayarlanmalıdır `false` arasındaki hareketi çakışmaları önlemek için `CarouselPage` ve `MasterDetailPage`.

Hakkında daha fazla bilgi için [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), bkz: [bölüm 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold'un'ın Xamarin.Forms defteri.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Bir şablonla CarouselPage doldurma

Aşağıdaki XAML kodu örnekte gösterildiği bir [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) atama tarafından oluşturulan bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) için [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) sayfalar için döndürülecek özelliği derlemedeki nesneler:

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

[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Ayarlayarak verilerle doldurmuş [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) arka plan kodu dosyanın oluşturucusu özelliğinde:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

Aşağıdaki kod örneğinde eşdeğer gösterir [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) C# ' ta oluşturuldu:

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

Her [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) yalnızca görüntüler bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) belirli bir renk ve [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) bu rengin.

> [!NOTE]
> **Not**: [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) UI sanallaştırma desteklemez. Bu nedenle, performansı etkilenebilir `CarouselPage` çok fazla sayıda alt öğeleri içerir.

Varsa bir [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) içine katıştırılmış [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) sayfasında bir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) özelliği ayarlanmalıdır `false` arasındaki hareketi çakışmaları önlemek için `CarouselPage` ve `MasterDetailPage`.

Hakkında daha fazla bilgi için [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), bkz: [bölüm 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold'un'ın Xamarin.Forms defteri.

## <a name="summary"></a>Özet

Bu makalede nasıl kullanılacağı gösterilmiştir bir [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) sayfaları toplulukta gidin. `CarouselPage` Bir galeri gibi içerik sayfaları arasında gezinmek için kullanıcıların yan yana doğru çekin bir sayfa ve doğal ve Windows Phone kullanıcıları tanıdık hissi bir gezinme deneyimi sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Sayfa çeşit](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)
