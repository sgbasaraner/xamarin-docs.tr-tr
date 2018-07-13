---
title: XAML biçimlendirme uzantıları oluşturma
description: Bu makalede, kendi özel Xamarin.Forms XAML biçimlendirme uzantıları tanımlanacağı açıklanmaktadır. XAML işaretleme uzantısı IMarkupExtension IMarkupExtension arabirimi uygulayan bir sınıftır.
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d4b3d5c65ddf8be433d1f8e182774aa839f60357
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995602"
---
# <a name="creating-xaml-markup-extensions"></a>XAML biçimlendirme uzantıları oluşturma

Programlı düzeyi, XAML işaretleme uzantısı uygulayan bir sınıf olan [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) veya [ `IMarkupExtension<T>` ](xref:Xamarin.Forms.Xaml.IMarkupExtension`1) arabirimi. Aşağıda açıklanan standart biçimlendirme uzantıları kaynak kodunu keşfedebilirsiniz [ **Markupextension'lar** dizin](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) Xamarin.Forms GitHub deposunun.

Kendi özel XAML biçimlendirme uzantıları türeterek tanımlamak da mümkündür `IMarkupExtension` veya `IMarkupExtension<T>`. İşaretleme uzantısı, belirli bir türün bir değeri alır, genel form kullanın. Bu, birkaç Xamarin.Forms biçimlendirme uzantıları ile durumdur:

- `TypeExtension` öğesinden türetilen `IMarkupExtension<Type>`
- `ArrayExtension` öğesinden türetilen `IMarkupExtension<Array>`
- `DynamicResourceExtension` öğesinden türetilen `IMarkupExtension<DynamicResource>`
- `BindingExtension` öğesinden türetilen `IMarkupExtension<BindingBase>`
- `ConstraintExpression` öğesinden türetilen `IMarkupExtension<Constraint>`

İki `IMarkupExtension` arabirimleri her biri tek bir yöntemi tanımlamak adlı `ProvideValue`:

```csharp
public interface IMarkupExtension
{
    object ProvideValue(IServiceProvider serviceProvider);
}

public interface IMarkupExtension<out T> : IMarkupExtension
{
    new T ProvideValue(IServiceProvider serviceProvider);
}
```

Bu yana `IMarkupExtension<T>` türetildiği `IMarkupExtension` ve içerir `new` anahtar sözcüğü, `ProvideValue`, her ikisini birden içerdiği `ProvideValue` yöntemleri.

Çok sık, XAML biçimlendirme uzantıları katkıda özellikleri için dönüş değerini tanımlayın. (Açık özel durum `NullExtension`, hangi `ProvideValue` yalnızca döndürür `null`.) `ProvideValue` Yöntemi sahip tek bir bağımsız değişken türü `IServiceProvider` , açıklanır bu makalenin sonraki bölümlerinde.

## <a name="a-markup-extension-for-specifying-color"></a>Renk belirtmeye yönelik bir işaretleme uzantısı

Oluşturmak aşağıdaki XAML işaretleme uzantısı sağlar bir `Color` ton, Doygunluk ve parlaklık bileşenlerini kullanarak değer. Bu dört bileşenin her renk 1 için başlatılmış olan bir alfa bileşeni de dahil olmak üzere dört özelliklerini tanımlar. Bu sınıfın türetildiği `IMarkupExtension<Color>` belirtmek için bir `Color` dönüş değeri:

```csharp
public class HslColorExtension : IMarkupExtension<Color>
{
    public double H { set; get; }

    public double S { set; get; }

    public double L { set; get; }

    public double A { set; get; } = 1.0;

    public Color ProvideValue(IServiceProvider serviceProvider)
    {
        return Color.FromHsla(H, S, L, A);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<Color>).ProvideValue(serviceProvider);
    }
}
```

Çünkü `IMarkupExtension<T>` türetildiği `IMarkupExtension`, iki sınıf içermelidir `ProvideValue` yöntemleri, döndüren bir `Color` döndüren başka `object`, ancak ikinci yöntem, sadece ilk yöntem çağırabilirsiniz.

**HSL renk tanıtım** çeşitli şekillerde sayfası gösterilmektedir `HslColorExtension` rengini belirlemek için bir XAML dosyasında görünür bir `BoxView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.HslColorDemoPage"
             Title="HSL Color Demo">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="WidthRequest" Value="80" />
                <Setter Property="HeightRequest" Value="80" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <BoxView>
            <BoxView.Color>
                <local:HslColorExtension H="0" S="1" L="0.5" A="1" />
            </BoxView.Color>
        </BoxView>

        <BoxView>
            <BoxView.Color>
                <local:HslColor H="0.33" S="1" L="0.5" />
            </BoxView.Color>
        </BoxView>

        <BoxView Color="{local:HslColorExtension H=0.67, S=1, L=0.5}" />

        <BoxView Color="{local:HslColor H=0, S=0, L=0.5}" />

        <BoxView Color="{local:HslColor A=0.5}" />
    </StackLayout>
</ContentPage>
```

Fark olduğunda `HslColorExtension` XML etiket, dört özellik öznitelikleri ayarlanır, ancak kaşlı ayraçlar göründüğünde, dört özellik tırnak işaretleri olmadan virgülle ayrılır. İçin varsayılan değerler `H`, `S`, ve `L` 0 ve varsayılan değerini `A` 1 olduğundan, bu özellikler için varsayılan değerleri ayarlamak istiyorsanız atlanmış olabilir. Son burada parlaklık normalde siyah olur, 0 ' dır ancak yarı saydamdır ve görünür 0,5, alfa kanalı olduğundan bir örnek gösterilir gri, beyaz arka plan sayfa karşı:

[![HSL renk tanıtım](creating-images/hslcolordemo-small.png "HSL renk tanıtım")](creating-images/hslcolordemo-large.png#lightbox "HSL renk Tanıtımı")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>Bit eşlemler erişmek için bir işaretleme uzantısı

Bağımsız değişkeni `ProvideValue` uygulayan bir nesnedir [ `IServiceProvider` ](xref:System.IServiceProvider) .NET içinde tanımlanan arabirimi `System` ad alanı. Bu arabirim bir üye, adında bir yöntemi olan `GetService` ile bir `Type` bağımsız değişken.

`ImageResourceExtension` Aşağıda gösterilen bir olası kullanımını gösterir `IServiceProvider` ve `GetService` almak için bir `IXmlLineInfoProvider` belirten burada belirli bir hata algılandı satır ve karakter bilgiler sağlayan nesne. Bu durumda, bir özel durum oluşturulur, `Source` özelliği ayarlanmamış:

```csharp
[ContentProperty("Source")]
class ImageResourceExtension : IMarkupExtension<ImageSource>
{
    public string Source { set; get; }

    public ImageSource ProvideValue(IServiceProvider serviceProvider)
    {
        if (String.IsNullOrEmpty(Source))
        {
            IXmlLineInfoProvider lineInfoProvider = serviceProvider.GetService(typeof(IXmlLineInfoProvider)) as IXmlLineInfoProvider;
            IXmlLineInfo lineInfo = (lineInfoProvider != null) ? lineInfoProvider.XmlLineInfo : new XmlLineInfo();
            throw new XamlParseException("ImageResourceExtension requires Source property to be set", lineInfo);
        }

        string assemblyName = GetType().GetTypeInfo().Assembly.GetName().Name;

        return ImageSource.FromResource(assemblyName + "." + Source);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension` bir XAML dosyası katıştırılmış bir kaynağı .NET Standard kitaplığı projesi olarak depolanan bir resim dosyasına erişmek gerektiğinde yararlıdır. Kullandığı `Source` statik çağırmak için özellik `ImageSource.FromResource` yöntemi. Bu yöntem, derleme adı, klasör adı ve dosya adı noktalarla ayrılmış oluşan bir tam kaynak adı gerektirir. `ImageResourceExtension` Yansıma kullanarak derleme adını alır ve ona ekler derleme bölümü ismi değil `Source` özelliği. Ne olursa olsun, `ImageSource.FromResource` görüntüleri de bu kitaplıkta olmadığı sürece bu XAML kaynak uzantısı bir harici kitaplık parçası olamayacağı anlamına gelir bit eşlem içeren derleme çağrılmalıdır. (Bkz [ **katıştırılmış görüntüler** ](~/xamarin-forms/user-interface/images.md#embedded_images) gömülü kaynak depolanan bir bit eşlemler erişme hakkında daha fazla bilgi için makalenin.)

Ancak `ImageResourceExtension` gerektirir `Source` ayarlamak için özellik `Source` özelliği öznitelik sınıfının içerik özelliği belirtilir. Diğer bir deyişle `Source=` küme ayraçları ifadenin atlanabilir. İçinde **görüntü kaynak tanıtım** sayfasında `Image` öğeleri klasör adı noktalarla ayrılmış dosya adı ile iki görüntü getirilemedi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.ImageResourceDemoPage"
             Title="Image Resource Demo">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Image Source="{local:ImageResource Images.SeatedMonkey.jpg}"
               Grid.Row="0" />

        <Image Source="{local:ImageResource Images.FacePalm.jpg}"
               Grid.Row="1" />

    </Grid>
</ContentPage>
```

Üç tüm platformlarda çalışan bir program şöyledir:

[![Görüntü kaynağı tanıtım](creating-images/imageresourcedemo-small.png "görüntü kaynak tanıtım")](creating-images/imageresourcedemo-large.png#lightbox "görüntü kaynak Tanıtımı")

## <a name="service-providers"></a>Hizmet sağlayıcıları

Kullanarak `IServiceProvider` bağımsız değişkeni `ProvideValue`, XAML biçimlendirme uzantıları, bunlar kullanılan XAML dosyası hakkında yararlı bilgiler erişim elde. Ancak kullanılacak `IServiceProvider` bağımsız değişkeni başarıyla belirli bağlamlarda hizmetler ne tür kullanılabilir bilmeniz gerekir. Bu özelliğin anlamak için en iyi yolu mevcut XAML biçimlendirme uzantıları kaynak kodunu incelemek tarafından olan [ **Markupextension'lar** klasör](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) github'da Xamarin.Forms depodaki. Bazı hizmet türleri için Xamarin.Forms iç olduğunu unutmayın.

Bu hizmet bazı XAML biçimlendirme uzantıları yararlı olabilir:

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget` Arabirimi tanımlayan iki özellik `TargetObject` ve `TargetProperty`. Ne zaman bu bilgileri elde edilir `ImageResourceExtension` sınıfı `TargetObject` olduğu `Image` ve `TargetProperty` olduğu bir `BindableProperty` nesnesi `Source` özelliği `Image`. XAML işaretleme uzantısı ayarlandı özelliğidir.

`GetService` Bağımsız değişkeninin çağrısıyla `typeof(IProvideValueTarget)` gerçekten türünde bir nesne döndürür `SimpleValueTargetProvider`, tanımlanan `Xamarin.Forms.Xaml.Internals` ad alanı. Dönüş değeri türüne, `GetService` o tür için de erişebilirsiniz bir `ParentObjects` içeren bir dizi özelliğinin `Image` öğesi `Grid` üst ve `ImageResourceDemoPage` üst `Grid`.

## <a name="conclusion"></a>Sonuç

XAML biçimlendirme uzantıları, çeşitli kaynaklardan öznitelikleri ayarlama özelliğini genişleterek, XAML içinde önemli bir rol oynar. Mevcut XAML biçimlendirme uzantıları tam olarak ihtiyacınız sağlamazsanız, ayrıca, ayrıca kendi yazabilirsiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [Biçimlendirme uzantıları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [XAML biçimlendirme uzantıları Xamarin.Forms kitabı bölümden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
