---
title: Xamarin.Forms temel bağlamalar
description: Bu makalede, biri genellikle bir kullanıcı arabirimi nesnesi olan bir çift özelliğe iki nesne arasındaki en az bağlayan Xamarin.Forms veri bağlaması kullanmayı açıklar. Bu iki nesne, hedef ve kaynak adı verilir.
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: b52f249b184d49731fd5decdb5877c70e29a3b84
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "38998078"
---
# <a name="xamarinforms-basic-bindings"></a>Xamarin.Forms temel bağlamalar

Xamarin.Forms veri bağlama, bir çift özelliğe en az biri genellikle bir kullanıcı arabirimi nesnesi olan iki nesne arasındaki bağlar. Bu iki nesne adında *hedef* ve *kaynak*:

- *Hedef* nesnesi (ve özellik), veri bağlama üzerinde ayarlanmıştır.
- *Kaynak* veri bağlama tarafından başvurulan nesne (ve özellik).

Bu ayrım bazen biraz kafa karıştırıcı olabilir: en basit durumda, veri akışları kaynaktan hedefe hedef özelliğinin değeri kaynak özelliğinin değerinden ayarlandığı anlamına gelir. Ancak, bazı durumlarda, veri alternatif olarak hedeften kaynağa ya da her iki yönde de akabilir. Karışıklığı önlemek için bu verileri sağlayan yerine olsa bile veri almak, veri bağlama ayarlanmış nesnesi hedeflediği her zaman unutmayın.

## <a name="bindings-with-a-binding-context"></a>Bağlama bağlamı bağlamaları

Veri bağlamaları genellikle tamamen XAML içinde belirtilmiş olsa da, kod veri bağlamaları görmek için öğretici. **Temel bağlama kodunu** sayfası ile bir XAML dosyası içeren bir `Label` ve `Slider`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicCodeBindingPage"
             Title="Basic Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="48"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider` Aralığı 0-360 için ayarlanır. Döndürmek için bu programı amacı olan `Label` düzenleme tarafından `Slider`.

Veri bağlamaları ayarlarsınız `ValueChanged` olayı `Slider` erişen bir olay işleyicisine `Value` özelliği `Slider` ve bu değeri ayarlar `Rotation` özelliği `Label`. Veri bağlama, o işin otomatikleştirir; olay işleyicisi ile içerdiği kod artık gerekli değildir.

Öğesinden türetilen herhangi bir sınıf örneği üzerinde bir bağlama ayarlayabilirsiniz [ `BindableObject` ](xref:Xamarin.Forms.BindableObject), içeren `Element`, `VisualElement`, `View`, ve `View` türevleri.  Bağlama, her zaman hedef nesne üzerinde ayarlanır. Bağlamanın kaynak nesnesine başvuruyor. Veri bağlamayı belirlemek için aşağıdaki iki üyesi hedef sınıfın kullanın:

- [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Özelliği kaynak nesnesi belirtir.
- [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) Yöntemi, özelliği kaynak ve hedef özelliği belirtir.

Bu örnekte, `Label` bağlama hedefi ve `Slider` bağlama kaynağı. Değişiklikleri `Slider` kaynağını etkilemez döndürmesini `Label` hedef. Veriler kaynaktan hedefe akar.

`SetBinding` Yöntemi tarafından tanımlanan `BindableObject` türünde bir bağımsız değişken olan [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) içinden [ `Binding` ](xref:Xamarin.Forms.Binding) sınıfı türer, ancak diğer vardır `SetBinding` yöntemleri tarafından tanımlanan [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions) sınıfı. Arka plan kod dosyasında **temel bağlama kodunu** daha basit bir örnek kullanan [ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) genişletme yöntemi bu sınıftaki.

```csharp
public partial class BasicCodeBindingPage : ContentPage
{
    public BasicCodeBindingPage()
    {
        InitializeComponent();

        label.BindingContext = slider;
        label.SetBinding(Label.RotationProperty, "Value");
    }
}
```

`Label` Nesne, nesne, bu özelliğin ayarlandığı ve yöntemin çağrıldığı, bu nedenle bağlama hedefi değil. `BindingContext` Özelliği gösterir, bağlama kaynağı `Slider`.

`SetBinding` Yöntem bağlama hedefi üzerinde çağrılır, ancak hedef özelliği hem kaynak özelliği belirtir. Hedef özelliği olarak belirtilen bir `BindableProperty` nesne: `Label.RotationProperty`. Source özelliği bir dize olarak belirtilen ve gösterir `Value` özelliği `Slider`.

`SetBinding` Veri bağlamalarının en önemli kurallardan biri yöntemi ortaya çıkarır:

*Hedef özelliği bağlanabilir bir özelliğe göre yedeklenmelidir.*

Bu kural hedef nesnenin türetildiği bir sınıf örneği olmalıdır gelir `BindableObject`. Bkz: [ **bağlanabilir Özellikler** ](~/xamarin-forms/xaml/bindable-properties.md) bağlanabilir nesneleri ve bağlanabilir özellikler genel bakış makalesi.

Bir dize olarak belirtilen kaynak özelliği için bu kural yoktur. Dahili olarak, yansıma gerçek bir özelliğe erişmek için kullanılır. Bu örnekte, ancak `Value` özelliği bağlanabilir bir özelliği tarafından da desteklenir.

Kod biraz Basitleştirilmiş: `RotationProperty` bağlanabilir özelliği tarafından tanımlanır `VisualElement`ve tarafından devralınan `Label` ve `ContentPage` de, bu nedenle sınıf adı değil gerekli içinde `SetBinding` çağırın:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Ancak, sınıf adı dahil olmak üzere, iyi bir anımsatıcı hedef nesnenin değil.

Değişiklik yaptıkça `Slider`, `Label` buna göre döndürür:

[![Basice bağlama kod](basic-bindings-images/basiccodebinding-small.png "temel kod bağlama")](basic-bindings-images/basiccodebinding-large.png#lightbox "temel kod bağlama")

**Temel Xaml bağlama** sayfasıdır aynı **temel kod bağlama** dışında XAML içinde tüm veri bağlama tanımlar:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicXamlBindingPage"
             Title="Basic XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Yalnızca kodum olduğu gibi veri bağlaması olan hedef nesne üzerinde ayarlanır `Label`. İki XAML biçimlendirme uzantıları faydalanırsınız. Küme ayracı sınırlayıcılarla anında tanınabilir şunlardır:

- `x:Reference` İşaretleme uzantısı olan kaynak nesneye başvurmak için gerekli `Slider` adlı `slider`.
- `Binding` İşaretleme uzantısı bağlantıları `Rotation` özelliği `Label` için `Value` özelliği `Slider`.

Makaleye göz atın [XAML biçimlendirme uzantıları](~/xamarin-forms/xaml/markup-extensions/index.md) XAML biçimlendirme uzantıları hakkında daha fazla bilgi. `x:Reference` İşaretleme uzantısı tarafından desteklenen [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) sınıfı; `Binding` tarafından desteklenen [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) sınıfı. XML olarak ad alanı öneklerini belirtmek, `x:Reference` XAML 2009 belirtiminin bir parçası olduğu sırada `Binding` Xamarin.Forms bir parçasıdır. Tırnak işareti ve küme ayraçlarının içinde göründüğüne dikkat edin.

Unutmak çok kolaydır `x:Reference` ayarlarken işaretleme uzantısı `BindingContext`. Yanlışlıkla özelliği doğrudan böyle bağlama kaynağı adını ayarlamak için ortaktır:

```xaml
BindingContext="slider"
```

Ancak, doğru değil. Bu işaretleme ayarlar `BindingContext` özelliğini bir `string` karakterine yazım "kaydırıcı" nesne!

Source özelliği ile belirtilen bildirim [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) özelliği `BindingExtension`, karşılık gelen ile [ `Path` ](xref:Xamarin.Forms.Binding.Path) özelliği [ `Binding` ](xref:Xamarin.Forms.Binding) sınıfı.

Gösterilen biçimlendirme **temel XAML bağlama** sayfası Basitleştirilmiş: XAML biçimlendirme uzantıları gibi `x:Reference` ve `Binding` olabilir *içerik özelliği* öznitelik tanımlanmamışsa, XAML için Biçimlendirme uzantıları, özellik adı görünür gerektirmeyeceği anlamına gelir. `Name` İçerik özelliği bir özelliktir `x:Reference`ve `Path` özelliktir içerik özelliğinin `Binding`, yani bunlar gelen ifadeleri kaldırılabilir:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Bağlama bağlamı olmadan bağlamaları

`BindingContext` Özelliktir veri bağlamaları önemli bir bileşenidir, ancak her zaman gerekli değildir. Kaynak nesne, bunun yerine belirtilebilir `SetBinding` arayın veya `Binding` işaretleme uzantısı.

Bu gösterilmiştir **alternatif bağlama kodunu** örnek. XAML dosyası benzer **temel bağlama kodunu** örnek hariç `Slider` denetlemek için tanımlanan `Scale` özelliği `Label`. Bu nedenle `Slider` aralığı için ayarlanmış &ndash;2 ila 2:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeCodeBindingPage"
             Title="Alternative Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Arka plan kod dosyasında olan bağlamanın ayarlar [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) yöntemi tarafından tanımlanan `BindableObject`. Bağımsız değişken bir [Oluşturucusu](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) için [ `Binding` ](xref:Xamarin.Forms.Binding) sınıfı:

```csharp
public partial class AlternativeCodeBindingPage : ContentPage
{
    public AlternativeCodeBindingPage()
    {
        InitializeComponent();

        label.SetBinding(Label.ScaleProperty, new Binding("Value", source: slider));
    }
}
```

`Binding` Oluşturucusu olan 6 parametreleri, bu nedenle `source` parametresi bir adlandırılmış bağımsız değişkeni ile belirtilen. Bağımsız değişken `slider` nesne.

Bu programı çalıştırmayı biraz şaşırtıcı olabilir:

[![Alternatif kod bağlama](basic-bindings-images/alternativecodebinding-small.png "alternatif kod bağlama")](basic-bindings-images/alternativecodebinding-large.png#lightbox "alternatif kod bağlama")

İOS ekranın sol taraftaki sayfası belirdiğinde ekranın nasıl göründüğünü gösterir. Nerede olduğunu `Label`?

Sorun `Slider` başlangıç değerinin 0. Bu neden `Scale` özelliği `Label` Ayrıca, varsayılan değer olan 1'geçersiz kılma 0 olarak ayarlanacak. Sonuçlanır `Label` olan başlangıçta görünmez. Android ve evrensel Windows Platformu (UWP) ekran görüntüleri göz atarak, işleyebileceğiniz `Slider` yapmak `Label` yeniden görünür, ancak ilk, kaybolması disconcerting.

İçinde keşfedeceksiniz [sonraki makalede](binding-mode.md) başlatarak bu sorundan kaçınmak nasıl `Slider` varsayılan değerinden `Scale` özelliği.

> [!NOTE]
> [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Sınıfı da tanımlar [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) ve [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) ölçeklendirebilirsiniz özellikleri `VisualElement` içinde farklı Yatay ve dikey yönde.

**Alternatif XAML bağlama** sayfası tamamen XAML içinde eşdeğer bağlama gösterir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeXamlBindingPage"
             Title="Alternative XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="{Binding Source={x:Reference slider},
                               Path=Value}" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Artık `Binding` işaretleme uzantısına sahip iki özellik ayarlanırsa, `Source` ve `Path`bir virgülle ayrılmış. Tercih ederseniz bunların aynı satırda görünebilir:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` Özelliği için bir katıştırılmış `x:Reference` aksi ayarı olarak aynı söz dizimine sahiptir işaretleme uzantısı `BindingContext`. Tırnak işareti ve küme ayraçlarının içinde görünür ve iki özelliği virgülle ayrılmalıdır dikkat edin.

İçerik özelliği `Binding` işaretleme uzantısı `Path`, ancak `Path=` işaretleme uzantısı bir parçası yalnızca ortadan ifadedeki ilk özelliği ise. Ortadan kaldırmak için `Path=` bölümü, gereken iki özelliklerini değiştirmek:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

XAML biçimlendirme uzantıları küme ayraçları genellikle ayrılmış olsa da, nesne öğeleri olarak da belirtilebilir:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Source="{x:Reference slider}"
                 Path="Value" />
    </Label.Scale>
</Label>
```

Artık `Source` ve `Path` özelliklerdir normal XAML öznitelikleri: değerleri tırnak içine görünür ve öznitelikleri virgülle ayrılmış değil. `x:Reference` İşaretleme uzantısı ayrıca bir nesne öğesi olur:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Path="Value">
            <Binding.Source>
                <x:Reference Name="slider" />
            </Binding.Source>
        </Binding>
    </Label.Scale>
</Label>
```

Bu söz dizimi yaygın değildir, ancak karmaşık nesneler söz konusu olduğunda bazen gereklidir.

Şu ana kadar gösterilen örneklerden ayarlamak `BindingContext` özelliği ve `Source` özelliği `Binding` için bir `x:Reference` sayfasında başka bir görünüme başvurmak için işaretleme uzantısı. Bu iki özellik türlerinin `Object`, ve kaynakları bağlama için uygun olan özellikleri içeren herhangi bir nesne için ayarlanabilir.

Makalelerde devam, ayarlayabilirsiniz keşfedeceksiniz `BindingContext` veya `Source` özelliğini bir `x:Static` statik özelliği veya alanı değerini başvurmak için işaretleme uzantısı veya `StaticResource` depolanan bir nesneye başvurmak için işaretleme uzantısı bir Kaynak sözlüğü veya doğrudan bir nesneye olduğu genel (ama her zaman kullanılmaz) bir ViewModel örneği.

`BindingContext` Özelliği de ayarlanabilir bir `Binding` nesne böylece `Source` ve `Path` özelliklerini `Binding` bağlama bağlamı tanımlar.

## <a name="binding-context-inheritance"></a>Bağlama bağlamı devralma

Bu makalede, kaynak nesnesini kullanarak belirtebilirsiniz gördünüz `BindingContext` özelliği veya `Source` özelliği `Binding` nesne. Her ikisi de ayarlanırsa `Source` özelliği `Binding` önceliklidir `BindingContext`.

`BindingContext` Özellik son derece önemli bir özellik vardır:

*Ayarını `BindingContext` özelliği aracılığıyla görsel ağacın devralınır.*

Gördüğünüz gibi bu bağlama ifadeleri basitleştirmek ve bazı durumlarda çok kullanışlı olabilir &mdash; Model-View-ViewModel (MVVM) senaryolarında özellikle &mdash; da önemlidir.

**Bağlama bağlamı devralma** örnek devralma bağlama bağlamı, basit bir örnek verilmiştir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BindingContextInheritancePage"
             Title="BindingContext Inheritance">
    <StackLayout Padding="10">

        <StackLayout VerticalOptions="FillAndExpand"
                     BindingContext="{x:Reference slider}">

            <Label Text="TEXT"
                   FontSize="80"
                   HorizontalOptions="Center"
                   VerticalOptions="EndAndExpand"
                   Rotation="{Binding Value}" />

            <BoxView Color="#800000FF"
                     WidthRequest="180"
                     HeightRequest="40"
                     HorizontalOptions="Center"
                     VerticalOptions="StartAndExpand"
                     Rotation="{Binding Value}" />
        </StackLayout>

        <Slider x:Name="slider"
                Maximum="360" />

    </StackLayout>
</ContentPage>
```

`BindingContext` Özelliği `StackLayout` ayarlanır `slider` nesne. Bu bağlama içeriği tarafından devralınır `Label` ve `BoxView`, hem sahip olduğunuz, kendi `Rotation` özelliklerini ayarlamak `Value` özelliği `Slider`:

[![Bağlama bağlamı devralma](basic-bindings-images/bindingcontextinheritance-small.png "bağlama bağlamı devralma")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "bağlam devralma bağlama")

İçinde [sonraki makalede](binding-mode.md), gördüğünüz nasıl *bağlama modu* hedef ve kaynak nesneler arasındaki veri akışını değiştirebilirsiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama tanıtımları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölümden Xamarin.Forms kitabı](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
