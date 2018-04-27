---
title: Yazı Tipleri
description: Xamarin.Forms ayarı yazı tipleri
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 52c86c63c328729211c4fbd22bd10b5eb1e56615
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="fonts"></a>Yazı Tipleri

Bu makalede nasıl Xamarin.Forms yazı tipi özniteliklerini (ağırlık ve boyuta dahil) belirtmenize olanak sağlar. metin görüntüleme denetimlere açıklanmaktadır. Yazı tipi bilgileri olabilir [kodunda belirtilen](#Setting_Font_in_Code) veya [XAML'de belirtilen](#Setting_Font_in_Xaml).
Kullanmak da mümkündür bir [özel yazı tipi](#Using_a_Custom_Font).

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>Yazı tipi kodda ayarlama

Metin görüntüleme herhangi bir denetim yazı tipi ile ilgili üç özelliklerini kullanın:

- **FontFamily** &ndash; `string` yazı tipi adı.
- **FontSize** &ndash; olarak yazı tipi boyutu bir `double`.
- **FontAttributes** &ndash; gibi stil bilgileri belirten bir dize *italik* ve **kalın** (kullanarak `FontAttributes` numaralandırma C#).

Bu kod, bir etiket oluşturmak ve yazı tipi boyutunu ve görüntülemek için ağırlığını belirlemek üzere gösterilmektedir:

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Yazı tipi boyutu

`FontSize` Özelliği ayarlanabilir bir çift değere örneği için:

```csharp
label.FontSize = 24;
```

Aynı zamanda `NamedSize` dört yerleşik seçenekleri; olan numaralandırması Xamarin.Forms her platform için en iyi boyutu seçer.

-  **Mikro**
-  **Küçük**
-  **Orta**
-  **Büyük**


`NamedSize` Numaralandırma olabilir yerde kullanılan bir `FontSize` kullanılarak belirtilebilir `Device.GetNamedSize` değerine dönüştürmek için yöntem bir `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Yazı tipi özniteliklerini

Yazı tipi stilleri gibi **kalın** ve *italik* ayarlanabilen `FontAttributes` özelliği. Aşağıdaki değerleri şu anda desteklenir:

-  **Yok**
-  **Kalın**
-  **İtalik**

`FontAttribute` Numaralandırma gibi kullanılabilir (tek bir öznitelik belirtebilirsiniz veya `OR` birlikte bunları):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="formattedstring"></a>FormattedString

Bazı Xamarin.Forms denetimler (gibi `Label`) kullanarak bir dize içindeki farklı yazı tipi özniteliklerini de destekler `FormattedString` sınıfı. A `FormattedString` bir-veya-daha fazla bilgi oluşur `Span`s, her biri kendi biçimlendirme öznitelikleri olabilir.

`Span` Sınıfı aşağıdaki özniteliklere sahiptir:

* **Metin** &ndash; görüntülenecek değer
* **FontFamily** &ndash; yazı tipi adı
* **FontSize** &ndash; yazı tipi boyutu
* **FontAttributes** &ndash; stil bilgilerini ister *italik* ve **kalın**
* **ForegroundColor** &ndash; metin rengi
* **BackgroundColor** &ndash; arka plan rengi

Oluşturma ve görüntüleme örneği bir `FormattedString` aşağıda gösterilen &ndash; etiketlerine atanan Not `FormattedText` özelliği ve `Text` özelliği.

```csharp
var labelFormatted = new Label ();
var fs = new FormattedString ();
fs.Spans.Add (new Span { Text="Red, ", ForegroundColor = Color.Red, FontSize = 20, FontAttributes = FontAttributes.Italic });
fs.Spans.Add (new Span { Text=" blue, ", ForegroundColor = Color.Blue, FontSize = 32 });
fs.Spans.Add (new Span { Text=" and green!", ForegroundColor = Color.Green, FontSize = 12 });
labelFormatted.FormattedText = fs;
```


### <a name="setting-font-info-per-platform"></a>Her Platform yazı tipi bilgilerini ayarlama

Alternatif olarak, `Device.RuntimePlatform` özelliği kullanılabilir her platformda farklı yazı tipi adlarını ayarlamak için bu kodda gösterildiği gibi:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

İOS için yazı tipi bilgileri iyi kaynağıdır [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>XAML'de yazı tipini ayarlama

Xamarin.Forms denetimleri tüm görüntüleme metni bir `Font` XAML'de ayarlanabilir özelliği. XAML'de yazı tipini ayarlamak için en basit yolu adlandırılmış boyutu numaralandırma değerlerinin bu örnekte gösterildiği gibi kullanmaktır:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

İçin yerleşik bir dönüştürücü yoktur `Font` tüm yazı tipi ayarlarını XAML'de bir dize değeri olarak ifade edilebilir imkan tanıyan özellik. Aşağıdaki örnek XAML'de nasıl yazı tipi özniteliklerini ve boyutları belirtebilirsiniz göster:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Birden çok belirtmek için `Font` ayarları, gerekli ayarları tek yazı tipi özniteliği dizeye birleştirin. Yazı tipi öznitelik dize olarak biçimlendirilmiş olması `"[font-face],[attributes],[size]"`. Parametreler sırası önemlidir, tüm parametreler isteğe bağlıdır ve birden çok `attributes` , örneğin belirtilebilir:

```xaml
<Label Text="Small bold text" FontAttributes="Bold" FontSize="Micro" />
<Label Text="Really big italic text" FontAttributes="Italic" FontSize="72" />
```

`FormattedString` Sınıfı ayrıca kullanılabilir XAML'de, aşağıda gösterildiği gibi:

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <FormattedString.Spans>
                <Span Text="Red, " ForegroundColor="Red" FontAttributes="Italic" FontSize="20" />
                <Span Text=" blue, " ForegroundColor="Blue" FontSize="32" />
                <Span Text=" and green! " ForegroundColor="Green" FontSize="12"/>
            </FormattedString.Spans>
        </FormattedString>
    </Label.FormattedText>
</Label>
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) Ayrıca XAML'de farklı bir yazı tipi her platformda işlemek için kullanılabilir. Aşağıdaki örnek, iOS özel yazı tipi kullanır (<span style="font-family:MarkerFelt-Thin">MarkerFelt ince</span>) ve diğer platformlarda yalnızca boyut/özniteliklerini belirtir:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

Özel yazı tipi belirtirken, her zaman kullanmak için iyi bir fikirdir `OnPlatform`, tüm platformlarda kullanılabilir bir yazı tipi bulmak zor olduğu gibi.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>Özel yazı tipi kullanma

Yerleşik yazı dışında bir yazı tipi kullanarak bazı platforma özgü kodlama gerektirir. Bu ekran özel yazı tipi gösterir **Lobster** gelen [Google açık kaynak yazı tipleri](https://www.google.com/fonts) Xamarin.Forms kullanılarak işlenir.

 [![İOS ve Android özel yazı tipi](fonts-images/custom-sml.png "özel yazı tipleri örnek")](fonts-images/custom.png#lightbox "özel yazı tipi örneği")

Her platform için gerekli adımlar aşağıda özetlenmiştir. Bir uygulama ile özel yazı tipi dosyalarını dahil ederken yazı tipinin lisans dağıtım için izin verdiğini doğrulamak emin olun.

### <a name="ios"></a>iOS

Özel yazı tipi ilk yüklenen emin olduktan sonra Xamarin.Forms kullanarak adıyla başvuruda tarafından görüntü mümkündür `Font` yöntemleri.
' Ndaki yönergeleri izleyin [bu blog gönderisine](http://blog.xamarin.com/custom-fonts-in-ios/):

1. Yazı tipi dosyasıyla eklemek **yapı eylemi: BundleResource**, ve
2. Güncelleştirme **Info.plist** dosyası (**uygulama tarafından sağlanan yazı tipleri**, veya `UIAppFonts`, anahtar), ardından
3. Xamarin.Forms içinde bir yazı tipi tanımladığınız yerlerde adıyla başvurduğu!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Xamarin.Forms Android için belirli bir adlandırma standardı izleyerek projeye eklenmiş olan özel bir yazı tipi başvuruda bulunabilir. İlk yazı tipi dosyası ekleyin **varlıklar** uygulama projesi ve set klasöründe *yapı eylemi: AndroidAsset*. Tam yolu kullanın ve *yazı tipi adı* aşağıdaki kod parçacığında gösterdiği gibi Xamarin.Forms, yazı tipi adı olarak bir karma (#) ile ayrılmış:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Xamarin.Forms Windows platformları için belirli bir adlandırma standardı izleyerek projeye eklenmiş olan özel bir yazı tipi başvuruda bulunabilir. İlk yazı tipi dosyası ekleyin **/Assets/yazı tipleri/** uygulama projesi ve set klasöründe <span class="UIItem">yapı eylemi: içerik</span>. Bir karma (#) ve ardından tam yolunu ve yazı tipi dosya adı kullanın ve <span class="UIItem">yazı tipi adı</span>, aşağıdaki kod parçacığında gösterir:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Yazı tipi dosya adı ve yazı tipi adı farklı olabileceğini unutmayın. Windows yazı tipi adı bulmak için .ttf dosyasını sağ tıklatın ve seçin **Önizleme**. Yazı tipi adı sonra aşağıdaki Önizleme penceresinden belirlenebilir.

Uygulama için ortak kodun tamamlanmıştır. Platforma özgü Telefon Çeviricisi kodu şimdi olarak gerçekleştirilen bir [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

### <a name="xaml"></a>XAML

Aynı zamanda [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) özel yazı tipi işlemek için XAML içinde:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<a name="Summary" />

## <a name="summary"></a>Özet

Xamarin.Forms izin verecek şekilde basit varsayılan ayarları sağlar kolayca desteklenen tüm platformlar için metin boyutu. Bu ayrıca yazı tipi ve boyutu belirtmenize olanak sağlar. &ndash; her platform için bile farklı &ndash; daha ayrıntılı denetim zaman gereklidir. `FormattedString` Sınıfı kullanarak farklı bir yazı tipi özellikleri içeren bir dize oluşturmak için kullanılabilir `Span` sınıfı.

Yazı tipi bilgileri doğru biçimlendirilmiş yazı tipi özniteliklerini kullanarak XAML'de de belirtilebilir veya `FormattedString` öğeyle `Span` alt.


## <a name="related-links"></a>İlgili bağlantılar

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
