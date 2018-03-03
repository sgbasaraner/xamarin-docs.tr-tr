---
title: "Hiyerarşik gezinme"
description: "NavigationPage sınıfı kullanıcının iletir ve geriye doğru istediğiniz gibi sayfalar arasında gezinme mümkün olduğu bir hiyerarşik gezinme deneyimi sağlar. Sınıf Gezinti son giren ilk çıkar (LIFO) yığınını sayfa nesneleri olarak uygular. Bu makalede NavigationPage sınıfı bir sayfa yığınını Gezinti gerçekleştirmek için nasıl kullanılacağı gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 95fc958beb71cba8f4d575eaa96d0612aa458966
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="hierarchical-navigation"></a>Hiyerarşik gezinme

_NavigationPage sınıfı kullanıcının iletir ve geriye doğru istediğiniz gibi sayfalar arasında gezinme mümkün olduğu bir hiyerarşik gezinme deneyimi sağlar. Sınıf Gezinti son giren ilk çıkar (LIFO) yığınını sayfa nesneleri olarak uygular. Bu makalede NavigationPage sınıfı bir sayfa yığınını Gezinti gerçekleştirmek için nasıl kullanılacağı gösterilmektedir._

Bu makalede aşağıdaki konular ele alınmıştır:

- [Gezinti gerçekleştirme](#Performing_Navigation) – kök sayfası oluşturma, sayfa gezinti yığını Ftp'den pencerelerinin Gezinti yığını sayfalarından ve sayfa geçişleri animasyon ekleme.
- [Gezinirken veri geçirme](#Passing_Data_when_Navigating) – bir sayfa oluşturucu ve aracılığıyla veri geçirme bir `BindingContext`.
- [Gezinti yığını düzenleme](#Manipulating_the_Navigation_Stack) – yığın ekleme veya kaldırma sayfaları düzenleme.

## <a name="overview"></a>Genel Bakış

Bir sayfadan diğerine taşımak için bir uygulama Burada, active sayfasında, aşağıdaki çizimde gösterildiği gibi olacak yeni bir sayfa gezinti yığına gönderir:

![](hierarchical-images/pushing.png "Bir sayfa için Gezinti yığını iletme")

Önceki sayfaya dönmek için uygulamanın geçerli sayfa gezinti yığından pop ve yeni en üstteki sayfa etkin sayfasında, aşağıdaki çizimde gösterildiği gibi olur:

![](hierarchical-images/popping.png "Bir sayfa gezinti yığından pencerelerinin")

Gezinti yöntemleri tarafından açığa [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) herhangi bir özellikte [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) türetilmiş tür. Bu yöntemler Gezinti yığından pop sayfalara Sayfa Gezinti yığına itme ve yığın işleme gerçekleştirme olanağı sunar.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Gezinti gerçekleştirme

Hiyerarşik gezintideki [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) sınıfı bir yığınından gitmek için kullanılan [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) nesneleri. Aşağıdaki ekran görüntüleri ana bileşenlerinin Göster `NavigationPage` her platformda:

![](hierarchical-images/navigationpage-components.png "NavigationPage bileşenleri")

Düzenini bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) platformuna bağlıdır:

- İos'ta, gezinti çubuğunda bir başlık görüntüler ve sahip sayfanın en üstünde bulunduğundan bir *geri* önceki sayfaya döner düğmesi.
- Android, gezinti çubuğunda bir simge, bir başlığı görüntüler sayfanın en üstünde bulunduğundan ve bir *geri* önceki sayfaya döner düğmesi. Simge tanımlanan `[Activity]` süsler özniteliği `MainActivity` Android platforma özgü projesinde sınıfı.
- Windows Phone üzerinde bir gezinti çubuğu bir başlık görüntüleyen sayfanın en üstünde mevcuttur. Windows Phone eksik *geri* çünkü gezinti çubuğunda düğmesini bir ekran *geri* düğmesi ekranın alt kısmında varsa.

Tüm platformlarda değerini [ `Page.Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) özellik sayfası başlık olarak görüntülenir.

> [!NOTE]
> Önerilen bir `NavigationPage` ile doldurulması gerekir `ContentPage` yalnızca örnekleri.

### <a name="creating-the-root-page"></a>Kök sayfası oluşturma

Bir gezinti yığını eklenen ilk sayfası olarak adlandırılır *kök* sayfasında uygulama ve aşağıdaki kod örneğinde gösterir bu nasıl gerçekleştirilir:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

Bu neden `Page1Xaml` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Gezinti yığına burada etkin sayfa ve uygulamanın kök sayfa hale edilmesini örneği. Bu, aşağıdaki ekran görüntülerinde gösterilir:

![](hierarchical-images/mainpage.png "Sayfa Gezinti yığınının kök")

> [!NOTE]
> [ `RootPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.RootPage/) Özelliği bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) örneğinin ilk sayfasının gezinti yığınında erişim sağlar.

### <a name="pushing-pages-to-the-navigation-stack"></a>Gezinti yığınına koymadan sayfaları

Gitmek için `Page2Xaml`, çağırmak gerekli olan [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) yöntemi [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) aşağıdaki kod örneğinde kanıtlanabilir olarak geçerli sayfanın özelliği:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

Bu neden `Page2Xaml` Gezinti yığına burada etkin sayfa haline gelir edilmesini örneği. Bu, aşağıdaki ekran görüntülerinde gösterilir:

![](hierarchical-images/secondpage.png "Sayfa Gezinti yığına gönderilir")

Zaman [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) yöntemi çağrıldığında, aşağıdaki olaylar gerçekleşir:

- Sayfa arama `PushAsync` sahip kendi [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) geçersiz kılma çağrılır.
- İçin ilk sayfasına sahip kendi [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) geçersiz kılma çağrılır.
- `PushAsync` Görev tamamlar.

Ancak, bu olayları kesin sırayla bağımlı platformudur. Daha fazla bilgi için bkz: [Bölüm 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold'un'ın Xamarin.Forms defteri.

> [!NOTE]
> Çağrılar [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) ve [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) geçersiz kılmaları sayfa gezintisi garantili belirtileri kabul edemez. Örneğin, ios'ta `OnDisappearing` geçersiz kılma uygulama sonlandırıldığında etkin sayfada çağrılır.

### <a name="popping-pages-from-the-navigation-stack"></a>Gezinti yığını pencerelerinin sayfalarından

Etkin sayfa gezinti yığınından tuşlarına basarak Sil'i *geri* düğmesini cihazda bağımsız olarak, bu aygıttaki fiziksel bir düğme olup olmadığını veya bir ekran düğmesini.

Program aracılığıyla özgün sayfaya geri dönmek için `Page2Xaml` örneği gerekir çağırma [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

Bu neden `Page2Xaml` etkin sayfa haline gelen yeni en üstteki sayfa gezinti yığınından kaldırılacak örneği. Zaman [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) yöntemi çağrıldığında, aşağıdaki olaylar gerçekleşir:

- Sayfa arama `PopAsync` sahip kendi [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) geçersiz kılma çağrılır.
- İçin döndürülen sayfasına sahip kendi [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) geçersiz kılma çağrılır.
- `PopAsync` Görev döndürür.

Ancak, bu olayları kesin sırayla bağımlı platformudur. Daha fazla bilgi için bkz: [Bölüm 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold'un'ın Xamarin.Forms defteri.

Yanı [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) ve [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) yöntemleri [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) her sayfanın özelliğini de sağlayan bir [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopToRootAsync()/) aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Bu yöntem tüm kök POP [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Gezinti yığını bu nedenle etkin sayfa uygulama kök sayfasının yapma.

### <a name="animating-page-transitions"></a>Sayfa geçişleri animasyon ekleme

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Her sayfanın özelliği de sağlar geçersiz kılınan itme ve içeren pop yöntemleri bir `boolean` gezinme sırasında sayfa animasyon görüntülenip görüntülenmeyeceğini aşağıdaki kodda gösterildiği gibi denetimleri parametresi Örnek:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushAsync (new Page2Xaml (), false);
}

async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopAsync (false);
}

async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopToRootAsync (false);
}
```

Ayarı `boolean` parametresi `false` parametre ayarı sırasında sayfa geçişi animasyon, devre dışı bırakır `true` koşuluyla temel alınan platformu tarafından desteklenen sayfa geçişi animasyon sağlar. Ancak, bu parametre yetersizliği nedeniyle anında iletme ve pop yöntemleri animasyonun varsayılan olarak etkinleştirir.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Gezinirken veri geçirme

Bazen bir sayfa gezinti sırasında başka bir sayfaya veri iletmek gereklidir. Bunu gerçekleştirmenin iki tekniği sayfa Oluşturucusu aracılığıyla ve yeni sayfa ayarlayarak veri geçirme [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) veriler. Her şimdi sırayla incelenecektir.

### <a name="passing-data-through-a-page-constructor"></a>Bir sayfa oluşturucu aracılığıyla veri geçirme

Aşağıdaki kod örneğinde gösterildiği bir sayfa Oluşturucusu parametresi aracılığıyla başka bir sayfaya gezinti sırasında veri geçirme için basit tekniği olan:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Bu kod oluşturur bir `MainPage` Bu örnek, geçerli tarih ve saat ISO8601 biçiminde geçirme, hangi kaydırılır içinde bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) örneği.

`MainPage` Örneği aşağıdaki kod örneğinde gösterildiği gibi bir oluşturucu parametresini verilerine alır:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Veri ayarlayarak sayfada sonra görüntülenen [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) özelliği, aşağıdaki ekran görüntülerinde gösterildiği gibi:

![](hierarchical-images/passing-data-constructor.png "Bir sayfa Oluşturucu aktarılan veriler")

### <a name="passing-data-through-a-bindingcontext"></a>Bir Bindingparameters'te aracılığıyla veri geçirme

Verileri başka bir sayfaya gezinti sırasında geçirme için alternatif bir yaklaşım yeni sayfanın ayarlanmasıdır [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) aşağıdaki kod örneğinde gösterildiği gibi verileri için:

```csharp
async void OnNavigateButtonClicked (object sender, EventArgs e)
{
  var contact = new Contact {
    Name = "Jane Doe",
    Age = 30,
    Occupation = "Developer",
    Country = "USA"
  };

  var secondPage = new SecondPage ();
  secondPage.BindingContext = contact;
  await Navigation.PushAsync (secondPage);
}
```

Bu kod ayarlar [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) , `SecondPage` için örnek `Contact` örneği ve ardından gider `SecondPage`.

`SecondPage` Görüntülemek için veri bağlama kullanır `Contact` aşağıdaki XAML kod örneğinde gösterildiği gibi veri örneği:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="PassingData.SecondPage"
             Title="Second Page">
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
            ...
            <Button x:Name="navigateButton" Text="Previous Page" Clicked="OnNavigateButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Aşağıdaki kod örneği, nasıl veri bağlama C# ' ta gerçekleştirilebilir gösterir:

```csharp
public class SecondPageCS : ContentPage
{
  public SecondPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var navigateButton = new Button { Text = "Previous Page" };
    navigateButton.Clicked += OnNavigateButtonClicked;

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
        navigateButton
      }
    };
  }

  async void OnNavigateButtonClicked (object sender, EventArgs e)
  {
    await Navigation.PopAsync ();
  }
}
```

Veriler sayfada bir dizi tarafından görüntülenir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) , aşağıdaki ekran görüntülerinde gösterildiği gibi denetler:

![](hierarchical-images/passing-data-bindingcontext.png "Bindingparameters'te aktarılan veriler")

Veri bağlama hakkında daha fazla bilgi için bkz: [veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>Gezinti yığını düzenleme

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Özelliği düzenlemenizi sağlayan bir [ `NavigationStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) özelliği kendisinden Gezinti yığını sayfalarında elde edilebilir. Xamarin.Forms Gezinti yığını erişimi sürdüren sırada `Navigation` özelliği sağlar [ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) ve [ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) ekleyerek yığını düzenleme yöntemleri sayfaları veya bunları kaldırma.

[ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) Yöntemi ekler belirtilen sayfa gezinti yığınında varolan belirtilen sayfayı önce aşağıdaki çizimde gösterildiği gibi:

![](hierarchical-images/insert-page-before.png "Bir sayfa gezinti yığınında ekleme")

[ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) Yöntemi aşağıdaki çizimde gösterildiği gibi belirtilen sayfa gezinti yığınından kaldırır:

![](hierarchical-images/remove-page.png "Bir sayfa gezinti yığınından kaldırma")

Bu yöntemler başarılı oturum açma aşağıdaki yeni içeren bir sayfa, oturum açma sayfasına değiştirme gibi bir özel gezinme deneyimi sağlar. Aşağıdaki kod örneğinde, bu senaryo gösterilmektedir:

```csharp
async void OnLoginButtonClicked (object sender, EventArgs e)
{
  ...
  var isValid = AreCredentialsCorrect (user);
  if (isValid) {
    App.IsUserLoggedIn = true;
    Navigation.InsertPageBefore (new MainPage (), this);
    await Navigation.PopAsync ();
  } else {
    // Login failed
  }
}

```

Kullanıcının kimlik bilgilerinin doğru olması koşuluyla `MainPage` örneğinin geçerli sayfasını önce Gezinti yığına eklenir. [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) Yöntemi sonra kaldırır geçerli sayfa gezinti yığından ile `MainPage` etkin sayfa olma örneği.

## <a name="summary"></a>Özet

Bu makalede nasıl kullanılacağı gösterilmiştir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) gezinti sayfaları yığınında gerçekleştirmek için sınıf. Bu sınıf, kullanıcının iletir ve geriye doğru istediğiniz gibi sayfalar arasında gezinme mümkün olduğu bir hiyerarşik gezinme deneyimi sağlar. Sınıf uygulayan Gezinti son giren ilk çıkar (LIFO) yığınını olarak [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) nesneleri.


## <a name="related-links"></a>İlgili bağlantılar

- [Sayfa gezintisi](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hiyerarşik (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Xamarin.Forms (Xamarin University Video) örnek ekran akışında bir oturum oluşturma](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Xamarin.Forms (Xamarin University Video) ekran akışında bir oturum oluşturma](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)
