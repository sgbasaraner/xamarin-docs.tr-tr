---
title: "C# Primer Objective-C geliştiricileri için"
description: "Xamarin.iOS platformlar genelinde paylaşılmak üzere C# yazılan platform belirsiz kod sağlar. Ancak, var olan iOS uygulamaları zaten oluşturulmuş Objective-C kodunu yararlanan isteyebilirsiniz. Bu makalede Xamarin ve C# dili taşımak isteyen Objective-C geliştiriciler için kısa bir primer görev yapar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 00285CBD-AE5E-4126-8F22-6B231B9467EA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bfc91ba92b2ed62e61d7ba99dec03784933295bd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="c-primer-for-objective-c-developers"></a>C# Primer Objective-C geliştiricileri için

_Xamarin.iOS platformlar genelinde paylaşılmak üzere C# yazılan platform belirsiz kod sağlar. Ancak, var olan iOS uygulamaları zaten oluşturulmuş Objective-C kodunu yararlanan isteyebilirsiniz. Bu makalede Xamarin ve C# dili taşımak isteyen Objective-C geliştiriciler için kısa bir primer görev yapar._

iOS ve OS X uygulamaları Objective-C geliştirilen avantajını Xamarin C# burada platforma özgü kodu gerekli değildir, yerlerde yararlanarak Apple olmayan aygıtlarda kullanılacak kodun izin verme. Web Hizmetleri gibi şeyler, JSON ve XML Ayrıştırma ve özel algoritmaları daha sonra bir platformlar arası şekilde kullanılabilir.

Varolan Objective-C varlıklarını korurken Xamarin yararlanmak için önceki C# bir teknoloji olarak yönetilen, C# dünyasına Objective-C kodunu yüzey bağlamaları bilinen Xamarin gelen gösterilebilir. Ayrıca, isterseniz, kod taşınmasını--satır için C# de olabilir. Bağımsız olarak bir yaklaşım ancak bağlama veya bağlantı noktası oluşturma, bazı bilgisine Objective-C hem C# Xamarin.iOS varolan Objective-C kodla etkili bir şekilde yararlanmak gereklidir.

## <a name="objective-c-interop"></a>Objective-C birlikte çalışma

Şu anda C# hedefi C'den çağrılabilir Xamarin.iOS kullanarak bir kitaplığı oluşturmak için desteklenen bir mekanizma yoktur Bu ana nedeni, Mono çalışma zamanı de yanı sıra bağlama gereklidir olmasıdır. Ancak, hedefi-kullanıcı arabirimleri de dahil olmak üzere C, mantığınızı çoğunluğu hala oluşturabilirsiniz. Bunu yapmak için kitaplığa Objective-C kodunu kaydırma ve ona bir bağlama oluşturun. Xamarin.iOS uygulaması bootstrap için gereklidir (oluşturmanız gerekir anlamına `Main` giriş noktası). Bundan sonra Objective-C C# bağlama (veya P/Invoke aracılığıyla) gösterilen, herhangi bir mantık olabilir. Bu şekilde, Objective-C platform belirli mantığı tutmak ve C# platform belirsiz bölümleri geliştirin.

Bu makalede bazı anahtar benzerlikler vurgular, aynı zamanda öncü C# ile Xamarin.iOS, taşırken Objective-C kodunu var veya bu C# taşıma bağlama olup olmadığını sunmak için her iki dilde birkaç farkları karşılaştırır.

Diğer belgelerde bağlamaları oluşturma hakkında bilgi için bkz [Objective-C bağlama](~/ios/platform/binding-objective-c/index.md).

## <a name="language-comparison"></a>Dil karşılaştırma

Objective-C ve C# çok farklı diller sözdizimsel olarak hem çalışma zamanı açısından ' dir. Objective-C dinamik bir dildir ve C# statik olarak yazılan bir ileti geçirme düzenini kullanır. Syntax-wise, içerdiği için Java dışındaki birçok yetenek Son yıllarda olgunlaştığından olsa da C#, Java'dan temel sözdizimini çoğunu türetilen ancak Objective-C Smalltalk gibi ' dir.

Bununla hem Objective-C ve C# işlevinde benzer birkaç dil özellikleri vardır. Objective-C C bu benzerlikler anlama #, Objective-C koddan C# bağlamaya oluştururken veya bağlantı noktası oluşturma zaman yararlıdır.

### <a name="protocols-vs-interfaces"></a>Protokolleri vs. Arabirimler

Objective-C hem C# tek devralma dili gösterilebilir. Bununla birlikte, her iki dilde belirli bir sınıf içinde birden çok arabirim uygulamak için destek vardır. Bu mantıksal birimleri Objective-C olarak adlandırılan *protokolleri* C# ' ta bunlar adlandırılır ancak *arabirimleri*. İmplementation-Wise, bir C# arabirimi ve bir Objective-C Protokolü arasındaki temel fark isteğe bağlı yöntemler ikinci olabilir değil. Daha fazla bilgi için bkz: [olayları, temsilciler ve protokolleri](~/ios/app-fundamentals/delegates-protocols-and-events.md) makalesi.

### <a name="categories-vs-extension-methods"></a>Kategoriler vs. Genişletme yöntemleri

Objective-C uygulama kodu kullanarak değil sahip olduğunuz bir sınıfa eklenecek yöntemleri sağlayan *kategorileri*. C# ' ta benzer bir kavram olarak Bilineni aracılığıyla kullanılabilir *genişletme yöntemleri*.

Genişletme yöntemleri statik yöntemler C# Objective-c sınıfı yöntemlere benzer olduğu bir sınıf için statik yöntemler eklemenize olanak sağlar Örneğin, aşağıdaki kodu adlı bir yöntem ekler `ScrollToBottom` için `UITextView` sırayla Objective-C bağlı bir yönetilen sınıf olan sınıf `UITextView` Uıkit sınıfından:

```csharp
public static class UITextViewExtensions
{
    public static void ScrollToBottom (this UITextView textView)
    {
        // code to scroll textView
    }
}
```

Ardından, bir örneği olduğunda bir `UITextView` oluşturulan kodda yöntemi aşağıda gösterildiği gibi otomatik tamamlama listesinde kullanılabilir olur:

 ![](primer-images/01-extensionmethodintellisense.png "AutoComplete seçeneğini kullanılabilir yöntemi")

Genişletme yöntemi çağrıldığında örneği bağımsız değişkeni gibi geçirilen `textView` Bu örnekte.

### <a name="frameworks-vs-assemblies"></a>Çerçeveler vs. Derlemeler

Objective-C paketleri çerçeveler bilinen özel dizinlerde ilgili sınıflar. Ancak, C# ve .NET derlemelerini önceden derlenmiş kod yeniden kullanılabilir BITS sağlamak için kullanılır. İOS dışında ortamlarda, çalışma zamanında derlenen tam zamanında (JIT) bulunduğu Ara dil kodu (IL) derlemeleri içerir. Ancak, Apple JIT iOS uygulamalarında izin vermiyor. Bu nedenle Xamarin iOS hedefleme C# kod derlenmiş öncesinde (Uygulama Nesne AĞACI) zamandır uygulama paketine eklenen meta veri dosyalarının yanı sıra tek bir UNIX yürütülebilir üretir.

### <a name="selectors-vs-named-parameters"></a>Seçici vs. Adlandırılmış parametreler

Objective-C yöntemleri, kendi yapısı gereği seçiciler parametre adları kendiliğinden içerir. Örneğin bir seçici gibi `AddCrayon:WithColor:` her bir parametreyi kodda kullanıldığında anlamı temizleyin kolaylaştırır. C# isteğe bağlı olarak adlandırılmış bağımsız değişkenler de destekler.

Örneğin, kullanarak C# bağımsız değişkenleri adlı benzer bir kod olacaktır:

```csharp
AddCrayon (crayon: myCrayon, color: UIColor.Blue);
```

C# sürüm dil 4.0 bu desteği eklendi ancak pratikte, çok sık kullanılmaz. Kodunuzda açık olmasını istiyorsanız, ancak desteği yoktur.

### <a name="headers-and-namespaces"></a>Üstbilgiler ve ad alanları

Objective-C C bir alt kümesi olan, uygulama dosyasından ayrı ortak bildirimleri üstbilgileri kullanır. C# üstbilgi dosyalarını kullanmaz. Objective-C C# kodu ad alanlarında yer alır. Kod bazı ad alanında kullanılabilir dahil etmek istiyorsanız, her iki kullanarak bir ekleme uygulama dosyasını veya üstüne yönergesi nitelemek tam ad alanını türüyle.

Örneğin, aşağıdaki kodu içeren `UIKit` ad alanı, her sınıf bu ad alanında uygulama kullanılabilir hale getirir:

```csharp
using UIKit
namespace MyAppNamespace
{
    // implementation of classes
}
```

Ayrıca, yukarıdaki kodu ad anahtar sözcüğü uygulama dosyasının kendisini için kullanılacak ad alanını ayarlar. Birden çok uygulama dosyaları aynı ad paylaşıyorsanız, kullanarak bir ad alanı içerecek şekilde gerek yoktur kapsanan olarak de yönergesi.

### <a name="properties"></a>Özellikler

Objective-C hem C# erişimci yöntemleri geçici üst düzey bir soyutlama sağlamak için özellikler kavramı vardır. Objective C'de @property derleyici yönergesi erişimci yöntemleri etkili bir şekilde oluşturmak için kullanılır. Buna karşılık, C# dili içinde özellikleri için destek içerir. Bir C# özelliği, aşağıdaki örneklerde gösterildiği gibi bir yedekleme alanını veya daha kısa bir kullanarak erişen uzun stili, otomatik özellik sözdizimi kullanılarak uygulanabilir:

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

*Statik* anahtar sözcüğü Objective-C ve C# arasında çok farklı anlama sahiptir. Objective-C statik işlevleri, bir işlev geçerli dosyasının kapsamını sınırlamak için kullanılır. C# ' ta ancak, kapsam aracılığıyla korunur *ortak*, *özel* ve *iç* anahtar sözcükler.

Static anahtar sözcüğü bir değişkende Objective-C uygulandığında, değişkenin değerini işlev çağrıları arasında tutar.

C# static anahtar sözcüğü de vardır. Bir yönteme uygulandığında, etkili bir şekilde aynı şey yapan `+` değiştiricisi mu Objective-c Yani, bir sınıf yöntemi oluşturur. Benzer şekilde, alanları, özellikleri ve olayları gibi diğer yapıların uygulandığında, bu bölümü içinde yerine bu türdeki herhangi bir örneğine sahip olarak bildirilen türü kolaylaştırır. İçinde sınıf içinde tanımlanan tüm yöntemleri de statik olması gerekir, statik bir sınıf de yapabilirsiniz.

### <a name="nsarray-vs-list-initialization"></a>NSArray vs. Liste Başlatma

Objective-C değişmez değer sözdizimi ile kullanmak için artık içerir `NSArray`, başlatmak kolaylaştırır. C# ancak adlı bir daha zengin türüne sahip bir `List` olduğu *genel*, türü de denir listesini tutar sağlanabilir kodla (C++ şablonlarında gibi) bir liste oluşturur. Ayrıca, listeler, aşağıda gösterildiği gibi otomatik başlatma sözdizimini destekler:

```csharp
MyClass object1 = new MyClass ();
MyClass object2 = new MyClass ();
List<MyClass> myList = new List<MyClass>{ object1, object2 };
```

### <a name="blocks-vs-lambda-expressions"></a>Blokları vs. Lambda İfadeleri

Objective-C kullanan *blokları* kapanışlar oluşturmak için oluşturabileceğiniz yapabilen bir işlev satır içi kullanın nerede içine durumu. C# lambda ifadeleri kullanımı ile benzer bir kavramı vardır. C# lambda ifadeleri ile oluşturulan `=>` aşağıda gösterildiği gibi işleci:

```csharp
(args) => {
    //  implementation code
};
```

Lambda ifadeleri hakkında daha fazla bilgi için bkz: Microsoft'un [C# programlama kılavuzu](http://msdn.microsoft.com/en-us/library/vstudio/bb397687.aspx).

## <a name="summary"></a>Özet

Bu makalede dil özellikleri çeşitli contrasted Objective-C ve C#. Bazı durumlarda, lambda ifadeleri bloklarına gibi her iki dilde ve genişletme yöntemleri kategorileri arasında mevcut benzer özellikleri çağrılır. Ayrıca, burada dilleri ayırmak, gibi ad alanları gibi C# ve static anahtar sözcüğü anlamını ile yerler contrasted.
