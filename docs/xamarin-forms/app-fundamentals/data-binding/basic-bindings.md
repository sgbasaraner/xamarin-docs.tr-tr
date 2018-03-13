---
title: "Temel bağlama"
description: "Veri bağlama hedefleri, kaynakları ve bağlama bağlamı"
ms.topic: article
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: b82c0471985306962133c3bf7b084b49d5588bb6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="basic-bindings"></a>Temel bağlama

Xamarin.Forms veri bağlama özellikleri en az biri genellikle bir kullanıcı arabirimi nesnesi olan iki nesne arasındaki çifti bağlar. Bu iki nesne adında *hedef* ve *kaynak*:

- *Hedef* nesnesi (ve özellik), veri bağlama üzerinde ayarlanmıştır.
- *Kaynak* veri bağlama tarafından başvurulan nesne (ve özellik).

Bu ayrım bazen biraz kafa karıştırıcı olabilir: en basit durumda, veri akışları kaynaktan hedefe hedef özelliğinin değeri source özelliği değerinden ayarlandığı anlamına gelir. Ancak, bazı durumlarda, veri alternatif olarak hedef kaynağına veya her iki yönde akabilir. Karışıklığı önlemek için her zaman bu verileri sağlayan yerine bile, veri alma üzerinde veri bağlama ayarlanmış nesne hedefidir aklınızda bulundurun.

## <a name="bindings-with-a-binding-context"></a>Bağlama bağlamı bağlamalarla

Veri bağlama genellikle tamamen XAML'de belirtildi ancak veri bağlamaları kodu görmek için eğitici. **Temel kod bağlama** sayfa ile bir XAML dosyası içeren bir `Label` ve `Slider`:

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

`Slider` 0-360 aralığı için ayarlanır. Döndürmek için bu programı amacı olan `Label` düzenleme tarafından `Slider`.

Veri bağlamaları ayarlamalısınız `ValueChanged` olayı `Slider` erişen bir olay işleyicisi için `Value` özelliği `Slider` ve bu değeri ayarlar `Rotation` özelliği `Label`. Veri bağlama bu iş otomatik hale getirir; olay işleyicisi ve onu içindeki kod artık gerekli değildir. 

Türetilen herhangi bir sınıf örneği üzerinde bir bağlama ayarlayabilirsiniz [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), içeren `Element`, `VisualElement`, `View`, ve `View` türevleri.  Bağlamanın hedef nesnede her zaman ayarlanır. Bağlama kaynak nesnesi başvuruda bulunuyor. Veri bağlama ayarlamak için aşağıdaki iki üyeleri hedef sınıfın kullanın:

- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Özelliği, kaynak nesne belirtir.
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Yöntemi hedef özelliği ve kaynak özelliği belirtir.

Bu örnekte, `Label` bağlama hedefi ve `Slider` bağlama kaynağıdır. Değişiklikleri `Slider` kaynak etkileyen dönüşünü `Label` hedef. Verileri kaynak sunucudan hedef akar.

`SetBinding` Yöntemi tarafından tanımlanan `BindableObject` türünde bir bağımsız değişken sahip [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) içinden [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) sınıfı türetilen, ancak diğer vardır `SetBinding` yöntemleri tarafından tanımlanan [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) sınıfı. Arka plan kod dosyasına **temel kod bağlama** daha basit bir örnek kullanan [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) bu sınıftan genişletme yöntemi.

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

`Label` Nesnesi, bu özellik ayarlanır ve bu yöntem çağrılır nesne olan bağlama hedefi olduğundan. `BindingContext` Özelliği olan bağlama kaynağı gösterir `Slider`.

`SetBinding` Yöntemi bağlama hedef olarak adlandırılır, ancak hedef özelliği ve kaynak özelliği belirtir. Target özelliği olarak belirtilen bir `BindableProperty` nesne: `Label.RotationProperty`. Source özelliği bir dize olarak belirtilir ve gösterir `Value` özelliği `Slider`. 

`SetBinding` Yöntemi veri bağlamaları en önemli kurallardan biri ortaya çıkarır:

*Target özelliği bağlanabilirse özelliği tarafından yedeklenmelidir.*

Bu kural hedef nesnenin türeyen bir sınıf örneği olması anlamına gelir `BindableObject`. Bkz: [ **bağlanabilir özellikleri** ](~/xamarin-forms/xaml/bindable-properties.md) bağlanabilirse nesneleri ve bağlanabilirse özelliklerine genel bir bakış için makale.

Bir dize olarak belirtilen kaynak özelliği için bu tür bir kuralı yok. Dahili olarak, yansıma gerçek özelliğine erişmek için kullanılır. Bu örnekte, ancak `Value` özelliği de bağlanabilir özelliği tarafından yedeklenir.

Kod olabilir biraz Basitleştirilmiş: `RotationProperty` bağlanabilirse özelliği tarafından tanımlanır `VisualElement`ve tarafından devralınan `Label` ve `ContentPage` de, bu nedenle sınıf adı değil gerekli içinde `SetBinding` çağırın:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Ancak, sınıf adı dahil olmak üzere hedef nesnenin iyi bir anımsatıcı değil.

İşlemek gibi `Slider`, `Label` buna göre döndürür:

[![Basice bağlama kod](basic-bindings-images/basiccodebinding-small.png "temel kod bağlama")](basic-bindings-images/basiccodebinding-large.png#lightbox "temel kod bağlama")

**Temel Xaml bağlama** sayfasıdır aynı **temel kod bağlama** dışında tüm veri bağlama XAML'de tanımlar:

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

Yalnızca kodu olduğu gibi veri bağlamanın hedef nesne üzerinde ayarlanır `Label`. İki XAML biçimlendirme uzantıları ilgilidir. Bu kuşak sınırlayıcılar anında tanımlanabilir:

- `x:Reference` Biçimlendirme uzantısı olan kaynak nesneye başvurmak için gerekli olduğunu `Slider` adlı `slider`.
- `Binding` Biçimlendirme uzantısı bağlantıları `Rotation` özelliği `Label` için `Value` özelliği `Slider`. 

Makalesine bakın [XAML işaretleme uzantılarına](~/xamarin-forms/xaml/markup-extensions/index.md) XAML biçimlendirme uzantıları hakkında daha fazla bilgi için. `x:Reference` Biçimlendirme uzantısı tarafından desteklenen [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) sınıf; `Binding` tarafından desteklenen [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) sınıfı. XML olarak ad alanı öneklerini belirtmek, `x:Reference` XAML 2009 belirtiminin bir parçası olduğu sırada `Binding` Xamarin.Forms bir parçasıdır. Tırnak işareti içinde süslü ayraçlar görüntülendiğine dikkat edin.

Unutursanız kolaydır `x:Reference` ayarlarken biçimlendirme uzantısı `BindingContext`. Yanlışlıkla özelliğini doğrudan şöyle bağlama kaynağının adı ayarlamak için ortaktır:

```xaml
BindingContext="slider"
```

Ancak, doğru değil. Bu biçimlendirme ayarlar `BindingContext` özelliğine bir `string` karakterine yazım "kaydırıcı" nesne!

Source özelliği ile belirtilen bildirimi [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) özelliği `BindingExtension`, ile karşılık geldiği [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) özelliği [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) sınıfı.

Gösterilen biçimlendirme **temel XAML bağlama** sayfa Basitleştirilmiş: XAML biçimlendirme uzantıları gibi `x:Reference` ve `Binding` olabilir *içerik özelliği* öznitelik tanımlanmamışsa, XAML için Biçimlendirme uzantıları gelir özellik adı görünür gerekmez. `Name` İçerik özelliği bir özelliktir `x:Reference`ve `Path` içerik özelliği bir özelliktir `Binding`, bunlar gelen ifadeleri kaldırılabilir anlamına gelir:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Bağlama bağlamı olmadan bağlamaları

`BindingContext` Özellik veri bağlamaları önemli bir bileşenidir, ancak her zaman gerekli değildir. Kaynak nesnesi, bunun yerine belirtilebilir `SetBinding` çağrısı veya `Binding` biçimlendirme uzantısı.

Bu, gösterilmiştir **alternatif kod bağlama** örnek. XAML dosyası benzer **temel kod bağlama** örnek; tek fark `Slider` denetlemek için tanımlanan `Scale` özelliği `Label`. Bu nedenle, `Slider` için bir aralığı ayarlanmıştır &ndash;2 için 2:

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

Arka plan kod dosyasına bağlama ile ayarlar [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) tarafından tanımlanan yöntemi `BindableObject`. Bağımsız değişken bir [Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) için [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) sınıfı:

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

`Binding` Kurucusunun 6 parametreleri, böylece `source` parametresi, adlandırılmış bir bağımsız değişkeni ile belirtilir. Bağımsız değişken `slider` nesnesi. 

Bu programı çalıştırmayı biraz şaşırtıcı olabilir:

[![Alternatif kod bağlama](basic-bindings-images/alternativecodebinding-small.png "alternatif kod bağlama")](basic-bindings-images/alternativecodebinding-large.png#lightbox "alternatif kod bağlama")

İOS ekranın sol sayfası belirdiğinde ekran nasıl görüneceği gösterilmektedir. Burada olan `Label`? 

Sorun `Slider` 0 ilk bir değere sahip. Bu neden `Scale` özelliği `Label` de, varsayılan değer olan 1 geçersiz kılma 0 olarak ayarlanacak. Bu, sonuçlanır `Label` başlangıçta görünmez bırakılıyor. Android ve evrensel Windows Platformu (UWP) ekran görüntüleri görüldüğü gibi işleyebileceğiniz `Slider` yapmak için `Label` yeniden görünür, ancak ilk disappearance disconcerting.

İçinde öğreneceksiniz [sonraki makalede](binding-mode.md) başlatarak bu sorundan kaçınmak nasıl `Slider` varsayılan değerinden `Scale` özelliği.

**Alternatif XAML bağlama** sayfası tamamen XAML'de eşdeğer bağlama gösterir:

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

Şimdi `Binding` biçimlendirme uzantısına sahip iki özellik ayarlanırsa, `Source` ve `Path`, virgülle ayrılmış. Tercih ederseniz bunlar aynı satırda görünebilir:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` Özelliği ayarlanmış katıştırılmış için `x:Reference` Aksi takdirde aynı sözdizimi ayarı olarak sahip biçimlendirme uzantısı `BindingContext`. Tırnak işareti süslü ayraçlar içinde görünür ve iki özellik virgülle ayrılmalıdır dikkat edin.

İçerik özelliği `Binding` biçimlendirme uzantısı `Path`, ancak `Path=` biçimlendirme uzantısı parçası yalnızca ortadan ifadedeki ilk özellik ise. Ortadan kaldırmak için `Path=` bölümü, iki özellik takas vermeniz gerekir:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

XAML işaretleme uzantılarına genellikle küme ayraçları ayrılmış olsa da, nesne öğeleri da belirtilebilir:

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

Şimdi `Source` ve `Path` özelliklerdir normal XAML öznitelikleri: değerleri tırnak içine görünür ve öznitelikleri virgülle ayrılmış değil. `x:Reference` Biçimlendirme uzantısı da nesne öğesindeki hale gelir:

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

Bu sözdiziminin yaygın değildir, ancak karmaşık nesneler söz konusu olduğunda bazen gerekli değil.

Şu ana kadar gösterilen örnekler set `BindingContext` özelliği ve `Source` özelliği `Binding` için bir `x:Reference` sayfasında başka bir görünüm başvurmak için işaretleme uzantısı. Bu iki özellik türlerinin `Object`, ve kaynakları bağlama için uygun olan özellikler içeren herhangi bir nesnesi için ayarlanabilir. 

Makalelerinde şimdi, ayarlayabilirsiniz öğreneceksiniz `BindingContext` veya `Source` özelliğine bir `x:Static` bir statik özellik veya alan değerinin başvurmak için işaretleme uzantısı veya `StaticResource` depolanan bir nesneye başvurmak için işaretleme uzantısı bir Kaynak sözlüğü veya doğrudan bir nesneye olan genellikle (ancak her zaman) bir ViewModel örneği. 

`BindingContext` Özelliği de ayarlanabilir bir `Binding` nesne böylece `Source` ve `Path` özelliklerini `Binding` bağlama bağlamı tanımlayın.

## <a name="binding-context-inheritance"></a>Bağlama bağlamı devralma

Bu makalede, kaynak nesne kullanarak belirtebilirsiniz gördünüz `BindingContext` özelliği veya `Source` özelliği `Binding` nesnesi. Her ikisi de ayarlanırsa `Source` özelliği `Binding` önceliklidir `BindingContext`.

`BindingContext` Özelliği son derece önemli bir özelliği vardır:

*Ayarını `BindingContext` özelliği aracılığıyla görsel ağacın devralınır.*

Gördüğünüz gibi bu bağlama ifadeleri basitleştirme ve bazı durumlarda çok kullanışlı olabilir &mdash; Model View ViewModel (MVVM) senaryolarında özellikle &mdash; bu gereklidir.

**Bağlama bağlamı devralma** örnek bağlama bağlamı devralma basit bir örnek verilmiştir:

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

`BindingContext` Özelliği `StackLayout` ayarlanır `slider` nesnesi. Bu bağlama bağlamı her ikisi için de devralınan `Label` ve `BoxView`, her iki olan, kendi `Rotation` özelliklerini ayarlamak `Value` özelliği `Slider`: 

[![İçerik devralma bağlama](basic-bindings-images/bindingcontextinheritance-small.png "içerik devralma bağlama")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "içerik devralma bağlama")

İçinde [sonraki makalede](binding-mode.md), göreceğiniz nasıl *bağlama modu* hedef ve kaynak nesneler arasındaki veri akışını değiştirebilirsiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama gösterileri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölüm Xamarin.Forms defterinden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
