---
title: 3. bölümü. XAML biçimlendirme uzantıları
description: XAML biçimlendirme uzantıları nesneleri veya diğer kaynaklardan dolaylı olarak başvurulan değerleri ayarlamak özellikler sağlayan XAML içinde önemli bir özellik oluşturur.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: charlespetzold
ms.author: chape
ms.date: 3/27/2018
ms.openlocfilehash: 6fcb051d2c24c7da169106b06ad5ebfc91edafa6
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935623"
---
# <a name="part-3-xaml-markup-extensions"></a>3. bölümü. XAML biçimlendirme uzantıları

_XAML biçimlendirme uzantıları nesneleri veya diğer kaynaklardan dolaylı olarak başvurulan değerleri ayarlamak özellikler sağlayan XAML içinde önemli bir özellik oluşturur. XAML biçimlendirme uzantıları nesneleri paylaşma ve bir uygulamanın tamamında kullanılan sabit başvuran özellikle önemlidir, ancak bunlar veri bağlamalara kendi büyük yardımcı programını bulun._

## <a name="xaml-markup-extensions"></a>XAML biçimlendirme uzantıları

Genel olarak, XAML, bir dize, sayı, bir numaralandırma üyesine veya arka planda bir değere dönüştürülmüş bir dize gibi açık değerler için bir nesnenin özelliklerini ayarlamak için kullanın.

Bazı durumlarda, ancak, özellikleri, bunun yerine başka bir yerde tanımlanmış değer başvurmalıdır veya az miktarda işleme, çalışma zamanında kodu tarafından gerektirebilir. Bu amaçla, XAML *biçimlendirme uzantıları* kullanılabilir.

Bu XAML biçimlendirme uzantıları XML uzantıları değildir. XAML tamamen yasal XML'dir. Uygulayan sınıflar içindeki kod tarafından desteklenen olduğundan "uzantıları" adlandırılırlar `IMarkupExtension`. Kendi özel biçimlendirme uzantıları yazabilirsiniz.

Küme ayraçları ayrılmış özniteliği ayarları olarak göründüğünden çoğu durumda, XAML biçimlendirme uzantıları XAML dosyalarında anında tanınmıyor: {ve}, ancak bazen biçimlendirme uzantıları işaretlemede geleneksel öğeleri olarak görünür.

## <a name="shared-resources"></a>Paylaşılan kaynaklar

Bazı XAML sayfaları birkaç görünüm ile aynı özellikleri içerir. Örneğin, birçok özellik ayarlarının bu `Button` nesneleri aynıdır:

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

Bu özelliklerden birini değiştirilmesi gerekiyorsa, üç kez yerine yalnızca bir kez değişiklik yapmak tercih edebilirsiniz. Bu kod mevcutsa, büyük olasılıkla sabitleri ve statik salt okunur nesneler tutarlı ve kolayca değiştirmek bu değerleri tutmak için kullanırlar.

XAML, popüler çözümlerden biri, bu değerleri depolamak için veya nesneler bir *kaynak sözlüğü*. `VisualElement` Sınıfı tanımlar adlı bir özellik `Resources` türü `ResourceDictionary`, anahtarları türü ile bir sözlük olduğu `string` ve türü değerlerinin `object`. Bu sözlük içine nesneleri yerleştirin ve tüm XAML biçimlendirme bunları başvuru.

Bir sayfada bir kaynak sözlüğü kullanma için bir çift ekleyin. `Resources` özellik öğesi etiketleri. Bu sayfanın üst kısmındaki koymak oldukça kullanışlıdır:

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

Açıkça içerecek şekilde gereklidir `ResourceDictionary` etiketler:

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

Şimdi nesneleri ve çeşitli türlerde değerler için kaynak sözlük eklenebilir. Bu tür instantiable olması gerekir. Örneğin, soyut sınıflar olamazlar. Bu türler de genel parametresiz oluşturucusu olmalıdır. Her bir öğe ile belirtilen bir sözlük anahtarı gerektirir `x:Key` özniteliği. Örneğin:

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

Bu iki öğe yapı türü değerleri `LayoutOptions`ve her bir benzersiz anahtar ve bir sahip veya bu iki özelliği ayarlayın. Kod ve biçimlendirme, statik alanları kullanmak için çok daha yaygındır `LayoutOptions`, ancak burada özelliklerini ayarlamak daha uygun olan.

Ayarlamak gerekli olan artık `HorizontalOptions` ve `VerticalOptions` bu kaynaklara bu düğmeler özelliklerini ve ile yapılır `StaticResource` XAML işaretleme uzantısı:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

`StaticResource` İşaretleme uzantısı her zaman küme ayracı ile sınırlandırılır ve sözlük anahtarı içerir.

Adı `StaticResource` ondan ayıran `DynamicResource`, hangi Xamarin.Forms da destekler. `DynamicResource` çalışma zamanı sırasında değişebilir değerler ile ilişkili sözlük anahtarları için sırada `StaticResource` yalnızca zaman öğeleri sayfada oluşturulduktan sonra öğeleri sözlükten erişir.

İçin `BorderWidth` özelliği olduğu bir çift sözlükte depolamak gerekli. XAML rahatça etiketler gibi ortak veri türleri için tanımlar `x:Double` ve `x:Int32`:

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

Üç satırlara yerleştirmek gerek yoktur. Bu sözlük girişi bu döndürme açısı için yalnızca bir satırlık yer kaplar:

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

Bu iki kaynak aynı şekilde başvurulabilir `LayoutOptions` değerleri:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

Kaynak türü için `Color`, doğrudan bu tür öznitelikleri atarken kullandığınız aynı dize gösterimleri kullanabilirsiniz. Tür dönüştürücüleri kaynak oluşturulduğunda çağrılır. Bir kaynak türü şu şekildedir `Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

Genellikle, kümesi programlar bir `FontSize` üyesi özelliğini `NamedSize` numaralandırma gibi `Large`. `FontSizeConverter` Sınıfını kullanarak bir platforma bağımlı değerini dönüştürmek için arka planda çalışır `Device.GetNamedSized` yöntemi. Ancak, bir yazı tipi boyutu kaynak tanımlarken, gösterilen bir sayısal değer kullanmak için daha fazla mantıklıdır olarak burada bir `x:Double` türü:

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

Artık tüm özellikleri dışında `Text` kaynak ayarları tarafından tanımlanır:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

Kullanmak da mümkündür `OnPlatform` platformları için farklı değerler tanımlamak üzere kaynak sözlüğünün içinde. İşte nasıl bir `OnPlatform` nesne kaynak sözlüğü farklı metin rengi için bir parçası olabilir:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Dikkat `OnPlatform` hem alır bir `x:Key` sözlüğe bir nesne olduğundan özniteliğini ve bir `x:TypeArguments` genel bir sınıf olmadığından öznitelik. `iOS`, `Android`, Ve `UWP` öznitelikleri dönüştürülür `Color` nesne başlatıldığında değerleri.

Üç düğme altı paylaşılan değerlerine erişim ile son tam XAML dosyası aşağıda verilmiştir:

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

Ekran görüntüleri, tutarlı bir stil ve platforma bağımlı stil doğrulayın:

[![](xaml-markup-extensions-images/sharedresources.png "Denetimler")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "denetimler")

Tanımlamak için yaygın olsa da `Resources` koleksiyonu sayfanın üst kısmındaki aklınızda tutun, `Resources` özelliği tarafından tanımlanan `VisualElement`, ve sahip olduğunuz `Resources` sayfadaki diğer öğeleri koleksiyonlarda. Örneğin, bir ekleme deneyin `StackLayout` Bu örnekte:

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

Düğmeleri metin rengini Mavi olduğunu keşfedeceksiniz. Temelde, XAML ayrıştırıcı olduğunda karşılaştığında bir `StaticResource` işaretleme uzantısı görsel ağacı arar ve ilk kullanır `ResourceDictionary` bu anahtarı içeren karşılaşır.

En sık karşılaşılan kaynak sözlükleri içinde depolanan nesneleri biri Xamarin.Forms `Style`, özelliği bir ayarlar koleksiyonu tanımlar. Stilleri makalesinde açıklanan [stilleri](~/xamarin-forms/user-interface/styles/index.md).

Bazen, bir görsel öğe gibi koyabilirsiniz, geliştiriciler için XAML yeni merak ediyor `Label` veya `Button` içinde bir `ResourceDictionary`. Elbette mümkün olsa da, bu çok mantıklı değildir. Amacı `ResourceDictionary` nesneleri paylaşmaktır. Bir görsel öğe paylaşılamaz. Aynı örneği, iki kez tek bir sayfada yer alamaz.

## <a name="the-xstatic-markup-extension"></a>X: Static işaretleme uzantısı

Adlarını benzerlikler rağmen `x:Static` ve `StaticResource` çok farklıdır. `StaticResource` bir kaynak sözlüğü sırasında bir nesne döndürür `x:Static` aşağıdakilerden birini erişir:

- Genel statik alan
- bir ortak statik özelliği
- Genel bir sabit alanı
- bir numaralandırma üyesine.

`StaticResource` İşaretleme uzantısı, bir kaynak sözlüğü tanımlayan XAML uygulamaları tarafından desteklenir ancak `x:Static` XAML, iç bir parçası olarak olduğu `x` önek ortaya çıkarır.

Gösteren bazı örnekler şunlardır nasıl `x:Static` statik alanları ve numaralandırma üyelerini açıkça başvurabilirsiniz:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

Şu ana kadar bu çok etkileyici değildir. Ancak `x:Static` işaretleme uzantısı de başvurabilir statik alanlar ve Özellikler kendi kodunuza. Örneğin, bir `AppConstants` uygulamanın tamamında birden çok sayfa kullanmak isteyebileceğiniz bazı statik alanlar içeren sınıf:

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

XAML dosyasında bu sınıfın statik alanları başvurmak için bu dosyasının bulunduğu bir XAML dosyasında belirtmek için bir yönteme gerekir. XML ad alanı bildirimi ile bunu yapabilirsiniz.

Standart Xamarin.Forms XAML şablonunun bir parçası oluşturulan XAML dosyaları iki XML ad alanı bildirimi içeren hatırlıyorsunuzdur: bir Xamarin.Forms sınıfları, diğeri için etiketleri ve öznitelikleri için XAML iç başvuran erişmek için:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Diğer sınıflara erişmek için ek XML ad alanı bildirimi gerekir. Her ek XML ad alanı bildirimi, yeni bir önek tanımlar. Paylaşılan uygulama .NET Standard kitaplığı için yerel sınıflar gibi erişmeye `AppConstants`, sık kullandığınız önek XAML programcılar `local`. Ad alanı bildirimi CLR (ortak dil çalışma zamanı) ad alanı adı, bir C# içinde görünen adı olarak da bilinen .NET ad alanı adı belirtmeniz gerekir `namespace` tanımı veya bir `using` yönergesi:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Bu gibi durumlarda, .NET ad alanları için XML ad alanı bildirimi ayrıca .NET Standard kitaplığı başvuran herhangi bir derleme tanımlayabilirsiniz. Örneğin, bir `sys` standart .NET için önek `System` bulunduğu ad alanı **mscorlib** bir kez "Ortak nesne çalışma zamanı için Microsoft kitaplığı" komutla, ancak artık "çok dilli standart anlamına gelir, derleme Genel nesne çalışma zamanı kitaplığı." Bu başka bir derleme olduğundan, ayrıca derleme adı, bu durumda belirtmelisiniz **mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Dikkat anahtar sözcüğü `clr-namespace` bir iki nokta üst üste ve ardından .NET ad alanı adı anahtar sözcüğü bir noktalı virgül ekleyin, ardından `assembly`, eşittir işareti ve derleme adı.

Evet, bir iki nokta üst üste izleyen `clr-namespace` ancak eşittir işaretinden `assembly`. Söz dizimi bu şekilde kasıtlı olarak tanımlandı: en XML ad alanı bildirimi bir URI düzeni adı gibi başlayan bir URI başvuru `http`, hangi her zaman ardından bir iki nokta üst üste. `clr-namespace` Bu dize bir parçası, bu kuralı taklit etmek üzere tasarlanmıştır.

Bu iki ad alanı bildirimi dahil **StaticConstantsPage** örnek. Dikkat `BoxView` boyutları ayarlandığında `Math.PI` ve `Math.E`, 100 faktörüyle ancak Ölçeklendirildi:

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

Sonuç boyutunu `BoxView` kutusunun ekrana göreli platform bağlıdır:

 [![](xaml-markup-extensions-images/staticconstants.png "X: Static işaretleme uzantısı kullanarak denetimleri")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "x: Static işaretleme uzantısı kullanarak denetimleri")

## <a name="other-standard-markup-extensions"></a>Diğer standart biçimlendirme uzantıları

Çeşitli biçimlendirme uzantıları için XAML iç ve Xamarin.Forms XAML dosyaları desteklenir. Bunlardan bazıları, çok sık kullanılmayan ancak ihtiyaç duyduğunuzda gereklidir:

-  Bir özelliği olmayan bir varsa `null` ayarlamak istediğiniz değere göre varsayılan ancak `null`, ayarlayın `{x:Null}` işaretleme uzantısı.
-  Özellik türü ise `Type`, kendisine atayabilirsiniz bir `Type` işaretleme uzantısı kullanarak nesne `{x:Type someClass}`.
-  XAML kullanarak dizileri tanımlayabilirsiniz `x:Array` işaretleme uzantısı. Bu işaretleme uzantısı adlı gerekli bir özniteliği olan `Type` dizideki öğelerin türünü gösterir.
- `Binding` İşaretleme uzantısı ele alınmıştır [bölüm 4. Veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="the-constraintexpression-markup-extension"></a>ConstraintExpression işaretleme uzantısı

Biçimlendirme uzantıları özelliklere sahip olabilir, ancak XML öznitelikleri gibi ayarlı değil. Bir işaretleme uzantısı özellik ayarları virgülle ayrılır ve küme ayraçlarının içinde tırnak işareti görünür.

Bu adlı Xamarin.Forms işaretleme uzantısı ile gösterilebilir `ConstraintExpression`, birlikte kullanılan `RelativeLayout` sınıfı. Bir sabit olarak veya bir üst ya da diğer adlandırılmış görünüm göreli konum veya alt görünümün boyutunu belirtebilirsiniz. Söz dizimi `ConstraintExpression` konumunu veya bir görünümünü kullanarak boyutu ayarlamanız sağlayan bir `Factor` bir özellik başka bir görünümünün yanı sıra bir kez `Constant`. Daha fazla karmaşık bir şey kod gerektirir.

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

Belki de bu örnekten aldığınız en önemli Ders biçimlendirme uzantısı sözdizimi şöyledir: tırnak işareti bir işaretleme uzantısı kaşlı ayraçlar içinde yer almalıdır. İşaretleme uzantısı bir XAML dosyasında yazarken özelliklerin değerleri tırnak içine alın istediğiniz doğaldır. Dürtüsüne karşı dayanıklılık!

Çalışan bir program şöyledir:

[![](xaml-markup-extensions-images/relativelayout.png "Göreli düzeni kısıtlamaları kullanılarak")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "göreli kısıtlamaları kullanılarak düzeni")

## <a name="summary"></a>Özet

XAML biçimlendirme uzantıları burada gösterilen önemli, XAML dosyaları için desteği. Ancak belki de en değerli XAML işaretleme uzantısı `Binding`, bu serisinin sonraki bölümünde ele [bölüm 4. Veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).



## <a name="related-links"></a>İlgili bağlantılar

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Bölüm 1. XAML Kullanmaya Başlarken](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Bölüm 2. Temel XAML Sözdizimi](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Bölüm 4. Temel Veri Bağlama Bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Bölüm 5. Verileri bağlama için MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
