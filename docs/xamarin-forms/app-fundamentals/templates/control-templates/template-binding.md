---
title: "Bağlamanın ControlTemplate dışında"
description: "Ortak özellikler için bir denetim şablonu verilere denetimler bağlama şablonu bağlamaları kolayca değiştirilecek denetim şablonu denetimlerinde özellik değerlerini etkinleştirme izin verin. Bu makalede, veri bağlama denetimi şablonundan gerçekleştirmek için şablonu bağlamalar kullanılarak gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 5b330c448a135cbcf8fc2745debc48924e29c103
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="binding-from-a-controltemplate"></a>Bağlamanın ControlTemplate dışında

_Ortak özellikler için bir denetim şablonu verilere denetimler bağlama şablonu bağlamaları kolayca değiştirilecek denetim şablonu denetimlerinde özellik değerlerini etkinleştirme izin verin. Bu makalede, veri bağlama denetimi şablonundan gerçekleştirmek için şablonu bağlamalar kullanılarak gösterilmektedir._

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) üst bağlanabilir bir özellik için bir denetim şablonu bir denetimin özelliği bağlamak için kullanılan *hedef* denetim şablona sahip görünümü. Örneğin, tarafından görüntülenen metni tanımlama yerine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) içinde örnekleri [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/), bağlamak için bir şablon bağlama kullanabilirsiniz [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) özellik için görüntülenecek metni tanımlayan bağlanabilir özellikleri.

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) mevcut bir benzer [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/)dışında *kaynak* , bir `TemplateBinding` üst öğesi için her zaman otomatik olarak ayarlanır *hedef* denetim şablona sahip görünümü. Ancak, bu not bir `TemplateBinding` dışında bir [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) desteklenmiyor.

## <a name="creating-a-templatebinding-in-xaml"></a>XAML'de TemplateBinding oluşturma

XAML'de bir [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) kullanılarak oluşturulan [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) biçimlendirme uzantısı, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

Yerine ayarlamak [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) özellikleri statik metne özellikleri kullanabilir şablon bağlamaları üst bağlanabilir özellikleri bağlamak için *hedef* sahibi Görünüm [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Ancak, bu şablonu bağlamaları için bağlama unutmayın `Parent.HeaderText` ve `Parent.FooterText`, yerine `HeaderText` ve `FooterText`. Bu örnekte iki üst varlık bağlanabilir özellikleri tanımlanır ve bunun nedeni *hedef* , üst yerine aşağıdaki kod örneğinde gösterildiği gibi görüntüleyin:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

*Kaynak* şablonunu bağlamayı üst öğesi için her zaman otomatik olarak ayarlamak *hedef* burada denetim şablona sahip Görünüm [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) örneği. Bağlamanın kullandığı şablon [ `Parent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Parent/) özelliği üst öğenin geri dönmek için `ContentView` olan örneği [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örneği. Bu nedenle, kullanarak bir [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) içinde [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) bağlamak için `Parent.HeaderText` ve `Parent.FooterText` üzerinde tanımlanan bağlanabilir özellikleri bulur `ContentPage`, olarak Aşağıdaki kod örneğinde gösterilmektedir:

```csharp
public static readonly BindableProperty HeaderTextProperty =
  BindableProperty.Create ("HeaderText", typeof(string), typeof(HomePage), "Control Template Demo App");
public static readonly BindableProperty FooterTextProperty =
  BindableProperty.Create ("FooterText", typeof(string), typeof(HomePage), "(c) Xamarin 2016");

public string HeaderText {
  get { return (string)GetValue (HeaderTextProperty); }
}

public string FooterText {
  get { return (string)GetValue (FooterTextProperty); }
}
```

Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

![](template-binding-images/teal-theme.png "Deniz denetim şablonu bağlamalar kullanılarak şablonu")

## <a name="creating-a-templatebinding-in-c35"></a>C &#35;TemplateBinding oluşturma;

C# ' ta, bir [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) kullanılarak oluşturulan `TemplateBinding` aşağıdaki kod örneğinde gösterildiği şekilde Oluşturucusu:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var topLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    topLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.HeaderText"));
    ...
    var bottomLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    bottomLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.FooterText"));
    ...
  }
}
```

Yerine ayarlamak [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) özellikleri statik metne özellikleri kullanabilir şablon bağlamaları üst bağlanabilir özellikleri bağlamak için *hedef* sahibi Görünüm [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Şablon Bağlama kullanılarak oluşturulan [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) yöntemi, belirten bir [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) ikinci parametre olarak örneği. Şablon bağlamaları bağlamak Not `Parent.HeaderText` ve `Parent.FooterText`, bağlanabilir özellikleri iki üst varlık tanımlandığından *hedef* , üst yerine aşağıdaki kod örneğinde gösterildiği gibi görüntüleyin:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    Content = new ContentView {
      ControlTemplate = tealTemplate,
      Content = new StackLayout {
        ...
      },
      ...
    };
    ...
  }
}
```

Bağlanabilir özellikler üzerinde tanımlanan `ContentPage`, daha önce özetlendiği gibi.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Bir BindableProperty ViewModel özelliği için bağlama

Daha önce belirtildiği gibi bir [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) üst bağlanabilir bir özellik için bir denetim şablonu bir denetimin özelliği bağlar *hedef* denetim şablona sahip görünümü. Buna karşılık, bu bağlanabilir özellikleri ViewModels özelliklerinde bağlanabilir.

Aşağıdaki kod örneği iki özellik ViewModel üzerinde tanımlar:

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText` Ve `FooterText` ViewModel özellikler için aşağıdaki XAML kod örneğinde gösterildiği gibi bağlanabilir:

```xaml
<ContentPage xmlns:local="clr-namespace:SimpleTheme;assembly=SimpleTheme"
             HeaderText="{Binding HeaderText}" FooterText="{Binding FooterText}" ...>
    <ContentPage.BindingContext>
        <local:HomePageViewModel />
    </ContentPage.BindingContext>
    <ContentView ControlTemplate="{StaticResource TealTemplate}" ...>
    ...
    </ContentView>
</ContentPage>
```

`HeaderText` Ve `FooterText` bağlanabilir özellikleri bağlı `HomePageViewModel.HeaderText` ve `HomePageViewModel.FooterText` ayarı nedeniyle özellikleri [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) örneğine `HomePageViewModel` sınıfı. Genel olarak, bu denetim özelliklerini sonuçlanır [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) bağlanan [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) üzerinde örnekleri [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), hangi sırayla bağlamak için ViewModel özellikleri.

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilir:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    BindingContext = new HomePageViewModel ();
    this.SetBinding (HeaderTextProperty, "HeaderText");
    this.SetBinding (FooterTextProperty, "FooterText");
    ...
  }
}
```

ViewModels veri bağlama hakkında daha fazla bilgi için bkz: [gelen veri bağlamalar MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Özet

Bu makalede veri bağlama denetimi şablonundan gerçekleştirmek için şablonu bağlamalar kullanılarak gösterilmektedir. Ortak özellikler için bir denetim şablonu verilere denetimler bağlama şablonu bağlamaları kolayca değiştirilecek denetim şablonu denetimlerinde özellik değerlerini etkinleştirme izin verin.



## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Veri bağlamaları MVVM için](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Şablon Bağlama (örnek) ile basit tema](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Basit tema şablon bağlama ve ViewModel (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
