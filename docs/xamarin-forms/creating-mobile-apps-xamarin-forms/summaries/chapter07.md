---
title: Bölüm 7 özeti. XAML ve kod karşılaştırması
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 7 özeti. XAML ve kod karşılaştırması'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 02e4ea44d87360deed361d161759fa3a2808100f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995163"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Bölüm 7 özeti. XAML ve kod karşılaştırması

Xamarin.Forms bir Extensible Application Markup Language adlı XML-tabanlı işaretleme dili veya XAML ("zammel" olarak okunur) destekler. XAML alternatif Düzen Xamarin.Forms uygulaması kullanıcı arabiriminin tanımlama ve kullanıcı arabirimi öğeleri arasındaki bağlantıları tanımlayarak ve temel alınan veriler için C# sağlar.

## <a name="properties-and-attributes"></a>Özelliklerini ve özniteliklerini

Xamarin.Forms sınıfları ve yapıları XAML XML öğeleri ve özellikleri bu sınıfları ve yapıları XML öznitelikleri haline gelir. XAML içinde örneği için bir sınıf genellikle genel parametresiz oluşturucusu olmalıdır. XAML içinde ayarlanmış tüm özellikleri ortak olan `set` erişimcileri.

Temel veri türlerini özelliklerinin (`string`, `double`, `bool`, vb.), XAML ayrıştırıcı standardını kullanan `TryParse` öznitelik ayarlarını bu türlerine dönüştürme yöntemleri. XAML ayrıştırıcı, ayrıca bir kolayca Numaralandırma türleri işleyebilir ve numaralandırma türü ile işaretlenmişse numaralandırma üyelerini birleştirebilirsiniz `Flags` özniteliği.

XAML ayrıştırıcı yardımcı olmak için daha karmaşık türleri (veya bu türlerin özellikleri) dahil edebilirsiniz bir [ `TypeConverterAttribute` ](xref:Xamarin.Forms.TypeConverterAttribute) türetildiği bir sınıf tanımlayan [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) dönüşümü destekler Bu tür dize değerlerine. Örneğin, [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) içine dönüştürür rengi adları ve dizeler, "#rrggbb" gibi `Color` değerleri.

## <a name="property-element-syntax"></a>Özellik öğesi sözdizimi

XAML içinde sınıflar ve bunları oluşturulan nesneler XML öğeleri olarak ifade edilir. Bunlar olarak bilinen *nesne öğeleri*. Bu nesnelerin özelliklerinin çoğu, XML özniteliği ifade edilir. Bunlar adlandırılır *özellik öznitelikleri*.

Bazen bir özellik basit bir dize olarak ifade edilen bir nesne için ayarlamanız gerekir. Böyle bir durumda, XAML adlı bir etiket destekleyen bir *özellik öğesi* sınıf adını ve özellik adı noktayla ayrılmış oluşur. Bir nesne öğesi, ardından bir çift özellik öğesi etiketleri içinde görünebilir.

## <a name="adding-a-xaml-page-to-your-project"></a>Projenize bir XAML sayfası ekleme

Taşınabilir sınıf kitaplığı Xamarin.Forms XAML sayfası, ilk oluşturulduğunda veya varolan bir projeye bir XAML sayfası ekleyebilirsiniz içerebilir. Yeni öğe ekleme iletişim kutusunda, bir XAML sayfasına başvuruyor öğesi seçin veya `ContentPage` ve XAML. (Değil bir `ContentView`.)

İki dosya oluşturulur: dosya adı uzantısı .xaml bir XAML dosyası ve bir C# dosya uzantısına sahip. xaml.cs. C# dosyası, genellikle olarak adlandırılır *arka plan kod* XAML dosyası. Arka plan kod dosyası, türetilen bir parçalı sınıf tanımı olduğunu `ContentPage`. Derleme zamanında XAML ayrıştırılır ve başka bir kısmi sınıf tanımı için aynı sınıf oluşturulur. Bu oluşturulan sınıf adlı bir yöntem içerir `InitializeComponent` arka plan kod dosyası oluşturucudan çağrılır.

Sonunda çalışma zamanı sırasında `InitializeComponent` çağrısı, tüm öğeleri XAML dosyasının örneği ve C# kodunda bunlar yalnızca oluşturulsaydı sanki başlatılır.

XAML dosyasında kök öğe `ContentPage`. En az iki XML ad alanı bildirimi, bir Xamarin.Forms öğeleri ve diğer tanımlama kök etiketi içeren bir `x` öğeler ve öznitelikler tüm XAML uygulamaları için iç için önek. Kök etiketi de içeren bir `x:Class` ad alanı ve türetilen sınıfın adını belirten bir özniteliği `ContentPage`. Bu, arka plan kod dosyasındaki ad alanını ve sınıf adıyla eşleşir.

XAML ve kod birleşimi tarafından gösterilen [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) örnek.

## <a name="the-xaml-compiler"></a>XAML derleyicisi

Xamarin.Forms XAML derleyicisi var, ancak kullanıma bağlı kullanımı isteğe bağlı bir [ `XamlCompilationAttribute` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute). XAML derlenmemişse, XAML derleme zamanında ayrıştırılır ve burada, ayrıca çalışma zamanında ayrıştırılır, PCL XAML dosyası katıştırılmış. XAML derlenirse, yapı işlemi ikili biçimi olarak XAML dönüştürür ve çalışma zamanı işlenmesinden daha verimlidir.

## <a name="platform-specificity-in-the-xaml-file"></a>XAML dosyasında Platform belirginliğe sahiptir

XAML içinde [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) sınıfı, platforma bağımlı biçimlendirme seçmek için kullanılabilir. Bu genel bir sınıf ve ile Değişkensiz bir `x:TypeArguments` hedef türüyle eşleşen öznitelik. [ `OnIdiom` ](xref:Xamarin.Forms.OnIdiom`1) Sınıfı, benzer ancak kullanılan çok daha az sıklıkta.

Kullanımını `OnPlatform` kitap yayınlandığı değişti. Başlangıçta adlı özellikleri ile birlikte kullanılan `iOS`, `Android`, ve `WinPhone`. Artık alt ile kullanılan [ `On` ](xref:Xamarin.Forms.On) nesneleri. Ayarlama [ `Platform` ](xref:Xamarin.Forms.On.Platform) genel tutarlı bir dize özelliğini `const` alanlarının [ `Device` ](xref:Xamarin.Forms.Device) sınıfı. Ayarlama [ `Value` ](xref:Xamarin.Forms.On.Value) ile tutarlı bir değere `x:TypeArguments` özniteliği `OnPlatform` etiketi.

`OnPlatform` gösterilmiştir [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) örnek, neredeyse aynı XAML bloklarını içerdiğinden şekilde çağrılır. Bu tekrarlayan biçimlendirme varlığını teknikleri bunu azaltmak kullanılabilir olması gerektiğini önerir.

## <a name="the-content-property-attributes"></a>İçerik özelliği öznitelikleri

Bazı özellik öğeleri oldukça sık meydana gibi `<ContentPage.Content>` kök öğesi etiketi bir `ContentPage`, veya `<StackLayout.Children>` kapsayan alt etiket `StackLayout`.

Her sınıfın bir özelliği tanımlamak için izin verilen bir [ `ContentPropertyAttribute` ](xref:Xamarin.Forms.ContentPropertyAttribute) sınıfta. Bu özellik için özellik öğesi etiketleri gerekli değildir. `ContentPage` içerik özelliği olarak tanımlar `Content`, ve `Layout<T>` (sınıf `StackLayout` türetilir) olarak içerik özelliğinin tanımlar `Children`. Bu özellik öğesi etiketleri gerekli değildir.

Özellik öğesi `Label` olduğu `Text`.

## <a name="formatted-text"></a>Biçimlendirilmiş metin

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) örnek ayar çeşitli örneklerini içeren `Text` ve `FormattedText` özelliklerini `Label`. XAML içinde `Span` nesneler öğesinin alt öğeleri olarak görünür `FormattedString` nesne.

 Çok satırlı string ayarlandığında `Text` özelliği, satır sonu karakterleri, boşluk karakterleri dönüştürülür, ancak çok satırlı string içeriği olarak göründüğünde satır sonu karakterleri korunur `Label` veya `Label.Text` etiketler:

 [![Üç ekran paylaşımı metin çeşitleri](images/ch07fg03-small.png "biçimlendirilmiş metin çeşitlemeleri")](images/ch07fg03-large.png#lightbox "biçimlendirilmiş metin farklılıkları")



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 7 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Bölüm 7 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Bölüm 7'de F # örnek](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
