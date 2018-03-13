---
title: "XAML'de yerel görünümleri"
description: "İOS, Android ve evrensel Windows platformu yerel görünümleri doğrudan Xamarin.Forms XAML dosyalarından başvurulabilir. Xamarin.Forms görünümleri ile etkileşim kurabilir ve özellikleri ve olay işleyicileri yerel görünümler üzerinde ayarlanabilir. Bu makalede Xamarin.Forms XAML dosyaları yerel görünümleri kullanma gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: f4345e107a32c3a583c246fe5dbe24590960c870
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="native-views-in-xaml"></a>XAML'de yerel görünümleri

_İOS, Android ve evrensel Windows platformu yerel görünümleri doğrudan Xamarin.Forms XAML dosyalarından başvurulabilir. Xamarin.Forms görünümleri ile etkileşim kurabilir ve özellikleri ve olay işleyicileri yerel görünümler üzerinde ayarlanabilir. Bu makalede Xamarin.Forms XAML dosyaları yerel görünümleri kullanma gösterilmektedir._

Bu makalede aşağıdaki konular ele alınmıştır:

- [Yerel görünümleri kullanma](#consuming) – XAML yerel bir görünümden tüketimi için işlem.
- [Yerel bağlamalar kullanılarak](#native_bindings) – veri ve yerel görünümleri özelliklerini gelen bağlama.
- [Yerel görünümlerine bağımsız değişkenleri geçirme](#passing_arguments) – yerel görünüm kurucusuna bağımsız değişkenleri geçirme ve yerel görünüm Fabrika yöntemleri çağırma.
- [Yerel görünümlerine koddan başvuran](#native_view_code) – kendi arka plan kodu dosyasından bir XAML dosyası içinde bildirilen yerel görünüm örneklerini alma.
- [Yerel görünümleri sınıflara](#subclassing) – bir XAML kolay API tanımlamak üzere yerel görünümler alt sınıf yapma.  

<a name="overview" />

## <a name="overview"></a>Genel Bakış

Xamarin.Forms XAML dosyası yerel bir görünüme eklemek için:

1. Ekleme bir `xmlns` XAML dosyasında yerel görünümü içerir ad alanı için ad alanı bildirimi.
1. XAML dosyası içinde yerel görünümü örneği oluşturun.

> [!NOTE]
> XAMLC yerel görünümleri kullanan XAML sayfaları için kapalı olmalıdır.

Bir arka plan kodu dosyanın yerel bir görünüm başvurmak için bir paylaşılan varlık proje (SAP) kullanın ve platforma özgü kodu koşullu derleme yönergeleri ile sarmalayın. Daha fazla bilgi için bkz: [koddan yerel görünümlerine başvuran](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Yerel görünümleri kullanma

Aşağıdaki kod örneğinde, her platform için bir Xamarin.Forms için yerel görünümleri kullanma gösterilmektedir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

Belirtme yanı sıra `clr-namespace` ve `assembly` bir yerel görünüm ad alanı için bir `targetPlatform` de belirtilmesi gerekir. Bu değerlerden birine ayarlanmalıdır [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) numaralandırma ve genellikle ayarlanacak `iOS`, `Android`, veya `Windows`. Çalışma zamanında XAML ayrıştırıcısı sahip tüm XML ad alanı önekleri göz ardı edecek bir `targetPlatform` uygulama çalışmakta olduğu platform eşleşmiyor.

Her ad alanı bildiriminin belirtilen ad alanından herhangi bir sınıf veya yapı başvurmak için kullanılabilir. Örneğin, `ios` ad alanı bildirimi, herhangi bir sınıf veya yapı iOS başvurmak için kullanılabilir `UIKit` ad alanı. Yerel görünümün özelliklerini XAML ayarlanabilir, ancak özellik ve nesne türlerinin eşleşmesi gerekir. Örneğin, `UILabel.TextColor` özelliği ayarlanmış `UIColor.Red` kullanarak `x:Static` biçimlendirme uzantısı ve `ios` ad alanı.

Bağlanabilir özellikleri ve ekli bağlanabilir özellikleri de ayarlanabilir yerel görünümleri kullanarak `Class.BindableProperty="value"` sözdizimi. Her yerel görünüm platforma özgü içinde paketlenir `NativeViewWrapper` türetilen örneği [ `Xamarin.Forms.View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) sınıfı. Yerel bir görünüm üzerinde bağlanabilirse özelliği veya ekli bağlanabilirse özellik ayarı özellik değeri için sarmalayıcı aktarır. Örneğin, ortalanmış yatay düzen ayarlayarak belirtilebilir `View.HorizontalOptions="Center"` yerel görünüm.

> [!NOTE]
> Stilleri yerel görünümlerle kullanılamaz stilleri yalnızca tarafından yedeklenen özellikleri hedefleyebilir çünkü dikkate `BindableProperty` nesneleri.

Android pencere öğesi oluşturucular genellikle Android gerektirir `Context` nesne bir bağımsız değişken ve bu bir statik özellik aracılığıyla kullanılabilir hale getirilebilir olarak `MainActivity` sınıfı. Bu nedenle, bir Android pencere öğesi XAML'de oluştururken `Context` nesne gerekir genellikle bayraklarıdır pencere öğesi'nin oluşturucuya kullanarak `x:Arguments` ile öznitelik bir `x:Static` biçimlendirme uzantısı. Daha fazla bilgi için bkz: [bağımsız değişkenleri geçirme yerel görünümlere](#passing_arguments).

> [!NOTE]
> Bu yerel bir görünümle adlandırma Not `x:Name` taşınabilir sınıf kitaplığı (PCL) proje veya bir paylaşılan varlık proje (SAP) mümkün değildir. Bunun yapılması bir derleme hatası neden olacak yerel türünde bir değişken oluşturur. Ancak, yerel görünümler içinde sarmalamak `ContentView` örnekleri ve SAP kullanılan koşuluyla arka plan kod dosyasına alınır. Daha fazla bilgi için bkz: [koddan yerel bir görünüme başvuran](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Yerel bağlamaları

Veri bağlama UI kendi veri kaynağıyla eşitlemek için kullanılır ve nasıl bir Xamarin.Forms uygulaması görüntüler ve verileri ile etkileşim basitleştirir. Kaynak nesnesi uygulayan koşuluyla `INotifyPropertyChanged` arabirim, değişiklikleri *kaynak* nesne için kalan otomatik olarak *hedef* bağlama çerçevesi ve değişiklikleritarafındannesnesi*hedef* nesnesi isteğe bağlı olarak gönderilen için *kaynak* nesnesi.

Yerel görünümleri özelliklerini veri bağlama de kullanabilirsiniz. Aşağıdaki kod örneğinde yerel görünümleri özelliklerini kullanarak veri bağlama gösterilmektedir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

Sayfayı içeren bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) , [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) özelliği bağlanır `NativeSwitchPageViewModel.IsSwitchOn` özelliği. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Sayfasında yeni bir örneğine ayarlanmış `NativeSwitchPageViewModel` ViewModel uygulayan sınıf ile arka plan kodu dosyanın sınıfında `INotifyPropertyChanged` arabirimi.

Sayfa ayrıca her platform için yerel bir anahtar içerir. Her yerel anahtar kullanan bir [ `TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) değerini güncelleştirmek için bağlama `NativeSwitchPageViewModel.IsSwitchOn` özelliği. Bu nedenle, anahtar olduğunda kapalı, `Entry` devre dışı bırakıldı ve anahtar açıkken `Entry` etkinleştirilir. Aşağıdaki ekran görüntüleri bu işlevselliği her platformda göster:

![](xaml-images/native-switch-disabled.png "Yerel anahtar devre dışı")
![](xaml-images/native-switch-enabled.png "etkin yerel anahtarı")

Yerel özellik uygulayan koşuluyla, iki yönlü bağlamaları desteklenen otomatik olarak `INotifyPropertyChanged`, ya da iOS anahtar-değer Gözlemleme (KVO) destekleyen ya bir `DependencyProperty` UWP üzerinde. Bununla birlikte, birçok yerel görünümleri özellik değişikliği bildirimi desteklemez. Bu görünümler, belirttiğiniz bir [ `UpdateSourceEventName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.UpdateSourceEventName/) bağlama ifadesi bir parçası olarak özellik değeri. Bu özellik, hedef özelliği değiştiğinde sinyalleri yerel görünümü olayda adına ayarlanmalıdır. Ardından, yerel anahtar değeri değiştiğinde, `Binding` sınıfı, kullanıcı anahtar değeri değiştirdi bildirilir ve `NativeSwitchPageViewModel.IsSwitchOn` özellik değeri güncelleştirilir.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Yerel görünümlere bağımsız değişkenleri geçirme

Oluşturucu bağımsız değişkenleri kullanarak yerel görünümleri geçirilebilir `x:Arguments` ile öznitelik bir `x:Static` biçimlendirme uzantısı. Ayrıca, yerel görünüm Fabrika yöntemleri (`public static` nesneleri veya yöntemleri tanımlar yapısı ve sınıf olarak aynı türde değerler döndüren yöntemler) yöntemin belirterek çağrılabilir kullanarak ad `x:FactoryMethod` özniteliği ve bağımsız değişkenleri kullanarak `x:Arguments` özniteliği.

Aşağıdaki kod örneği iki tekniği gösterir:

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

[ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) Ayarlamak için kullanılan Üreteç yöntemi [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) yeni bir özellik [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) iOS. `UIFont` Adı ve boyutu alt yöntemi bağımsız değişken tarafından belirtilen `x:Arguments` özniteliği.

[ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) Ayarlamak için kullanılan Üreteç yöntemi [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) yeni bir özellik [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) android'de. `Typeface` Aile adı ve stil alt yöntemi bağımsız değişken tarafından belirtilen `x:Arguments` özniteliği.

[ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) Ayarlamak için kullanılan Oluşturucu [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) yeni bir özellik `FontFamily` Evrensel Windows Platformu (UWP) üzerinde. `FontFamily` Adı bir alt öğesi yöntem bağımsız değişkeni tarafından belirtilen `x:Arguments` özniteliği.

> [!NOTE]
> Bağımsız değişkenler Oluşturucusu veya Fabrika yöntemi tarafından gerekli türlerinin eşleşmesi gerekir.

Aşağıdaki ekran görüntüleri yazı tipini farklı yerel görünümleri ayarlamak için fabrika yöntemi ve oluşturucu bağımsız değişkenleri belirtme sonucu göster:

![](xaml-images/passing-arguments.png "Yazı tipleri yerel görünümleri ayarlama")

XAML'de bağımsız değişkenleri geçirme hakkında daha fazla bilgi için bkz: [bağımsız değişkenleri geçirme XAML'de](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Yerel görünümlerine koddan başvurma

Yerel bir görünümle ad mümkün olmasa da `x:Name` özniteliği, onu bir altyerelgörünümdürkoşuluyla,birpaylaşılanerişimprojesinde,arkaplankodudosyasındanXAMLdosyasıbildirilenbiryerelgörünümüörneğialmakmümkün[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) belirleyen bir `x:Name` öznitelik değeri. Ardından, koşullu derleme yönergeleri arka plan kod dosyasına içinde yapmanız gerekir:

1. Alma [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) özellik değeri ve platforma özgü cast `NativeViewWrapper` türü.
1. Alma `NativeViewWrapper.NativeElement` özelliği ve yerel görünüm türüne dönüştürün.

Yerel API, ardından istenen işlemleri gerçekleştirmek için yerel görünümde çağrılabilir. Ayrıca bu yaklaşımı farklı platformlar için birden çok XAML yerel görünüm aynı alt olabilir avantajını sunar [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Aşağıdaki kod örneği, bu tekniği gösterir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
            <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

Yukarıdaki örnekte, yerel her platform için alt görünümlerdir [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) denetimleri ile `x:Name` almak için kullanılan öznitelik değeri `ContentView` arka plan kod:

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Özelliği Sarmalanan yerel görünüm platforma özgü olarak almak için erişildiğinde `NativeViewWrapper` örneği. `NativeViewWrapper.NativeElement` Özelliği yerel görünüm doğal türü olarak almak için erişilen sonra. Yerel görünümün API, ardından istenen işlemleri gerçekleştirmek için çağrılır.

İOS ve Android yerel düğmeler aynı paylaşmak `OnButtonTap` olay işleyicisi her yerel düğmesi kullandığından bir `EventHandler` temsilci dokunma olaya yanıt olarak. Ancak, ayrı bir evrensel Windows Platformu (UWP) kullanan `RoutedEventHandler`, hangi sırayla tüketir `OnButtonTap` Bu örnekte olay işleyicisi. Bu nedenle, ne zaman bir yerel düğmesine tıklandığında, `OnButtonTap` olay işleyicisi yürütür, ölçeklendirir ve içindeki yerel denetim döndüğü [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) adlı `contentViewTextParent`. Aşağıdaki ekran görüntüleri, bu her platformda gerçekleştiğini gösterir:

![](xaml-images/contentview.png "Yerel bir denetimi içeren ContentView")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Yerel görünümler alt sınıf yapma

Birçok iOS ve Android yerel görünümleri denetimini Ayarla için özellikleri yerine yöntemleri kullanmak için XAML'de oluşturmak için uygun değildir. Bu sorun için bir alt kümesi yerel denetimi kurulumu için özelliklerini kullanır ve platformdan bağımsız olayları kullanan bir daha fazla bir XAML kolay API tanımlamak sarmalayıcıları görünümlerde için çözümüdür. Sarmalanan yerel görünümleri daha sonra bir paylaşılan varlık proje (SAP içinde) yerleştirilir ve koşullu derleme yönergeleri ile çevrelenmiş veya platforma özgü projelerinde yerleştirilir ve taşınabilir sınıf kitaplığı (PCL) projede XAML başvurulan.

Aşağıdaki kod örneğinde, yerel görünümler kullanan bir Xamarin.Forms sayfası sınıflandırma gösterilmektedir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

Sayfayı içeren bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) yerel denetimden kullanıcı tarafından seçilen ulaşılacak durumlardır görüntüler. `Label` Bağlar `SubclassedNativeControlsPageViewModel.SelectedFruit` özelliği. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Sayfasında yeni bir örneğine ayarlanmış `SubclassedNativeControlsPageViewModel` ViewModel uygulayan sınıf ile arka plan kodu dosyanın sınıfında `INotifyPropertyChanged` arabirimi.

Sayfa ayrıca her platform için bir yerel Seçici görünümü içerir. Her yerel fruits koleksiyonunu bağlayarak görüntüleyen kendi `ItemSource` özelliğine `SubclassedNativeControlsPageViewModel.Fruits` koleksiyonu. Aşağıdaki ekran görüntülerinde gösterildiği gibi kullanıcıya bir ulaşılacak durumlardır çekme sağlar:

![](xaml-images/sub-classed.png "Alt sınıflı yerel görünümleri")

İOS ve Android yerel seçiciler denetimleri kurulumu için yöntemlerini kullanın. Bu nedenle, bu seçiciler XAML kolay yapmak için özellikleri kullanıma sunmak için sınıflandırma gerekir. Üzerinde Evrensel Windows Platformu (UWP), `ComboBox` zaten XAML kolay ve bu nedenle sınıflara gerektirmez.

### <a name="ios"></a>iOS

İOS uygulaması alt sınıfların [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) görünümü ve çıkarır özelliklerini ve XAML kolayca kullanılabilecek bir olay:

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

`MyUIPickerView` Sınıf çıkarır `ItemsSource` ve `SelectedItem` özelliklerini ve `SelectedItemChanged` olay. A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) bir arka plandaki gerektirir [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) tarafından erişilen veri modeli `MyUIPickerView` özellikleri ve olay. `UIPickerViewModel` Veri modeli tarafından sağlanan `PickerModel` sınıfı:

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

`PickerModel` Sınıf için temel alınan depolama sağlar `MyUIPickerView` sınıfı aracılığıyla `Items` özelliği. Zaman içinde seçilen öğenin `MyUIPickerView` değişiklikleri [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) yöntemi yürütüldüğünde, hangi etkinleşir ve seçili dizin güncelleştirmelerin `ItemChanged` olay. Bu sağlar `SelectedItem` özelliği her zaman kullanıcı tarafından seçilen son öğeyi döndürür. Ayrıca, `PickerModel` sınıf kurulumu için kullanılan yöntemleri geçersiz kılar `MyUIPickerView` örneği.

### <a name="android"></a>Android

Android uygulaması alt sınıfların [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) görünümü ve çıkarır özelliklerini ve XAML kolayca kullanılabilecek bir olay:

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

`MySpinner` Sınıf çıkarır `ItemsSource` ve `SelectedObject` özelliklerini ve `ItemSelected` olay. Tarafından görüntülenen öğelerin `MySpinner` sınıfı tarafından sağlanan [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) görünüm ile ilişkilendirilen ve öğeleri içine doldurulur `Adapter` zaman `ItemsSource` özelliği ilk ayarlayın. Zaman içinde seçilen öğenin `MySpinner` sınıf değişiklikleri `OnBindableSpinnerItemSelected` olay işleyicisi güncelleştirmeleri `SelectedObject` özelliği.

## <a name="summary"></a>Özet

Bu makalede Xamarin.Forms XAML dosyaları yerel görünümleri kullanma gösterilmektedir. Xamarin.Forms görünümleri ile etkileşim kurabilir ve özellikleri ve olay işleyicileri yerel görünümler üzerinde ayarlanabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [NativeSwitch (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [Yerel Formlar](~/xamarin-forms/platform/native-forms.md)
- [XAML'de bağımsız değişkenleri geçirme](~/xamarin-forms/xaml/passing-arguments.md)
