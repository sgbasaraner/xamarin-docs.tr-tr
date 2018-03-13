---
title: "ControlTemplate oluşturma"
description: "Denetim şablonları, uygulama düzeyinde veya sayfa düzeyinde tanımlanabilir. Bu makalede, oluşturma ve denetim şablonları kullanma gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 53309be2712f14c79b84c2eabb519b86dd73a404
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="creating-a-controltemplate"></a>ControlTemplate oluşturma

_Denetim şablonları, uygulama düzeyinde veya sayfa düzeyinde tanımlanabilir. Bu makalede, oluşturma ve denetim şablonları kullanma gösterilmektedir._

## <a name="creating-a-controltemplate-in-xaml"></a>XAML'de ControlTemplate oluşturma

Tanımlamak için bir [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) uygulama düzeyinde bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) eklenmeli `App` sınıfı. Varsayılan olarak, bir şablondan oluşturulan tüm Xamarin.Forms uygulamaların kullanmak **uygulama** uygulamak için sınıf [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) bir alt kümesi. Bildirmek için bir `ControlTemplate` uygulama düzeyinde uygulamanın `ResourceDictionary` XAML, varsayılan kullanılarak **uygulama** XAML ile sınıfı yerini **uygulama** sınıfı ve ilişkili arka plan kodu, olarak Aşağıdaki kod örneğinde gösterildiği:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.App">
    <Application.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                <Grid>
                    ...
                    <BoxView ... />
                    <Label Text="Control Template Demo App"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                    <ContentPresenter ... />
                    <BoxView Color="Teal" ... />
                    <Label Text="(c) Xamarin 2016"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                </Grid>
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Her [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) örneği yeniden kullanılabilir bir nesne olarak oluşturulur bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).  Her bildirim benzersiz bir vererek elde edilen `x:Key` açıklayıcı bir anahtarla sağlar özniteliği `ResourceDictionary`.

Aşağıdaki kod örneği ilişkili gösterir `App` arka plan kodu:

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent ();
    MainPage = new HomePage ();
  }
}
```

Ayar yanı sıra [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) özelliği, arka plan kodu ayrıca çağırmalısınız `InitializeComponent` yöntemi yüklenemedi ve ilişkili XAML ayrıştırılamadı.

Aşağıdaki örnekte gösterildiği kod bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) uygulama `TealTemplate` için [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0"
                 ControlTemplate="{StaticResource TealTemplate}">
        <StackLayout VerticalOptions="CenterAndExpand">
            <Label Text="Welcome to the app!" HorizontalOptions="Center" />
            <Button Text="Change Theme" Clicked="OnButtonClicked" />
        </StackLayout>
    </ContentView>
</ContentPage>
```

`TealTemplate` Atandığı [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) kullanarak özellik `StaticResource` biçimlendirme uzantısı. [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Özelliği ayarlanmış bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) görüntülenecek içeriği tanımlar [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Bu içerik tarafından görüntülenen [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) içinde yer alan `TealTemplate`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

![](creating-images/teal-theme.png "Deniz denetim şablonu")

### <a name="re-theming-an-application-at-runtime"></a>RE Tema oluşturma zamanında bir uygulama

Tıklatarak **değişiklik tema** düğmesi yürütür `OnButtonClicked` aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Bu yöntem etkin değiştirir [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) alternatif örneğiyle `ControlTemplate` aşağıdaki ekran görüntüsünde kaynaklanan örneği:

![](creating-images/aqua-theme.png "Açık mavi denetim şablonu")

> [!NOTE]
> Üzerinde bir `ContentPage`, `Content` özelliği atanabilir ve `ControlTemplate` özelliği de ayarlanabilir. Bu meydana geldiğinde, varsa `ControlTemplate` içeren bir `ContentPresenter` örneği, atanmış içerik `Content` özelliği tarafından sunulabilir `ContentPresenter` içinde `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>ControlTemplate bir stil ile ayarlama

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) aracılığıyla uygulanabilir bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) daha fazla tema özelliği genişletin. Bu oluşturarak elde edilebilir bir *örtük* veya *açık* hedef görüntüle stil bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)ve ayarı `ControlTemplate` hedef özelliği görünümünde [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örneği. Aşağıdaki örnekte gösterildiği kod bir *örtük* uygulama düzeyinde eklenir stili [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Çünkü [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örneği *örtük*, onu tümüne uygulanacak `ContentView` uygulama örneği. Bu nedenle, onu artık ayarlamak gerekli olan [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) özelliği, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Stilleri hakkında daha fazla bilgi için bkz: [stilleri](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>Sayfa düzeyinde ControlTemplate oluşturma

Oluşturma yanı sıra [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) örnekleri uygulama düzeyinde, bunlar da oluşturulabilir sayfa düzeyinde aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                ...
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
        ...
    </ContentView>
</ContentPage>
```

Eklerken bir [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) sayfa düzeyinde bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) eklenen [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)ve ardından `ControlTemplate` örnekleri dahil içinde `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>C'de ControlTemplate oluşturma&#35;

Tanımlamak için bir [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) uygulama düzeyinde bir `class` bu temsil oluşturulmalıdır `ControlTemplate`. Sınıf türetin [düzeni](~/xamarin-forms/user-interface/layouts/index.md) şablonu, aşağıdaki kod örneğinde gösterildiği gibi kullanılan:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var contentPresenter = new ContentPresenter ();
    Children.Add (contentPresenter, 0, 1);
    Grid.SetColumnSpan (contentPresenter, 2);
    ...
  }
}

class AquaTemplate : Grid
{
  ...
}
```

`AquaTemplate` Sınıftır aynı `TealTemplate` farklı renkler için kullanılan dışında sınıfının [ `BoxView.Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) ve [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) özellikleri.

Aşağıdaki örnekte gösterildiği kod bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) uygulama `TealTemplate` için [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
public class HomePageCS : ContentPage
{
  ...
  ControlTemplate tealTemplate = new ControlTemplate (typeof(TealTemplate));
  ControlTemplate aquaTemplate = new ControlTemplate (typeof(AquaTemplate));

  public HomePageCS ()
  {
    var button = new Button { Text = "Change Theme" };
    var contentView = new ContentView {
      Padding = new Thickness (0, 20, 0, 0),
      Content = new StackLayout {
        VerticalOptions = LayoutOptions.CenterAndExpand,
        Children = {
          new Label { Text = "Welcome to the app!", HorizontalOptions = LayoutOptions.Center },
          button
        }
      },
      ControlTemplate = tealTemplate
    };
    ...
    Content = contentView;
  }
}
```

[ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Örnekleri denetim şablonları tanımlayın sınıfları türü belirterek oluşturulur `ControlTemplate` Oluşturucusu.

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Özelliği ayarlanmış bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) görüntülenecek içeriği tanımlar [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Bu içerik tarafından görüntülenen [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) içinde yer alan `TealTemplate`. Özetlenen aynı mekanizmayı önceden tema için çalışma zamanında değiştirmek için kullanılan `AquaTheme`.

## <a name="summary"></a>Özet

Bu makalede nasıl oluşturulacağı ve denetim şablonları kullanma gösterilmektedir. Denetim şablonları, uygulama düzeyinde veya sayfa düzeyinde tanımlanabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Stiller](~/xamarin-forms/user-interface/styles/index.md)
- [Basit tema (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
