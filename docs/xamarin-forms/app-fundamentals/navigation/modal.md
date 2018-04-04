---
title: Kalıcı sayfaları
description: Xamarin.Forms kalıcı sayfalar için destek sağlar. Kalıcı bir sayfa, görev tamamlandı veya iptal kadar uzağa erişilemeyeceğini kendi içinde bulunan bir görevi tamamlamak için kullanıcıların önerir. Bu makalede, kalıcı sayfalara gösterilmiştir.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 909a04398043a3c2f0c30e4da82d174a6bfaf148
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="modal-pages"></a>Kalıcı sayfaları

_Xamarin.Forms kalıcı sayfalar için destek sağlar. Kalıcı bir sayfa, görev tamamlandı veya iptal kadar uzağa erişilemeyeceğini kendi içinde bulunan bir görevi tamamlamak için kullanıcıların önerir. Bu makalede, kalıcı sayfalara gösterilmiştir._

Bu makalede aşağıdaki konular ele alınmıştır:

- [Gezinti gerçekleştirme](#Performing_Navigation) – kalıcı yığını pencerelerinin sayfalarından kalıcı yığınına sayfaları Ftp'den geri düğmesini devre dışı bırakma ve sayfa geçişleri animasyon ekleme.
- [Gezinirken veri geçirme](#Passing_Data_when_Navigating) – bir sayfa oluşturucu ve aracılığıyla veri geçirme bir `BindingContext`.

## <a name="overview"></a>Genel Bakış

Kalıcı bir sayfa herhangi biri olabilir [sayfa](~/xamarin-forms/user-interface/controls/pages.md) Xamarin.Forms tarafından desteklenen türleri. Kalıcı bir sayfasını görüntülemek için uygulama, kalıcı yığına Burada, active sayfasında, aşağıdaki çizimde gösterildiği gibi olacak gönderir:

![](modal-images/pushing.png "Bir sayfa kalıcı yığına iletme")

Aşağıdaki çizimde gösterildiği gibi önceki sayfaya uygulama kalıcı yığından geçerli sayfa pop ve yeni en üstteki sayfa etkin sayfa haline gelir döndürmek için:

![](modal-images/popping.png "Kalıcı yığını sayfasından pencerelerinin")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Gezinti gerçekleştirme

Kalıcı Gezinti yöntemleri tarafından açığa [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) herhangi bir özellikte [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) türetilmiş tür. Bu yöntemler yeteneği sağlamak [anında kalıcı sayfaları](#Pushing_Pages_to_the_Modal_Stack) kalıcı yığına ve [pop kalıcı sayfaları](#Popping_Pages_from_the_Modal_Stack) kalıcı yığından.

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Özelliği de kullanıma sunan bir [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) özelliği kendisinden kalıcı yığını kalıcı sayfalarında elde edilebilir. Ancak, kalıcı yığını işleme gerçekleştirmek ya da kalıcı Gezinti kök sayfası pencerelerinin hiçbir kavramı yoktur. Bu işlemler temel platformlarda Evrensel desteklenmeyen olmasıdır.

> [!NOTE]
> A [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) örneği kalıcı sayfa gezintisi gerçekleştirmek için gerekli değildir.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>Kalıcı yığınına koymadan sayfaları

Gitmek için `ModalPage` çağırmak gerekli olan [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) yöntemi [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) aşağıdaki kod örneğinde kanıtlanabilir olarak geçerli sayfanın özelliği:

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

Bu neden `ModalPage` kalıcı yığına burada etkin sayfa haline gelir, edilmesini örneği sağlanan bir öğe içinde seçili [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) üzerinde `MainPage` örneği. `ModalPage` Örneğini aşağıdaki ekran görüntülerinde gösterilir:

![](modal-images/modalpage.png "Kalıcı sayfası örneği")

Zaman [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) çağrılan, aşağıdakiler gerçekleşir:

- Sayfa arama `PushModalAsync` sahip kendi [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) geçersiz kılma çağrılan koşuluyla Android temel platform değil.
- İçin ilk sayfasına sahip kendi [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) geçersiz kılma çağrılır.
- `PushAsync` Görev tamamlar.

Ancak, bu olayları kesin sırayla bağımlı platformudur. Daha fazla bilgi için bkz: [Bölüm 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold'un'ın Xamarin.Forms defteri.

> [!NOTE]
> Çağrılar [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) ve [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) geçersiz kılmaları sayfa gezintisi garantili belirtileri kabul edemez. Örneğin, ios'ta `OnDisappearing` geçersiz kılma uygulama sonlandırıldığında etkin sayfada çağrılır.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>Kalıcı yığını pencerelerinin sayfalarından

Etkin sayfa kalıcı yığınından tuşlarına basarak Sil'i *geri* düğmesini cihazda bağımsız olarak, bu aygıttaki fiziksel bir düğme olup olmadığını veya bir ekran düğmesini.

Program aracılığıyla özgün sayfaya geri dönmek için `ModalPage` örneği gerekir çağırma [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

Bu neden `ModalPage` etkin sayfa haline gelen yeni en üstteki sayfa kalıcı yığınından kaldırılacak örneği. Zaman [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) çağrılan, aşağıdakiler gerçekleşir:

- Sayfa arama `PopModalAsync` sahip kendi [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) geçersiz kılma çağrılır.
- İçin döndürülen sayfasına sahip kendi [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) geçersiz kılma çağrılan koşuluyla Android temel platform değil.
- `PopModalAsync` Görev döndürür.

Ancak, bu olayları kesin sırayla bağımlı platformudur. Daha fazla bilgi için bkz: [Bölüm 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold'un'ın Xamarin.Forms defteri.

### <a name="disabling-the-back-button"></a>Geri düğmesini devre dışı bırakma

Android ve Windows Phone, kullanıcının her zaman önceki sayfaya standart tuşlarına basarak geri dönebilirsiniz *geri* cihazın düğmesini. Kalıcı sayfa sayfadan ayrılmadan önce kendi içinde bulunan bir görevi tamamlamak kullanıcının gerektiriyorsa, uygulamayı devre dışı bırakmalısınız *geri* düğmesi. Bu geçersiz kılma tarafından gerçekleştirilebilir [ `Page.OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed/) kalıcı sayfasında yöntemi. Daha fazla bilgi için bkz: [Bölüm 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold'un'ın Xamarin.Forms defteri.

### <a name="animating-page-transitions"></a>Sayfa geçişleri animasyon ekleme

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Her sayfanın özelliği de sağlar geçersiz kılınan itme ve içeren pop yöntemleri bir `boolean` gezinme sırasında sayfa animasyon görüntülenip görüntülenmeyeceğini aşağıdaki kodda gösterildiği gibi denetimleri parametresi Örnek:

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

Ayarı `boolean` parametresi `false` parametre ayarı sırasında sayfa geçişi animasyon, devre dışı bırakır `true` koşuluyla temel alınan platformu tarafından desteklenen sayfa geçişi animasyon sağlar. Ancak, bu parametre yetersizliği nedeniyle anında iletme ve pop yöntemleri animasyonun varsayılan olarak etkinleştirir.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Gezinirken veri geçirme

Bazen bir sayfa gezinti sırasında başka bir sayfaya veri iletmek gereklidir. Bu işlemi gerçekleştirmek için iki tekniklerdir sayfa Oluşturucusu geçirme verilerine ve yeni sayfa ayarlayarak [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) veriler. Her şimdi sırayla incelenecektir.

### <a name="passing-data-through-a-page-constructor"></a>Bir sayfa oluşturucu aracılığıyla veri geçirme

Aşağıdaki kod örneğinde gösterildiği bir sayfa Oluşturucusu parametresi aracılığıyla başka bir sayfaya gezinti sırasında veri geçirme için basit tekniği olan:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Bu kod oluşturur bir `MainPage` örnek, geçerli tarih ve saat ISO8601 biçiminde geçirme.

`MainPage` Örneği aşağıdaki kod örneğinde gösterildiği gibi bir oluşturucu parametresini verilerine alır:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Veri ayarlayarak sayfada sonra görüntülenen [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) özelliği.

### <a name="passing-data-through-a-bindingcontext"></a>Bir Bindingparameters'te aracılığıyla veri geçirme

Verileri başka bir sayfaya gezinti sırasında geçirme için alternatif bir yaklaşım yeni sayfanın ayarlanmasıdır [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) aşağıdaki kod örneğinde gösterildiği gibi verileri için:

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

Bu kod ayarlar [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) , `DetailPage` için örnek `Contact` örneği ve ardından gider `DetailPage`.

`DetailPage` Görüntülemek için veri bağlama kullanır `Contact` aşağıdaki XAML kod örneğinde gösterildiği gibi veri örneği:

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

Aşağıdaki kod örneği, nasıl veri bağlama C# ' ta gerçekleştirilebilir gösterir:

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

Veriler sayfada bir dizi tarafından görüntülenir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontrol eder.

Veri bağlama hakkında daha fazla bilgi için bkz: [veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/index.md).

## <a name="summary"></a>Özet

Bu makalede kalıcı sayfalarında gezinmek nasıl gösterilmektedir. Kalıcı bir sayfa, görev tamamlandı veya iptal kadar uzağa erişilemeyeceğini kendi içinde bulunan bir görevi tamamlamak için kullanıcıların önerir.


## <a name="related-links"></a>İlgili bağlantılar

- [Sayfa gezintisi](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Kalıcı (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
