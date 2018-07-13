---
title: ControlTemplate oluşturarak
description: Denetim şablonları, uygulama düzeyinde veya sayfa düzeyinde tanımlanabilir. Bu makalede, denetim şablonları oluşturup gösterilmektedir.
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b83668f6836b1d5d98f67592bf3e2b01e7319edc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998200"
---
# <a name="creating-a-controltemplate"></a>ControlTemplate oluşturarak

_Denetim şablonları, uygulama düzeyinde veya sayfa düzeyinde tanımlanabilir. Bu makalede, denetim şablonları oluşturup gösterilmektedir._

## <a name="creating-a-controltemplate-in-xaml"></a>ControlTemplate içinde XAML oluşturma

Tanımlamak için bir [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) uygulama düzeyinde bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) eklenmelidir `App` sınıfı. Varsayılan olarak, bir şablondan oluşturulan tüm Xamarin.Forms uygulamaları kullanın **uygulama** uygulamak için sınıfı [ `Application` ](xref:Xamarin.Forms.Application) öğesinin alt sınıfı. Bildirmek için bir `ControlTemplate` içinde uygulamanın uygulama düzeyinde `ResourceDictionary` kullanarak XAML, varsayılan **uygulama** ile bir XAML sınıfı yerine **uygulama** sınıfı ve ilişkili kodunu arka plan, olarak Aşağıdaki kod örneğinde gösterilen:

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

Her [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) örneği yeniden kullanılabilir bir nesne olarak oluşturulur bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary).  Bu bildirimi her bir benzersiz sağlayarak gerçekleştirilir `x:Key` açıklayıcı bir anahtarla sağlayan öznitelik `ResourceDictionary`.

Aşağıdaki kod örneği ilişkili gösterir `App` arka plan kod:

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

Ayar yanı sıra [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) özelliği, arka plan kod çağırmalıdır ayrıca `InitializeComponent` yöntemi yüklenemedi ve ilişkili XAML ayrıştırılamadı.

Aşağıdaki kod örnekte gösterildiği bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) uygulama `TealTemplate` için [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

`TealTemplate` Atandığı [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) kullanarak özellik `StaticResource` işaretleme uzantısı. [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) Özelliği bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) tanımlayan içeriği görüntülenmesine izin [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Bu içerik tarafından görüntülenen [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) bulunan `TealTemplate`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

![](creating-images/teal-theme.png "Deniz Mavisi denetim şablonu")

### <a name="re-theming-an-application-at-runtime"></a>RE Tema oluşturma zamanında bir uygulama

Tıklayarak **temayı Değiştir** düğmesi yürütür `OnButtonClicked` aşağıdaki kod örneğinde gösterilen yöntemi:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Bu yöntem etkin değiştirir [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) diğer örnekle `ControlTemplate` aşağıdaki ekran görüntüsünde kaynaklanan örneği:

![](creating-images/aqua-theme.png "Deniz Mavisi denetim şablonu")

> [!NOTE]
> Üzerinde bir `ContentPage`, `Content` özelliği atanabilir ve `ControlTemplate` özelliği de ayarlanabilir. Bu gerçekleştiğinde, varsa `ControlTemplate` içeren bir `ContentPresenter` örneği için atanan içerikler `Content` özelliği tarafından gösterilir `ContentPresenter` içinde `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>ControlTemplate bir stil ile ayarlama

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) aracılığıyla uygulanabilir bir [ `Style` ](xref:Xamarin.Forms.Style) daha fazla tema özelliğini genişletin. Bu oluşturarak gerçekleştirilebilir bir *örtük* veya *açık* hedef Görünümü'nde stili bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ve ayarı `ControlTemplate` hedef özelliği görünümünde [ `Style` ](xref:Xamarin.Forms.Style) örneği. Aşağıdaki kod örnekte gösterildiği bir *örtük* uygulama düzeyine eklenmiş stili [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Çünkü [ `Style` ](xref:Xamarin.Forms.Style) örneği *örtük*, onu tümüne uygulanacak `ContentView` uygulama örnekleri. Bu nedenle, artık ayarlamak gerekli değildir [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) aşağıdaki kod örneğinde gösterildiği gibi özelliği:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Stilleri hakkında daha fazla bilgi için bkz. [stilleri](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>ControlTemplate sayfa düzeyinde oluşturma

Oluşturmaya ek olarak [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) örnekleri uygulama düzeyinde, bunlar da oluşturulabilir sayfa düzeyinde aşağıdaki kod örneğinde gösterildiği gibi:

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

Eklerken bir [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) sayfa düzeyinde bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) eklenir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), ardından `ControlTemplate` örnekleri dahil edilir içinde `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>ControlTemplate içinde C oluşturma&#35;

Tanımlamak için bir [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) uygulama düzeyinde bir `class` temsil eden oluşturulmalıdır `ControlTemplate`. Sınıfın türetilmesi [Düzen](~/xamarin-forms/user-interface/layouts/index.md) şablon olarak kullanılan aşağıdaki kod örneğinde gösterildiği gibi:

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

`AquaTemplate` Sınıfı, aynı `TealTemplate` farklı renkler için kullanılan dışında sınıf [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) ve [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) özellikleri.

Aşağıdaki kod örnekte gösterildiği bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) uygulama `TealTemplate` için [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

[ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) Örnekleri, denetim şablonları tanımlamak sınıfları türünü belirterek oluşturulur `ControlTemplate` Oluşturucusu.

[ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) Özelliği bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) tanımlayan içeriği görüntülenmesine izin [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Bu içerik tarafından görüntülenen [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) bulunan `TealTemplate`. Özetlenen mekanizma daha önce çalışma zamanında temasını değiştirmek için kullanılan `AquaTheme`.

## <a name="summary"></a>Özet

Bu makalede, denetim şablonları oluşturup nasıl gösterilmiştir. Denetim şablonları, uygulama düzeyinde veya sayfa düzeyinde tanımlanabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Stiller](~/xamarin-forms/user-interface/styles/index.md)
- [Basit tema (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
