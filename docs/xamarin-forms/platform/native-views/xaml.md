---
title: XAML içindeki yerel görünümler
description: Yerel iOS, Android ve evrensel Windows platformu görünümlerinden Xamarin.Forms XAML dosyaları doğrudan başvurulabilir. Özellikler ve olay işleyicileri yerel görünümler üzerinde ayarlanabilir ve Xamarin.Forms görünümleri ile etkileşim kurabilir. Bu makalede Xamarin.Forms XAML dosyaları yerel görünümleri kullanma gösterilmektedir.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: 4afdf1210a435e4631b1fe43e9415f4f9f599350
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935497"
---
# <a name="native-views-in-xaml"></a>XAML içindeki yerel görünümler

_Yerel iOS, Android ve evrensel Windows platformu görünümlerinden Xamarin.Forms XAML dosyaları doğrudan başvurulabilir. Özellikler ve olay işleyicileri yerel görünümler üzerinde ayarlanabilir ve Xamarin.Forms görünümleri ile etkileşim kurabilir. Bu makalede Xamarin.Forms XAML dosyaları yerel görünümleri kullanma gösterilmektedir._

Bu makalede, aşağıdaki konular ele alınmaktadır:

- [Yerel görünümler kullanan](#consuming) – yerel bir XAML görünümünde kullanma işlemi.
- [Yerel bağlamalar kullanılarak](#native_bindings) – veri yerel görünümler özelliklerini gelen ve giden bağlama.
- [Yerel görünümler için bağımsız değişkenleri geçirme](#passing_arguments) – yerel görünümü oluşturucular bağımsız değişkenleri geçirme ve yerel görünümü Fabrika yöntemleri çağırma.
- [Yerel görünümler için koddan başvuran](#native_view_code) –, arka plan kod dosyasından bir XAML dosyasında bildirilen yerel görünümü örneklerini alma.
- [Yerel görünümler sınıflara](#subclassing) – XAML kullanımı kolay bir API'ye tanımlamak için yerel görünümler alt sınıf yapma.  

<a name="overview" />

## <a name="overview"></a>Genel Bakış

Xamarin.Forms XAML dosyasının yerel bir görünüme eklemek için:

1. Ekleme bir `xmlns` yerel görünüm içeren ad uzayı için XAML dosyasında ad alanı bildirimi.
1. XAML dosyasında yerel görünümü örneği oluşturun.

> [!NOTE]
> Yerel görünümler kullanan XAML sayfaları için XAMLC kapatılmalıdır.

Yerel bir görünüm bir arka plan kod dosyasından başvurmak için paylaşılan varlık projesi (SAP) kullanın ve platforma özgü kod koşullu derleme yönergeleri ile kaydır. Daha fazla bilgi için [koddan yerel görünümlerle başvuran](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Yerel görünümler kullanma

Aşağıdaki kod örneği, bir Xamarin.Forms için her platform için yerel görünümler kullanan gösterir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/):

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

Belirtme yanı sıra `clr-namespace` ve `assembly` yerel görünümü ad alanı için bir `targetPlatform` de belirtilmelidir. Bu değerlerden birine ayarlanmalıdır [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) numaralandırma ve genellikle ayarlanacak `iOS`, `Android`, veya `Windows`. Çalışma zamanında, XAML ayrıştırıcı sahip herhangi bir XML ad alanı öneklerini yoksayacak bir `targetPlatform` uygulamasının çalıştığı platforma eşleşmiyor.

Her ad alanı bildirimi, belirtilen ad alanından herhangi bir sınıf veya yapının başvurmak için kullanılabilir. Örneğin, `ios` ad alanı bildirimi, herhangi bir sınıf veya yapının iOS başvurmak için kullanılabilir `UIKit` ad alanı. Yerel görünümün özelliklerini XAML ayarlanabilir, ancak özellik ve nesne türlerinin eşleşmesi gerekir. Örneğin, `UILabel.TextColor` özelliği `UIColor.Red` kullanarak `x:Static` işaretleme uzantısı ve `ios` ad alanı.

Bağlanabilir özellikler ve ekli bağlanabilir özellikler de ayarlanabilir yerel görünümleri kullanarak `Class.BindableProperty="value"` söz dizimi. Platforma özgü yerel görünümler sarmalandıktan `NativeViewWrapper` türetilen örneği [ `Xamarin.Forms.View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) sınıfı. Özellik değeri, ayarı yerel bir görünüm üzerinde bir bağlanılabilir özellik ya da ekli bağlanılabilir özellik için bir sarmalayıcı aktarır. Örneğin, ortalanmış bir yatay düzeni ayarlayarak belirtilebilir `View.HorizontalOptions="Center"` yerel görünümü.

> [!NOTE]
> Stilleri tarafından desteklenen özellikler yalnızca hedefleyebilir stilleri yerel görünümlerle kullanılamaz dikkat edin çünkü `BindableProperty` nesneleri.

Android pencere öğesi kurucular genellikle Android gerektirir `Context` nesnesi bağımsız değişken ve bu statik özellik aracılığıyla kullanılabilir hale getirilebilir olarak `MainActivity` sınıfı. Bu nedenle, bir Android pencere öğesi, XAML içinde oluştururken `Context` nesne pencere öğesinin oluşturucuya genel olarak geçirilmelidir kullanarak `x:Arguments` özniteliğini bir `x:Static` işaretleme uzantısı. Daha fazla bilgi için [Passing Arguments yerel görünümler için](#passing_arguments).

> [!NOTE]
> Bu yerel bir görünümle adlandırma Not `x:Name` bir .NET Standard kitaplığı projesi veya paylaşılan varlık projesi (SAP) mümkün değildir. Bunun yapılması, bir derleme hatasına neden olur yerel türünde bir değişken oluşturur. Ancak, yerel görünümler içinde sarmalanabilir `ContentView` örnekler ve SAP kullanılıyor olması koşuluyla arka plan kod dosyasında alınır. Daha fazla bilgi için [koddan yerel görünümüne başvuran](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Yerel bağlama

Veri bağlama, bir kullanıcı Arabirimi kendi veri kaynağı ile eşitlemek için kullanılır ve nasıl bir Xamarin.Forms uygulaması görüntüler ve kendi verilerle etkileşim basitleştirir. Kaynak nesne uygulayan koşuluyla `INotifyPropertyChanged` arabirim, değişiklikleri *kaynak* nesne için otomatik olarak itilir *hedef* bağlama çerçevesine ve değişikliklerinesnesiyle*hedef* nesnesi isteğe bağlı olarak gönderilen için *kaynak* nesne.

Yerel görünümler özelliklerini de veri bağlamayı kullanabilirsiniz. Aşağıdaki kod örneği, yerel görünümler özelliklerini kullanarak veri bağlama gösterilmektedir:

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

Sayfayı içeren bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) olan [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) özelliği bağlanır `NativeSwitchPageViewModel.IsSwitchOn` özelliği. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Sayfasında yeni bir örneğine ayarlanır `NativeSwitchPageViewModel` ViewModel uygulayan sınıf ile arka plan kod dosyasında, sınıf `INotifyPropertyChanged` arabirimi.

Sayfa aynı zamanda her platform için yerel bir anahtar içeriyor. Her yerel anahtar kullanan bir [ `TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay) değerini güncelleştirmek için bağlama `NativeSwitchPageViewModel.IsSwitchOn` özelliği. Bu nedenle, anahtar olduğunda kapalı `Entry` devre dışı bırakıldı ve anahtar açıkken `Entry` etkinleştirilir. Aşağıdaki ekran görüntüleri, bu işlev her platformda göster:

![](xaml-images/native-switch-disabled.png "Yerel anahtar devre dışı")
![](xaml-images/native-switch-enabled.png "yerel anahtar etkin")

Yerel özelliği uygulayan şartıyla, iki yönlü bağlamaları desteklenen otomatik olarak `INotifyPropertyChanged`, anahtar-değer gözleme (KVO) İos'ta destekler veya gibi bir `DependencyProperty` UWP üzerinde. Ancak, birçok yerel görünümler özellik değişikliği bildirimi desteklemeyen. Bu görünümler için belirttiğiniz bir [ `UpdateSourceEventName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.UpdateSourceEventName/) bağlama ifadesi bir parçası olarak özellik değeri. Bu özellik bir olay görünümünde hedef özelliği değiştiğinde bildirir yerel adına ayarlanmalıdır. Ardından, yerel anahtar değeri değiştiğinde, `Binding` sınıfı, kullanıcı anahtar değeri değiştirildi bildirilir ve `NativeSwitchPageViewModel.IsSwitchOn` özellik değeri güncelleştirilir.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Yerel görünümler için bağımsız değişkenleri geçirme

Oluşturucu bağımsız değişkenleri kullanarak yerel görünümler geçirilebilir `x:Arguments` özniteliğini bir `x:Static` işaretleme uzantısı. Ayrıca, yerel görünümü Fabrika yöntemleri (`public static` nesneler veya değerleri aynı türde bir sınıf ya da yöntemlerini yapı döndüren yöntemler) yöntemin belirterek çağrılabilir kullanarak ad `x:FactoryMethod` özniteliği ve bağımsız değişkenleri kullanarak `x:Arguments` özniteliği.

Aşağıdaki kod örneği, iki teknik gösterilmektedir:

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

[ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) Ayarlamak için kullanılan Üreteç yöntemi [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) yeni bir özellik [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) ios'ta. `UIFont` Adını ve boyutunu alt öğesi olan yöntemi bağımsız değişken tarafından belirtilen `x:Arguments` özniteliği.

[ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) Ayarlamak için kullanılan Üreteç yöntemi [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) yeni bir özellik [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) Android. `Typeface` Aile adı ve stil alt öğesi olan yöntemi bağımsız değişken tarafından belirtilen `x:Arguments` özniteliği.

[ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) Ayarlamak için kullanılan Oluşturucu [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) yeni bir özellik `FontFamily` Evrensel Windows Platformu (UWP) üzerinde. `FontFamily` Adı alt yöntemi bağımsız değişken tarafından belirtilen `x:Arguments` özniteliği.

> [!NOTE]
> Bağımsız değişken oluşturucu veya Fabrika yöntemi tarafından gerekli olan türleri eşleşmelidir.

Aşağıdaki ekran görüntüleri, farklı yerel görünümleri yazı ayarlamak için Fabrika yöntem ve oluşturucu bağımsız değişkenleri belirtme sonucu göster:

![](xaml-images/passing-arguments.png "Yerel görünümler yazı tiplerini ayarlama")

XAML bağımsız değişkenleri geçirme hakkında daha fazla bilgi için bkz: [Passing Arguments XAML içinde](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Koddan yerel görünümler için başvuru

Yerel bir görünümle ad mümkün olmasa da `x:Name` özniteliği mümkündür yerel görünümün alt öğesi bir olmasışartıyla,arkaplankoddosyasıbirpaylaşılanerişimprojeiçindebirXAMLdosyasındabildirilenbiryerelgörünümüörneğialınacak[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) belirten bir `x:Name` öznitelik değeri. Ardından, koşullu derleme yönergeleri arka plan kod dosyasında içinde yapmanız gerekir:

1. Alma [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) özellik değeri ve platforma özgü cast `NativeViewWrapper` türü.
1. Alma `NativeViewWrapper.NativeElement` özelliği ve yerel görünüm türüne.

Yerel API, ardından istenen işlemleri gerçekleştirmek için yerel görünümde çağrılabilir. Ayrıca bu yaklaşım farklı platformlar için birden fazla XAML yerel görünümler aynı alt olabilir avantajını sunar [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Aşağıdaki kod örneği, bu tekniği gösterir:

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

Yukarıdaki örnekte, her platform için yerel görünümler alt öğeleri olan [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) denetimleri ile `x:Name` almak için kullanılan öznitelik değeri `ContentView` arka plan kod içinde:

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

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Sarmalanan yerel görünüm platforma özgü olarak alınacak özelliğine erişinceye `NativeViewWrapper` örneği. `NativeViewWrapper.NativeElement` Özelliği yerel görünüm doğal türü olarak alınacak erişildiği sonra. Yerel görünümün API, ardından istenen işlemleri gerçekleştirmek için çağrılır.

İOS ve Android yerel düğmeleri aynı paylaşmak `OnButtonTap` olay işleyicisi, yerel düğmelerin kullandığından bir `EventHandler` temsilci touch olaya yanıt olarak. Ancak, ayrı bir evrensel Windows Platformu (UWP) kullanan `RoutedEventHandler`, hangi sırayla tüketir `OnButtonTap` Bu örnekte olay işleyicisi. Bu nedenle, ne zaman bir yerel düğmesine tıklandığında, `OnButtonTap` olay işleyicisi yürütür, ölçeklendirilebilen ve içinde yer alan yerel denetim döndürür [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) adlı `contentViewTextParent`. Aşağıdaki ekran görüntüleri, bu her platformda oluşma gösterilmektedir:

![](xaml-images/contentview.png "Yerel bir denetimi içeren ContentView")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Yerel görünümler alt sınıf yapma

Çok sayıda iOS ve Android yerel görünümler denetimini ayarlamak için yöntemler yerine özellikleri kullanmak için XAML içinde oluşturmak için uygun değildir. Bu sorunun çözümü platformdan bağımsız olayları kullanan ve denetimi ayarlama özellikleri kullanan daha fazla bir XAML kullanımı kolay API tanımlama sarmalayıcıları içindeki yerel görünümler alt sağlamaktır. Sarmalanan yerel görünümler sonra yerleştirilen bir paylaşılan varlık projesi (SAP içinde) ve koşullu derleme yönergeleri ile çevrelenmiş veya platforma özgü projelerinde yerleştirilir ve XAML bir .NET Standard kitaplığı projesinde başvurulan.

Aşağıdaki kod örneği, yerel görünümler kullanan bir Xamarin.Forms sayfa sınıflandırma gösterir:

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

Sayfayı içeren bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) yerel denetiminden kullanıcı tarafından seçmiş Meyve görüntüler. `Label` Bağlar `SubclassedNativeControlsPageViewModel.SelectedFruit` özelliği. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Sayfasında yeni bir örneğine ayarlanır `SubclassedNativeControlsPageViewModel` ViewModel uygulayan sınıf ile arka plan kod dosyasında, sınıf `INotifyPropertyChanged` arabirimi.

Sayfa, her platform için bir yerel Seçici görünümü de içerir. Bağlama tarafından MEYVELERİ koleksiyonunu görüntüleyen yerel her görünüm kendi `ItemSource` özelliğini `SubclassedNativeControlsPageViewModel.Fruits` koleksiyonu. Aşağıdaki ekran görüntülerinde gösterildiği gibi bu kullanıcının bir Meyve sağlar:

![](xaml-images/sub-classed.png "Alt sınıf yerel görünümler")

İOS ve Android'de yerel seçiciler denetimleri kurulumu için yöntemlerini kullanın. Bu nedenle, bunları XAML kullanımı kolay hale getirmek için özellikleri göstermek için bu seçiciler sınıflandırılacak gerekir. Üzerindeki Evrensel Windows Platformu (UWP), `ComboBox` zaten XAML kullanımı kolay ve bu nedenle sınıflara gerektirmez.

### <a name="ios"></a>iOS

İOS uygulaması alt sınıflarından [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) görünümü ve özellikleri kullanıma sunan ve XAML kolayca kullanılabilecek olay:

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

`MyUIPickerView` Sınıfı kullanıma sunan `ItemsSource` ve `SelectedItem` özellikleri ve `SelectedItemChanged` olay. A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) bir temel alınan gerektirir [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) tarafından erişilen veri modeli `MyUIPickerView` özellikleri ve olay. `UIPickerViewModel` Veri modeli tarafından sağlanır `PickerModel` sınıfı:

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

`PickerModel` Sınıf için temel alınan depolama sağlar `MyUIPickerView` sınıfı aracılığıyla `Items` özelliği. Zaman içinde seçilen öğenin `MyUIPickerView` değişiklikleri [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) yöntemi yürütüldüğünde, hangi güncelleştirmelerin etkinleşir ve seçili dizin `ItemChanged` olay. Bu, sağlar `SelectedItem` özelliği her zaman kullanıcı tarafından seçilen son öğeyi döndürür. Ayrıca, `PickerModel` sınıfı kurulumu için kullanılan yöntemleri geçersiz kılan `MyUIPickerView` örneği.

### <a name="android"></a>Android

Android uygulaması alt sınıflarından [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) görünümü ve özellikleri kullanıma sunan ve XAML kolayca kullanılabilecek olay:

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

`MySpinner` Sınıfı kullanıma sunan `ItemsSource` ve `SelectedObject` özellikleri ve `ItemSelected` olay. Tarafından görüntülenen öğelerin `MySpinner` sınıfı tarafından sağlanan [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) görünüm ile ilişkilendirilen ve öğeleri içine doldurulur `Adapter` olduğunda `ItemsSource` özelliği ilk kez ayarlanır. Her seçili öğenin `MySpinner` sınıfı değişiklikleri `OnBindableSpinnerItemSelected` olay işleyicisi güncelleştirmeleri `SelectedObject` özelliği.

## <a name="summary"></a>Özet

Bu makalede Xamarin.Forms XAML dosyaları yerel görünümleri kullanma gösterilmektedir. Özellikler ve olay işleyicileri yerel görünümler üzerinde ayarlanabilir ve Xamarin.Forms görünümleri ile etkileşim kurabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [NativeSwitch (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [Yerel Formlar](~/xamarin-forms/platform/native-forms.md)
- [XAML bağımsız değişkenleri geçirme](~/xamarin-forms/xaml/passing-arguments.md)
