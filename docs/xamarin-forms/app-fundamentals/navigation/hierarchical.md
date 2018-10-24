---
title: Hiyerarşik gezinme
description: Bu makalede, son giren ilk çıkar (LIFO) sayfaları yığında Gezinti gerçekleştirmek için NavigationPage sınıfını kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: a0a58cf05c97221a73cd0784b7859bb9c84cef86
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "38994682"
---
# <a name="hierarchical-navigation"></a>Hiyerarşik gezinme

_NavigationPage sınıf kullanıcının iletir ve geriye doğru istediğiniz şekilde, sayfada gezinme mümkün olduğu bir hiyerarşik Gezinti deneyimi sağlar. Sınıfı, gezinti sayfası nesnelerin son giren ilk çıkar (LIFO) yığını olarak uygular. Bu makalede bir sayfa yığınını Gezinti gerçekleştirmek için NavigationPage sınıfını kullanmayı gösterir._

Bir sayfadan diğerine taşımak için bir uygulama Burada, etkin sayfa, aşağıdaki diyagramda gösterildiği gibi olacak gezinme yığınında yeni bir sayfaya gönderir:

![](hierarchical-images/pushing.png "Bir sayfa gezinme yığınında için gönderme")

Önceki sayfaya geri dönmek için gezinme yığınında geçerli sayfadaki uygulama açılır ve aşağıdaki çizimde gösterildiği gibi yeni en üst sayfaya etkin sayfa olur:

![](hierarchical-images/popping.png "Gezinme yığınında sayfasından dosyasından alınıyor")

Gezinti yöntemleri tarafından sunulur [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) herhangi bir özellikte [ `Page` ](xref:Xamarin.Forms.Page) türetilmiş türler. Bu yöntemler, sayfa gezinti yığına gezinme yığınında pop sayfalarından gönderin ve yığın işleme gerçekleştirmek için olanağı sunar.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Gezinti gerçekleştirme

Hiyerarşik Gezinti [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) yığını gezinmek için kullanılan sınıfı [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) nesneleri. Aşağıdaki ekran görüntüleri ana bileşenleri Göster `NavigationPage` her platformda:

![](hierarchical-images/navigationpage-components.png "NavigationPage bileşenleri")

Düzenini bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) seçtiğiniz platforma bağlıdır:

- İos'ta bir gezinti çubuğu mevcut olan ve bir başlık görüntüleyen sayfanın en üstünde bir *geri* önceki sayfasına döndürür düğmesine.
- Android, gezinti çubuğunda bir başlık bir simge görüntüleyen sayfanın en üstünde mevcut olduğundan ve *geri* önceki sayfasına döndürür düğmesine. Simgenin tanımlandığı `[Activity]` düzenler özniteliği `MainActivity` Android platforma özgü projede sınıfı.
- Evrensel Windows platformu üzerinde bir gezinti çubuğunda bir başlık görüntüler sayfanın en üstündeki mevcuttur.

Tüm platformlarda değerini [ `Page.Title` ](xref:Xamarin.Forms.Page.Title) özellik sayfa başlığı görüntülenir.

> [!NOTE]
> Bırakmanız önerilir bir `NavigationPage` ile doldurulmalıdır `ContentPage` yalnızca örnekler.

### <a name="creating-the-root-page"></a>Kök sayfası oluşturma

Bir gezinme yığınına eklenen ilk sayfa olarak adlandırılır *kök* sayfasında uygulama ve aşağıdaki kod örneği gösterir bu nasıl yapılır:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

Bu neden `Page1Xaml` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) gezinme yığınına, burada etkin sayfa ve uygulamanın kök sayfa olur itilecek örneği. Bu, aşağıdaki ekran görüntülerinde gösterilir:

![](hierarchical-images/mainpage.png "Gezinme yığınında kök sayfası")

> [!NOTE]
> [ `RootPage` ](xref:Xamarin.Forms.NavigationPage.RootPage) Özelliği bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) örneğinin ilk sayfasının gezinme yığınında erişim sağlar.

### <a name="pushing-pages-to-the-navigation-stack"></a>Gezinme yığınında koymadan sayfalarına

Gitmek için `Page2Xaml`, çağırmak gerekli olan [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) metodunda [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) geçerli sayfasında, aşağıdaki kod örneğinde kanıtlanabilir olarak özelliği:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

Bu neden `Page2Xaml` gezinme yığınına, burada etkin sayfa olur itilecek örneği. Bu, aşağıdaki ekran görüntülerinde gösterilir:

![](hierarchical-images/secondpage.png "Sayfa Gezinme yığınına gönderildi")

Zaman [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) yöntemi çağrılır, aşağıdaki olaylar gerçekleşir:

- Sayfa arama `PushAsync` sahip kendi [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) geçersiz kılma çağrılır.
- Geçtiğiniz için sayfanın alt [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) geçersiz kılma çağrılır.
- `PushAsync` Görev tamamlanır.

Ancak, bu olayları oluşabilen kesin sırası bağımlı platformudur. Daha fazla bilgi için [Bölüm 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold'ın Xamarin.Forms rehberi.

> [!NOTE]
> Çağrılar [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) ve [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) geçersiz kılmaları sayfa gezintisi garantili belirtileri kabul edemez. Örneğin, ios'ta `OnDisappearing` geçersiz kılma uygulama sonlandırıldığında etkin sayfa üzerinde çağrılır.

### <a name="popping-pages-from-the-navigation-stack"></a>Gezinme yığınında yığından kaldırılıyor sayfaları

Etkin sayfa gezinti yığından tuşlarına basarak POP *geri* cihazda düğme bağımsız olarak, bu cihaz üzerinde fiziksel bir düğme olup olmadığını veya bir ekrandaki bir düğme.

Program aracılığıyla geldikleri sayfaya geri dönmek için `Page2Xaml` örneği çağırmalıdır [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

Bu neden `Page2Xaml` örneği etkin sayfa olma yeni en üstte sayfa gezinti yığından kaldırılacak. Zaman [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) yöntemi çağrılır, aşağıdaki olaylar gerçekleşir:

- Sayfa arama `PopAsync` sahip kendi [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) geçersiz kılma çağrılır.
- İade edilen sayfanın kendi [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) geçersiz kılma çağrılır.
- `PopAsync` Görevi döndürür.

Ancak, bu olayları oluşabilen kesin sırası bağımlı platformudur. Daha fazla bilgi için [Bölüm 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold'ın Xamarin.Forms rehberi.

Yanı [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) ve [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) yöntemleri [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) her sayfanın özelliği de sağlar bir [ `PopToRootAsync` ](xref:Xamarin.Forms.NavigationPage.PopToRootAsync) aşağıdaki kod örneğinde gösterilen yöntemi:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Bu yöntem tüm kök POP [ `Page` ](xref:Xamarin.Forms.Page) gezinme yığınında, bu nedenle kök sayfa etkin sayfa uygulamanın yapma.

### <a name="animating-page-transitions"></a>Sayfa geçişleri animasyon ekleme

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Her sayfanın özelliği de sağlar geçersiz kılınan İtme hem de içeren pop yöntemleri bir `boolean` aşağıdaki kodda gösterildiği gibi denetleyen bir sayfa animasyon, gezinti sırasında görüntülenip görüntülenmeyeceğini parametresi Örnek:

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

Ayarı `boolean` parametresi `false` parametre ayarlanırken sayfa geçişi animasyon devre dışı bırakır `true` şartıyla temel alınan platformu tarafından desteklenen sayfa geçişi animasyon sağlar. Ancak, bu parametre eksik anında iletme ve pop yöntemleri animasyon varsayılan olarak etkinleştirir.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Gezinirken veri geçirme

Bazen bir sayfa gezinti sırasında başka bir sayfaya veri iletmek gereklidir. Bu işlemi gerçekleştirmek için iki teknik bir sayfa oluşturucusu ve yeni sayfa ayarlayarak veri geçirme [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) veriler. Her artık sırasıyla açıklanmıştır.

### <a name="passing-data-through-a-page-constructor"></a>Bir sayfa Oluşturucusu aracılığıyla veri geçirme

Gezinti sırasında başka bir sayfaya verileri geçirmek için en basit yöntem aşağıdaki kod örneğinde gösterilen bir sayfa Oluşturucu parametresi üzerinden verilmiştir:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Bu kod oluşturur bir `MainPage` örnek, geçerli tarih ve saat ISO8601 biçiminde geçirme, hangi içinde kaydırılır bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) örneği.

`MainPage` Örneği aşağıdaki kod örneğinde gösterildiği gibi bir oluşturucu parametresi aracılığıyla verileri alır:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Veriler daha sonra sayfada ayarlayarak görüntülenir [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) aşağıdaki ekran görüntülerinde gösterildiği özelliği:

![](hierarchical-images/passing-data-constructor.png "Bir sayfa Oluşturucusu aktarılan veriler")

### <a name="passing-data-through-a-bindingcontext"></a>Bir BindingContext arasında veri geçirme

Yeni sayfa ayarlayarak verileri başka bir sayfaya gezinti sırasında geçirmek için alternatif bir yaklaşım olan [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) aşağıdaki kod örneğinde gösterildiği gibi verileri için:

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

Bu kod ayarlar [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) , `SecondPage` için örnek `Contact` örneği ve ardından gider `SecondPage`.

`SecondPage` Ardından görüntülemek için veri bağlama kullanır `Contact` aşağıdaki XAML kod örneğinde gösterildiği gibi veri örneği:

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

Aşağıdaki kod örneği, nasıl veri bağlama C# dilinde gerçekleştirilebilir gösterir:

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

Veriler bir dizi tarafından sonra sayfada görüntülenir [ `Label` ](xref:Xamarin.Forms.Label) , aşağıdaki ekran görüntülerinde gösterildiği denetler:

![](hierarchical-images/passing-data-bindingcontext.png "Bir BindingContext aktarılan veriler")

Veri bağlama hakkında daha fazla bilgi için bkz. [temel veri bağlama bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>Gezinme yığınında düzenleme

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Özelliği kullanıma sunan bir [ `NavigationStack` ](xref:Xamarin.Forms.INavigation.NavigationStack) özelliği kendisinden gezinme yığınında sayfalarında elde edilebilir. Xamarin.Forms gezinme yığınında erişimi korur ancak `Navigation` özelliği sağlar [ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*) ve [ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*) ekleyerek yığın yönlendirmeye yönelik yöntemleri sayfaları veya bunları kaldırma.

[ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*) Yöntemi ekler belirtilen bir sayfa gezinme yığınında varolan belirtilen sayfayı önce aşağıdaki diyagramda gösterildiği gibi:

![](hierarchical-images/insert-page-before.png "Gezinme yığınında bir sayfa ekleme")

[ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*) Yöntemi Aşağıdaki diyagramda gösterildiği gibi belirtilen sayfa gezinti yığından kaldırır:

![](hierarchical-images/remove-page.png "Bir sayfa gezinti yığından kaldırılıyor")

Bu yöntemlerin başarılı bir oturum açma izleyerek yeni bir sayfa ile bir oturum açma sayfası değiştirme gibi ek olarak, özel gezinti deneyimine olanak tanıyın. Aşağıdaki kod örneği, bu senaryo gösterilmektedir:

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

Kullanıcının kimlik bilgilerinin doğru olması koşuluyla `MainPage` örneği önce geçerli sayfa gezinme yığınında eklenir. [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) Yöntemi ardından kaldırır gezinme yığınında geçerli sayfadaki ile `MainPage` etkin sayfa olma örneği.

## <a name="displaying-views-in-the-navigation-bar"></a>Gezinti çubuğunda görünümlerini görüntüleme

Tüm Xamarin.Forms [ `View` ](xref:Xamarin.Forms.View) gezinti çubuğunda görüntülenen bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Bu ayarı gerçekleştirilir [ `NavigationPage.TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) özelliğine bağlı bir `View`. Bu ekli özellik ayarlanabilir [ `Page` ](xref:Xamarin.Forms.Page)ve ne zaman `Page` üzerine itilir bir `NavigationPage`, `NavigationPage` özelliğinin değeri dikkate alır.

Aşağıdaki örnek runbook'undan [başlığı görünüm örnek](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TitleView/), nasıl ayarlanacağını gösterir [ `NavigationPage.TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) XAML ekli özellik:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="NavigationPageTitleView.TitleViewPage">
    <NavigationPage.TitleView>
        <Slider HeightRequest="44" WidthRequest="300" />
    </NavigationPage.TitleView>
    ...
</ContentPage>
```

Eşdeğer işte C# kod:

```csharp
public class TitleViewPage : ContentPage
{
    public TitleViewPage()
    {
        var titleView = new Slider { HeightRequest = 44, WidthRequest = 300 };
        NavigationPage.SetTitleView(this, titleView);
        ...
    }
}
```

Sonuçlanır bir [ `Slider` ](xref:Xamarin.Forms.Slider) gezinti çubuğunda üzerinde görüntülenmesini [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage):

[![Kaydırıcı TitleView](hierarchical-images/titleview-small.png "kaydırıcı TitleView")](hierarchical-images/titleview-large.png#lightbox "kaydırıcı TitleView")

> [!IMPORTANT]
> Görünüm boyutu ile belirtilmediği sürece çok sayıda görünümleri Gezinti bölmesinde görünmez [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) ve [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) özellikleri. Alternatif olarak, görünümü içinde sarmalanabilir bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) ile [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) ve [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özellikleri uygun değerlere ayarlayın.

Unutmayın, çünkü [ `Layout` ](xref:Xamarin.Forms.Layout) sınıf türetilir [ `View` ](xref:Xamarin.Forms.View) sınıfı [ `TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) ekli özellik, bir düzen görüntülemek için ayarlanabilir birden çok görünüm içeren sınıf. İOS ve evrensel Windows Platformu (UWP) için gezinti çubuğunun yüksekliği değiştirilemez ve gezinti çubuğunda görüntülenen görünüm gezinti çubuğu varsayılan boyutundan büyükse, bu nedenle kırpma meydana gelir. Ancak, Android, gezinti çubuğunun yüksekliği ayarlayarak değiştirilebilir [ `NavigationPage.BarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) bağlanılabilir özellik için bir `double` yeni yüksekliğini temsil eden. Daha fazla bilgi için [üzerinde bir NavigationPage gezinti çubuğunun yüksekliği ayarlama](~/xamarin-forms/platform/platform-specifics/consuming/android.md#navigationpage-barheight).

Alternatif olarak, genişletilmiş bir gezinti üst kısmında, renk için gezinti çubuğunu uyan sayfa içeriği, içeriğin gezinti çubuğunda ve bazı durumlarda bir görünüm yerleştirerek önerilebilir. Ayrıca, İos'ta ayırıcı çizginin ve gezinti çubuğunun alt kısmında bulunan olan gölge ayarlayarak kaldırılabilir [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) bağlanılabilir özellik için `true`. Daha fazla bilgi için [bir NavigationPage gezinti çubuğu ayırıcı gizleme](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#navigationpage-hideseparatorbar).

> [!NOTE]
> [ `BackButtonTitle` ](xref:Xamarin.Forms.NavigationPage.BackButtonTitleProperty), [ `Title` ](xref:Xamarin.Forms.Page.Title), [ `TitleIcon` ](xref:Xamarin.Forms.NavigationPage.TitleIconProperty), Ve [ `TitleView` ](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) özellikleri tüm tanımlayabilirsiniz Gezinti çubuğunda yer kaplayan değerleri. Gezinti çubuğu boyutu platform ve ekran boyutuna göre farklılık gösterir, ancak bu özelliklerin tümünü ayarı çakışmaları nedeniyle sınırlı alanınız neden olur. Bu özelliklerin birleşimi kullanmayı denemeden yerine, daha iyi istenen gezinti çubuğu tasarımınızı yalnızca ayarlayarak ulaşabileceği bulabilirsiniz `TitleView` özelliği.

### <a name="limitations"></a>Sınırlamalar

Kısıtlamaları görüntülenirken dikkat edilmesi gereken sayıda bir [ `View` ](xref:Xamarin.Forms.View) gezinti çubuğunda bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage):

- İOS, görünümler Gezinti çubuğunda yerleştirilen bir `NavigationPage` büyük başlıklar etkinleştirilip etkinleştirilmediği bağlı olarak farklı bir konumda görüntülenir. Büyük başlıklar etkinleştirme hakkında daha fazla bilgi için bkz: [görüntüleme büyük başlıklar](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title).
- Android, görünümler Gezinti çubuğunda yerleştirme bir `NavigationPage` yalnızca uygulama uyumluluğu kullanan uygulamalarda gerçekleştirilebilir.
- Büyük ve karmaşık görünümleri gibi yerleştirmek için önermedi [ `ListView` ](xref:Xamarin.Forms.ListView) ve [ `TableView` ](xref:Xamarin.Forms.TableView), gezinti çubuğunda bir `NavigationPage`.

## <a name="related-links"></a>İlgili bağlantılar

- [Sayfa gezintisi](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hiyerarşik (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [TitleView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TitleView/)
- [(Xamarin University Video) Xamarin.Forms örnek ekran Flow'da oturum oluşturma](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Xamarin.Forms (Xamarin University Video) ekran Flow'da oturum oluşturma](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](xref:Xamarin.Forms.NavigationPage)
