---
title: Xamarin.Forms kalıcı sayfalar
description: Xamarin.Forms kalıcı sayfalar için destek sağlar. Kalıcı bir sayfa, görev tamamlandı veya iptal kadar UZAĞINIZDA erişilemeyeceğini kendi içinde bir görevi tamamlamak için kullanıcıların önerir. Bu makalede, kalıcı sayfalara gösterilmiştir.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 44aee8500c7de2ae56b59049368d6025ec49cc5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994834"
---
# <a name="xamarinforms-modal-pages"></a>Xamarin.Forms kalıcı sayfalar

_Xamarin.Forms kalıcı sayfalar için destek sağlar. Kalıcı bir sayfa, görev tamamlandı veya iptal kadar UZAĞINIZDA erişilemeyeceğini kendi içinde bir görevi tamamlamak için kullanıcıların önerir. Bu makalede, kalıcı sayfalara gösterilmiştir._

Bu makalede, aşağıdaki konular ele alınmaktadır:

- [Gezinti gerçekleştirme](#Performing_Navigation) – sayfaları kalıcı yığınına, yığından kaldırılıyor sayfalarından kalıcı yığın gönderme, geri düğmesini devre dışı bırakma ve sayfa geçişleri animasyon ekleme.
- [Gezinirken veri geçirme](#Passing_Data_when_Navigating) – sayfa oluşturucusu ve aracılığıyla veri geçirme bir `BindingContext`.

## <a name="overview"></a>Genel Bakış

Kalıcı bir sayfa herhangi biri olabilir [sayfa](~/xamarin-forms/user-interface/controls/pages.md) Xamarin.Forms tarafından desteklenen türleri. Kalıcı bir sayfasını görüntülemek için uygulama bunu kalıcı yığınına, burada, etkin sayfa, aşağıdaki diyagramda gösterildiği gibi olacaktır gönderir:

![](modal-images/pushing.png "Bir sayfa için kalıcı yığın gönderme")

Önceki sayfaya uygulaması kalıcı yığından geçerli sayfa açılır ve yeni en üstte sayfa etkin sayfa olur Aşağıdaki diyagramda gösterildiği gibi döndürmek için:

![](modal-images/popping.png "Bir sayfadan kalıcı yığın dosyasından alınıyor")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Gezinti gerçekleştirme

Kalıcı Gezinti yöntemleri tarafından sunulur [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) herhangi bir özellikte [ `Page` ](xref:Xamarin.Forms.Page) türetilmiş türler. Bu yöntemler şu olanakları sağlar [itme kalıcı sayfalar](#Pushing_Pages_to_the_Modal_Stack) kalıcı yığına ve [pop kalıcı sayfalar](#Popping_Pages_from_the_Modal_Stack) kalıcı yığından.

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Özelliğini de sunan bir [ `ModalStack` ](xref:Xamarin.Forms.INavigation.ModalStack) özelliği kendisinden kalıcı yığınında kalıcı sayfalar elde edilebilir. Ancak, kalıcı yığın düzenleme gerçekleştirme ya da kalıcı Gezinti kök sayfasına pencerelerinin konsepti yoktur. Bu işlemler temel platformlarda Evrensel desteklenmeyen olmasıdır.

> [!NOTE]
> A [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) örneği kalıcı sayfa gezintisi gerçekleştirmek için gerekli değildir.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>Sayfaları için kalıcı yığın gönderme

Gitmek için `ModalPage` çağırmak gerekli olan [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) metodunda [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) geçerli sayfasında, aşağıdaki kod örneğinde kanıtlanabilir olarak özelliği:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    ...
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Bu neden `ModalPage` örneği kalıcı burada etkin sayfa olur, yığına için sağlanan bir öğe içinde seçili [ `ListView` ](xref:Xamarin.Forms.ListView) üzerinde `MainPage` örneği. `ModalPage` Örneğini aşağıdaki ekran görüntülerinde gösterilen:

![](modal-images/modalpage.png "Kalıcı sayfası örneği")

Zaman [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) çağrılır, aşağıdaki olaylar gerçekleşir:

- Sayfa arama `PushModalAsync` sahip kendi [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) geçersiz kılma koşuluyla Android temel platform değilse çağrılır.
- Geçtiğiniz için sayfanın alt [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) geçersiz kılma çağrılır.
- `PushAsync` Görev tamamlanır.

Ancak, bu olayları kesin sırayla bağımlı platformudur. Daha fazla bilgi için [Bölüm 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold'ın Xamarin.Forms rehberi.

> [!NOTE]
> Çağrılar [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) ve [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) geçersiz kılmaları sayfa gezintisi garantili belirtileri kabul edemez. Örneğin, ios'ta `OnDisappearing` geçersiz kılma uygulama sonlandırıldığında etkin sayfa üzerinde çağrılır.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>Kalıcı yığından yığından kaldırılıyor sayfaları

Etkin sayfa kalıcı yığından tuşlarına basarak POP *geri* cihazda düğme bağımsız olarak, bu cihaz üzerinde fiziksel bir düğme olup olmadığını veya bir ekrandaki bir düğme.

Program aracılığıyla geldikleri sayfaya geri dönmek için `ModalPage` örneği çağırmalıdır [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

Bu neden `ModalPage` örneği kalıcı yığından etkin sayfa olma yeni en üst sayfaya kaldırılacak. Zaman [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) çağrılır, aşağıdaki olaylar gerçekleşir:

- Sayfa arama `PopModalAsync` sahip kendi [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) geçersiz kılma çağrılır.
- İade edilen sayfanın kendi [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) geçersiz kılma koşuluyla Android temel platform değilse çağrılır.
- `PopModalAsync` Görevi döndürür.

Ancak, bu olayları kesin sırayla bağımlı platformudur. Daha fazla bilgi için [Bölüm 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold'ın Xamarin.Forms rehberi.

### <a name="disabling-the-back-button"></a>Geri düğmesini devre dışı bırakma

Android, kullanıcının her zaman önceki sayfaya standart tuşlarına basarak döndürebilir *geri* cihazda düğmesini. Kalıcı sayfa sayfadan ayrılmadan önce kendi içinde bir görevi tamamlamak kullanıcı gerektiriyorsa, uygulamayı devre dışı bırakmanız gerekir *geri* düğmesi. Bu geçersiz kılma tarafından gerçekleştirilebilir [ `Page.OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed) kalıcı sayfasında yöntemi. Daha fazla bilgi için [Bölüm 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold'ın Xamarin.Forms rehberi.

### <a name="animating-page-transitions"></a>Sayfa geçişleri animasyon ekleme

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Her sayfanın özelliği de sağlar geçersiz kılınan İtme hem de içeren pop yöntemleri bir `boolean` aşağıdaki kodda gösterildiği gibi denetleyen bir sayfa animasyon, gezinti sırasında görüntülenip görüntülenmeyeceğini parametresi Örnek:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushModalAsync (new DetailPage (), false);
}

async void OnDismissButtonClicked (object sender, EventArgs args)
{
  // Page appearance not animated
  await Navigation.PopModalAsync (false);
}
```

Ayarı `boolean` parametresi `false` parametre ayarlanırken sayfa geçişi animasyon devre dışı bırakır `true` şartıyla temel alınan platformu tarafından desteklenen sayfa geçişi animasyon sağlar. Ancak, bu parametre eksik anında iletme ve pop yöntemleri animasyon varsayılan olarak etkinleştirir.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Gezinirken veri geçirme

Bazen bir sayfa gezinti sırasında başka bir sayfaya veri iletmek gereklidir. Bu işlemi gerçekleştirmek için iki teknik şunlardır: sayfa Oluşturucu üzerinden geçen veri ve yeni sayfa ayarlayarak [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) veriler. Her artık sırasıyla açıklanmıştır.

### <a name="passing-data-through-a-page-constructor"></a>Bir sayfa Oluşturucusu aracılığıyla veri geçirme

Gezinti sırasında başka bir sayfaya verileri geçirmek için en basit yöntem aşağıdaki kod örneğinde gösterilen bir sayfa Oluşturucu parametresi üzerinden verilmiştir:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Bu kod oluşturur bir `MainPage` örneği, geçerli tarih ve saat ISO8601 biçiminde geçirme.

`MainPage` Örneği aşağıdaki kod örneğinde gösterildiği gibi bir oluşturucu parametresi aracılığıyla verileri alır:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Veriler daha sonra sayfada ayarlayarak görüntülenir [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) özelliği.

### <a name="passing-data-through-a-bindingcontext"></a>Bir BindingContext arasında veri geçirme

Yeni sayfa ayarlayarak verileri başka bir sayfaya gezinti sırasında geçirmek için alternatif bir yaklaşım olan [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) aşağıdaki kod örneğinde gösterildiği gibi verileri için:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    detailPage.BindingContext = e.SelectedItem as Contact;
    listView.SelectedItem = null;
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Bu kod ayarlar [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) , `DetailPage` için örnek `Contact` örneği ve ardından gider `DetailPage`.

`DetailPage` Ardından görüntülemek için veri bağlama kullanır `Contact` aşağıdaki XAML kod örneğinde gösterildiği gibi veri örneği:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ModalNavigation.DetailPage">
    <ContentPage.Padding>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,40,0,0" />
      </OnPlatform>
    </ContentPage.Padding>
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" FontSize="Medium" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
              ...
            <Button x:Name="dismissButton" Text="Dismiss" Clicked="OnDismissButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Aşağıdaki kod örneği, nasıl veri bağlama C# dilinde gerçekleştirilebilir gösterir:

```csharp
public class DetailPageCS : ContentPage
{
  public DetailPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var dismissButton = new Button { Text = "Dismiss" };
    dismissButton.Clicked += OnDismissButtonClicked;

    Thickness padding;
    switch (Device.RuntimePlatform)
    {
        case Device.iOS:
            padding = new Thickness(0, 40, 0, 0);
            break;
        default:
            padding = new Thickness();
            break;
    }

    Padding = padding;
    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        dismissButton
      }
    };
  }

  async void OnDismissButtonClicked (object sender, EventArgs args)
  {
    await Navigation.PopModalAsync ();
  }
}
```

Veriler bir dizi tarafından sonra sayfada görüntülenir [ `Label` ](xref:Xamarin.Forms.Label) kontrol eder.

Veri bağlama hakkında daha fazla bilgi için bkz. [temel veri bağlama bilgileri](~/xamarin-forms/xaml/xaml-basics/index.md).

## <a name="summary"></a>Özet

Bu makalede nasıl kalıcı sayfalara gösterilmiştir. Kalıcı bir sayfa, görev tamamlandı veya iptal kadar UZAĞINIZDA erişilemeyeceğini kendi içinde bir görevi tamamlamak için kullanıcıların önerir.


## <a name="related-links"></a>İlgili bağlantılar

- [Sayfa gezintisi](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Kalıcı (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
