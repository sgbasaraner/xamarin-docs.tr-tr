---
title: Cihaz yönlendirmesini
description: Dikey ve yatay yönler görünümlü uygulamaları yerleştirme anlayın.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: b06b17ce8f19f7f7cabe35c23de5b61db8f71dbe
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="device-orientation"></a>Cihaz yönlendirmesini

Uygulamanızı nasıl kullanılacağını ve kullanıcı deneyimini geliştirmek için yatay yönlendirme nasıl birleştirilebilir dikkate almak önemlidir. Tek tek düzenleri birden çok yönler uyum sağlamak için tasarlanmış ve en iyi kullanılabilir alanı kullanın. Uygulama düzeyinde döndürme devre dışı veya etkinleştirilebilir.

Bu makalede, cihaz yönlendirmesini özelliklerden yararlanmak uygulamaları oluşturmada size yol gösterir ve aşağıdaki bölümleri içerir:

- **[Yönlendirme denetleme](#Controlling_Orientation)**  &ndash; uygulama düzeyinde yönlendirme her bir platformda denetlemek anlayın.
- **[Yönlendirme değişikliklere tepki](#Reacting_to_Changes_in_Orientation)**  &ndash; bildirilmesini ve tepki öğrenin, yönde değiştirir.
- **[Esnek düzeni](#Responsive_Layout)**  &ndash; yatay ve dikey yönler arasında otomatik olarak iş düzenleri oluşturma hakkında bilgi edinin.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>Yönlendirme denetleme

Xamarin.Forms kullanırken, cihaz yönlendirmesini denetlemek için desteklenen yöntem her proje için ayarları kullanmaktır.

### <a name="ios"></a>iOS

İos'ta, cihaz yönlendirmesini kullanan uygulamalar için yapılandırılmış **Info.plist** dosya. İsteğe bağlı olarak uygulama hedef olarak içeriyorsa, bu dosyayı iPhone & iPod yönlendirme ayarlarını yanı sıra, iPad için ayarları içerir. IDE'yi için özel yönergeler verilmiştir. IDE seçenekleri bu belgenin üst kısmında hangi yönergeleri görmek istediğiniz seçmek için kullanın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da iOS projesi açın ve açık **Info.plist**. Dosya, iPhone dağıtım bilgileri sekmesi ile başlayarak, bir yapılandırma paneline açılır:

![iPhone Visual Studio'da dağıtım bilgileri](device-orientation-images/orientation-vs-iphone.png)

İPad yönlendirmesini yapılandırmak için seçin **iPad dağıtım bilgileri** panelinde, ardından kullanılabilir yönler select sol üst sekmesini:

![Visual Studio'da desteklenen cihaz yönler](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Visual Studio'da Mac için iOS projesi açın ve açık **Info.plist**. Altında **uygulama** sekmesinde bölümleri kullanılabilir yönlendirmesini ayarlamak:

![iPhone Mac için Visual Studio'da dağıtım bilgileri](device-orientation-images/orientation-xam-ui.png)

Select bir anahtar-değer Düzenleyici arabirimi kullanarak değerlerini düzenlemek tercih ederseniz **kaynak**> sekmesinde ekranın altındaki:

![Cihaz yönler Visual Studio'da Mac için desteklenir.](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Android üzerinde yönlendirme denetlemek için açık **MainActivity.cs** ve öznitelik dekorasyon kullanarak yönlendirmesini ayarlamak `MainActivity` sınıfı:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android yönünü belirtmek için çeşitli seçenekler destekler:

- **Yatay** &ndash; algılayıcı verilerini bağımsız olarak yatay olarak uygulama yönlendirmesini zorlar.
- **Dikey** &ndash; algılayıcı verilerini bağımsız olarak dikey olarak uygulama yönlendirmesini zorlar.
- **Kullanıcı** &ndash; kullanıcının tercih edilen yönlendirme kullanarak sunulacak uygulamanın neden olur.
- **Arkasında** &ndash; yönünü aynı olacak şekilde uygulamanın yönlendirme neden [etkinlik](https://developer.xamarin.com/api/type/Android.App.Activity/) arkasındaki.
- **Algılayıcı** &ndash; otomatik döndürme kullanıcı devre dışı olsa bile uygulamanın yönlendirme algılayıcı tarafından belirlenmesine neden olur.
- **SensorLandscape** &ndash; (ters olarak ekranını değil böylece) ekran karşılıklı yönünü değiştirmek için algılayıcı verileri kullanırken yatay yönlendirme kullanmak için uygulamayı neden olur.
- **SensorPortrait** &ndash; (ters olarak ekranını değil böylece) ekran karşılıklı yönünü değiştirmek için algılayıcı verileri kullanırken dikey yönde kullanmak için uygulamayı neden olur.
- **ReverseLandscape** &ndash; "ters." görünen amacıyla normal karşıt yönünden karşılıklı yatay yönlendirme kullanmak için uygulamayı neden olur.
- **ReversePortrait** &ndash; "ters." görünen amacıyla normal karşıt yönünden karşılıklı dikey yönlendirme kullanmak için uygulamayı neden olur.
- **FullSensor** &ndash; doğru yönünü (dışında olası 4) seçmek için algılayıcı verilerini güvenemeyeceklerini uygulamanın neden olur.
- **FullUser** &ndash; kullanıcının yönlendirmesini tercihlerini kullanmak için uygulamayı neden olur. Otomatik döndürme etkinleştirilirse, tüm 4 yönler kullanılabilir.
- **UserLandscape** &ndash; _\[desteklenmiyor\]_ kullanıcı; bu durumda kullanmak etkinse, otomatik döndürme olmadıkça yatay yönlendirme kullanmak için uygulamayı neden olur. Yönlendirme belirlemek için algılayıcı. Bu seçenek derleme çalışmamasına neden olur.
- **UserPortrait** &ndash; _\[desteklenmiyor\]_ kullanıcı; bu durumda kullanmak etkinse, otomatik döndürme olmadıkça dikey yönlendirme kullanmak için uygulamayı neden olur. Yönlendirme belirlemek için algılayıcı. Bu seçenek derleme çalışmamasına neden olur.
- **Kilitli** &ndash; _\[desteklenmiyor\]_ ekran yönünü kullanmak için uygulamayı neden ne olursa olsun, başlatılırken aygıt değişikliklere yanıt olmadan olduğu fiziksel yönü. Bu seçenek derleme çalışmamasına neden olur.

Yerel Android API çok sayıda yönlendirme nasıl yönetilir üzerinde denetim sağlar, kullanıcının açıkça çelişen seçenekleri de dahil olmak üzere Tercihler ifade dikkat edin.

### <a name="universal-windows-platform"></a>Evrensel Windows platformu

Evrensel Windows Platformu (UWP üzerinde), desteklenen yönler ayarlanmış **Package.appxmanifest** dosya. Bildirim açma desteklenen yönler burada seçilebilir yapılandırma Panosu gösterecek.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>Yönlendirme değişikliklere tepki

Xamarin.Forms uygulamanız paylaşılan kod yön değişimleri bilgilendirmek için yerel olay sağlamaz. Ancak, `SizeChanged` olayı `Page` tetiklenen genişliği veya yüksekliği `Page` değişiklikler. Zaman genişliğini `Page` yüksekliği büyük yatay modunda aygıttır. Daha fazla bilgi için bkz: [ekran yönünü dayalı bir görüntüyü](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/).

> [!NOTE]
> Paylaşılan kodda yönlendirmesini değişikliklerin bildirimleri almak için bir var olan, ücretsiz NuGet paketi yoktur. Bkz: [GitHub deposuna](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) daha fazla bilgi için.

Alternatif olarak, geçersiz kılmak olası [ `OnSizeAllocated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnSizeAllocated(System.Double,System.Double)/) yöntemi bir `Page`, herhangi bir düzeni eklemeden değiştirmek mantığı vardır. `OnSizeAllocated` Yöntemi çağrıldığında her bir `Page` aygıt Döndürülmüş whenver olur yeni boyutunu tahsis edilir. Unutmayın, temel uygulamayı `OnSizeAllocated` geçersiz kılmada temel uygulamayı çağırması önemlidir önemli düzeni işlevleri gerçekleştirir:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Bu adımı çalışmayan bir sayfasında neden.

Unutmayın `OnSizeAllocated` yöntemi çağrılabilir birçok kez bir aygıtı döndürüldüğünde. Her zaman düzeninizi değiştirme kaynakları kayıp ve titremeyi için yol açabilir. Yönlendirmesini yatay veya dikey olup olmadığını izlemek için bir örnek değişkeni sayfanızın içinden kullanmayı düşünün ve yalnızca bir değişiklik olduğunda yeniden boyutlandırmaya:

```csharp
private double width = 0;
private double height = 0;

protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
    if (this.width != width || this.height != height)
    {
        this.width = width;
        this.height = height;
        //reconfigure layout
    }
}
```

Cihaz yönlendirmesini içinde bir değişiklik algıladı sonra eklemek veya grafikten kullanılabilir alanı değişikliği tepki vermek için kullanıcı arabiriminizi ek görünümler kaldırmak isteyebilirsiniz. Örneğin, dikey olarak her platformda yerleşik hesaplayıcı göz önünde bulundurun:

![](device-orientation-images/calculator-portrait.png "Dikey hesaplayıcı uygulamada")

Yatay ve dikey:

![](device-orientation-images/calculator-landscape.png "Yatay hesaplayıcı uygulamada")

Uygulamaları yatay olarak daha fazla işlevsellik ekleyerek kullanılabilir alan avantajlarından yararlanmak dikkat edin.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>Esnek düzeni

Böylece cihaz döndürüldüğünde, düzgün biçimde geçiş yerleşik düzenleri kullanarak tasarım arabirimlerine mümkündür. Yönlendirme değişikliklere yanıt verirken çekici olmaya devam edecek arabirimleri tasarlarken aşağıdaki genel kurallar göz önünde bulundurun:

- **Oranları dikkat** &ndash; bazı varsayımlarda göre oranları yapıldığında, değişiklikleri yönde sorunlara neden olabilir. Örneğin, yeterince alan 1/3 dikey bir ekranda dikey alanının olması gereken bir görünüm 1/3 Yatay Dikey alanının içine sığması değil.
- **Mutlak değerler ile dikkatli olun** &ndash; dikey anlamlı mutlak (piksel) değerler hale algılama yatay olarak. Mutlak değerler gerekli olduğunda, iç içe geçmiş düzenleri etkilerini ayırmak için kullanın. Örneğin, bunu mutlak değerleri kullanmak mantıklı bir `TableView` `ItemTemplate` öğe şablonu garantili Tekdüzen yüksekliği olduğunda.

Arabirimler birden çok ekran boyutlarına ve uygulamaları için genellikle uygulama en iyi yöntem olarak düşünüldüğünde yukarıdaki kuralları da geçerlidir. Bu kılavuzda kalan her birincil düzenleri içinde Xamarin.Forms kullanarak esnek düzenleri belirli örnekleri açıklanmaktadır.

> [!NOTE]
> Daha anlaşılır olması için aşağıdaki bölümlerde tek türünü kullanan esnek düzenleri uygulamak göstermektedir `Layout` birer birer. Uygulamada, bu genellikle karışık kolaydır `Layout`daha basit ve en kolay anlaşılır kullanarak istenen bir düzen elde etmek için s `Layout` her bileşen için.

### <a name="stacklayout"></a>StackLayout

Dikey görüntülenen aşağıdaki uygulama göz önünde bulundurun:

![](device-orientation-images/photo-stack-portrait.png "Dikey fotoğraf uygulamada")

Yatay ve dikey:

![](device-orientation-images/photo-stack-landscape.png "Yatay fotoğraf uygulamada")

Aşağıdaki XAML ile gerçekleştirilir:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.StackLayoutPageXaml"
Title="Stack Photo Editor - XAML">
    <ContentPage.Content>
        <StackLayout Spacing="10" Padding="5" Orientation="Vertical"
        x:Name="outerStack"> <!-- can change orientation to make responsive -->
            <ScrollView>
                <StackLayout Spacing="5" HorizontalOptions="FillAndExpand"
                    WidthRequest="1000">
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Name: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer.jpg"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Date: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="07/05/2015"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Tags:" WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer, tiger"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Button Text="Save" HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                </StackLayout>
            </ScrollView>
            <Image  Source="deer.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Bazı C# yönünü değiştirmek için kullanılan `outerStack` cihaz yönünü göre:

```csharp
protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            outerStack.Orientation = StackOrientation.Horizontal;
        } else {
            outerStack.Orientation = StackOrientation.Vertical;
        }
    }
}
```

Şunlara dikkat edin:

- `outerStack` görüntü denetimleri yatay veya dikey yığın kullanılabilir alanı en iyi şekilde yararlanmak için yönlendirme, bağlı olarak sunmak üzere ayarlanır.


### <a name="absolutelayout"></a>AbsoluteLayout

Dikey görüntülenen aşağıdaki uygulama göz önünde bulundurun:

![](device-orientation-images/photo-abs-portrait.png "Dikey fotoğraf uygulamada")

Yatay ve dikey:

![](device-orientation-images/photo-abs-landscape.png "Yatay fotoğraf uygulamada")

Aşağıdaki XAML ile gerçekleştirilir:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImage="deer.jpg">
    <ContentPage.Content>
        <AbsoluteLayout>
            <ScrollView AbsoluteLayout.LayoutBounds="0,0,1,1"
                AbsoluteLayout.LayoutFlags="PositionProportional,SizeProportional">
                <AbsoluteLayout>
                    <Image Source="deer.jpg"
                        AbsoluteLayout.LayoutBounds=".5,0,300,300"
                        AbsoluteLayout.LayoutFlags="PositionProportional" />
                    <BoxView Color="#CC1A7019" AbsoluteLayout.LayoutBounds=".5
                        300,.7,50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" />
                    <Label Text="deer.jpg" AbsoluteLayout.LayoutBounds = ".5
                        310,1, 50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" HorizontalTextAlignment="Center" TextColor="White" />
                </AbsoluteLayout>
            </ScrollView>
            <Button Text="Previous" AbsoluteLayout.LayoutBounds="0,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional"
                BackgroundColor="White" TextColor="Green" BorderRadius="0" />
            <Button Text="Next" AbsoluteLayout.LayoutBounds="1,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional" BackgroundColor="White"
                    TextColor="Green" BorderRadius="0" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Şunlara dikkat edin:

- Sayfa yerleştirilmiş giden yolu nedeniyle yanıtlama tanıtmak yordam kodu için gerek yoktur.
- `ScrollView` Ekran yüksekliğini düğmeleri ve görüntünün sabit uzunluklarının toplamından daha az olduğunda bile görünmesi için etiket izin vermek için kullanılır.


### <a name="relativelayout"></a>RelativeLayout

Dikey görüntülenen aşağıdaki uygulama göz önünde bulundurun:

![](device-orientation-images/photo-rel-portrait.png "Dikey fotoğraf uygulamada")

Yatay ve dikey:

![](device-orientation-images/photo-rel-landscape.png "Yatay fotoğraf uygulamada")

Aşağıdaki XAML ile gerçekleştirilir:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImage="deer.jpg">
    <ContentPage.Content>
        <RelativeLayout x:Name="outerLayout">
            <BoxView BackgroundColor="#AA1A7019"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}" />
            <ScrollView
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}">
                <RelativeLayout>
                    <Image Source="deer.jpg" x:Name="imageDeer"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.8}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.1}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=10}" />
                    <Label Text="deer.jpg" HorizontalTextAlignment="Center"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=1}"
                        RelativeLayout.HeightConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=75}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToView,ElementName=imageDeer,Property=Height,Factor=1,Constant=20}" />
                </RelativeLayout>

            </ScrollView>

            <Button Text="Previous" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                 />
            <Button Text="Next" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                />
        </RelativeLayout>
    </ContentPage.Content>
</ContentPage>

```

Şunlara dikkat edin:

- Sayfa yerleştirilmiş giden yolu nedeniyle yanıtlama tanıtmak yordam kodu için gerek yoktur.
- `ScrollView` Ekran yüksekliğini düğmeleri ve görüntünün sabit uzunluklarının toplamından daha az olduğunda bile görünmesi için etiket izin vermek için kullanılır.

### <a name="grid"></a>Kılavuz

Dikey görüntülenen aşağıdaki uygulama göz önünde bulundurun:

![](device-orientation-images/photo-grid-portrait.png "Dikey fotoğraf uygulamada")

Yatay ve dikey:

![](device-orientation-images/photo-grid-landscape.png "Yatay fotoğraf uygulamada")

Aşağıdaki XAML ile gerçekleştirilir:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.GridPageXaml"
Title="Grid - XAML">
    <ContentPage.Content>
        <Grid x:Name="outerGrid">
            <Grid.RowDefinitions>
                <RowDefinition Height="*" />
                <RowDefinition Height="60" />
            </Grid.RowDefinitions>
            <Grid x:Name="innerGrid" Grid.Row="0" Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="deer.jpg" Grid.Row="0" Grid.Column="0" HeightRequest="300" WidthRequest="300" />
                <Grid x:Name="controlsGrid" Grid.Row="0" Grid.Column="1" >
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <Label Text="Name:" Grid.Row="0" Grid.Column="0" />
                    <Label Text="Date:" Grid.Row="1" Grid.Column="0" />
                    <Label Text="Tags:" Grid.Row="2" Grid.Column="0" />
                    <Entry Grid.Row="0" Grid.Column="1" />
                    <Entry Grid.Row="1" Grid.Column="1" />
                    <Entry Grid.Row="2" Grid.Column="1" />
                </Grid>
            </Grid>
            <Grid x:Name="buttonsGrid" Grid.Row="1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Previous" Grid.Column="0" />
                <Button Text="Save" Grid.Column="1" />
                <Button Text="Next" Grid.Column="2" />
            </Grid>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Döndürme değişiklikleri işlemek için aşağıdaki yordam kod ile birlikte:

```csharp
private double width;
private double height;

protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.RowDefinitions.Add (new RowDefinition{ Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 1, 0);
        } else {
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Auto) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 0, 1);
        }
    }
}
```

Şunlara dikkat edin:

- Sayfa yerleştirilmiş giden yolu nedeniyle denetimleri kılavuz yerleşimini değiştirmek için bir yöntem yoktur.


## <a name="related-links"></a>İlgili bağlantılar

- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [Esnek Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [Ekran yönünü dayanan bir görüntü görüntüleme](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/)
