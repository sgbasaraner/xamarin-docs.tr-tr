---
title: Xamarin.Forms ControlTemplate bağlama
description: Şablon bağlamaları genel özellikleri için bir denetim şablonu verilere denetimler bağlama denetimleri kolayca değiştirilecek denetim şablonu içindeki özellik değerlerini etkinleştirme izin verin. Bu makalede, veri bağlama denetimi şablondan gerçekleştirmek için şablon bağlamaları kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 86de2ad6009365b3debbe1a2310651002b023219
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995573"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Xamarin.Forms ControlTemplate bağlama

_Şablon bağlamaları genel özellikleri için bir denetim şablonu verilere denetimler bağlama denetimleri kolayca değiştirilecek denetim şablonu içindeki özellik değerlerini etkinleştirme izin verin. Bu makalede, veri bağlama denetimi şablondan gerçekleştirmek için şablon bağlamaları kullanmayı gösterir._

A [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) denetim özelliği Denetim şablonunda üst bağlanabilir bir özellikte bağlamak için kullanılan *hedef* denetim şablona sahip bir görünüm. Örneğin, tarafından görüntülenen metni tanımlamak yerine [ `Label` ](xref:Xamarin.Forms.Label) içinde örnekler [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate), bağlamak için bir şablon bağlamayı kullanabilirsiniz [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) özellik için görüntülenecek metni tanımlayan bağlanabilir özellikleri.

A [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) varolan benzer [ `Binding` ](xref:Xamarin.Forms.Binding)dışında *kaynak* , bir `TemplateBinding` üst öğesi için her zaman otomatik olarak ayarlanır *hedef* denetim şablona sahip bir görünüm. Ancak, bu not bir `TemplateBinding` dışında bir [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) desteklenmiyor.

## <a name="creating-a-templatebinding-in-xaml"></a>XAML içinde bir TemplateBinding oluşturma

XAML içinde bir [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) kullanılarak oluşturulan [ `TemplateBinding` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) işaretleme uzantısı, aşağıdaki kod örneğinde gösterildiği gibi:

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

Yerine ayarlamak [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) statik metin özelliklerle özelliklerini kullanabilir şablon bağlamaları üst bağlanabilir özellikleri bağlamak için *hedef* sahip Görünüm [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Ancak, şablon bağlamaları için bağlama unutmayın `Parent.HeaderText` ve `Parent.FooterText`, yerine `HeaderText` ve `FooterText`. Bu örnekte iki üst varlık bağlanabilir özellikler tanımlanan olmasıdır *hedef* , üst yerine aşağıdaki kod örneğinde gösterildiği gibi görüntüleyin:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

*Kaynak* şablonunu bağlama üst için her zaman otomatik olarak ayarlamaya *hedef* burada denetim şablonu sahip Görünüm [ `ContentView` ](xref:Xamarin.Forms.ContentView) örneği. Bağlamanın kullandığı şablon [ `Parent` ](xref:Xamarin.Forms.Element.Parent) üst öğesi döndürülecek özellik `ContentView` olan örneği [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) örneği. Bu nedenle, kullanarak bir [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) içinde [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) bağlamak için `Parent.HeaderText` ve `Parent.FooterText` üzerinde tanımlanan bağlanabilir özellikler bulur `ContentPage`, olarak Aşağıdaki kod örneğinde gösterilmiştir:

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

Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

![](template-binding-images/teal-theme.png "Deniz Mavisi denetim şablonu şablon bağlamaları kullanma")

## <a name="creating-a-templatebinding-in-c35"></a>C dilinde bir TemplateBinding oluşturma&#35;

C# ' ta, bir [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) kullanılarak oluşturulan `TemplateBinding` oluşturucusu, aşağıdaki kod örneğinde gösterildiği gibi:

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

Yerine ayarlamak [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) statik metin özelliklerle özelliklerini kullanabilir şablon bağlamaları üst bağlanabilir özellikleri bağlamak için *hedef* sahip Görünüm [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Şablon bağlaması kullanılarak oluşturulan [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) yöntemini belirten bir [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) örneği ikinci parametre olarak. Şablon bağlamaları için bağlama Not `Parent.HeaderText` ve `Parent.FooterText`bağlanabilir özellikler dizinleriyle üzerinde tanımlanan çünkü *hedef* , üst yerine aşağıdaki kod örneğinde gösterildiği gibi görüntüleyin:

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

Bağlanabilir özellikler üzerinde tanımlanan `ContentPage`, daha önce belirtildiği gibi.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Bir BindableProperty ViewModel özelliğine bağlama

Daha önce belirtildiği gibi bir [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) denetim özelliği Denetim şablonunda üst bağlanabilir bir özelliğe bağlar. *hedef* denetim şablona sahip bir görünüm. Sırayla bu bağlanabilir özellikler Viewmodel'lar özelliklerinde bağlanabilir.

Aşağıdaki kod örneği, iki özellik üzerinde bir ViewModel tanımlar:

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText` Ve `FooterText` ViewModel özellikleri için aşağıdaki XAML kod örneğinde gösterildiği gibi bağlanabilir:

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

`HeaderText` Ve `FooterText` bağlanabilir özellikler için bağlı `HomePageViewModel.HeaderText` ve `HomePageViewModel.FooterText` ayarı nedeniyle özellikleri [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) örneğine `HomePageViewModel` sınıfı. Genel olarak, bu denetim özelliklerini sonuçları [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) bağlanan [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) üzerinde örnekleri [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), hangi sırayla bağlanma ViewModel özellikleri.

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilmiştir:

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

Viewmodel'lar veri bağlama hakkında daha fazla bilgi için bkz. [gelen veri bağlamaları-MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Özet

Bu makalede, şablon bağlamaları'ı kullanarak veri bağlama denetimi şablondan gerçekleştirmeyi gösterilmiştir. Şablon bağlamaları genel özellikleri için bir denetim şablonu verilere denetimler bağlama denetimleri kolayca değiştirilecek denetim şablonu içindeki özellik değerlerini etkinleştirme izin verin.



## <a name="related-links"></a>İlgili bağlantılar

- [Temel veri bağlama bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Veri bağlamaları-MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Şablon Bağlama (örnek) ile basit tema](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Şablon Bağlama ve ViewModel (örnek) ile basit tema](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](xref:Xamarin.Forms.TemplateBinding)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentView](xref:Xamarin.Forms.ContentView)
