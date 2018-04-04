---
title: XAML biçimlendirme uzantıları oluşturma
description: Kendi özel XAML biçimlendirme uzantıları tanımlayın
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d4ae3b42c5c926749310da6e36b6f4e9754d398c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-xaml-markup-extensions"></a>XAML biçimlendirme uzantıları oluşturma

Programsal düzeyi, XAML biçimlendirme uzantısı uygulayan sınıftır [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) veya [ `IMarkupExtension<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension%3CT%3E/) arabirimi. Aşağıda açıklanan standart biçimlendirme uzantıları kaynak kodunu keşfedebilirsiniz [ **MarkupExtensions** directory](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) Xamarin.Forms GitHub depo. 

Türetme tarafından kendi özel XAML biçimlendirme uzantıları tanımlamak mümkündür `IMarkupExtension` veya `IMarkupExtension<T>`. Belirli bir tür değeri biçimlendirme uzantısı elde ederse genel formu kullanın. Bu, birkaç Xamarin.Forms biçimlendirme uzantıları ile durumdur:

- `TypeExtension` türetilen `IMarkupExtension<Type>`
- `ArrayExtension` türetilen `IMarkupExtension<Array>`
- `DynamicResourceExtension` türetilen `IMarkupExtension<DynamicResource>`
- `BindingExtension` türetilen `IMarkupExtension<BindingBase>`
- `ConstraintExpression` türetilen `IMarkupExtension<Constraint>`

İki `IMarkupExtension` arabirimleri her, yalnızca bir yöntemi tanımlamak adlı `ProvideValue`: 

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

Bu yana `IMarkupExtension<T>` türetilen `IMarkupExtension` ve içerir `new` on anahtar sözcüğü `ProvideValue`, her ikisini birden içerdiği `ProvideValue` yöntemleri.

Sıklıkla, XAML işaretleme uzantılarına katkıda özellikleri için dönüş değerini tanımlayın. (Belirgin özel durum `NullExtension`, burada `ProvideValue` yalnızca döndürür `null`.) `ProvideValue` Yöntemi sahip tek bir bağımsız değişken türü `IServiceProvider` , açıklanır bu makalenin sonraki bölümlerinde.

## <a name="a-markup-extension-for-specifying-color"></a>Renk belirtmek için biçimlendirme uzantısı

Aşağıdaki XAML biçimlendirme uzantısı oluşturmanıza olanak sağlayan bir `Color` ton, Doygunluk ve parlaklığını Bileşenleri'ni kullanarak değer. 1 ile başlatılmış bir alfa bileşeni de dahil olmak üzere rengi, dört bileşenleri için dört özelliklerini tanımlar. Sınıfın türetildiği `IMarkupExtension<Color>` belirtmek için bir `Color` dönüş değeri:

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

Çünkü `IMarkupExtension<T>` türetilen `IMarkupExtension`, sınıf iki içermelidir. `ProvideValue` yöntemleri, döndüren bir `Color` diğeri döndüren `object`, ancak ikinci yöntem yalnızca ilk yöntemini çağırabilirsiniz.

**HSL renk Demo** sayfası, çeşitli yollarla gösterir `HslColorExtension` rengini belirtmek için XAML dosyası görünebilir bir `BoxView`:

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

Fark olduğunda `HslColorExtension` bir XML etiket, dört özellikleri öznitelik olarak ayarlanmış, ancak süslü ayraçlar arasında görüntülendiğinde, dört özellikleri tırnak işaretleri olmadan virgülle ayrılır. İçin varsayılan değerler `H`, `S`, ve `L` 0 ve varsayılan değerini `A` için varsayılan değerleri ayarlamak istiyorsanız bu özellikleri atlanabilir için 1 ' dir. Son burada parlaklığını normalde siyah sonuçları, 0, ancak yarı saydam ve görünür alfa kanal 0,5, bir örnek gösterilir sayfasının beyaz arka karşı gri:

[![HSL renk Demo](creating-images/hslcolordemo-small.png "HSL renk Demo")](creating-images/hslcolordemo-large.png#lightbox "HSL renk Tanıtımı")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>Bit eşlemler erişmek için biçimlendirme uzantısı

Bağımsız değişkeni `ProvideValue` uygulayan bir nesne [ `IServiceProvider` ](https://developer.xamarin.com/api/type/System.IServiceProvider/) .NET içinde tanımlanan arabirimi `System` ad alanı. Bu arabirim bir üye, adlandırılmış bir yöntem sahip `GetService` ile bir `Type` bağımsız değişkeni. 

`ImageResourceExtension` Aşağıda gösterilen sınıfı gösterir bir olası kullanımını `IServiceProvider` ve `GetService` elde etmek için bir `IXmlLineInfoProvider` burada belirli bir hata algılandı gösteren satır ve karakter bilgiler sağlayabilir nesne. Bu durumda, özel durum oluşturuldu olduğunda `Source` özelliği ayarlanmamış:

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

`ImageResourceExtension` Taşınabilir sınıf kitaplığı proje katıştırılmış bir kaynağı olarak saklanan bir görüntü dosyasına erişmek XAML dosyası ihtiyacı olduğunda yararlıdır. Kullandığı `Source` statik çağırmak için özellik `ImageSource.FromResource` yöntemi. Bu yöntem, derleme adı, klasör adı ve noktalarla ayrılmış filename oluşan bir tam nitelikli kaynak adı gerektirir. `ImageResourceExtension` Yansıma kullanarak derleme adını alır ve kendisine başına çünkü derleme bölümü adı olmayan `Source` özelliği. Ne olursa olsun, `ImageSource.FromResource` görüntüleri de bu kitaplıkta olmadığı sürece bu XAML kaynak uzantısı harici bir kitaplığı parçası olması anlamına gelir bit eşlem'i içeren derlemenin çağrılmalıdır. (Bkz [ **katıştırılmış görüntüler** ](~/xamarin-forms/user-interface/images.md#embedded_images) katıştırılmış kaynaklar olarak depolanan bit eşlemler erişme hakkında daha fazla bilgi için makalenin.) 

Ancak `ImageResourceExtension` gerektirir `Source` ayarlanacak, özelliği `Source` özelliği bir öznitelikte sınıfın içerik özelliği belirtilir. Bunun anlamı `Source=` süslü ayraçlar ifade parçası etmeyebilirsiniz. İçinde **görüntü kaynak Demo** sayfasında `Image` öğeleri fetch klasör adı ve noktalarla ayrılmış dosya adını kullanarak iki görüntü:

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

Aşağıda, tüm üç platformlarında çalışan program verilmiştir:

[![Görüntü kaynağı Demo](creating-images/imageresourcedemo-small.png "görüntü kaynak Demo")](creating-images/imageresourcedemo-large.png#lightbox "görüntü kaynak Tanıtımı")

## <a name="service-providers"></a>Hizmet sağlayıcıları

Kullanarak `IServiceProvider` bağımsız değişkeni `ProvideValue`, XAML işaretleme uzantılarına erişimi, bunlar kullanılan XAML dosyası hakkında yararlı bilgiler alabilirsiniz. Ancak kullanmak için `IServiceProvider` bağımsız değişkeni başarıyla, ne tür bir Hizmetleri belirli bağlamlarında kullanılabilir bilmeniz gerekir. Bu özelliğin anlamak için en iyi varolan XAML işaretleme uzantılarında kaynak kodunu kavramlarını tarafından yoludur [ **MarkupExtensions** klasörü](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) github'da Xamarin.Forms deposunda. Bazı türlerdeki Hizmetleri için Xamarin.Forms iç olduğunu unutmayın.

İçindeki bazı XAML biçimlendirme uzantıları, bu hizmet yararlı olabilir:

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget` Arabirimi tanımlar iki özellik `TargetObject` ve `TargetProperty`. Ne zaman bu bilgileri elde içinde `ImageResourceExtension` sınıfı, `TargetObject` olan `Image` ve `TargetProperty` olan bir `BindableProperty` için nesne `Source` özelliği `Image`. XAML biçimlendirme uzantısı ayarlanmış özelliğidir.

`GetService` Bağımsız değişkeninin çağrısıyla `typeof(IProvideValueTarget)` gerçekten türünde bir nesne döndürür `SimpleValueTargetProvider`, içinde tanımlanan `Xamarin.Forms.Xaml.Internals` ad alanı. Dönüş değerini cast varsa `GetService` bu tür için de erişebilirsiniz bir `ParentObjects` içeren bir dizi özellik `Image` öğesi, `Grid` , üst ve `ImageResourceDemoPage` üst `Grid`.

## <a name="conclusion"></a>Sonuç

XAML işaretleme uzantılarına, çeşitli kaynaklardan özniteliklerini ayarlama özelliği genişleterek, XAML'de önemli bir rol oynar. Varolan XAML biçimlendirme uzantıları tam olarak gerekenler sağlamazsanız, ayrıca, ayrıca kendi yazabilirsiniz. 


## <a name="related-links"></a>İlgili bağlantılar

- [Biçimlendirme uzantıları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [XAML biçimlendirme uzantıları bölüm Xamarin.Forms defterinden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
