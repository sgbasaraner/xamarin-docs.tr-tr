---
title: Objective-C geliştiricileri için C# temel bilgileri
description: Bu belgede C# Objective-C geliştiricileri için açıklanmaktadır. Karşılaştırır ve iki dilden protokolleri ve arabirimler, kategoriler ve genişletme yöntemleri, çerçeveler ve derlemeleri, seçicileri ve adlandırılmış parametreleri ve daha fazlasını inceleyerek karşılaştırır.
ms.prod: xamarin
ms.assetid: 00285CBD-AE5E-4126-8F22-6B231B9467EA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 7fbc37a561b0a1c0b0d5a16fea2892e7faf1a86b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351515"
---
# <a name="c-primer-for-objective-c-developers"></a>Objective-C geliştiricileri için C# temel bilgileri

_Xamarin.iOS platformlar arasında paylaşılması için C# dilinde yazılmış platformdan kod sağlar. Bununla birlikte, mevcut iOS uygulamaları zaten oluşturulmuş Objective-C kodunun yararlanmak isteyebilirsiniz. Bu makalede, Xamarin ve C# dili taşımak isteyen Objective-C geliştiricileri için kısa bir öncü görev yapar._

iOS ve OS X uygulamaları Objective-C içinde geliştirilen avantajını Xamarin C# platforma özgü kod olduğu gerekli yerlerde yararlanarak Apple olmayan cihazlarda kullanılmak üzere bu tür kod izin verme. Web Hizmetleri gibi şeyler, JSON ve XML Ayrıştırma ve özel algoritmalarını sonra bir platformlar arası şekilde kullanılabilir.

Objective-C mevcut varlıklarınızı korurken Xamarin avantajlarından yararlanmak için eski C# için bir teknoloji olarak yönetilen, C# dünyasına Objective-C kodunun yüzey bağlamaları, bilinen Xamarin gelen sunulabilir. Ayrıca isterseniz, kod taşınmasını-satır satır için C# de olabilir. Bağımsız olarak bir yaklaşım ancak bağlama veya bağlantı noktası oluşturma, hem Objective-C ve C# biraz bilgi Xamarin.iOS ile mevcut Objective-C kodunu etkili bir şekilde yararlanmak gereklidir.

## <a name="objective-c-interop"></a>Objective-C birlikte çalışma

Şu anda C# kullanarak Objective-c çağrılabilir Xamarin.iOS kitaplık oluşturmak için desteklenen bir mekanizma yoktur Bu temel nedeni Mono çalışma zamanı ayrıca yanı sıra bağlama gereklidir. Ancak, Objective-kullanıcı arabirimleri de dahil olmak üzere C dilinde mantığınızı çoğunu yine de oluşturabilirsiniz. Bunu yapmak için bir kitaplıkta Objective-C kodu kaydırmak ve ona bir bağlama oluşturun. Xamarin.iOS uygulama bootstrap için gereklidir (oluşturmanız gerekir yani `Main` giriş noktası). Bundan sonra başka bir mantık Objective-C, C# için sunulan bağlama (veya P/Invoke aracılığıyla) olabilir. Bu şekilde, platforma özgü mantık Objective-C içinde tutmak ve C# platformu belirsiz parçaları geliştirin.

Bu makalede bazı önemli benzerlikler vurgular, aynı zamanda bazı farklılıklar öncü C# ile Xamarin.iOS, taşırken Objective-C kodunun mevcut ya da bunu C# için taşıma bağlama olup olmadığını sunmak için her iki dilde karşılaştırır.

Diğer belgelerin içinde bağlamalar oluşturma hakkında bilgi edinmek için bkz [Objective-C bağlama](~/ios/platform/binding-objective-c/index.md).

## <a name="language-comparison"></a>Dil karşılaştırması

Objective-C ve C# çok farklı dillerde sözdizimsel olarak hem de çalışma zamanı açısından ' dir. Objective-C, dinamik bir dildir ve C# statik olarak yazılmış bir ileti geçirme düzeni kullanır. Syntax-wise, dahil edilmesi için Java ötesinde birçok özellikleri Son yıllarda olgunlaştığından olsa da C# temel sözdizimi, Java'dan çoğunu türetilen Objective-C Smalltalk gibi takvimidir.

Objective-C ve C# işlevinde benzer birçok dil özellikleri vardır. Bununla birlikte. Objective-C ile C bu benzerlikler anlama # Objective-C kodunu C# bağlama oluştururken veya taşırken kullanışlıdır.

### <a name="protocols-vs-interfaces"></a>Protokolleri vs. Arabirimler

Objective-C hem C# tek devralma dilleri edilir. Ancak, her iki dil belirli bir sınıfta birden çok arabirim uygulamak için desteği vardır. Objective-C içinde bu mantıksal arabirimleri olarak adlandırılan *protokolleri* C# dilinde çağrılır ancak *arabirimleri*. İsteğe bağlı yöntemler ikinci olabilir implementation-Wise, Objective-C protokolü ile bir C# arabirimi arasındaki ana fark olduğu. Daha fazla bilgi için [olaylar, temsilciler ve protokolleri](~/ios/app-fundamentals/delegates-protocols-and-events.md) makalesi.

### <a name="categories-vs-extension-methods"></a>Kategorileri vs. Genişletme yöntemleri

Objective-C, uygulama kodu kullanarak değil olan bir sınıfa eklenecek yöntemleri sağlayan *kategorileri*. C# benzer bir kavram olarak Bilineni aracılığıyla kullanılabilir *genişletme yöntemleri*.

Genişletme yöntemleri statik yöntemler C# sınıfı yöntemleri Objective-c benzer olduğu bir sınıf için statik yöntemler eklemenize olanak sağlar Örneğin, adında bir yöntem aşağıdaki kodu ekler `ScrollToBottom` için `UITextView` sırayla Objective-C bağlı yönetilen bir sınıf olan sınıf `UITextView` Uıkit sınıfı:

```csharp
public static class UITextViewExtensions
{
    public static void ScrollToBottom (this UITextView textView)
    {
        // code to scroll textView
    }
}
```

Ardından, örneği bir `UITextView` oluşturulan kodda yöntemi aşağıda gösterildiği gibi otomatik tamamlama listesinde kullanılabilir olur:

 ![](primer-images/01-extensionmethodintellisense.png "Otomatik Tamamlama'yı kullanılabilir yöntemi")

Genişletme yöntemi çağrıldığında örneği bağımsız değişken gibi geçirilen `textView` Bu örnekte.

### <a name="frameworks-vs-assemblies"></a>Çerçeveleri vs. Derlemeleri

Objective-C paketleri çerçeveler bilinen özel dizinlerde ilgili sınıflar. Ancak, C# ve .NET derlemeleri önceden derlenmiş kod yeniden kullanılabilir BITS sağlamak için kullanılır. İOS dışında ortamlarda, çalışma zamanında derlenmiş tam zamanında (JIT) olan ara dil kodu (IL) derlemeleri içerir. Ancak, Apple JIT iOS uygulamalarında izin vermez. Xamarin ile İos'u hedefleyen C# kodu derlenmiş zaman önce (AOT), bu nedenle tek bir UNIX yürütülebilir dosya uygulama paketine dahil edilen meta verileri dosyaların yanı sıra üretir.

### <a name="selectors-vs-named-parameters"></a>Seçiciler vs. Adlandırılmış parametreler

Objective-C yöntemleri, doğaları gereği Seçici parametre adları kendiliğinden içerir. Örneğin bir seçici gibi `AddCrayon:WithColor:` her bir parametre kod içinde kullanıldığında anlamı Temizle kolaylaştırır. C# isteğe bağlı olarak adlandırılmış bağımsız değişkenler de destekler.

Örneğin, benzer kodu kullanarak C# adlandırılmış bağımsız değişkenler şu şekilde olur:

```csharp
AddCrayon (crayon: myCrayon, color: UIColor.Blue);
```

C# dilinin 4.0 sürümünde eklenen bu destek olsa da, uygulamada, çok sık kullanılmaz. Kodunuzda açık olmasını istiyorsanız, ancak desteği yoktur.

### <a name="headers-and-namespaces"></a>Üst bilgiler ve ad alanları

Objective-C C bir alt kümesi olan, uygulama dosyasından ayrı genel bildirimleri üst bilgileri kullanır. C# üstbilgi dosyası kullanmaz. Objective-C, C# kodu alanlarında yer alır. Bazı ad alanında kullanılabilir bir kodu eklemek istiyorsanız, her iki kullanarak bir ekleme yönergesi uygulama dosyası veya üstüne uygun türü ile tam ad alanı.

Örneğin, aşağıdaki kodu içeren `UIKit` ad alanı, o ad alanında her sınıfın uygulaması için kullanılabilir hale getirme:

```csharp
using UIKit
namespace MyAppNamespace
{
    // implementation of classes
}
```

Ayrıca, yukarıdaki kod ad alanı anahtar sözcüğü uygulama dosyası kendisi için kullanılan ad alanını ayarlar. Birden çok uygulama dosyaları aynı ad paylaşıyorsanız, using ad alanı içerecek şekilde gerek yoktur de yönergesi olarak belirtilir.

### <a name="properties"></a>Özellikler

Objective-C hem C# erişimci metotlarını etrafında üst düzey bir soyutlama sağlamak için özellikler kavramı vardır. Objective C'de @property derleyici yönergesi, erişimci metotlarını etkili bir şekilde oluşturmak için kullanılır. Buna karşılık, C# dili içinde özellikleri için destek içerir. Aşağıdaki örneklerde gösterildiği gibi bir destek alanı veya daha kısa bir kullanarak erişen uzun stili, otomatik özelliği söz dizimini kullanarak bir C# özelliği uygulanabilir:

```csharp
// automatic property syntax
public string Name { get; set; }

// property implemented with a backing field
string address;
public string Address {
    get {
        // could add additional code here
        return address;
    }
    set {
        address = value;
    }
}
```

### <a name="static-keyword"></a>Static anahtar sözcüğü

*Statik* anahtar sözcüğü Objective-C ve C# arasında çok farklı bir anlama sahiptir. Objective-C içinde statik İşlevler, geçerli dosya için bir işlev kapsamını sınırlamak için kullanılır. C# dilinde ancak kapsam aracılığıyla korunur *genel*, *özel* ve *iç* anahtar sözcükleri.

Static anahtar sözcüğü, Objective-C, bir değişkene uygulandığında değişken işlev çağrıları arasında değerini korur.

C# statik bir anahtar sözcüğü de vardır. Bir yönteme uygulandığında, bu etkili bir şekilde aynı şeyi yapan `+` değiştiricisi mu Objective-c Yani, bir sınıf yöntemi oluşturur. Benzer şekilde, alanlar, özellikler ve olaylar gibi diğer yapıları uygulandığında, bu bölümü içinde yerine bu türün bir örneği ile bildirildikleri türü kolaylaştırır. Ayrıca, bir sınıf içinde tanımlanan tüm yöntemleri de statik olmalıdır, bir statik sınıf yapabilirsiniz.

### <a name="nsarray-vs-list-initialization"></a>NSArray vs. Liste Başlatma

Objective-C değişmez değer söz dizimi ile kullanmak için artık içerir `NSArray`, başlatmak kolaylaştırır. C# değiştirilirse adlı daha zengin bir türü bir `List` olduğu *genel*, türü de denir listesini tutar sağlanabilir kodla (c++ şablonları gibi) bir liste oluşturur. Ayrıca, listeler, aşağıda gösterildiği gibi otomatik başlatma söz dizimi destekler:

```csharp
MyClass object1 = new MyClass ();
MyClass object2 = new MyClass ();
List<MyClass> myList = new List<MyClass>{ object1, object2 };
```

### <a name="blocks-vs-lambda-expressions"></a>Blokları vs. Lambda İfadeleri

Objective-C kullanan *blokları* kapanışlar oluşturmak için oluşturabileceğiniz yapabilen bir işlevi satır içi kullanın nerede içine durumu. C# lambda ifadeleri kullanarak benzer bir kavram vardır. C# lambda ifadeleri ile oluşturulan `=>` aşağıda gösterildiği gibi işleç:

```csharp
(args) => {
    //  implementation code
};
```

Lambda ifadeleri hakkında daha fazla bilgi için bkz: Microsoft'un [C# programlama kılavuzu](http://msdn.microsoft.com/library/vstudio/bb397687.aspx).

## <a name="summary"></a>Özet

Bu makalede çeşitli dil özellikleri karşıtlıklar Objective-C ve C#. Bazı durumlarda, lambda ifadeleri için blokları gibi her iki dil ve genişletme yöntemleri için kategoriler arasındaki farklara benzer özellikleri çağrılır. Ayrıca, burada dilleri birleştirmek, gibi ad olarak C# ve anlamı static anahtar sözcüğü ile yerler karşıtlıklar.
