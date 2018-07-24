---
title: Yazı tipi xamarin.Forms'taki
description: Bu makalede, yazı tipi bilgi Xamarin.Forms uygulamalarında metin görüntüleyen denetimler belirtmeniz açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: e6635bc13214a5a4e728fa3e71db86a8ea1c39d6
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202961"
---
# <a name="fonts-in-xamarinforms"></a>Yazı tipi xamarin.Forms'taki

Bu makalede nasıl Xamarin.Forms yazı tipi özniteliklerini (ağırlık ve boyutu dahil) belirtmenize olanak tanır, metin görüntüleyen denetimler açıklanır. Yazı tipi bilgiler [kodda belirtilen](#Setting_Font_in_Code) veya [XAML içinde belirtilen](#Setting_Font_in_Xaml).
Kullanmak da mümkündür bir [özel yazı tipi](#Using_a_Custom_Font).

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>Yazı tipi kodda ayarlama

Metin görüntüleyen herhangi bir denetim yazı tipi ile ilgili üç özelliklerini kullanın:

- **FontFamily** &ndash; `string` yazı tipi adı.
- **FontSize** &ndash; olarak yazı tipi boyutu bir `double`.
- **FontAttributes** &ndash; gibi stil bilgilerini belirten bir dize *italik* ve **kalın** (kullanarak `FontAttributes` numaralandırmada C#).

Bu kod, bir etiket oluşturun ve yazı tipi boyutu ve görüntülemek için ağırlık belirtin gösterilmektedir:

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

Ayrıca `NamedSize` dört yerleşik seçenek; olan bir sabit listesi Xamarin.Forms her platform için en iyi boyut seçer.

-  **Mikro**
-  **Küçük**
-  **Orta**
-  **Büyük**

`NamedSize` Numaralandırma olabilir yerde kullanılan bir `FontSize` kullanılarak belirtilebilir `Device.GetNamedSize` değerine dönüştürmek için yöntemi bir `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Yazı tipi özniteliklerini

Yazı tipi stilleri gibi **kalın** ve *italik* ayarlanabilir `FontAttributes` özelliği. Şu anda, aşağıdaki değerleri desteklenir:

-  **Yok**
-  **Kalın**
-  **İtalik**

`FontAttribute` Numaralandırma gibi kullanılabilir (tek bir öznitelik belirtebilirsiniz veya `OR` bunları birlikte):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="setting-font-info-per-platform"></a>Platform başına yazı tipi bilgilerini ayarlama

Alternatif olarak, `Device.RuntimePlatform` özelliği kullanılabilir her platformda farklı yazı tipi adları ayarlamak için bu kodda gösterildiği gibi:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Yazı tipi bilgi iOS için iyi bir kaynaktır [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Yazı tipi XAML içinde ayarlama

Xamarin.Forms denetimleri tüm görünen metnin bir `Font` XAML içinde ayarlanabilir özelliği. XAML yazı tipini Ayarla en basit yolu adlandırılmış boyutu numaralandırma değerlerinin bu örnekte gösterildiği gibi kullanmaktır:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

İçin yerleşik bir dönüştürücü yok `Font` dize değer XAML olarak ifade edilen tüm yazı tipi ayarlarının özelliği. Aşağıdaki örnekler, XAML içinde nasıl yazı tipi özniteliklerini ve boyutları belirtebilirsiniz gösterir:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Birden çok belirtmek için `Font` ayarları, tek bir gerekli ayarları birleştirmeniz `Font` dize özniteliği. Yazı tipi öznitelik dize olarak biçimlendirilmelidir `"[font-face],[attributes],[size]"`. Parametreler sırası önemlidir, tüm parametreler isteğe bağlıdır ve birden çok `attributes` , örneğin belirtilebilir:

```xaml
<Label Text="Small bold text" Font="Bold, Micro" />
<Label Text="Medium custom font" Font="MarkerFelt-Thin, 42" />
<Label Text="Really big bold and italic text" Font="Bold, Italic, 72"  />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) Ayrıca her platformda farklı bir yazı tipi işlemek için XAML içinde kullanılabilir. Aşağıdaki örnek bir özel yazı tipini İos'ta kullanır (<span style="font-family:MarkerFelt-Thin">MarkerFelt ince</span>) ve diğer platformlarda yalnızca boyut/özniteliklerini belirtir:

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

Özel yazı tipinin belirtirken onu her zaman kullanmak iyi bir fikirdir `OnPlatform`, tüm platformlarda kullanılabilir olan bir yazı tipi bulmak zor olduğu gibi.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>Özel yazı tipi kullanarak

Yerleşik yazı dışındaki bir yazı tipi kullanarak bazı platforma özgü kodlama gerektirir. Özel yazı tipi bu ekran görüntüsünde gösterilmiştir **Lobster** gelen [Google'nın açık kaynak yazı tipleri](https://www.google.com/fonts) Xamarin.Forms kullanılarak işlenir.

 [![İOS ve Android özel yazı tipi](fonts-images/custom-sml.png "özel yazı tipleri örnek")](fonts-images/custom.png#lightbox "özel yazı tipi örneği")

Her platform için gereken adımlar aşağıda özetlenmiştir. Bir uygulama ile özel yazı tipi dosyalarını dahil ederken yazıtipinin lisans dağıtımı için izin verdiğini doğrulayın emin olun.

### <a name="ios"></a>iOS

Özel yazı tipi, ilk yüklendiğinden emin olduktan sonra kullanarak Xamarin.Forms adıyla başvuran duyarlılığıyla görüntülemek mümkün `Font` yöntemleri.
Bölümündeki yönergeleri [bu blog gönderisini](http://blog.xamarin.com/custom-fonts-in-ios/):

1. Yazı tipi dosyasıyla ekleme **derleme eylemi: BundleResource**, ve
2. Güncelleştirme **Info.plist** dosyası (**uygulama tarafından sağlanan yazı tipleri**, veya `UIAppFonts`, anahtar), ardından
3. Xamarin.Forms içinde bir yazı tipi tanımladığınız her yerde için ada göre başvurun!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Android için Xamarin.Forms belirli bir adlandırma standardı takip ederek projeye eklenmiş olan özel bir yazı tipi başvurabilirsiniz. İlk yazı tipi dosyaya ekleyin **varlıklar** ayarlama ve uygulama proje klasöründe *derleme eylemi: AndroidAsset*. Daha sonra tam yolu kullanın ve *yazı tipi adı* aşağıdaki kod parçacığında gösterildiği gibi Xamarin.Forms, yazı tipi adı olarak bir karma (#) ayrılmış:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Windows platformları için Xamarin.Forms belirli bir adlandırma standardı takip ederek projeye eklenmiş olan özel bir yazı tipi başvurabilirsiniz. İlk yazı tipi dosyaya ekleyin **/Assets/yazı tipleri/** ayarlama ve uygulama proje klasöründe <span class="UIItem">derleme eylemi: içerik</span>. Ardından bir karma (#) ve ardından tam yol ve yazı tipi dosya adı kullanın ve <span class="UIItem">yazı tipi adı</span>, aşağıdaki kod parçacığında gösterildiği gibi:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Yazı tipi dosya adını ve yazı tipi adı farklı olabileceğini unutmayın. Windows üzerinde yazı tipi adı bulmak için .ttf dosyasını sağ tıklatın ve seçin **Önizleme**. Yazı tipi adı, ardından aşağıdaki Önizleme penceresinden belirlenebilir.

Uygulama için ortak kodun tamamlanmıştır. Platforma özgü telefon çevirici kodu artık uygulanmasını olarak bir [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

### <a name="xaml"></a>XAML

Ayrıca [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) özel yazı tipi işlemek için XAML içinde:

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

Xamarin.Forms izin verecek şekilde basit varsayılan ayarları sağlar kolayca desteklenen tüm platformlar için metin boyutu. Ayrıca yazı biçimini ve boyutunu belirtin sağlar &ndash; her platform için bile farklı &ndash; daha hassas bir denetim zaman gereklidir.

Yazı tipi bilgileri, doğru biçimlendirilmiş bir yazı tipi özniteliklerini kullanarak XAML içinde de belirtilebilir.

## <a name="related-links"></a>İlgili bağlantılar

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
