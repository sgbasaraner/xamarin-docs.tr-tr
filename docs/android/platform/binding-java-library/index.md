---
title: Java kitaplığı bağlama
description: Android topluluk uygulamanızı kullanmayı düşünebilirsiniz birçok Java kitaplıkları; yine de sahip istiyor musunuz? Bu kılavuz, Java kitaplıkları bağlamaları kitaplığı oluşturarak Xamarin.Android uygulamanıza eklemenizi açıklanmaktadır.
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 3c2ed92da07519516db94697bbeaac9f328baa22
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30768827"
---
# <a name="binding-a-java-library"></a>Java kitaplığı bağlama

_Android topluluk uygulamanızı kullanmayı düşünebilirsiniz birçok Java kitaplıkları; yine de sahip istiyor musunuz? Bu kılavuz, Java kitaplıkları bağlamaları kitaplığı oluşturarak Xamarin.Android uygulamanıza eklemenizi açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Android için üçüncü taraf kitaplığı ekosistemi büyük. Bu nedenle, sık daha yeni bir tane oluşturmak için varolan Android kitaplığını kullanmak için mantıklıdır. Xamarin.Android Bu kitaplıklar kullanmak için iki yol sunar:

-   Oluşturma bir *bağlamaları Kitaplığı* , otomatik olarak kaydırılıp C# sarmalayıcıları kitaplıkla Java kodu C# çağrıları aracılığıyla çağırabileceği şekilde.

-   Kullanım *Java yerel arabirimi* (*JNI*) Java kitaplık kodu çağrılarında doğrudan çağırmak için. JNI çağırın ve yerel uygulamalar veya kitaplıkları tarafından çağrılması için Java kod sağlayan bir programlama çerçevesidir.

Bu kılavuz, ilk seçenek açıklar: oluşturmak nasıl bir *bağlamaları Kitaplığı* , saran bir veya daha fazla var olan Java kitaplıkları uygulamanızda bağlayabilirsiniz bir bütünleştirilmiş. JNI kullanma hakkında daha fazla bilgi için bkz: [JNI ile çalışma](~/android/platform/java-integration/working-with-jni.md).

Xamarin.Android kullanarak bağlamaları uygulayan *yönetilen aranabilir sarmalayıcılar* (*MCW*). Yönetilen kod Java kod çağırma gerektiğinde, kullanılan JNI köprü MCW olur. Yönetilen aranabilir sarmalayıcılar Java türleri sınıflara ve Java türlerinde sanal yöntemleri geçersiz kılmak için de destek sağlar. Yönetilen kod çağırmak Android çalışma zamanı (resim) kod istediği zaman, benzer şekilde, bunu Android aranabilir sarmalayıcıları (ACW olarak) bilinen başka bir JNI köprüsü aracılığıyla yapar. Bu [mimarisi](~/android/internals/architecture.md) Aşağıdaki diyagramda gösterilmiştir:

[![Android JNI köprüsü mimarisi](images/architecture.png)](images/architecture.png#lightbox)

Java türleri için yönetilen aranabilir sarmalayıcılar içeren bir derleme bağlamaları kitaplığıdır. Örneğin, bir Java türü işte `MyClass`, bağlamaları kitaplığa sarmalamak istiyoruz:

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

Biz için bağlamaları kitaplığı oluşturduktan sonra **.jar** içeren `MyClass`, biz Bu örneği ve arama yöntemleri üzerinde C# ' dan:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Xamarin.Android kullandığınız bu bağlamaları kitaplığı oluşturmak için *Java bağlamaları Kitaplığı* şablonu. Sonuçta elde edilen bağlama proje MCW sınıflarıyla bir .NET derlemesi oluşturur **.jar** dosyaları ve katıştırılmış Android kitaplık projeleri için kaynakları. Bağlamaları kitaplıkları için Android arşiv oluşturabilirsiniz (. AAR) dosyaları ve Eclipse Android kitaplık projeleri. Sonuçta elde edilen bağlamaları kitaplığı DLL derleme başvurarak Xamarin.Android projenizi varolan Java kitaplığı yeniden kullanabilirsiniz.

Bağlama kitaplığınızda türleri başvurduğunuzda bağlama kitaplığınızın ad kullanmanız gerekir. Genellikle, eklediğiniz bir `using` C# kaynak üstündeki yönergesi dosyaları başka bir deyişle Java Paket adı .NET ad alanı sürümü. Örneğin, bağlama için Java Paket adı, **.jar** aşağıdaki gibidir:

```csharp
com.company.package
```

Aşağıdaki koyabilirsiniz sonra `using` erişmek için C# kaynak dosyalarınız üstündeki deyimi bağımlı türleri **.jar** dosyası:

```csharp
using Com.Company.Package;
```


Varolan bir Android kitaplığıdır bağlanırken, aşağıdaki noktaları göz önünde bulundurmanız gereklidir:

* **Kitaplık için dış bağımlılıklar vardır?** &ndash; Android kitaplığı tarafından gerekli Java bağımlılıkları Xamarin.Android projesinde eklenmelidir bir **ReferenceJar** veya farklı bir **EmbeddedReferenceJar**. Tüm yerel derlemelerde olarak bağlama projeye eklenmelidir bir **EmbeddedNativeLibrary**.  

* **Hangi Android API sürümü Android kitaplığı hedef mu?** &ndash; "; Android API düzeyini düşürmek" olası değil Xamarin.Android bağlama proje düzeyi (veya sonrası) aynı API hedeflediğinden emin olarak Android kitaplığıdır.

* **JDK hangi sürümünün kitaplık derlemek için kullanılan?** &ndash; Android kitaplığı Xamarin.Android tarafından kullanımda JDK farklı bir sürümü ile oluşturulmuş bağlama hataları oluşabilir. Mümkünse, Xamarin.Android yüklemenizin tarafından kullanılan JDK'ın aynı sürümünü kullanarak Android kitaplığı yeniden derleyin.


## <a name="build-actions"></a>Eylemler derleme

Bağlamaları kitaplığı oluşturduğunuzda, ayarladığınız *Eylemler yapı* üzerinde **.jar** veya. Bağlamaları kitaplığını projenize ekleyeceğiniz AAR dosyalarını &ndash; her bir yapı eylemi belirler nasıl **.jar** veya. AAR dosyası içine katıştırılmış (veya kaldırılacak tarafından başvurulan) bağlamaları kitaplığınız. Aşağıdaki listede bu özetlenmektedir Eylemler oluşturabilir:

* `EmbeddedJar` &ndash; Katıştırır **.jar** katıştırılmış bir kaynağı olarak elde edilen bağlamaları kitaplığı DLL içine. Bu basit ve en yaygın olarak kullanılan yapı eylem değildir. İstediğiniz zaman bu seçeneği kullanın **.jar** otomatik olarak bayt koda derlenmiş ve bağlamaları kitaplığa paketlenir.

* `InputJar` &ndash; Değil katıştırmak **.jar** sonuç bağlamaları kitaplığa. DLL. Bağlamaları kitaplığınız. DLL bir bağımlılığa sahip olacak bu **.jar** çalışma zamanında. Dahil etmek istemiyorsanız bu seçeneği kullanın **.jar** bağlamaları kitaplığınızdaki (örneğin, nedeniyle lisans). Bu seçeneği kullanırsanız, giriş emin olmalısınız **.jar** uygulamanızın çalıştırdığı cihazda kullanılabilir.

* `LibraryProjectZip` &ndash; Katıştırır bir. Sonuçta elde edilen bağlamaları kitaplığa AAR dosyası. DLL. Bağımlı kaynaklar (yanı sıra kodu) erişmek için bu EmbeddedJar için benzerdir. AAR dosyası. Katıştırmak istediğinizde bu seçeneği kullanın bir. AAR bağlamaları kitaplığa.

* `ReferenceJar` &ndash; Bir başvuru belirtir **.jar**: başvuru **.jar** olan bir **.jar** bağımlı birinin **.jar** veya. AAR dosyalarını bağlıdır. Bu başvuru **.jar** yalnızca derleme zamanı bağımlılıklarını karşılamak için kullanılır. Bu yapı eylemini kullandığınızda, C# bağlamaları başvurusunu oluşturulmaz **.jar** ve sonuçta elde edilen bağlamaları Kitaplığı'nda ekli değil. DLL. Başvuru için bağlamaları kitaplığı yapacak olduğunda bu seçeneği kullanın **.jar** ancak henüz yapmadıysanız. Bu derleme birden çok paketlemek için yararlı bir eylemdir **.jar**s (ve/veya. AARs) birden çok birbirine bağlamaları kitaplıkları içine.

* `EmbeddedReferenceJar` &ndash; Bir başvuru katıştırır **.jar** sonuç bağlamaları kitaplığa. DLL. C# hem giriş bağlantılarında oluşturmak istediğinizde bu yapı eylemi kullanın **.jar** (veya. AAR) ve tüm alt başvuru **.jar**(s) bağlamaları kitaplığınızdaki.

* `EmbeddedNativeLibrary` &ndash; Yerel katıştırır **.so** bağlama içine. Bu yapı eylemi için kullanılan **.so** tarafından gerekli olan dosyalar **.jar** bağlanan dosya. El ile yüklemek gerekli olabilir **.so** Java Kitaplığı'ndan kod çalıştırmadan önce kitaplığı. Bu aşağıda açıklanmıştır.

Bu yapı eylemleri aşağıdaki kılavuzlardan daha ayrıntılı açıklanmıştır.

Ayrıca, aşağıdaki yapı eylemleri Java API belgelerine alma yardımcı olmak için kullanılır ve C# XML belgeleri dönüştürün:

* `JavaDocJar` Maven paket stiline uygun bir Java kitaplığı Javadoc arşiv Jar işaret etmek için kullanılan (genellikle `FOOBAR-javadoc**.jar**`).
* `JavaDocIndex` işaret etmek için kullanılan `index.html` API başvuru belgeleri HTML dosyasında.
* `JavaSourceJar` tamamlamak için kullanılan `JavaDocJar`, ilk JavaDoc kaynaklardan oluşturup ardından sonuçları kabul et `JavaDocIndex`, bir Maven uyan bir Java kitaplığı için stil paketini (genellikle `FOOBAR-sources**.jar**`).

API belgelerine Java8, Java7 ya da Java6 (oldukları tüm farklı biçimde) SDK'sı, varsayılan doclet olmalıdır veya DroidDoc stili.

## <a name="including-a-native-library-in-a-binding"></a>Yerel bir kitaplık içinde bir bağlaması dahil

Dahil etmek gerekli olabilir bir **.so** bir Java kitaplığı bağlama bir parçası olarak Xamarin.Android bağlama projesinde kitaplığı. Sarmalanan Java kod yürütüldüğünde, Xamarin.Android JNI çağrısı ve hata iletisi hale getirmek başarısız olur _java.lang.UnsatisfiedLinkError: yerel yöntem bulunamadı:_ logcat uygulama için görünür.

El ile yüklemek için bu düzeltmenin mi **.so** çağrısıyla Kitaplığı `Java.Lang.JavaSystem.LoadLibrary`. Örneğin bir Xamarin.Android projesi paylaşılan kitaplığı olduğunu varsayarak **libpocketsphinx_jni.so** bir yapı eylemi bağlama projeyle dahil **EmbeddedNativeLibrary**, aşağıdaki (paylaşılan kitaplık kullanmadan önce yürütülen) parçacığı yükler **.so** kitaplığı:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Java API'ları c uyarlama&eparsl;

Xamarin.Android bağlama Oluşturucu bazı Java deyimleri ve desenler .NET desenlerini karşılık gelecek şekilde değişir. Aşağıdaki listede nasıl Java C# eşlenmiş açıklanmaktadır / .NET:

-   _Ayarlayıcı/alıcı yöntemlere_ Java'da olan _özellikleri_ .NET içinde.

-   _Alanları_ Java'da olan _özellikleri_ .NET içinde.

-   _Dinleyicileri/dinleyicisi arabirimleri_ Java'da olan _olayları_ .NET içinde. Geri çağırma arabirimleri yöntemlerinde parametrelerinin gösterdiği bir `EventArgs` bir alt kümesi.

-   A _statik iç içe geçmiş sınıf_ Java'da olduğu bir _iç içe geçmiş sınıf_ .NET içinde.

-   Bir _iç sınıfı_ Java'da olduğu bir _iç içe geçmiş sınıf_ C# örnek oluşturucu ile.



## <a name="binding-scenarios"></a>Bağlama senaryoları

Aşağıdaki bağlama senaryo kılavuzları kurulduğu uygulamanız için bir Java kitaplığı (veya kitaplıklar) bağlamak yardımcı olabilir:

-   [Bağlama bir. JAR](~/android/platform/binding-java-library/binding-a-jar.md) için bağlamaları kitaplıkları oluşturmak için bir kılavuz olan **.jar** dosyaları.

-   [Bağlama bir. AAR](~/android/platform/binding-java-library/binding-an-aar.md) için bağlamaları kitaplıkları oluşturmak için bir kılavuz olduğu. AAR dosyalarını. Android Studio kitaplıkları bağlamak öğrenmek için bu kılavuzu okuyun.

-   [Eclipse kitaplığı projesinde bağlama](~/android/platform/binding-java-library/binding-a-library-project.md) Android kitaplığı projelerden bağlama kitaplıkları oluşturmak için bir kılavuz değil. Eclipse Android kitaplık projeleri bağlamak öğrenmek için bu kılavuzu okuyun.

-   [Bağlamaları özelleştirme](~/android/platform/binding-java-library/customizing-bindings/index.md) derleme hataları çözümleyin ve böylece daha fazla sonuç API şekil bağlama el ile yapılan değişiklikler yapma açıklanmaktadır "C#-gibi".

-   [Bağlamaları sorun giderme](~/android/platform/binding-java-library/troubleshooting-bindings.md) bağlama hatası senaryoları listeler, olası nedenleri açıklanmaktadır ve bu hatalar çözümü için öneriler sunar.


## <a name="related-links"></a>İlgili bağlantılar

- [JNI ile çalışma](~/android/platform/java-integration/working-with-jni.md)
- [GAPI Metadata](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [Yerel Kitaplıkları Kullanma](~/android/platform/native-libraries.md)
