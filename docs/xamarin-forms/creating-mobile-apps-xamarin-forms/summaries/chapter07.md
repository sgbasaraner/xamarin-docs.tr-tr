---
title: "Bölüm 7 özeti. XAML kodu karşılaştırması"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1104f7576cabfed9988154f3b6a8beb429136fb3
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Bölüm 7 özeti. XAML kodu karşılaştırması

Xamarin.Forms bir Genişletilebilir uygulama biçimlendirme dili adlı XML tabanlı biçimlendirme dili veya XAML ("zammel" denilir) destekler. XAML alternatif bir Xamarin.Forms uygulaması kullanıcı arabiriminin düzenini tanımlama ve kullanıcı arabirimi öğeleri arasında bağlamaları tanımlama ve arka plandaki veri C# sağlar.

## <a name="properties-and-attributes"></a>Özellikler ve öznitelikleri

Xamarin.Forms sınıfları ve yapıları XAML'de XML öğeleri ve özelliklerini bu sınıfları ve yapıları XML öznitelikleri haline gelir. XAML'de örneğinin oluşturulması için bir sınıf genellikle genel bir parametresiz oluşturucuya sahip olmalıdır. XAML'de ayarlanan tüm özellikleri ortak olmalıdır `set` erişimciler.

Temel veri türlerinin özellikleri için (`string`, `double`, `bool`, vb.), standart XAML ayrıştırıcısı kullanan `TryParse` öznitelik ayarlarını bu türlerine dönüştürmek için yöntemleri. XAML ayrıştırıcısı Numaralandırma türleri de kolayca karşılayabilir ve numaralandırma türü ile işaretlenmişse numaralandırma üyeleri birleştirebilirsiniz `Flags` özniteliği.

XAML ayrıştırıcısı yardımcı olmak için daha karmaşık türleri (veya bu türlerin özellikleri) dahil edebilirsiniz bir [ `TypeConverterAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/) türeyen bir sınıf tanımlayan [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) dönüştürme destekler Bu türlerde değerler dize. Örneğin, [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) içine dönüştürür renk adları ve "#rrggbb" gibi dizeleri `Color` değerleri.

## <a name="property-element-syntax"></a>Özellik öğesi sözdizimi

XAML'de, sınıflar ve bunları oluşturulan nesneler XML öğeleri ifade edilir. Bunlar olarak bilinir *nesne öğeleri*. Bu nesnelerin özelliklerinin çoğu XML öznitelikleri olarak ifade edilir. Bunlar adlandırılır *özellik öznitelikleri*.

Bazen bir özellik, basit bir dize olarak ifade bir nesneye ayarlanmalıdır. Böyle bir durumda XAML adlı bir etiket destekleyen bir *özellik öğesi* sınıfı adı ve özellik adı noktayla ayrılmış oluşur. Object öğesi, ardından bir özellik öğesi etiketleri çifti içinde yer alabilir.

## <a name="adding-a-xaml-page-to-your-project"></a>XAML sayfası projenize ekleme

Xamarin.Forms taşınabilir sınıf kitaplığı XAML sayfası ilk oluşturulduğunda veya varolan bir projeye XAML sayfası ekleyebilirsiniz içerebilir. Yeni Öğe Ekle iletişim kutusunda, bir XAML sayfasına başvuruyor öğesi seçin veya `ContentPage` ve XAML. (Olmayan bir `ContentView`.)

İki dosya oluşturulur: dosya adı uzantısı .xaml ile bir XAML dosyası ve bir C# dosya uzantısına sahip. xaml.cs. C# dosyasına, genellikle olarak adlandırılır *arka plan kodu* XAML dosyasının. Türetilen bir parçalı sınıf tanımı arka plan kodu dosyasıdır `ContentPage`. Derleme zamanında XAML ayrıştırılır ve başka bir parçalı sınıf tanımını aynı sınıfı için oluşturulur. Bu oluşturulan sınıf adlı bir yöntem içerir `InitializeComponent` arka plan kod dosyasına oluşturucusundan çağrılır.

Sonunda çalışma zamanı sırasında `InitializeComponent` çağrısı, tüm öğeleri XAML dosyasının örneği ve yalnızca bunlar C# kodunda oluşturulmuş olan sanki başlatıldı.

XAML dosyasının kök öğesindeki `ContentPage`. En az iki XML ad alanı bildirimi, bir Xamarin.Forms öğeleri ve diğer tanımlama kök etiketini içeren bir `x` öğeleri ve özniteliklerinin tüm XAML uygulamaları iç için önek. Kök etiketi de içeren bir `x:Class` ad alanı ve türetilen sınıf adını belirten özniteliği `ContentPage`. Bu arka plan kod dosyasına ad alanını ve sınıf adı ile eşleşir.

XAML ve kod birleşimi tarafından gösterilen [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) örnek.

## <a name="the-xaml-compiler"></a>XAML derleyici

Xamarin.Forms XAML derleyici, ancak var. kullanımını kullanılmasına göre isteğe bağlı bir [ `XamlCompilationAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/). XAML derlenmemiş, XAML derleme zamanında ayrıştırılır ve XAML dosyası burada, ayrıca çalışma zamanında ayrıştırılır PCL katıştırılır. XAML derlenmiş ise yapı işlemi XAML ikili biçimi olarak dönüştürür ve çalışma zamanı işleme daha etkilidir.

## <a name="platform-specificity-in-the-xaml-file"></a>XAML dosyasındaki Platform belirginliğe

XAML'de [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) sınıfı, platforma bağımlı biçimlendirme seçmek için kullanılabilir. Bu genel bir sınıf ve ile örneğinin oluşturulması bir `x:TypeArguments` hedef türüyle eşleşen özniteliği. [ `OnIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnIdiom%3CT%3E/) Sınıfı benzer ancak kullanılan genellikle daha az olur.

Kullanımını `OnPlatform` defteri yayımlandığından bu yana değişti. İlk olarak adlı özellikleri ile birlikte kullanılan `iOS`, `Android`, ve `WinPhone`. Şimdi alt ile kullanılan [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) nesneleri. Ayarlama [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) public ile tutarlı bir dize özelliği `const` alanlarının [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) sınıfı. Ayarlama [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Value/) özelliği ile tutarlı bir değere `x:TypeArguments` özniteliği `OnPlatform` etiketi.

`OnPlatform` örneklerde gösterildiği [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) örnek, neredeyse aynı XAML bloklarını içerdiğinden bu nedenle çağrılır. Bu tekrarlayan biçimlendirme varlığını teknikleri bunu azaltmak kullanılabilir olması gerektiğini önerir.

## <a name="the-content-property-attributes"></a>İçerik özelliği öznitelikleri

Bazı özellik öğeleri oldukça sık ortaya gibi `<ContentPage.Content>` üzerinde kök öğesinin etiket bir `ContentPage`, veya `<StackLayout.Children>` alt barındırır etiketi `StackLayout`.

Her sınıf bir özelliğiyle tanımlamak için izin verilen bir [ `ContentPropertyAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPropertyAttribute/) sınıfı. Bu özellik için özellik öğesi etiketleri gerekli değildir. `ContentPage` içerik özelliği olarak tanımlayan `Content`, ve `Layout<T>` (sınıfı `StackLayout` türetilen) olarak içerik özelliği tanımlar `Children`. Bu özellik öğe etiketleri gerekli değildir.

Özellik öğesi `Label` olan `Text`.

## <a name="formatted-text"></a>Biçimlendirilmiş metin

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) örnek içeren birkaç ayar örnekleri `Text` ve `FormattedText` özelliklerini `Label`. XAML'de `Span` nesneler alt olarak görünür `FormattedString` nesnesi.

 Çok satırlı bir dize ayarlandığında `Text` özelliği, satır sonu karakterleri, boşluk karakterleri dönüştürülür, ancak çok satırlı bir dize içeriği olarak görüntülendiğinde satır sonu karakterleri korunur `Label` veya `Label.Text` etiketler:

 [![Üçlü ekran paylaşımı metin çeşitleri](images/ch07fg03-small.png "biçimlendirilmiş metin Çeşitlemeler")](images/ch07fg03-large.png#lightbox "biçimlendirilmiş metin farklılıkları")



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 7 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Bölüm 7 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Bölüm 7 F # örnek](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
