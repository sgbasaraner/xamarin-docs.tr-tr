---
title: Cihaz yönü
description: Bu makalede açıklanır dikey ve yatay yönler harika görünecek yerleşimi Xamarin.Forms uygulamaları nasıl.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: 7f0e1c27f7d6a62dc43ac447c4f796d685a6cd91
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241217"
---
# <a name="device-orientation"></a>Cihaz yönü

Uygulamanızı nasıl kullanılacağını ve nasıl yatay yönde kullanıcı deneyimini geliştirmek için birleştirilebilir göz önünde bulundurmanız önemlidir. Tek tek düzenleri birden çok yönleri uyum sağlamak için tasarlanan ve en iyi kullanılabilir alanı kullanın. Uygulama düzeyinde döndürmeyi devre dışı bırakabilir veya etkinleştirilebilir.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>Yönünü denetleme

Xamarin.Forms kullanırken, cihaz yönünü denetleme için desteklenen yöntem her bir proje için ayarları kullanmaktır.

### <a name="ios"></a>iOS

İOS, cihaz yönü kullanan uygulamalar için yapılandırılmış **Info.plist** dosya. İsteğe bağlı olarak uygulamayı hedef olarak içeriyorsa, bu dosyayı iPad için ayarları yanı sıra, iPhone ve iPod yönlendirme ayarlarını içerir. IDE'nizi için özel yönergeler verilmiştir. IDE seçenekleri, bu belgenin üst kısmında hangi yönergeleri görmek istediğiniz seçmek için kullanın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio iOS projesini açın ve açık **Info.plist**. İPhone dağıtım bilgisi sekmesi ile başlayarak, bir yapılandırma paneline dosya açılır:

![iPhone dağıtım bilgisi Visual Studio'da](device-orientation-images/orientation-vs-iphone.png)

İPad yönlendirmesini yapılandırmak için seçin **iPad dağıtım bilgisi** panelinde, ardından kullanılabilir yönlendirmeler select sol üstteki sekmesine:

![Visual Studio'da desteklenen cihaz yönleri](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio, bir iOS projesi açın ve açık **Info.plist**. Altında **uygulama** sekmesinde bölümleri yönlendirmesini ayarlamak kullanılabilir olacak:

![iPhone dağıtım bilgisi Mac için Visual Studio](device-orientation-images/orientation-xam-ui.png)

Bir anahtar-değer Düzenleyicisi arabirimi, select değerlerini düzenlemek tercih ediyorsanız **kaynak**> sekmesinde ekranın alt kısmındaki:

![Cihaz yönleri, Mac için Visual Studio'da desteklendiğine](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Android'de yönlendirmesini denetlemek için açık **MainActivity.cs** ve dekorasyon özniteliğini kullanarak yönlendirmeyi ayarlayın `MainActivity` sınıfı:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android yönünü belirtmek için çeşitli seçenekler destekler:

- **Yatay** &ndash; uygulama yönünü yatay, sensör verilerini bağımsız olarak olmasını zorlar.
- **Dikey** &ndash; sensör verilerini bağımsız olarak, dikey olarak uygulama yönlendirme zorlar.
- **Kullanıcı** &ndash; kullanıcının tercih edilen yönlendirme kullanarak sunulacak uygulamanın neden olur.
- **Arkasında** &ndash; yönünü aynı olması uygulamanın yönünü neden [etkinlik](https://developer.xamarin.com/api/type/Android.App.Activity/) arkasındaki.
- **Algılayıcı** &ndash; kullanıcı Otomatik döndürmeyi devre dışı olsa bile algılayıcı tarafından belirlenecek uygulamanın yönünü neden olur.
- **SensorLandscape** &ndash; (ters olarak ekranını değil böylece) ekran dönmüş olduğu Yön değiştirmek için sensör verilerini kullanırken yatay yönde kullanmak için uygulamayı neden olur.
- **SensorPortrait** &ndash; dikey yönde olacak şekilde (ters olarak ekranını değil) ekran dönmüş olduğu Yön değiştirmek için sensör verilerini kullanırken kullanmak için uygulamayı neden olur.
- **ReverseLandscape** &ndash; yatay yönlendirme kullanıldığında, normal "tersyüz." görünmesi için ters yönlerde karşılıklı kullanmak için uygulamayı neden olur
- **ReversePortrait** &ndash; dikey yönlendirme kullanıldığında, normal "tersyüz." görünmesi için ters yönlerde karşılıklı kullanmak için uygulamayı neden olur
- **FullSensor** &ndash; uygulamanın doğru yönlendirmeyi (dışı olası 4) seçmek için sensör verilerini kullanır.
- **FullUser** &ndash; kullanıcının yönlendirme tercihlerini kullanmak için uygulamayı neden olur. Otomatik döndürme etkinleştirilirse, tüm 4 yönleri kullanılabilir.
- **UserLandscape** &ndash; _\[desteklenmiyor\]_ , bu durumda kullanacağı otomatik döndürme etkin, kullanıcının sahip olduğu sürece yatay yöndeki kullanmak için uygulamayı neden olur. Yönlendirme saptamak için sensör. Bu seçenek, derleme çalışmamasına neden olur.
- **UserPortrait** &ndash; _\[desteklenmiyor\]_ , bu durumda kullanacağı otomatik döndürme etkin, kullanıcının sahip olduğu sürece uygulamayı dikey yönlendirme kullanıldığında, kullanacak şekilde neden olur. Yönlendirme saptamak için sensör. Bu seçenek, derleme çalışmamasına neden olur.
- **Kilitli** &ndash; _\[desteklenmiyor\]_ ekran yönünü kullanmak için uygulamayı neden olan her şeyi başlatma sırasında cihaz değişikliklere yanıt olmadan olduğu fiziksel yönü. Bu seçenek, derleme çalışmamasına neden olur.

Yerel Android API birçok yönünü nasıl yönetilir denetim sağlamasına, kullanıcının açıkça çelişen seçenekleri dahil olmak üzere ifade tercihleri dikkat edin.

### <a name="universal-windows-platform"></a>Evrensel Windows platformu

Evrensel Windows Platformu (UWP üzerinde), desteklenen yönleri ayarlanmış **Package.appxmanifest** dosya. Bildirim açma desteklenen yönleri burada seçilebilir bir yapılandırma bölmesi ortaya çıkarır.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>Yönlendirme değişikliklere tepki verme

Xamarin.Forms, uygulamanızı paylaşılan kod yönünü değişiklikleri bildiren tüm yerel olaylarını sunmaz. Ancak, `SizeChanged` olayı `Page` tetiklenen genişliği veya yüksekliği `Page` değişiklikler. Zaman genişliğini `Page` yüksekliği büyükse yatay modda cihazdır. Daha fazla bilgi için [ekran yönlendirmeyi temel alarak bir görüntüyü](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation).

> [!NOTE]
> Paylaşılan kod yönlendirme değişikliklerin bildirimleri almak için bir var olan ve ücretsiz NuGet paketi yoktur. Bkz: [GitHub deposunu](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) daha fazla bilgi için.

Alternatif olarak, geçersiz kılmak olası [ `OnSizeAllocated` ](xref:Xamarin.Forms.Page.OnSizeAllocated*) metodunda bir `Page`, herhangi bir düzen ekleme mantık uyguladığımızı değiştirin. `OnSizeAllocated` Yöntemi çağrıldığında her bir `Page` cihaz Döndürülmüş whenver'olmuyor yeni bir boyut ayrılır. Unutmayın taban uygulamasını `OnSizeAllocated` geçersiz kılma seçeneğinde temel uygulamayı çağırması önemlidir önemli Düzen işlevleri gerçekleştirir:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Hata, bir aşamaya geçmeye işlevsiz bir sayfada neden olur.

Unutmayın `OnSizeAllocated` yöntemi çağrılabilir birden çok kez bir cihaz döndürüldüğünde. Her zaman, Düzen değiştiriliyor kaynakları kısıp ve titremeyi için yol açabilir. Yatay veya dikey hizalama olup olmadığını izlemek için bir örnek değişkeni, sayfada kullanmayı göz önünde bulundurun ve yalnızca bir değişiklik olduğunda yeniden Çiz:

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

Cihaz yönü değişikliği algıladığında eklemek veya ek görünümler kullanılabilir alanı değiştirmeyi tepki vermek için kullanıcı arabirimi öğesine/öğesinden kaldırmak isteyebilirsiniz. Örneğin, dikey durumda her platformda yerleşik hesap makinesi göz önünde bulundurun:

![](device-orientation-images/calculator-portrait.png "Dikey durumda hesaplayıcısı uygulaması")

Yatay ve dikey:

![](device-orientation-images/calculator-landscape.png "Yatay durumda hesaplayıcısı uygulaması")

Görünüme ilişkin daha fazla işlevsellik ekleyerek uygulamaları güvenilir kullanılabilir alanı avantajlarından yararlanmak dikkat edin.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>Esnek Düzen

Böylece cihaz döndürüldüğünde, düzgün bir şekilde geçiş yerleşik düzenleri kullanarak tasarım arabirimlerine mümkündür. Yönlendirme değişikliklere yanıt verirken çekici olmaya devam edecek arabirimleri tasarlarken aşağıdaki genel kurallar göz önünde bulundurun:

- **Oranları dikkat** &ndash; değişiklikleri yönde bazı varsayımlarda bakımından oranları yapıldığında sorunlara neden olabilir. Örneğin, 1/3. dikey boşluğu dikey bir ekran alanı bolca olması gereken bir görünüm 1/3 Dikey alanı yatay durumda içine çözüm karşılamayabilir.
- **Mutlak değerlerle dikkatli olun** &ndash; dikey anlamlı (piksel) mutlak değerler mantıklı değildir yatay olarak. Mutlak değerler, gerekli olduğunda, bunların etkisini yalıtmak için iç içe geçmiş düzenleri kullanın. Örneğin, bunun mutlak değerler kullanılacak mantıklı bir `TableView` `ItemTemplate` öğe şablonu garantili Tekdüzen yüksekliğe sahip olduğunda.

Birden çok ekran boyutları ve olan arabirimler genellikle uygulama en iyi uygulama olarak düşünüldüğünde yukarıdaki kuralları da geçerlidir. Bu kılavuzun geri kalanını her birincil düzenleri kullanarak Xamarin.Forms içinde hızlı yanıt veren düzenleri belirli örnekleri açıklanmaktadır.

> [!NOTE]
> Daha anlaşılır olması için aşağıdaki bölümlerde yalnızca bir tür kullanarak hızlı yanıt veren düzenleri kullanılmasını göstermek `Layout` birer güncelleştirir. Uygulamada, bu genellikle karıştırmak kolaydır `Layout`daha basit ve kullanımı en kolay kullanarak istenen bir düzen elde etmek için s `Layout` her bileşeni için.

### <a name="stacklayout"></a>StackLayout

Şu uygulama dikey görüntülenen göz önünde bulundurun:

![](device-orientation-images/photo-stack-portrait.png "Fotoğraf uygulama dikey")

Yatay ve dikey:

![](device-orientation-images/photo-stack-landscape.png "Yatay fotoğraf uygulama")

Aşağıdaki XAML ile elde edilir:

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

Bazı C# ' ın yönünü değiştirmek için kullanılan `outerStack` cihazın yönlendirmeyi temel alarak:

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

- `outerStack` yatay veya dikey yığını kullanılabilir alanı en iyi şekilde yararlanmak için yön, bağlı olarak görüntü ve denetimler sunmak için ayarlanır.


### <a name="absolutelayout"></a>AbsoluteLayout

Şu uygulama dikey görüntülenen göz önünde bulundurun:

![](device-orientation-images/photo-abs-portrait.png "Fotoğraf uygulama dikey")

Yatay ve dikey:

![](device-orientation-images/photo-abs-landscape.png "Yatay fotoğraf uygulama")

Aşağıdaki XAML ile elde edilir:

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

- Sayfa düzenlendiği şekli nedeniyle, yanıt verme hızını tanıtmak yordam kodu için gerek yoktur.
- `ScrollView` Etiketi ekran yüksekliği sabit yüksekliklerini düğmeler ve resim toplamından daha az olduğunda bile görünür olması için izin vermek için kullanılır.


### <a name="relativelayout"></a>RelativeLayout

Şu uygulama dikey görüntülenen göz önünde bulundurun:

![](device-orientation-images/photo-rel-portrait.png "Fotoğraf uygulama dikey")

Yatay ve dikey:

![](device-orientation-images/photo-rel-landscape.png "Yatay fotoğraf uygulama")

Aşağıdaki XAML ile elde edilir:

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

- Sayfa düzenlendiği şekli nedeniyle, yanıt verme hızını tanıtmak yordam kodu için gerek yoktur.
- `ScrollView` Etiketi ekran yüksekliği sabit yüksekliklerini düğmeler ve resim toplamından daha az olduğunda bile görünür olması için izin vermek için kullanılır.

### <a name="grid"></a>Kılavuz

Şu uygulama dikey görüntülenen göz önünde bulundurun:

![](device-orientation-images/photo-grid-portrait.png "Fotoğraf uygulama dikey")

Yatay ve dikey:

![](device-orientation-images/photo-grid-landscape.png "Yatay fotoğraf uygulama")

Aşağıdaki XAML ile elde edilir:

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

Döndürme değişiklikleri işlemek için aşağıdaki yordam kodu ile birlikte:

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

- Sayfa düzenlendiği şekli nedeniyle, denetimleri kılavuz yerleşimini değiştirmek için bir yöntem yoktur.


## <a name="related-links"></a>İlgili bağlantılar

- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [Esnek Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [Ekran yönünü üzerinde temel alan bir görüntü görüntüleme](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)
