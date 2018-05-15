---
title: 1. bölüm. XAML ile çalışmaya başlama
description: Bir Xamarin.Forms uygulaması XAML çoğunlukla bir sayfa ve bir arka plan kod dosyası birlikte çalışır visual içeriğini tanımlamak için kullanılır.
ms.prod: xamarin
ms.assetid: 9073FA0E-BD5A-4492-8A93-54C466F6EDB9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/10/2018
ms.openlocfilehash: 5883564841a4ef0e19518dd3b12ee00fe35ed778
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="part-1-getting-started-with-xaml"></a>1. bölüm. XAML ile çalışmaya başlama

_Bir Xamarin.Forms uygulaması XAML çoğunlukla bir sayfa ve C# arka plan kod dosyasına birlikte çalışır visual içeriğini tanımlamak için kullanılır._

Arka plan kodu dosya için biçimlendirme kodu desteği sağlar. Birlikte, bu iki dosya alt görünümleri ve özellik başlatma içeren yeni bir sınıf tanımına katkıda. XAML dosyası içinde sınıfları ve özellikleri XML öğeleri ve öznitelikleri ile başvurulan ve biçimlendirme ve kodun arasında bağlantı kurulur.

## <a name="creating-the-solution"></a>Çözüm oluşturma

İlk XAML dosyanızı düzenlemeye başlamak için yeni bir Xamarin.Forms çözüm oluşturmak için Visual Studio veya Mac için Visual Studio'ı kullanın. (Ortamınıza karşılık gelen aşağıda sekmesini seçin.)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Windows, Visual Studio seçmek için kullanın. **Dosya > Yeni > Proje** menüsünde. İçinde **yeni proje** iletişim kutusunda **Visual C# > Çapraz Platform** soldaki ve ardından **mobil uygulama (Xamarin.Forms)** merkezi listesinden. 

![](get-started-with-xaml-images/win/newprojectdialog.w157.png "Yeni Proje iletişim kutusu")

Çözüm için bir konum seçin, bir ad verin **XamlSamples** (veya ne olursa olsun, tercih ettiğiniz) ve basın **Tamam**.

Sonraki ekranda, seçin **boş uygulama** şablonu ve **.NET standart** kod paylaşımını stratejisi:

![](get-started-with-xaml-images/win/newcrossplatformapp.png "Yeni uygulama iletişim kutusu")

Press **OK**. 

Dört projeleri çözümde oluşturulur: **XamlSamples** .NET standart kitaplığı, **XamlSamples.Android**, **XamlSamples.iOS**ve evrensel Windows platformu Çözüm, **XamlSamples.UWP**.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da seçin **Dosya > Yeni bir çözüm** menüsünde. İçinde **yeni proje** iletişim kutusunda **Multiplatform > Uygulama** soldaki ve **boş Forms uygulaması** (*değil* **Forms uygulama** ) şablonu listeden:

![](get-started-with-xaml-images/mac/newprojectdialog1.png "Yeni Proje iletişim kutusu 1")

Tuşuna **sonraki**.

Sonraki iletişim kutusunda projenin adını verin **XamlSamples** (veya ne olursa olsun, tercih ettiğiniz). Olduğundan emin olun **kullanım .NET standart** radyo düğmesinin seçili:

![](get-started-with-xaml-images/mac/newprojectdialog2.png "Yeni Proje iletişim kutusu 2")

Tuşuna **sonraki**. 

Aşağıdaki iletişim kutusunda, proje için bir konum seçebilirsiniz:

![](get-started-with-xaml-images/mac/newprojectdialog3.png "Yeni Proje iletişim kutusu 3")

Tuşuna **oluşturma**

Üç projenin çözümde oluşturulur: **XamlSamples** .NET standart kitaplığı, **XamlSamples.Android**, ve **XamlSamples.iOS**. 

-----

Oluşturduktan sonra **XamlSamples** çözümü, çözüm başlangıç projesi olarak çeşitli platform projeleri seçerek geliştirme ortamınızı test etmek isteyebilirsiniz ve tarafından oluşturulan oluşturma ve basit uygulama dağıtma Proje şablonu telefon Öykünücüler ya da gerçek aygıtlar.

Paylaşılan platforma özgü kod yazmaya gerekmedikçe **XamlSamples** .NET standart kitaplığı projedir Burada, neredeyse tüm programlama zamanınızı harcama. Bu makaleler dışında bu proje girmemeyi.

### <a name="anatomy-of-a-xaml-file"></a>XAML dosyası anatomisi

İçinde **XamlSamples** .NET standart kitaplığı aşağıdaki adlara sahip dosyaların çifti şunlardır:

- **App.XAML**, XAML dosyası; ve
- **App.xaml.cs**, C# *arka plan kodu* XAML dosyayla ilişkili dosya.

Yanındaki oka tıklayın gerekir **App.xaml** için arka plan kod dosyasına bakın. 

Her ikisi de **App.xaml** ve **App.xaml.cs** adlı bir sınıf katkıda `App` , türetilen `Application`. Çoğu XAML dosyaları sınıflarıyla türeyen bir sınıf katkıda `ContentPage`; tüm sayfanın visual içeriği tanımlamak için XAML dosyaları kullanın. Bu dosyaların diğer iki geçerlidir **XamlSamples** proje:

- **MainPage.xaml**, XAML dosyası; ve
- **MainPage.xaml.cs**, C# arka plan kod dosyasına.

**MainPage.xaml** dosya şu şekilde görünür (biçimlendirme biraz farklı olabilir, ancak):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <StackLayout>
        <!-- Place new controls here -->
        <Label Text="Welcome to Xamarin Forms!" 
               VerticalOptions="Center" 
               HorizontalOptions="Center" />
    </StackLayout>

</ContentPage>
```

İki XML ad alanı ( `xmlns`) bildirimleri URI, Xamarin'ın web sitesinde gibi görünen ilk ve ikinci Microsoft'un bakın. Hangi bu URI noktasına denetimi rahatsız yok. Var. hiçbir şey yoktur. Yalnızca Xamarin ve Microsoft tarafından sahip olunan URI'ler oldukları ve bunlar temel sürümü tanımlayıcıları işlev.

Önek ile XAML dosyası içinde tanımlanan etiketleri sınıflara Xamarin.Forms, örneğin başvurmadığından ilk XML ad alanı bildirimi anlamına gelir `ContentPage`. İkinci ad alanı bildiriminin öneki tanımlar `x`. Bu kullanılan çeşitli öğeleri ve XAML için iç öznitelikleri kendisi ve hangi diğer XAML uygulamaları tarafından desteklenir. Ancak, bu öğeleri ve özniteliklerinin URI'de katıştırılmış yıl bağlı olarak biraz farklılık gösterir. Xamarin.Forms 2009 XAML belirtimi, ancak bunu tüm destekler.

`local` Ad alanı bildiriminin diğer sınıflar .NET standart kitaplığı projeden erişmenize olanak sağlar.

İlk metnimizi sonunda `x` öneki adlı bir özniteliği için kullanılan `Class`. Çünkü bu kullanımını `x` önektir XAML ad uzayı için XAML öznitelikleri neredeyse Evrensel gibi `Class` neredeyse her zaman denir `x:Class`.

`x:Class` Özniteliği, tam bir .NET sınıfı adı belirtir: `MainPage` sınıfını `XamlSamples` ad alanı. Bu XAML dosyası adlı yeni bir sınıf tanımlar yani `MainPage` içinde `XamlSamples` türetilen ad alanı `ContentPage`— etiketinde `x:Class` özniteliği görüntülenir.

`x:Class` Özniteliği yalnızca türetilmiş bir C# sınıf tanımlamak için bir XAML dosyasının kök öğesinin görünür. Bu XAML dosyasında tanımlanmış yalnızca yeni bir sınıftır. XAML dosyasında göründüğü herşeyi bunun yerine yalnızca varolan sınıflardan örneği ve başlatıldı.

**MainPage.xaml.cs** dosya şu şekilde görünür (kullanılmayan yanı sıra `using` yönergeleri):

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```

`MainPage` Sınıfı türer `ContentPage`, ancak fark `partial` sınıf tanımının. Bunun için başka bir parçalı sınıf tanımını olmalıdır öneren `MainPage`, ancak olduğu? Ve ne `InitializeComponent` yöntemi? 

Visual Studio proje oluşturduğunda, bir C# kod dosyası oluşturmak için XAML dosyası ayrıştırır. Bakarsanız **XamlSamples\XamlSamples\obj\Debug** dizin adlı bir dosya bulabilirsiniz **XamlSamples.MainPage.xaml.g.cs**. 'g' oluşturulan için anlamına gelir. Bu diğer parçalı sınıf tanımıdır `MainPage` tanımını içeren `InitializeComponent` yöntemi çağrılır `MainPage` Oluşturucusu. Bu iki kısmi `MainPage` sınıf tanımları sonra derlenmesi birlikte. XAML dosyası ya da bir ikili biçimindeki XAML dosyasının mı XAML veya derlenir bağlı olarak, yürütülebilir dosya katıştırılır.

Çalışma zamanında belirli platform proje çağrılarında kod bir `LoadApplication` için yeni bir örneğini geçirerek yöntemini `App` .NET standart Kitaplığı'nda sınıfı. `App` Sınıfı oluşturucusu başlatır `MainPage`. Bu sınıf çağırır `InitializeComponent`, ardından çağıran `LoadFromXaml` XAML dosyası (ya da kendi derlenmiş ikili) .NET standart Kitaplığı'ndan ayıklar yöntemi. `LoadFromXaml` XAML dosyası içinde tanımlanan tüm nesnelerini başlatır, bunları birlikte tüm üst-alt ilişkilerinde bağlandığında, XAML dosyasında ayarlanan olayları kodda tanımlanan olay işleyicileri ekler ve nesnelerin sonuç ağaç sayfasının içeriği ayarlar.

Normal olarak oluşturulan kod dosyalarıyla kadar zaman harcamanız gerekmez ancak bunlarla bilgi sahibi olmanız gerekir böylece bazen çalışma zamanı özel durumları oluşturulan dosyalar kodunda üzerinde oluşturulur.

Derleme ve bu program çalıştırıldığında `Label` öğe XAML da anlaşılacağı gibi sayfasının ortasında görünür. Üç soldan sağa iOS, Android ve UWP platformlarına:

[![](get-started-with-xaml-images/xamlsamples.png "Xamarin.Forms görüntü varsayılan")](get-started-with-xaml-images/xamlsamples-large.png#lightbox "Xamarin.Forms varsayılan görüntü")

Daha ilginç görseller için ihtiyacınız olan tek şey daha fazla XAML ilginç.

## <a name="adding-new-xaml-pages"></a>Yeni XAML sayfaları ekleme

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

XAML tabanlı diğer eklemek için `ContentPage` , projeniz sınıflarını seçin **XamlSamples** .NET standart kitaplığı proje ve çağırma **Proje > Yeni Öğe Ekle** menü öğesi. Sol tarafındaki **Yeni Öğe Ekle** iletişim kutusunda **Visual C#** ve **Xamarin.Forms**. Listeden seçin **içerik sayfasını** (değil **içerik sayfası (C#)**, yalnızca kod sayfası oluşturur veya **içerik görünümü**, bir sayfa değil). Örneğin, sayfa, bir ad verin **HelloXamlPage.xaml**:

![](get-started-with-xaml-images/win/addnewitemdialog.w157.png "Yeni öğe iletişim ekleyin")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

XAML tabanlı diğer eklemek için `ContentPage` , projeniz sınıflarını seçin **XamlSamples** .NET standart kitaplığı proje ve çağırma **Dosya > yeni dosya** menü öğesi. Sol tarafındaki **yeni dosya** iletişim kutusunda **Forms** soldaki ve **Forms ContentPage Xaml** (değil **Forms ContentPage**, hangi yalnızca kod sayfası oluşturur veya **içerik görünümü**, bir sayfa değil). Örneğin, sayfa, bir ad verin **HelloXamlPage**:

![](get-started-with-xaml-images/mac/newfiledialog.png "Yeni dosya iletişim kutusu")

-----

İki dosya projeye eklenen **HelloXamlPage.xaml** ve arka plan kodu dosya **HelloXamlPage.xaml.cs**. 

## <a name="setting-page-content"></a>Sayfa içeriği ayarlama

Düzen **HelloXamlPage.xaml** yalnızca etiketler için olanlar dosyasını `ContentPage` ve `ContentPage.Content`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>
        
    </ContentPage.Content>
</ContentPage>
```

`ContentPage.Content` Etiketleri XAML benzersiz söz dizimi bir parçasıdır. Öncelikle, geçersiz XML görünebilir, ancak yasal. Dönem XML özel bir karakter değil.

`ContentPage.Content` Etiketleri çağrılır *özellik öğesi* etiketler. `Content` bir özelliği `ContentPage`ve genellikle tek bir görünüm veya alt görünümleri bir düzen için özel olarak ayarlayın. Normalde özellikleri XAML öznitelikleri olur, ancak ayarlamak sabit bir `Content` özniteliği için karmaşık bir nesne. Bu nedenle, özellik sınıf adı ve bir noktayla ayrılmış özellik adı oluşan bir XML öğesi olarak ifade edilir. Şimdi `Content` özelliği, arasında ayarlanabilir `ContentPage.Content` etiketleri, şuna benzer:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage"
             Title="Hello XAML Page">
    <ContentPage.Content>

        <Label Text="Hello, XAML!"
               VerticalOptions="Center"
               HorizontalTextAlignment="Center"
               Rotation="-15"
               IsVisible="true"
               FontSize="Large"
               FontAttributes="Bold"
               TextColor="Blue" />

    </ContentPage.Content>
</ContentPage>
```

Ayrıca, fark bir `Title` kök etiketinde özniteliği ayarlandı.

Sınıfları, özellikleri ve XML arasında ilişki şu anda açık olması gerekir: bir Xamarin.Forms sınıfı (gibi `ContentPage` veya `Label`) XAML dosyası bir XML öğesi olarak görünür. Bu sınıf özelliklerini — dahil olmak üzere `Title` üzerinde `ContentPage` ve yedi özelliklerini `Label`— genellikle XML öznitelikleri görünür.

Bu özelliklerin değerlerini ayarlamak için birçok kısayolları mevcut. Bazı özellikler temel veri türleri şunlardır: Örneğin, `Title` ve `Text` özelliklerdir türü `String`, `Rotation` türü `Double`, ve `IsVisible` (olduğu `true` varsayılan olarak ve burada yalnızca için özel olarak ayarla Çizim) türüdür `Boolean`.

`HorizontalTextAlignment` Özelliği türüdür `TextAlignment`, numaralandırma olduğu. Herhangi bir numaralandırma türü özelliği için tüm tedarik gereken olan bir üye adı.

Daha karmaşık türlerin özellikleri için XAML ayrıştırmak için dönüştürücüler ancak kullanılır. Öğesinden türetilen Xamarin.Forms sınıflarda bunlar `TypeConverter`. Birçok ortak sınıflardır fakat bazıları güncellenmedi. Belirli bir XAML dosyası için birkaç bu sınıfların arka planda bir rol oynar:

-  `LayoutOptionsConverter` için `VerticalOptions` özelliği
-  `FontSizeConverter` için `FontSize` özelliği
-  `ColorTypeConverter` için `TextColor` özelliği

Bu dönüştürücüler izin verilen sözdizimi özellik ayarları yönetir.

`ThicknessTypeConverter` Bir, iki veya dört sayıları virgülle ayırarak işleyebilir. Bir sayı belirtilirse, dört kenara geçerlidir. İki sayı ile ilk sol ve sağ doldurma olduğunu ve üst ve alt saniyedir. Dört numaraları sipariş sol, üst, sağ ve alt bağlıdır.

`LayoutOptionsConverter` Ortak statik alanların adlarını dönüştürebilirsiniz `LayoutOptions` türü değerleri yapısına `LayoutOptions`.

`FontSizeConverter` İşleyebilecek bir `NamedSize` üyesi veya bir sayısal yazı tipi boyutu.

`ColorTypeConverter` Ortak statik alanların adlarını kabul `Color` yapısı veya onaltılık RGB değerleri olan veya olmayan bir alfa kanal öncesinde sayı işareti (#). Alfa kanal olmadan sözdizimi şöyledir:

 `TextColor="#rrggbb"`

Her bir küçük harf onaltılık bir sayıdır. İşte nasıl alfa kanalı bulunur:

 `TextColor="#aarrggbb">`

Alfa kanal için FF tamamıyla opak ve 00 tamamen saydam olduğunu göz önünde bulundurun.

Diğer iki biçim yalnızca tek bir onaltılık rakam her kanal belirtmenizi sağlar:

 `TextColor="#rgb"``TextColor="#argb"`

Bu durumlarda, değer oluşturmak üzere basamaklı yinelenir. Örneğin, #CF3 CC FF 33 RGB rengi olur.

## <a name="page-navigation"></a>Sayfa gezintisi

Çalıştırdığınızda **XamlSamples** program `MainPage` görüntülenir. Yeni görmek için `HelloXamlPage` sayfası, yeni başlangıç olarak ayarlayın ya da yapabilirsiniz **App.xaml.cs** dosya ya da yeni sayfasından gidin `MainPage`.

Gezinti uygulamak için önce kodda değişiklik **App.xaml.cs** oluşturucusu böylece bir `NavigationPage` nesnesi oluşturulur:

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

İçinde **MainPage.xaml.cs** oluşturucusu, oluşturabileceğiniz basit bir `Button` ve gitmek için olay işleyicisini kullanmak `HelloXamlPage`:

```csharp
public MainPage()
{
    InitializeComponent();

    Button button = new Button
    {
        Text = "Navigate!",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    button.Clicked += async (sender, args) =>
    {
        await Navigation.PushAsync(new HelloXamlPage());
    };

    Content = button;
}
```

Ayarı `Content` sayfanın özelliğini değiştirir ayarını `Content` XAML dosyası bir özellik. Derleme ve yeni sürümü bu program, dağıtmak, ekranda bir düğme görüntülenir. Bunu tuşuna basarak gider `HelloXamlPage`. İPhone, Android ve UWP sonuç sayfasında şöyledir:

[![](get-started-with-xaml-images/helloxaml1.png "Etiket metni Döndürülmüş")](get-started-with-xaml-images/helloxaml1-large.png#lightbox "etiket metnini Döndürülmüş")

Geri gidebilir `MainPage` kullanarak **< geri** sol oka sayfanın üst veya alt kısmındaki Android phone'da veya sol oka sayfanın en üstündeki Windows 10 kullanarak iOS, düğme.

XAML işlemek için farklı yollar için deneme çekinmeyin `Label`. Unicode karakterlerin metne katıştırmak gerekirse, standart XML söz dizimini kullanabilirsiniz. Örneğin, tebrik akıllı tırnak işaretleri içine alın için kullanın:

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

İşte bu şekilde görünür:

[![](get-started-with-xaml-images/helloxaml2.png "Etiket metni Unicode karakterler içeren Döndürülmüş")](get-started-with-xaml-images/helloxaml2-large.png#lightbox "Unicode karakterler içeren etiket metnini Döndürülmüş")

## <a name="xaml-and-code-interactions"></a>XAML ve kod etkileşimleri

**HelloXamlPage** örnek içeren tek bir `Label` sayfasında, ancak bu oldukça olağandışıdır. Çoğu `ContentPage` türevleri kümesi `Content` gibi bazı yerleşimini özelliğine sıralama bir `StackLayout`. `Children` Özelliği `StackLayout` türü olarak tanımlanan `IList<View>` ancak gerçekte bir nesne türü `ElementCollection<View>`, ve koleksiyon birden çok görünüm veya diğer düzenleri ile doldurulabilir. XAML'de normal XML hiyerarşisiyle bu üst-alt ilişkisi oluşturulur. Adlı yeni bir sayfa için XAML dosyası işte **XamlPlusCodePage**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Bu XAML dosyası sözdizimsel olarak tamamlandıktan ve İşte bu şekilde görünür:

[![](get-started-with-xaml-images/xamlpluscode1.png "Bir sayfa üzerinde birden çok denetim")](get-started-with-xaml-images/xamlpluscode1-large.png#lightbox "bir sayfada birden çok denetimleri")

Ancak, bu programın işlevsel olarak deficient olması için dikkate alınması gereken olasıdır. Belki de `Slider` neden olması `Label` geçerli değerini görüntülemek için ve `Button` büyük olasılıkla program içinde bir şeyler için tasarlanmıştır.

İçinde anlatıldığı gibi [bölümü 4. Veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md), görüntüleme iş bir `Slider` kullanarak değer bir `Label` tamamen XAML içinde veri bağlama ile işlenebilir. Ancak çözüm kod ilk görmek yararlıdır. Buna rağmen işleme `Button` tıklatın kesinlikle kodu gerektirir. Arka plan kodu dosyayı buna `XamlPlusCodePage` için işleyiciler içermelidir `ValueChanged` olayı `Slider` ve `Clicked` olayı `Button`. Bunları ekleyelim:

```csharp
namespace XamlSamples
{
    public partial class XamlPlusCodePage
    {
        public XamlPlusCodePage()
        {
            InitializeComponent();
        }

        void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
        {

        }

        void OnButtonClicked(object sender, EventArgs args)
        {

        }
    }
}
```

Bu olay işleyicileri ortak olması gerekmez.

Geri XAML dosyasındaki `Slider` ve `Button` etiketleri öznitelikleri yer gerek `ValueChanged` ve `Clicked` bu işleyiciler başvuru olayları:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Clicked="OnButtonClicked" />
    </StackLayout>
</ContentPage>
```

Bir olay için bir işleyici atama bir özellik için bir değer atama olarak aynı sözdizimini olduğuna dikkat edin.

Varsa işleyicisi `ValueChanged` olayı `Slider` kullanacağınız `Label` geçerli değerini görüntülemek için işleyici söz konusu nesne koddan başvurmalıdır. `Label` İle belirtilen bir adı olmalıdır `x:Name` özniteliği.

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

`x` Öneki `x:Name` özniteliği, bu öznitelik için XAML iç olduğunu gösterir.

Atamak için ad `x:Name` özniteliğine sahip C# değişken adları olarak aynı kuralları. Örneğin, bir harf veya alt çizgi ile başlamalı ve hiç katıştırılmış boşluklar içeremez gerekir.

Şimdi `ValueChanged` olay işleyicisi ayarlayabilirsiniz `Label` yeni görüntülenecek `Slider` değeri. Yeni değer olay bağımsız değişkenlerden kullanılabilir:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

Ya da işleyicinin elde edilemedi `Slider` bu olaydan oluşturma nesnesi `sender` bağımsız değişkeni ve elde `Value` , özelliğinden:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

Programı ilk kez çalıştırdığınızda `Label` görüntülemez `Slider` çünkü değer `ValueChanged` olay henüz şu kurmadı. Ancak herhangi bir değişiklik `Slider` değeri görüntülenmesine neden olur:

[![](get-started-with-xaml-images/xamlpluscode2.png "Görüntülenen kaydırıcı değeri")](get-started-with-xaml-images/xamlpluscode2-large.png#lightbox "görüntülenen kaydırıcı değeri")

İçin şimdi `Button`. Şimdi bir yanıta benzetimini bir `Clicked` olan bir uyarı görüntüleyerek olay `Text` düğmesinin. Olay işleyicisi güvenli bir şekilde atama `sender` bağımsız değişkeni bir `Button` ve özelliklerini erişebilirsiniz:

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

Yöntemi olarak tanımlanır `async` çünkü `DisplayAlert` yöntemi zaman uyumsuz ve ile başlayan `await` yöntemi tamamlandığında döndüren işleci. Bu yöntem alındığından `Button` olay tetikleme `sender` bağımsız değişkeni, aynı işleyici birden çok düğmeleri için kullanılabilir.

XAML içinde tanımlanan bir nesne arka plan kod dosyasına işlenen olay tetikleyebilir ve arka plan kodu dosya ile atanan adı kullanarak XAML içinde tanımlanan bir nesne erişebilir gördünüz `x:Name` özniteliği. Bu kod ve XAML etkileşime iki temel yöntemleri sunulmaktadır.

İçine nasıl XAML works yeni oluşturulan inceleyerek konusunda bazı ek Öngörüler **XamlPlusCode.xaml.g.cs dosya**, şimdi herhangi birine atanan tüm adını içeren `x:Name` özel bir alan olarak özniteliği. Bu dosyayı basitleştirilmiş bir sürümünü şöyledir:

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

Bu alan bildirimi serbestçe herhangi bir yere içinde kullanılacak değişken verir `XamlPlusCodePage` kısmi sınıf dosyası, dairesi altında. XAML Ayrıştırılan sonra çalışma zamanında alanı atanır. Bunun anlamı `valueLabel` alandır `null` zaman `XamlPlusCodePage` Oluşturucusu başlar ancak geçerli sonra `InitializeComponent` olarak adlandırılır.

Sonra `InitializeComponent` döndürür denetiminde geri oluşturucuya, yalnızca bunlar var örneği ve kodda başlatılmış sanki sayfasının görselleri yapılandırılmış. XAML dosyası artık sınıfında herhangi bir rol oynar. Bu sayfadaki, örneğin, görünümlere ekleyerek istediğiniz herhangi bir şekilde işleyebilir `StackLayout`, veya ayar `Content` başka bir sayfaya özelliği tamamen. "Ağaç inceleyerek yol" `Content` özellik sayfasının ve öğeleri `Children` düzenleri topluluğu. Bu şekilde erişilen görünümleri özelliklerini ayarlamak veya olay işleyicileri dinamik olarak atayabilirsiniz.

Çekinmeyin. Sayfanız olduğunu ve XAML içeriği oluşturmak için yalnızca bir araçtır.

## <a name="summary"></a>Özet

Bu giriş ile XAML dosyasını ve kod dosyası için bir sınıf tanımı nasıl katkıda ve XAML ve kod dosyaları nasıl etkileşim gördünüz. Ancak XAML daha çok esnek bir şekilde kullanılacak izin kendi benzersiz söz dizimi özelliklere sahiptir. Bu araştırma başlayabilirsiniz [Kısım 2. Temel XAML sözdizimi](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md).



## <a name="related-links"></a>İlgili bağlantılar

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Bölüm 2. Temel XAML Sözdizimi](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Bölüm 3. XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Bölüm 4. Temel Veri Bağlama Bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Bölüm 5. Verileri için MVVM bağlama](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
