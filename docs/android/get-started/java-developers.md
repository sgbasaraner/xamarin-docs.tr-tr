---
title: "Java geliştiricileri için Xamarin"
description: "Java geliştiriciyseniz yararlanarak için iyi yolunuzu üzerinde olan becerileri ve var olan kodu kodu yararlanın sırasında Xamarin platformunda yararları C# yeniden kullanabilirsiniz. C# sözdizimi Java sözdizimine çok benzer ve her iki dilde birden çok benzer özellikler sağlamak bulacaksınız. Ayrıca, C, geliştirme hayatınızı daha kolay hale getirecektir # benzersiz özelliklerini öğreneceksiniz."
ms.topic: article
ms.prod: xamarin
ms.assetid: A3B6C041-4052-4E7D-999C-C4FA10BE3D67
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: fb74e0efd62c6347534e6f301953325bd4d378d2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-for-java-developers"></a>Java geliştiricileri için Xamarin

_Java geliştiriciyseniz yararlanarak için iyi yolunuzu üzerinde olan becerileri ve var olan kodu kodu yararlanın sırasında Xamarin platformunda yararları C# yeniden kullanabilirsiniz. C# sözdizimi Java sözdizimine çok benzer ve her iki dilde birden çok benzer özellikler sağlamak bulacaksınız. Ayrıca, C, geliştirme hayatınızı daha kolay hale getirecektir # benzersiz özelliklerini öğreneceksiniz._

<a name="overview" />

## <a name="overview"></a>Genel Bakış

Bu makalede C Java geliştiriciler için programlama, Xamarin.Android uygulamaları geliştirirken karşılaşır öncelikle C# dili özelliklere odaklanan # tanıtılmaktadır. Ayrıca, bu makalede, bu özellikleri Java dekiler nasıl farklılıklar açıklanır ve Java kullanılamayan önemli C# özellikleri (Xamarin.Android için ilgili) sunar. Bu makalede daha fazla olay incelemesi C# ve .NET için bir "kapalı atlama" noktası olarak kullanabilmeniz için ek başvuru malzemesinde bağlantılar eklenmiştir.

Java ile bilginiz varsa, C# sözdizimi ile evde anında devam edebilir. C# sözdizimi Java sözdizimine çok benzer &ndash; C# olan "küme parantezi" dil Java, C ve C++ gibi. Birçok yolla C# sözdizimi bir alt kümesi olan Java sözdizimi, ancak birkaç yeniden adlandırılmış ve eklenen anahtar sözcükler gibi okur.

Java birçok anahtar özelliklerini C# ' ta bulunabilir:

-   Sınıf tabanlı nesne odaklı programlama

-   Yazarak tanımlayıcı

-   Arabirimler için destek

-   Genel Türler

-   Atık toplama

-   Çalışma zamanı derlemesi

Yönetilen yürütme ortamında çalışan bir ara dile, Java ve C# derlenmiş. Statik olarak yazılan hem C# ve Java ve her iki dilde dizeleri değişmez türleri olarak ele alın.
Her iki dilde tek kökü sınıf hiyerarşisi kullanın. Java gibi C# yalnızca tek devralma destekler ve genel yöntemler için izin vermez.
Her iki dilde yığın kullanarak nesneleri oluşturma `new` anahtar sözcüğü ve nesneleri çöp-toplanan artık kullanıldığında. Her iki dilde resmi özel durum işleme ile ilgili destek sağlayan `try` / `catch` semantiği. Her iki iş parçacığı yönetimi ve eşitleme desteği sağlar.

Ancak, Java ve C# arasındaki çok sayıda fark vardır. Örneğin:

-   Java örtük olarak yazılan yerel değişkenler desteklemez (C# destekler `var` anahtar sözcüğü).

-   Java, C# başvurusu tarafından ve aynı zamanda değeriyle geçirebilirsiniz ancak yalnızca değer ile parametreleri geçirebilirsiniz. (C# sağlar `ref` ve `out` parametreleri geçirme için anahtar sözcükler başvuru; bu Java eşdeğeri yoktur).

-   Java gibi önişlemci yönergeleri desteklemediği `#define`.

-   Java C imzasız tamsayı türleri gibi sağlarken işaretsiz tamsayı türlerini desteklemediğini `ulong`, `uint`, `ushort` ve `byte`.

-   Java İşleç aşırı yüklemesi desteklemiyor; C# işleçleri ve dönüşümleri aşırı yüklenebilir.

-   Bir Java `switch` deyimi, kod atlayabilir sonraki anahtar bölümüne ancak C# sonuna her `switch` bölüm anahtarı sonlandırmak gerekir (her bölümün sonuna ile kapatmalısınız bir `break` deyimi).

-   Java'da, belirttiğiniz bir yöntemle tarafından oluşturulan özel durumları `throws` anahtar sözcüğü, ancak C# checked özel durumlar kavramına sahiptir &ndash; `throws` anahtar sözcüğü desteklenmiyor C# '.

-   C# dil ile tümleşik sorgu (ayrılmış sözcükler kullanmanıza olanak sağlayan LINQ), destekler `from`, `select`, ve `where` veritabanı sorguları için benzer bir şekilde koleksiyonları sorguları yazmaya.


Elbette, C# ve bu makalede ele alınan daha Java arasında pek çok daha fazla fark vardır. Ayrıca, Java ve C# gelişmeye devam edin (örneğin, henüz Android zincirinin değil, 8, Java C# destekler-stil lambda ifadeleri) Bu farklılıklar zamanla değiştirecek şekilde. Yalnızca şu anda Java geliştiricilerinin Xamarin.Android yeni tarafından karşılaşılan en önemli farklar burada özetlenmektedir.

-   [C# geliştirme Java'dan giderek](#fundamentals) C# ve Java arasındaki temel farklar tanıtılmaktadır.

-   [Nesne odaklı programlama özellikleri](#oopfeatures) iki dil arasındaki en önemli özellik nesne yönelimli farklar özetlenmektedir.

-   [Anahtar sözcüğü farklar](#keywords) yararlı anahtar sözcüğü eşdeğerlerini, C# tablosu sağlar-yalnızca anahtar sözcükleri ve C# anahtar sözcüğü tanımları bağlanır.

Şu anda Android üzerinde Java geliştiricileri için kullanıma hazır olmayan Xamarin.Android için C# anahtar özelliklerinin getirir. Bu özellikleri daha kısa sürede daha iyi bir kod yazmanıza yardımcı olabilir:

-   [Özellikler](#properties) &ndash; C# ' ın özellik sistemi ile üye değişkenleri doğrudan ve güvenli bir şekilde ayarlama ve alıcı yöntemleri yazmak zorunda kalmadan erişebilirsiniz.

-   [Lambda Expressionss](#lambdas) &ndash; C anonim yöntemler kullanabilir #'ta (olarak da bilinir *lambdas*), işlevselliği daha temellerini ve daha verimli bir şekilde ifade etmek için. Bir kerelik kullanım nesneleri yazmak zorunda yükünü önlemek ve parametreleri eklemek zorunda kalmadan bir yönteme yerel durumu geçirebilirsiniz.

-   [Olay işleme](#events) &ndash; C# dil düzeyi desteği sağlar *olay kaynaklı programlama*, ilgilendiğiniz bir olay meydana geldiğinde bildirim almak için bir nesne kaydolabilecekleri. `event` Anahtar sözcüğü bir yayımcı sınıf olay aboneleri bildirmek için kullanabileceğiniz bir çok noktaya yayın yayın mekanizması tanımlar.

-   [Zaman uyumsuz programlama](#async) &ndash; zaman uyumsuz programlama özelliklerini C# (`async`/`await`) uygulamaları yanıt verebilir durumda tutun.
    Bu özelliğin dil düzeyi destek uygulamak kolay ve hataya daha az zaman uyumsuz programlama hale getirir.


Son olarak, Xamarin sayesinde [varolan Java varlıkları yararlanan](#interop) olarak bilinen bir teknoloji aracılığıyla *bağlama*. Varolan Java kod, çerçeveler ve kitaplıklar C# ' dan yaparak çağırabilirsiniz Xamarin'ın otomatik bağlama oluşturucuları kullanın. Bunu yapmak için yalnızca Java'da statik kitaplık oluşturma ve, C# için bir bağlama aracılığıyla kullanıma sunar.

<a name="fundamentals" />

## <a name="going-from-java-to-c-development"></a>C# geliştirme Java'dan gitme

Aşağıdaki bölümlerde temel "Başlarken" farklarını C# ve Java verilmiştir; daha sonraki bir bölüme bu diller nesne yönelimli farklılıkları açıklar.


<a name="assemblies" />

### <a name="libraries-vs-assemblies"></a>Kitaplıkları vs. Derlemeler

Java genellikle ilgili sınıflarda paketleri **.jar** dosyaları. C# ve .NET, ancak, önceden derlenmiş kod yeniden kullanılabilir BITS içine paketlenir *derlemeleri*, hangi genellikle paketlenir olarak *.dll* dosyaları. Derleme dağıtımı için C# birimidir .NET kodu ve her derleme genellikle bir C# proje ile ilişkili olduğu. Derlemeleri yalnızca çalışma zamanında derlenen zaman ara kodları (IL) içerir.

Derlemeler hakkında daha fazla bilgi için bkz. MSDN [derlemeler ve genel derleme önbelleği](https://msdn.microsoft.com/en-us/library/ms173099.aspx) konu.

<a name="namespaces" />

### <a name="packages-vs-namespaces"></a>Paketleri vs. Ad Alanları

C# kullanan `namespace` anahtar sözcüğü grubuna ilgili türleri birlikte; bu Java için 's benzer `package` anahtar sözcüğü. Genellikle, bir Xamarin.Android uygulaması, bu uygulama için oluşturulan bir ad alanı içinde yer alacaktır. Örneğin, aşağıdaki C# kod bildirir `WeatherApp` uygulama hava durumu raporlama için ad alanı sarmalayıcı:

```csharp
namespace WeatherApp
{
    ...
```

<a name="imports" />

### <a name="importing-types"></a>Türlerini alma

Yaptığınızda dış ad alanlarında tanımlanmış türlerinin kullanın, bu türleriyle alma bir `using` deyimi (Java için çok benzer olduğu `import` deyimi). Java'da, tek bir aşağıdaki gibi bir ifade türüyle alabilir:

```java
import javax.swing.JButton
```

Bu gibi bir ifade ile tüm bir Java Paketi alabilir:

```java
import javax.swing.*
```

C# `using` deyimi çok benzer şekilde çalışır, ancak bir joker karakter belirtmeden tüm paketi almanıza izin verir. Örneğin, genellikle bir dizi görürsünüz `using` Xamarin.Android başında deyimleri kaynak dosyaları, bu örnekte görüldüğü gibi:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Net;
using System.IO;
using System.Json;
using System.Threading.Tasks;
```

Bu deyimler işlevinden alma `System`, `Android.App`, `Android.Content`, vb. ad alanları.


<a name="generics" />

### <a name="generics"></a>Genel Türler

Java ve C# Destek *genel türler*, olanak tanıyan yer tutucuları olan farklı türde derleme zamanında takın. Ancak, genel türler biraz farklı C# içinde çalışır. Java, [yazın silinme](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html) kullanılabilir hale getirir türü bilgileri yalnızca derleme zaman, ancak çalışma zamanında değil. Bunun aksine, .NET ortak dil çalışma zamanı (CLR) açık genel türleri için destek, C# çalışma zamanında bilgileri yazın erişebileceği anlamına gelir sağlar. Günlük Xamarin.Android geliştirme Bu ayrım importantance genellikle açık, değildir ancak kullanıyorsanız [yansıma](https://msdn.microsoft.com/en-us/library/ms173183.aspx), çalışma zamanında tür bilgilere erişmek için bu özelliği bağlıdır.

Xamarin.Android içinde genel yöntem genellikle görürsünüz `FindViewById` düzen denetimi bir başvuru almak için kullanılır. Bu yöntem, arama yapmak için denetim türünü belirten bir genel tür parametre kabul eder. Örneğin:

```csharp
TextView label = FindViewById<TextView> (Resource.Id.Label);
```

Bu kod örneğinde, `FindViewById` bir başvuru edinir `TextView` düzeninde tanımlanan Denetim **etiket**, olarak döndürür bir `TextView` türü.

Genel türler hakkında daha fazla bilgi için bkz. MSDN [genel türler](https://msdn.microsoft.com/en-us/library/512aeb7t.aspx) konu.
Xamarin.Android Destek genel C# sınıflar için bazı kısıtlamalar olduğunu unutmayın; Daha fazla bilgi için bkz: [sınırlamalar](~/android/internals/limitations.md).


<a name="oopfeatures" />

## <a name="object-oriented-programming-features"></a>Nesne odaklı programlama özellikleri

Java ve C# çok benzer nesne odaklı programlama deyimleri kullanın:

-   Tüm sınıflar sonuç olarak tek bir kök nesneden türetildiği &ndash; tüm Java nesnelerini türetin `java.lang.Object`tüm C# nesnelerini türetin yaparken `System.Object`.

-   Başvuru türleri sınıfları örnekleridir.

-   Özellikleri ve yöntemleri bir örneğinin eriştiğinizde, kullandığınız "<code>.</code>" işleci.

-   Tüm sınıf örneklerinin yığında oluşturulan `new` işleci.

-   Her iki dilde çöp toplama kullandığından, açıkça kullanılmayan nesneleri serbest bırakmak için bir yolu yoktur (yani, yok. bir `delete` sözcüktür orada olarak c++'ta).

-   Devralma yoluyla sınıfları genişletebilir ve her iki dilde yalnızca tür başına tek bir temel sınıf sağlar.

-   Bir sınıf devralınmalıdır ve arabirimleri tanımlayabilirsiniz (yani, uygulama) birden çok arabirimi tanımlar.

Ancak, aynı zamanda bazı önemli farklılıklar vardır:

-   Java C# desteklemediği iki güçlü özelliklere sahiptir: Anonim sınıfları ve iç sınıfları. (Ancak, C# iç içe geçmiş sınıf tanımları izin vermez &ndash; C# ' ın iç içe geçmiş sınıflar olan Java'nın statik iç içe geçmiş sınıflar gibi.)

-   C# C tarzı yapısı türlerini destekler (`struct`) Java desteklemez.

-   C# ' ta, bir sınıf tanımı ayrı kaynak dosyalarında kullanarak uygulayabileceğiniz `partial` anahtar sözcüğü.

-   C# arabirimleri alanları bildiremezsiniz.

-   C# sonlandırıcılar express için C++ stili yıkıcı sözdizimini kullanır. Java'dan ait farklı söz dizimi `finalize` yöntemi, ancak semantiğini neredeyse aynı değildir. (C# ' ta Yıkıcılar otomatik olarak temel sınıf yıkıcı çağırdığını unutmayın &ndash; Java aksine için açık bir çağrı burada `super.finalize` kullanılır.)


<a name="inheritance" />

### <a name="class-inheritance"></a>Sınıf Devralma

Java sınıfında genişletmek için kullandığınız `extends` anahtar sözcüğü. C# sınıfı genişletmek için iki nokta kullanın (`:`) türetme belirtmek için. Örneğin, Xamarin.Android uygulamaları aşağıdaki kod parçası benzer sınıfı türetme sık sık görürsünüz:

```csharp
public class MainActivity : Activity
{
    ...
```

Bu örnekte, `MainActivity` devraldığı `Activity` sınıfı.

Java bir arabirim için destek bildirmek için kullandığınız `implements` anahtar sözcüğü. Ancak, C# ' ta, yalnızca arabirim adlarını, devralınacak sınıfların listesi için bu kodu parçadaki gösterildiği gibi ekleyin:

```csharp
public class SensorsActivity : Activity, ISensorEventListener
{
    ...
```

Bu örnekte, `SensorsActivity` devraldığı `Activity` ve bildirilen işlevselliğini hayata Geçiren `ISensorEventListener` arabirimi. Not arabirimleri listesine temel sınıf sonra gelmelidir (veya bir derleme zamanı hatası alırsınız emin). Kurala göre C# arabirim adına bir büyük harf "ı ile"; $a Bu gerektirmeden arabirimleri hangi sınıfların belirlemek mümkün kılar bir `implements` anahtar sözcüğü.

Daha fazla C# ' ta sınıflandırma öğesinden bir sınıf engellemek istediğinizde, sınıf adından `sealed` &ndash; , Java, sınıf adından `final`.

C# hakkında daha fazla sınıf tanımları için MSDN bkz [sınıflar ve yapılar](https://msdn.microsoft.com/en-us/library/x9afc042.aspx) ve [devralma](https://msdn.microsoft.com/en-us/library/x9afc042.aspx) Konular.


<a name="properties" />

### <a name="properties"></a>Özellikler

Java'da, değiştirici (ayarlayıcılar) ve Inspector yöntemleri (Alıcılar) genellikle nasıl yapılan bir değişiklik sınıf üyelerine gizleme ve dış kodundan bu üyeleri korumaya çalışırken denetlemek için kullanılır. Örneğin, Android `TextView` SAX `getText` ve `setText` yöntemleri. C# olarak bilinen benzerdir, ancak daha doğrudan bir mekanizma sağlar *özellikleri*.
Bir C# sınıfı kullanıcılarının bir özelliği bir alan erişir, ancak her erişim çağırana saydam bir yöntem çağrısı gerçekte sonuçlanır gibi aynı şekilde erişebilirsiniz. Bu "altında"kapsar yöntemi diğer değerleri ayarlama, dönüştürme gerçekleştirmek veya nesnenin durumunu değiştirme gibi yan etkileri uygulayabilirsiniz.

Özellikleri genellikle erişme ve UI (kullanıcı arabirimi) nesne üyeleri değiştirmek için kullanılır. Örneğin:

```csharp
int width = rulerView.MeasuredWidth;
int height = rulerView.MeasuredHeight;
...
rulerView.DrawingCacheEnabled = true;
```

Bu örnekte, genişlik ve yükseklik değerleri gelen salt okunurdur `rulerView` erişerek nesne kendi `MeasuredWidth` ve `MeasuredHeight` özellikleri. Bu özellikleri okurken ilişkili (ancak gizli) alan değerlerine değerlerinden arka planda getirildi ve yapana. `rulerView` Nesne genişlik ve yükseklik değerleri ölçüm (örneğin, piksel) bir birimde depolamak ve bu değerleri üzerinde-çalışma sırasında farklı bir ölçü birimi (örneğin, milimetre) için dönüştürürken `MeasuredWidth` ve `MeasuredHeight` özellikleri erişilir.

`rulerView` Nesnesi aynı zamanda adlı bir özelliğe sahiptir `DrawingCacheEnabled` &ndash; kod örneği bu özelliği ayarlar `true` çizim önbellekte etkinleştirmek için `rulerView`. Arka planda, ilişkili bir gizli alan yeni bir değer ve diğer yönlerini ile büyük olasılıkla güncelleştirildiğinden `rulerView` durumu değiştirildiğinde. Örneğin, `DrawingCacheEnabled` ayarlanır `false`, `rulerView` nesnesinde zaten toplanan tüm çizim önbellek bilgilerini de silebilir.

Özelliklere erişim okuma/yazma, salt okunur veya sadece yazılabilir olabilir. Ayrıca, farklı erişim değiştiricileri okumak ve yazmak için kullanabilirsiniz. Örneğin, özel yazma erişimi ancak ortak okuma erişimine sahip bir özellik tanımlayabilirsiniz.

C# özellikleri hakkında daha fazla bilgi için bkz. MSDN [özellikleri](https://msdn.microsoft.com/en-us/library/x9fsa0sw.aspx) konu.


<a name="basemethods" />

### <a name="calling-base-class-methods"></a>Arama taban sınıf yöntemlerini

Bir temel sınıf oluşturucu C# ' ta çağırmak için iki nokta kullanın (`:`) ve ardından `base` anahtar sözcüğü ve bir başlatıcı listesi; bu `base` oluşturucu çağrısı hemen türetilmiş Oluşturucusu parametre listesi sonra yerleştirilir. Temel sınıf oluşturucu türetilmiş oluşturucuya girişindeki çağrılır; derleyici temel oluşturucuyu çağrısı yöntem gövdesi başlangıcında ekler. Aşağıdaki kod parçası, bir Xamarin.Android uygulaması türetilmiş bir oluşturucu çağrılır temel oluşturucu gösterilmektedir:

```csharp
public class PictureLayout : ViewGroup
{
    ...
    public class PictureLayout (Context context)
           : base (context)
    {
        ...
    }
    ...
}

```

Bu örnekte, `PictureLayout` sınıfı türetilir `ViewGroup` sınıfı. `PictureLayout` Bu örnekte gösterilen Oluşturucusu kabul bir `context` bağımsız değişkeni ve buna ileten `ViewGroup` oluşturucu aracılığıyla `base(context)` çağırın.

Bir temel sınıf yöntemi C# ' ta çağırmak için `base` anahtar sözcüğü. Örneğin, Xamarin.Android uygulamaları genellikle yöntemleri aşağıda gösterildiği gibi temel çağrılar yapın:

```csharp
public class MainActivity : Activity
{
    ...
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
```

Bu durumda, `OnCreate` türetilmiş sınıf tarafından tanımlanan yöntemi (`MainActivity`) çağrıları `OnCreate` temel sınıfın yöntemi (`Activity`).


<a name="accessmodifiers" />

### <a name="access-modifiers"></a>Erişim Değiştiricileri

Java ve C# hem `public`, `private`, ve `protected` erişim değiştiricileri. Ancak, C# iki ek erişim değiştiricileri destekler:

-   **`internal`** &ndash; Sınıf üyesi, yalnızca geçerli derlemede erişilebilir.

-   **`protected internal`** &ndash; Sınıf üyesi tanımlayan bir sınıf tanımlama derlemede erişilebilir ve türetilmiş sınıfları (türetilen sınıflar hem içinde hem de derleme dışına erişimi).

C# erişim değiştiricileri hakkında daha fazla bilgi için bkz. MSDN [erişim değiştiricileri](https://msdn.microsoft.com/en-us/library/ms173121.aspx) konu.


<a name="virtualoverride" />

### <a name="virtual-and-override-methods"></a>Sanal ve geçersiz kılma yöntemleri

Java ve C# Destek *çok biçimlilik*, ilgili nesneleri aynı şekilde işleyebilme özelliği. Her iki dilde da türetilmiş sınıf nesneye başvurmak için bir temel sınıf başvurusu kullanabilirsiniz ve türetilmiş bir sınıf yöntemlerini temel sınıflarının yöntemleri geçersiz kılabilirsiniz. Her iki dilde kavramı sahip bir *sanal* yöntemi, bir yöntem türetilmiş bir sınıf yönteminde değiştirilmesi için tasarlanmış bir temel sınıf.
İstediğiniz Java, C# destekler `abstract` sınıflar ve yöntemler.

Ancak, nasıl sanal yöntemler bildirme ve bunları geçersiz kılma Java ve C# arasındaki bazı farklar vardır:

-   C# ' ta varsayılan olarak sanal olmayan yöntemleridir. Üst sınıf gereken açıkça etiket yöntemleri kullanarak geçersiz kılınacak olan `virtual` anahtar sözcüğü. Bunun aksine, tüm yöntemler Java sanal varsayılan yöntemlerdir.

-   Bir yöntem C# içinde geçersiz kılınmasını önlemek için yalnızca bırakın `virtual` anahtar sözcüğü. Bunun aksine, Java kullanan `final` "geçersiz kılma izin." ile bir yöntem işaretlemek için anahtar sözcüğü

-   C# türetilmiş sınıfları kullanmalıdır `override` sanal bir temel sınıf yöntemi geçersiz kılınıyor açıkça belirtmek için anahtar sözcüğü.

C# ' ın çok biçimlilik desteği hakkında daha fazla bilgi için bkz. MSDN [çok biçimlilik](https://msdn.microsoft.com/en-us/library/ms173152.aspx) konu.


<a name="lambdas" />

## <a name="lambda-expressions"></a>Lambda İfadeleri

C# oluşturmak mümkün kılar *kapanışlar*: satır içi, bunlar içine yöntemi durumunu erişmek için anonim yöntemler.
Lambda ifadeleri kullanma, daha az satır pek çok daha fazla kod satırıyla Java'da uygulanan aynı işlevselliği uygulamak için kod yazabilirsiniz.

Lambda ifadeleri olanaklı hale getirir, Java gibi bir kerelik kullanım sınıfı veya anonim sınıf oluşturmaya dahil ek seremoni atlamak &ndash; bunun yerine, yalnızca sizin yöntemi kodu satır içi iş mantığını yazabilirsiniz. Ayrıca, Lambda'lar değişkenleri çevresindeki yönteminde erişiminiz olduğundan, yöntemi kodunuzu duruma geçmesine uzun parametre listesi oluşturmanız gerekmez.

C# ' ta lambda ifadeleri ile oluşturulan `=>` aşağıda gösterildiği gibi işleci:

```csharp
(arg1, arg2, ...) => {
    // implementation code
};
```

Xamarin.Android içinde lambda ifadeleri genellikle olay işleyicileri tanımlamak için kullanılır. Örneğin:

```csharp
button.Click += (sender, args) => {
    clickCount += 1;    // access variable in surrounding code
    button.Text = string.Format ("Clicked {0} times.", clickCount);
};
```

Bu örnekte, bir tıklatma sayısı ve güncelleştirmeleri lambda ifadesi kod (süslü ayraçlar içindeki kod) artırır `button` tıklatın sayısı görüntülenecek metin. Bu lambda ifadesi kayıtlı `button` nesnesi olarak düğmesi dokunduğunuz her durumda çağrılacak bir click olay işleyicisi. (Olay işleyicileri aşağıdaki daha ayrıntılı olarak açıklanmıştır.) Bu basit örnekte, `sender` ve `args` parametreleri lambda ifadesi kodla kullanılmaz, ancak olay kaydı yöntemi imza gereksinimlerini karşılamak üzere lambda ifadesinde gereklidir. Başlık altında C# Derleyici çağrılan bir anonim yöntemiyle lambda ifadesi çevirir düğmesini tıklattığınızda olaylar gerçekleşir.

C# ve lambda ifadeleri hakkında daha fazla bilgi için bkz. MSDN [Lambda ifadeleri](https://msdn.microsoft.com/en-us/library/bb397687.aspx) konu.


<a name="events" />

## <a name="event-handling"></a>Olay İşleme

Bir *olay* ilgi çekici bir şey bu nesneye olduğunda kayıtlı aboneleri bildirmek bir nesne için bir yoldur. Aksine Java, burada bir abone genellikle uygulayan bir `Listener` bir geri çağırma yöntemi içeren arabirimi C# dil düzeyi için destek sağlar olay aracılığıyla işleme *Temsilciler*. A *temsilci* gibi bir nesne yönelimli tür kullanımı uyumlu işlev işaretçisi olan &ndash; bir nesne başvurusu ve bir yöntem belirteci yalıtır. Bir olaya abone olmak istemci nesnesi istiyorsa, bir temsilci oluşturur ve temsilci bildiriliyor nesnesine iletir.
Olay ortaya çıktığında, bildirimde bulunan nesne olay abone istemci nesnesinin bildiren temsilci nesnesiyle temsil edilen yöntemini çağırır. C# ' ta olay işleyicileri temelde temsilciler çağrılan yöntemler'den fazla bir şey var.

Temsilciler hakkında daha fazla bilgi için bkz. MSDN [Temsilciler](https://msdn.microsoft.com/en-us/library/ms173171.aspx) konu.

C# ' ta olaylardır *çok noktaya yayın*; bir olay gerçekleştiğinde başka bir deyişle, birden çok dinleyici bildirilebilir. Bu fark, Java ve C# olay kaydı söz dizimi farklılıkları göz önüne aldığınızda, dikkate alınır. Java'da çağırmanız `SetXXXListener` C# kullanın; olay bildirimleri için kaydedilecek `+=` "temsilciniz olay dinleyicileri listesine ekleyerek" için olay bildirimleri kaydetmek için işleci.
Java'da, çağrı `SetXXXListener` kaydını silmek için while C kullandığınız #'ta `-=` "temsilciniz dinleyicileri listesinden çıkarmak için".

Xamarin.Android olaylar genellikle bir kullanıcı bir kullanıcı Arabirimi denetim için bir şeyler yaptığında nesneleri bildirmek için kullanılır. Normalde, bir kullanıcı Arabirimi kullanılarak tanımlanan üyeleri denetiminiz olur `event` anahtar sözcüğü; bu UI denetimi olaylarına abone olmak için bu üyeleri temsilcilerinizi iliştirin.

Bir olaya abone olmak için:

1.  Olay ortaya çıktığında çağrılan istediğiniz yönteme başvuran bir temsilci nesnesi oluşturun.

2.  Kullanım `+=` için abone olay temsilciniz iliştirmek için işleci.

Aşağıdaki örnek, bir temsilci tanımlar (açık kullanımını ile `delegate` anahtar sözcüğü) düğme tıklamalarına abone olmak için.
Bu düğme tıklama işleyicinin yeni bir etkinlik başlar:

```csharp
startActivityButton.Click += delegate {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

Ancak, aynı zamanda bir lambda ifadesi atlanıyor olaylar için kaydetmek için kullanabilirsiniz `delegate` anahtar sözcüğü tamamen. Örneğin:

```csharp
startActivityButton.Click += (sender, e) => {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

Bu örnekte, `startActivityButton` nesnesinin bir temsilci belirli yöntemi imzayla bekliyor bir olay vardır: gönderen ve olay bağımsız değişkenleri kabul eder ve void döndürür. Ancak, biz bildirme yöntemiyle imza böyle bir temsilci ya da kendi yöntemi açıkça tanımlamak için sorun Git istemediğiniz çünkü `(sender, e)` ve olay işleyicisi gövdesi uygulamak için bir lambda ifadesi kullanın.
Biz kullanmadığınız olsa bile bu parametre listesi bildirmek için sahibiz Not `sender` ve `e` parametreleri.

Bir temsilci erdirebilirsiniz unutulmaması önemlidir; (aracılığıyla `-=` işleci), ancak bir lambda ifadesi abonelik kaldırılamıyor &ndash; denenirse bellek sızıntıları neden olabilir. Yalnızca işleyicinizi olayından aboneliği değil, olay kaydının lambda formu kullanın.

Genellikle, lambda ifadeleri Xamarin.Android kodundaki olay işleyicileri bildirmek için kullanılır. Olay işleyicileri ilk, ancak karmaşık görünebilir bildirmek için bu toplu şekilde yazma ve okuma kod olduğunuzda muazzam alt dizin süreyi kaydeder. Benzerlik artan, (hangi sıklıkla Xamarin.Android kodda ortaya) bu deseni tanıma için alışkın olur ve iş mantığı uygulamanız ve daha az zaman söz dizimi yükü debelenmekten hakkında daha fazla zaman düşünmeye harcadığınız.


<a name="async" />

## <a name="asynchronous-programming"></a>Zaman uyumsuz programlama

*Zaman uyumsuz programlama* , uygulamanızın genel yanıt hızını artırmak için bir yoldur. Zaman uyumsuz programlama özellikler uygulamanızı kısmı uzun bir işlem tarafından engellendi sırada çalıştırmaya devam etmek için uygulama kodunuzun geri kalanı için mümkün kılar.
Web'e erişme, görüntülerin işlenmesi ve okuma/yazma dosyaları zaman uyumsuz olarak yazılmaz varsa donmasına görünmesi tüm bir uygulamayı neden olan işlemleri bir örnektir.

C# dil düzeyi üzerinden zaman uyumsuz programlama desteği içeren `async` ve `await` anahtar sözcükler. Bu dil özellikleri, uygulamanın ana iş parçacığı engellenmeden uzun süre çalışan görevler gerçekleştiren kod yazmaya çok kolay hale getirir. Kısaca, kullandığınız `async` yöntemindeki kod zaman uyumsuz olarak çalıştırın ve arayanın iş parçacığı engellemek değil olduğunu belirtmek için bir yöntem on anahtar sözcüğü. Kullandığınız `await` yöntemlerini çağırdığınızda anahtar sözcüğü ile işaretlenmiş `async`. Derleyici yorumlar `await` olduğu yöntemi yürütme (bir görevin çağırana döndürülür) bir arka plan iş parçacığı taşınmasına noktası olarak. Bu görev tamamlandığında arayanın parçacığında kodunun yürütülmesini sürdürür `await` noktası sonuçları döndüren kodunuzda `async` çağırın. Kurala göre zaman uyumsuz olarak çalıştır yöntemlerine sahip `Async` adlarını son ekini alır.

Xamarin.Android uygulamaları `async` ve `await` genellikle kullanıcı girişine yanıt vermesini sağlayabilirsiniz kullanıcı Arabirimi iş parçacığını boşaltmak için kullanılır (dokunma, gibi bir **iptal** düğmesi) uzun süre çalışan işlemi gerçekleşir ancak bir arka plan görevi.

Aşağıdaki örnekte, bir düğme, görüntüyü Web'den için zaman uyumsuz bir işlem olay işleyicisi nedenler tıklatın:


```csharp
downloadButton.Click += downloadAsync;
...
async void downloadAsync(object sender, System.EventArgs e)
{
    webClient = new WebClient ();
    var url = new Uri ("http://photojournal.jpl.nasa.gov/jpeg/PIA15416.jpg");
    byte[] bytes = null;

    bytes = await webClient.DownloadDataTaskAsync(url);

    // display the downloaded image ...
```

Bu örnekte, kullanıcı tıklattığında `downloadButton` denetimi `downloadAsync` olay işleyicisi oluşturur bir `WebClient` nesne ve bir `Uri` görüntüyü belirtilen URL'den getirmek için nesne. Ardından, çağıran `WebClient` nesnenin `DownloadDataTaskAsync` görüntüyü almak için bu URL'yi yöntemiyle.

Dikkat yöntemi bildirimi `downloadAsync` tarafından başında `async` , zaman uyumsuz olarak çalışacak ve bir görev döndürür olduğunu göstermek için anahtar sözcüğü. Ayrıca çağrısı `DownloadDataTaskAsync` öncesinde `await` anahtar sözcüğü. Uygulama olay işleyicisi yürütme taşır (noktasından başlayarak burada `await` görünür) kadar bir arka plan iş parçacığı için `DownloadDataTaskAsync` tamamlandıktan ve döndürür.
Bu sırada, uygulamanın kullanıcı Arabirimi iş parçacığı hala kullanıcı girişine yanıt verir ve diğer denetimler için olay işleyicileri tetiklenecek. Zaman `DownloadDataTaskAsync` tamamlandıktan (hangi birkaç saniye sürebilir), yürütme sürdürür nerede `bytes` değişkeni çağrının sonucunu ayarlanmış `DownloadDataTaskAsync`, ve olay işleyicisi kodunu geri kalanı indirilen görüntü arayanın üzerinde (UI) görüntüler. iş parçacığı.

Giriş için `async` / `await` C# ' ta MSDN bakın [uyumsuz ve bekleme ile zaman uyumsuz programlama](https://msdn.microsoft.com/en-us/library/hh191443.aspx) konu.
Zaman uyumsuz programlama özellikler Xamarin desteği hakkında daha fazla bilgi için bkz: [zaman uyumsuz desteğe genel bakış](~/cross-platform/platform/async.md).


<a name="keywords" />

## <a name="keyword-differences"></a>Anahtar sözcüğü farklar

C# ' de Java'da kullanılan birçok dil anahtar sözcükleri kullanılır. Aynı zamanda bu tabloda listelendiği gibi aynı zamanda bir eşdeğer ancak farklı adlandırılmış karşılık gelen C# ' ta sahip Java anahtar sözcükleri sayısı vardır:

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
    <th>
        <strong>Java</strong>
    </th>
    <th>
        <strong>C#</strong>
    </th>
    <th>
        <strong>Açıklama</strong>
    </th>
  </thead>
  <tbody>
    <tr>
      <td valign="top">
        <code>boolean</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/c8f5xwh7.aspx">bool</a>
      </td>
      <td valign="top">
Boole değerleri bildirmek için kullanılan <code>true</code> ve <code>false</code>.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>extends</code>
      </td>
      <td valign="top">
        <code>:</code>
      </td>
      <td valign="top">
Sınıf ve arabirimler devralmak için önce gelir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>implements</code>
      </td>
      <td valign="top">
        <code>:</code>
      </td>
      <td valign="top">
Sınıf ve arabirimler devralmak için önce gelir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>import</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/zhdeatwt.aspx">Kullanma</a>
      </td>
      <td valign="top">
Ayrıca bir ad alanı diğer adı oluşturmak için kullanılan bir ad alanı türlerini alır.
      </td>
    </tr>
    <tr>
      <td valign="final">
        <code>final</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/88c54tsw.aspx">korumalı</a>
      </td>
      <td valign="top">
Sınıf türetme engeller; yöntemleri ve özellikleri türetilmiş sınıflarda geçersiz kılınmış engeller.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>instanceof</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/scekt9xw.aspx">is</a>
      </td>
      <td valign="top">
Bir nesneyi belirtilen bir türle uyumlu olup olmadığını değerlendirir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>native</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/e59b22c5.aspx">extern</a>
      </td>
      <td valign="top">
Harici olarak uygulanan bir yöntem bildirir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>package</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/z2kcy19k.aspx">Namespace</a>
      </td>
      <td valign="top">
İlgili nesne kümesi için bir kapsam bildirir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>T...</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/w5zay9db.aspx">params T[]</a>
      </td>
      <td valign="top">
Değişken sayıda bağımsız değişken alan bir yöntem parametresi belirtir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>super</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/hfw7t1ce.aspx">temel</a>
      </td>
      <td valign="top">
Türetilmiş bir sınıf içinde üst sınıftan üyelerine erişmek için kullanılır.
      </td>
    </tr>
    <tr>
      <td valign="synchronized">
        <code>synchronized</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/c5kehkcz.aspx">kilitleme</a>
      </td>
      <td valign="top">
Kilit edinme ve sürüm koduyla önemli bir bölümünü sarmalar.
      </td>
    </tr>
  </tbody>
</table>


Ayrıca, C# için benzersiz ve Java hiçbir karşılık gelen birçok anahtar vardır. Xamarin.Android kodu genellikle yapar aşağıdaki C# anahtar sözcüklerini kullanın (Bu tablonun Xamarin.Android okunurken başvurmak yararlıdır [örnek koduna](https://developer.xamarin.com/samples/android/all/)):


<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
    <th>
        <strong>C#</strong>
    </th>
    <th>
        <strong>Açıklama</strong>
    </th>
  </thead>
  <tbody>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/cscsdfbt.aspx">olarak</a>
      </td>
      <td valign="top">
Uyumlu başvuru türleri veya boş değer atanabilir türleri arasında dönüştürmeler gerçekleştirir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/hh156513.aspx">Zaman uyumsuz</a>
      </td>
      <td valign="top">
Bir yöntem veya lambda ifadesi zaman uyumsuz olduğunu belirtir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/hh156528.aspx">await</a>
      </td>
      <td valign="top">
Bir görevi tamamlanana kadar bir yönteminin yürütülmesi askıya alır.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/5bdb6693.aspx">Bayt</a>
      </td>
      <td valign="top">
İmzasız 8 bit tam sayı türü.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/900fyy8e.aspx">Temsilci</a>
      </td>
      <td valign="top">
Bir yöntemi veya anonim yöntemi kapsüllemek için kullanılır.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/sbbt4032.aspx">Enum</a>
      </td>
      <td valign="top">
Bir numaralandırma adlandırılmış sabitler kümesini bildirir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/8627sbea.aspx">Olay</a>
      </td>
      <td valign="top">
Bir yayımcı sınıf olayda bildirir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/f58wzh21.aspx">Sabit</a>
      </td>
      <td valign="top">
Bir değişken taşınmasını engeller.
      </td>
    </tr>
    <tr>
      <td valign="top">
get </td>
      <td valign="top">
Bir özelliğin değerini alan bir erişimci yöntemi tanımlar.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/dd469484.aspx">İçinde</a>
      </td>
      <td valign="top">
Daha az türetilmiş bir tür genel arabiriminde kabul etmek bir parametre sağlar.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/9kkx3h3c.aspx">object</a>
      </td>
      <td valign="top">
İçin bir diğer ad <code>Object</code> .NET Framework türü.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/t3c3bfhx.aspx">Çıkışı</a>
      </td>
      <td valign="top">
Parametresi değiştiricisi veya genel tür parametresi bildirimi.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/ebca9ah3.aspx">geçersiz kılma</a>
      </td>
      <td valign="top">
Genişletir ya da bir devralınan üye uyarlamasını değiştirir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/6b0scde8.aspx">Kısmi</a>
      </td>
      <td valign="top">
Birden çok dosyaya bölme için tanımını bildirir veya kendi uygulama yöntemi tanımından böler.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/acdd6hb7.aspx">salt okunur</a>
      </td>
      <td valign="top">
Sınıf üyesi bildirim zamanında yalnızca veya sınıf oluşturucu tarafından atanabilir bildirir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/14akc2c7.aspx">Ref</a>
      </td>
      <td valign="top">
Başvuru yerine değere göre geçirilecek bağımsız değişken neden olur.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/ms228368.aspx">ayarlama</a>
      </td>
      <td valign="top">
Bir özelliğin değerini ayarlar bir erişimci yöntemi tanımlar.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/362314fe.aspx">Dize</a>
      </td>
      <td valign="top">
Alias için <code>String</code> .NET Framework türü.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/ah19swz4.aspx">yapısı</a>
      </td>
      <td valign="top">
Bir grup ilgili değişkenlerin yalıtan bir değer türü.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/58918ffs.aspx">typeof</a>
      </td>
      <td valign="top">
Bir nesne türünü alır.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/bb383973.aspx">var</a>
      </td>
      <td valign="top">
Örtük olarak yazılan yerel değişken bildirir.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/a1khb4f8.aspx">Değer</a>
      </td>
      <td valign="top">
Bir özellik atamak için istemci kodu istediği değeri başvuruyor.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/9fkccyh4.aspx">Sanal</a>
      </td>
      <td valign="top">
Türetilen bir sınıfta geçersiz kılınmak üzere bir yöntem sağlar.
      </td>
    </tr>
  </tbody>
</table>


<a name="interop" />

## <a name="interoperating-with-existing-java-code"></a>Varolan Java kod ile birlikte çalışma

C# dönüştürmek istiyor musunuz mevcut Java işlevselliğini varsa, varolan Java kitaplıklarınızda iki teknikleri üzerinden Xamarin.Android uygulamaları yeniden kullanabilir:

-  **Java bağlamaları kitaplığı oluşturma** &ndash; bu yaklaşımı kullanarak, Xamarin araçları C# Java türleri geçici sarmalayıcılar için kullanırsınız. Bu sarmalayıcıları adlı *bağlamaları*. Sonuç olarak, Xamarin.Android uygulamanız kullanabilir, *.jar* bu sarmalayıcıları çağırarak dosya.

-  **Java yerel arabirimi** &ndash; *Java yerel arabirimi* (JNI) uygulamaların arayın veya Java kodu tarafından çağrılması C# olanaklı kılan bir çerçevedir.

Bu teknikler hakkında daha fazla bilgi için bkz: [Java tümleştirmesine genel bakış](~/android/platform/java-integration/index.md).


<a name="further" />

## <a name="for-further-reading"></a>Daha Fazla Bilgi İçin

MSDN [C# programlama kılavuzu](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx) C# programlama dili öğrenme içinde başlamak için mükemmel bir yoldur ve kullanabileceğiniz [C# başvurusu](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx) belirli C# dil özellikleri aramak için.

Java bilgi Java dil bilerek olarak Java sınıf kitaplıkları aşina hakkında daha fazla en az olduğunu aynı şekilde pratik bilgiye C# .NET framework aşina gerektirir. Microsoft'un [C# ve .NET Framework için Java geliştiriciler için taşıma](https://www.microsoft.com/en-us/download/details.aspx?id=6073) pakettir (C# daha derin bir anlayış elde ederken) .NET framework Java açısından hakkında daha fazla bilgi edinmek için en iyi yolu öğrenme.

C# ' ta, ilk Xamarin.Android projenizi üstesinden gelmek hazır olduğunuzda bizim [Hello, Android](~/android/get-started/hello-android/index.md) serisi, ilk Xamarin.Android uygulamanızı oluşturmak ve Android temelleri Anlayışınızı daha gelişmiş yardımcı olabilir Xamarin ile uygulama geliştirme.


<a name="summary" />

## <a name="summary"></a>Özet

Bu makalede bir Java geliştirme açısından Xamarin.Android C# programlama ortamı için bir giriş sağlanır. Bu C# ve Java arasındaki benzerlikler çıkışı pratik farklılıkları açıklayan sırasında işaret. Derlemeler ve ad alanları sunulan, dış türlerini içeri aktarma açıklandığı ve erişim değiştiricileri, genel türler, taban sınıf yöntemlerini, yöntemini geçersiz kılma ve olay işleme çağrılırken, sınıf türetme farklılıkları genel bir bakış sağlanan. Bulunmayan özellikler gibi Java, C# özelliklerini sunulan `async` / `await` zaman uyumsuz programlama, Lambda'lar, C# Temsilciler ve sistem C# olay işleme. Önemli C# anahtar sözcükleri tablo da dahil, varolan Java kitaplıklarla birlikte çalışmak nasıl açıklanmıştır ve daha fazla olay incelemesi için ilgili belgelere yönelik bağlantılar sağlanmaktadır.


## <a name="related-links"></a>İlgili bağlantılar

- [Java tümleştirmesine genel bakış](~/android/platform/java-integration/index.md)
- [C# Programlama Kılavuzu](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx)
- [C# başvurusu](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx)
- [C# ve .NET Framework için Java geliştiriciler için taşıma](https://www.microsoft.com/en-us/download/details.aspx?id=6073)
