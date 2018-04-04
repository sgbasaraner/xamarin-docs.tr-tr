---
title: Bölüm 3. XAML işaretleme uzantıları
description: XAML işaretleme uzantılarına XAML'de nesneleri veya diğer kaynaklardan dolaylı olarak başvurulan değerleri ayarlamak özellikler sağlayan bir önemli özellik oluşturur. XAML işaretleme uzantılarına nesneleri paylaşımı ve bir uygulama genelinde kullanılan sabitleri başvuran için özellikle önemlidir, ancak bunların en büyük yardımcı programı veri bağlamaları buldukları.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: charlespetzold
ms.author: chape
ms.date: 3/27/2018
ms.openlocfilehash: 104a3adb5d59bc7feafa3c993290247b749ce312
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="part-3-xaml-markup-extensions"></a>Bölüm 3. XAML işaretleme uzantıları

_XAML işaretleme uzantılarına XAML'de nesneleri veya diğer kaynaklardan dolaylı olarak başvurulan değerleri ayarlamak özellikler sağlayan bir önemli özellik oluşturur. XAML işaretleme uzantılarına nesneleri paylaşımı ve bir uygulama genelinde kullanılan sabitleri başvuran için özellikle önemlidir, ancak bunların en büyük yardımcı programı veri bağlamaları buldukları._

## <a name="xaml-markup-extensions"></a>XAML işaretleme uzantıları

Genel olarak, bir dizeyi bir sayı, bir numaralandırma üyesine veya arka planda bir değere dönüştürülmüş bir dize gibi açık değerler için bir nesne özelliklerini ayarlamak için XAML kullanın.

Bazı durumlarda, ancak özellikleri yerine başka bir yerde tanımlanmış değer başvurmalıdır veya çalışma zamanında kodu tarafından az miktarda işleme gerektiren. XAML amaçlar için *biçimlendirme uzantıları* kullanılabilir.

Bu XAML biçimlendirme uzantıları XML uzantıları değildir. XAML tamamen yasal XML'dir. Kod içinde uygulayan sınıflar tarafından yedeklenen olduğundan "uzantılarla" adlandırılırlar `IMarkupExtension`. Kendi özel biçimlendirme uzantıları yazabilirsiniz.

Öznitelik ayarlarını küme ayraçları ayrılmış olarak göründüğünden çoğu durumda, XAML işaretleme uzantılarına XAML dosyaları hemen tanınabilir: {ve}, ancak bazen biçimlendirme uzantıları biçimlendirmede geleneksel öğeleri olarak görünür.

## <a name="shared-resources"></a>Paylaşılan kaynaklar

Bazı XAML sayfaları özellikleri aynı değerlere ayarlanmış olan çeşitli görünümler içerir. Örneğin, çoğu bu özellik ayarları `Button` nesneleri aynıdır:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

    </StackLayout>
</ContentPage>
```

Bu özelliklerden birini değiştirilmesi gerekiyorsa, üç kez yerine yalnızca bir kez değişiklik yapmak tercih edebilirsiniz. Bu kod olsaydı, büyük olasılıkla sabitleri ve statik salt okunur nesneler tutarlı ve kolayca değiştirmek bu değerleri tutmak için kullanırlar.

XAML'de bir popüler çözüm böyle değerlerini depolamak için veya nesneler bir *kaynak sözlüğü*. `VisualElement` Sınıfı tanımlayan adlı bir özellik `Resources` türü `ResourceDictionary`, anahtarları türü ile bir sözlük olduğu `string` ve türü değerleri `object`. Bu sözlükteki nesneleri yerleştirin ve biçimlendirmeden tüm XAML'de başvuru.

Bir sayfa üzerinde bir kaynak sözlüğü kullanmak için bir çift ekleyin `Resources` özellik öğesi etiketleri. Bu sayfanın en üstündeki koymak en kullanışlıdır:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>

    </ContentPage.Resources>
    ...
</ContentPage>
```

Açık içerecek şekilde gereklidir `ResourceDictionary` etiketler:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Şimdi nesneleri ve çeşitli türlerde değerler için kaynak sözlük eklenebilir. Bu tür instantiable olması gerekir. Soyut sınıflar, örneğin olamazlar. Bu tür ayrıca genel bir parametresiz oluşturucuya sahip olmalıdır. Her bir öğe ile belirtilen bir sözlük anahtarı gerektirir `x:Key` özniteliği. Örneğin:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Bu iki öğeyi yapısı türü değerleri `LayoutOptions`, her bir benzersiz anahtar ve bir sahip ve iki özellikleri ayarlayın. Kod ve biçimlendirme, statik alanlarını kullanmak için çok daha yaygın bir sorundur `LayoutOptions`, ancak burada özelliklerini ayarlamak daha uygun değil.

Ayarlamak gerekli şimdi `HorizontalOptions` ve `VerticalOptions` bu kaynaklar bu düğmelere özelliklerini ve ile yapılır `StaticResource` XAML biçimlendirme uzantısı:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

`StaticResource` Biçimlendirme uzantısı ile süslü ayraçlar her zaman ayrılmış ve sözlük anahtarı içerir.

Adı `StaticResource` buradan ayırt `DynamicResource`, hangi Xamarin.Forms de destekler. `DynamicResource` çalışma zamanı sırasında değişebilir değerleri ile ilişkili sözlük anahtarları ilgili olduğu sırada `StaticResource` yalnızca öğeleri sayfada ne zaman oluşturulur sonra öğeleri sözlükten erişir.

İçin `BorderWidth` özelliği, bir çift sözlükte depolamak için gereken. XAML rahat etiketler gibi ortak veri türleri için tanımlar `x:Double` ve `x:Int32`:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

        <x:Double x:Key="borderWidth">
            3
        </x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Üç satırlarında put gerek yoktur. Bu sözlük girdisi bu döndürme açısı için yalnızca bir satır yukarı alır:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

         <x:Double x:Key="borderWidth">
            3
         </x:Double>

        <x:Double x:Key="rotationAngle">-15</x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Bu iki kaynak aynı şekilde başvurulabilir `LayoutOptions` değerler:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

Türündeki kaynaklar için `Color`, doğrudan bu tür özniteliklerini atarken kullandığınız aynı dize Beyanları kullanabilirsiniz. Tür dönüştürücüleri kaynak oluşturulduğunda çağrılır. Bir kaynak türü işte `Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

Genellikle, kümesi'ni programları bir `FontSize` üyesi özelliğine `NamedSize` numaralandırma gibi `Large`. `FontSizeConverter` Sınıfı kullanarak bir platforma bağımlı değer dönüştürmek için planda çalışır `Device.GetNamedSized` yöntemi. Ancak, bir yazı tipi boyutunu kaynak tanımlarken gösterilen sayısal bir değer kullanmak için daha fazla mantıklıdır burada olarak bir `x:Double` türü:

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

Şimdi dışındaki tüm özelliklerini `Text` kaynak ayarları tarafından tanımlanır:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

Kullanmak da mümkündür `OnPlatform` platformları için farklı değerler tanımlamak için kaynak sözlüğü içinde. İşte nasıl bir `OnPlatform` nesne farklı bir metin renkler için kaynak sözlüğü parçası olabilir:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Dikkat `OnPlatform` her ikisi de alır bir `x:Key` sözlüğe bir nesne olduğundan özniteliğini ve bir `x:TypeArguments` genel bir sınıf olduğundan özniteliği. `iOS`, `Android`, Ve `UWP` öznitelikleri dönüştürülür `Color` nesne başlatıldığında değerleri.

Altı paylaşılan değerleri erişme üç düğme ile son tam XAML dosyası şöyledir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />

            <x:Double x:Key="borderWidth">3</x:Double>

            <x:Double x:Key="rotationAngle">-15</x:Double>

            <OnPlatform x:Key="textColor"
                        x:TypeArguments="Color">
                <On Platform="iOS" Value="Red" />
                <On Platform="Android" Value="Aqua" />
                <On Platform="UWP" Value="#80FF80" />
            </OnPlatform>

            <x:String x:Key="fontSize">Large</x:String>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do that!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do the other thing!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

    </StackLayout>
</ContentPage>
```

Ekran görüntüleri tutarlı stil ve platforma bağımlı stil doğrulayın:

[![](xaml-markup-extensions-images/sharedresources.png "Stilde denetimler")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "stilde denetimler")

Tanımlamak için yaygın olsa da `Resources` koleksiyonu sayfanın üstündeki göz önünde bulundurun, `Resources` özelliği tarafından tanımlanır `VisualElement`, ve bulundurabilirsiniz `Resources` diğer öğeleri sayfada koleksiyonlarında. Örneğin, bir tane için eklemeyi deneyin `StackLayout` Bu örnekte:

```xaml
<StackLayout>
    <StackLayout.Resources>
        <ResourceDictionary>
            <Color x:Key="textColor">Blue</Color>
        </ResourceDictionary>
    </StackLayout.Resources>
    ...
</StackLayout>
```

Düğmeleri metin rengi mavi olduğunu öğreneceksiniz. Temel olarak, her XAML ayrıştırıcısı karşılaştığı bir `StaticResource` biçimlendirme uzantısı görsel ağacı arar ve ilk kullanır `ResourceDictionary` bu anahtarı içeren karşılaşır.

En sık karşılaşılan kaynak sözlükte depolanan nesneler biri Xamarin.Forms `Style`, özellik ayarları koleksiyonunu tanımlar. Stilleri makalesinde açıklanan [stilleri](~/xamarin-forms/user-interface/styles/index.md).

Bazen, bir görsel öğe gibi koyabilirsiniz, geliştiriciler için XAML yeni merak ediyor `Label` veya `Button` içinde bir `ResourceDictionary`. Kötülerinden mümkün olsa da, kadar doesn't make Sense. Amacı `ResourceDictionary` nesneleri paylaşılmasıdır. Bir görsel öğe paylaşılamaz. Aynı örneği iki kez tek bir sayfada yer alamaz.

## <a name="the-xstatic-markup-extension"></a>X: Static işaretleme uzantısı

Adlarını benzerlikler rağmen `x:Static` ve `StaticResource` çok farklıdır. `StaticResource` bir kaynak sözlüğü sırasında bir nesne döndürür `x:Static` aşağıdakilerden birini erişen:

- public static alanı
- Genel statik özelliği
- Genel sabit alan 
- bir numaralandırma üyesi. 

`StaticResource` Biçimlendirme uzantısı bir kaynak sözlüğü tanımlayan XAML uygulamaları tarafından desteklenen sırada `x:Static` bir iç XAML olarak parçasıdır `x` önek ortaya çıkarır.

İşte gösteren birkaç örnek nasıl `x:Static` statik alanları ve numaralandırma üyeleri açıkça başvurabilir:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

Şu ana kadar bu çok etkileyici değil. Ancak `x:Static` biçimlendirme uzantısı de başvuru statik alanları veya özellikleri kendi koddan. Örneğin, bir `AppConstants` bir uygulama boyunca birden çok sayfaya kullanmak isteyebilirsiniz bazı statik alanları içeren sınıf:

```csharp
using System;
using Xamarin.Forms;

namespace XamlSamples
{
    static class AppConstants
    {
        public static readonly Thickness PagePadding;

        public static readonly Font TitleFont;

        public static readonly Color BackgroundColor = Color.Aqua;

        public static readonly Color ForegroundColor = Color.Brown;

        static AppConstants()
        {
            switch (Device.RuntimePlatform)
            {
                case Device.iOS:
                    PagePadding = new Thickness(5, 20, 5, 0);
                    TitleFont = Font.SystemFontOfSize(35, FontAttributes.Bold);
                    break;

                case Device.Android:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(40, FontAttributes.Bold);
                    break;
                    
                case Device.UWP:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(50, FontAttributes.Bold);
                    break;
            }
        }
    }
}
```

XAML dosyasındaki bu sınıfın statik alanları başvurmak için bu dosyanın bulunduğu XAML dosyası içinde göstermek için bazı yol gerekir. Bu bir XML ad alanı bildirimi ile yapın.

Standart Xamarin.Forms XAML şablonunun parçası olarak oluşturulan XAML dosyaları iki XML ad alanı bildirimleri içeren geri çağırma: bir Xamarin.Forms sınıfları ve etiketleri ve öznitelikleri için XAML iç başvuran için başka bir erişmek için:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Diğer sınıflara erişmek için ek XML ad alanı bildirimleri gerekir. Her ek XML ad alanı bildirimi yeni bir önek tanımlar. Paylaşılan uygulama PCL, yerel sınıflara gibi erişmek için `AppConstants`, sık kullandığınız öneki XAML programcıları `local`. Ad alanı bildirimi CLR (ortak dil çalışma zamanı) ad alanı adı, bir C# dilinde görünen adı olarak da bilinen .NET ad alanı adı belirtmeniz gerekir `namespace` tanımı veya bir `using` yönergesi:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Bu gibi durumlarda, .NET ad alanları için XML ad alanı bildirimleri de PCL başvuran hiçbir derleme tanımlayabilirsiniz. Örneğin, bir `sys` standart .NET için önek `System` içinde ad alanı **mscorlib** kez "Microsoft ortak nesne çalışma zamanı kitaplığı için" stood, ancak şimdi "çoklu dil standart anlamına gelir, derleme Genel nesne çalışma zamanı kitaplığı." Bu başka bir derleme olduğundan, ayrıca derleme adı, bu durumda belirtmeniz gerekir **mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Dikkat anahtar sözcüğü `clr-namespace` bir iki nokta üst üste ve ardından anahtar sözcüğü bir noktalı virgül ekleyin .NET ad alanı adı tarafından izlenen `assembly`, eşittir işareti ve derleme adı.

Evet, izleyen iki nokta `clr-namespace` ancak eşittir işaretinden `assembly`. Sözdizimi bu şekilde kasıtlı olarak tanımlandı: en XML ad alanı bildirimleri başvuru URI düzeni adı gibi başlayan bir URI `http`, hangi her zaman ardından iki nokta ile. `clr-namespace` Bu dize bir parçası, bu kuralı taklit etmek üzere tasarlanmıştır.

Bu iki ad alanı bildirimleri içinde yer alan **StaticConstantsPage** örnek. Dikkat `BoxView` boyutları ayarlandığında `Math.PI` ve `Math.E`, ancak ölçeklendirilmiş 100 faktörüyle:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.StaticConstantsPage"
             Title="Static Constants Page"
             Padding="{x:Static local:AppConstants.PagePadding}">

    <StackLayout>
       <Label Text="Hello, XAML!"
              TextColor="{x:Static local:AppConstants.BackgroundColor}"
              BackgroundColor="{x:Static local:AppConstants.ForegroundColor}"
              Font="{x:Static local:AppConstants.TitleFont}"
              HorizontalOptions="Center" />

      <BoxView WidthRequest="{x:Static sys:Math.PI}"
               HeightRequest="{x:Static sys:Math.E}"
               Color="{x:Static local:AppConstants.ForegroundColor}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="100" />
    </StackLayout>
</ContentPage>
```

Sonuç boyutunu `BoxView` platforma bağımlı ekrandır göre:

 [![](xaml-markup-extensions-images/staticconstants.png "X: Static işaretleme uzantısı kullanarak denetimleri")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "x: Static işaretleme uzantısı kullanarak denetimleri")

## <a name="other-standard-markup-extensions"></a>Diğer standart biçimlendirme uzantıları

Birçok biçimlendirme uzantıları için XAML iç ve Xamarin.Forms XAML dosyaları desteklenir. Bunlardan bazıları, çok sık kullanılmayan ancak gereksinim duyduğunuzda gereklidir:

-  Bir özellik olmayan bir varsa `null` varsayılan ancak değeriyle istediğiniz ayarlamak için `null`, ayarlamak `{x:Null}` biçimlendirme uzantısı.
-  Bir özellik türü ise `Type`, kendisine atadığınız bir `Type` biçimlendirme uzantısı kullanılarak nesne `{x:Type someClass}`.
-  XAML kullanarak dizileri tanımlayabilirsiniz `x:Array` biçimlendirme uzantısı. Bu biçimlendirme uzantısı adlı gerekli bir özniteliği var `Type` dizideki öğeler türünü gösterir.
- `Binding` Biçimlendirme uzantısı ele alınmıştır [bölümü 4. Veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="the-constraintexpression-markup-extension"></a>ConstraintExpression biçimlendirme uzantısı

Biçimlendirme uzantıları özelliklere sahip olabilir, ancak XML öznitelikleri gibi ayarlı değil. İşaretleme uzantısı özellik ayarları virgülle ayrılır ve kaşlı ayraçlar içinde tırnak işareti görünür.

Bu adlı Xamarin.Forms biçimlendirme uzantısı ile gösterilebilir `ConstraintExpression`, ile kullanılan `RelativeLayout` sınıfı. Bir sabit olarak veya bir üst veya diğer adlandırılmış bir görünümü göreli konum veya alt görünüm boyutunu belirtebilirsiniz. Söz dizimi `ConstraintExpression` konum veya görünümü kullanarak boyutunu ayarladığınız sağlayan bir `Factor` başka bir görünüm özelliğinin bir artı bir kez `Constant`. Daha karmaşık bir şey kodu gerektirir.

Örnek aşağıda verilmiştir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.RelativeLayoutPage"
             Title="RelativeLayout Page">

    <RelativeLayout>

        <!-- Upper left -->
        <BoxView Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Upper right -->
        <BoxView Color="Green"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Lower left -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />
        <!-- Lower right -->
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"  />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=X}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Y}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Height,
                                            Factor=0.33}"  />
    </RelativeLayout>
</ContentPage>
```

Belki de bu örnekten aldığınız en önemli Ders biçimlendirme uzantısı sözdizimi şöyledir: tırnak işareti biçimlendirme uzantısı süslü ayraçlar içinde görünmesi gerekir. İşaretleme uzantısı bir XAML dosyasına yazarken özelliklerin değerlerine tırnak işaretleri içine alın isteyebilirsiniz. Buradaki eðilim kaçının!

Çalışan program şöyledir:

[![](xaml-markup-extensions-images/relativelayout.png "Kısıtlamaları kullanarak göreli Düzen")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "göreli kısıtlamaları kullanarak Düzen")

## <a name="summary"></a>Özet

Burada gösterilen XAML biçimlendirme uzantıları için XAML dosyaları önemli destek sağlar. Ancak belki de en değerli XAML biçimlendirme uzantısı `Binding`, bu serinin sonraki bölümünde ele [bölümü 4. Veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).



## <a name="related-links"></a>İlgili bağlantılar

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Bölüm 1. XAML Kullanmaya Başlarken](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Bölüm 2. Temel XAML Sözdizimi](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Bölüm 4. Temel Veri Bağlama Bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Bölüm 5. Verileri için MVVM bağlama](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
