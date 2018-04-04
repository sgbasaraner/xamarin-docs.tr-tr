---
title: Bağlama bir. JAR
description: Bu kılavuz, bir Android bir Xamarin.Android Java bağlamaları kitaplığı oluşturmak için adım adım yönergeler sağlar. JAR dosyasını.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: a6cb08f19aac46ffa089914e28c732660caa52b2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-a-jar"></a>Bağlama bir. JAR

_Bu kılavuz, bir Android bir Xamarin.Android Java bağlamaları kitaplığı oluşturmak için adım adım yönergeler sağlar. JAR dosyasını._

## <a name="overview"></a>Genel Bakış

Android topluluk uygulamanızı kullanmayı düşünebilirsiniz birçok Java kitaplıkları sunar. Bu Java kitaplıkları genellikle içinde paketlenir. JAR (Java arşivi) biçimi, ancak paket bir. İçinde JAR bir *Java bağlamaları Kitaplığı* işlevselliğini Xamarin.Android uygulamaları için kullanılabilir olmasını sağlayın. Java bağlamaları kitaplığı amacı API'leri olmasını sağlamaktır. JAR dosyasını otomatik olarak oluşturulan kod sarmalayıcıları aracılığıyla C# kod için kullanılabilir.

Xamarin araç bağlamaları kitaplığı bir veya daha fazla girişten oluşturabilir. JAR dosyaları. Bağlamaları kitaplığı (. DLL derleme) aşağıdakileri içerir: 

-   Özgün içeriği. JAR dosyaları.

-   Yönetilen aranabilir sarmalayıcıları (C# olan MCW), karşılık gelen Java türleri içinde bu kaydırma türleri. JAR dosyaları.

Oluşturulan MCW kod için temel API çağrılarını iletmek için JNI (Java yerel arabirimi) kullanır. JAR dosyasını. Bağlamaları kitaplıkları için oluşturabilirsiniz. Android (Not Xamarin araçları şu anda Android Java olmayan kitaplıkların bağlamayı desteklemez) ile kullanılacak hedeflendiğini JAR dosyasını. İçeriği de dahil olmak üzere olmadan bağlamaları kitaplığını oluşturmak de seçebilirler. DLL bir bağımlılığa sahip olması dosya JAR. Çalışma zamanında JAR.

Bu kılavuzda, biz tek bir için bağlamaları kitaplığı oluşturma temelleri adım. JAR dosyasını. Burada her şeyi gider doğru bir örnek ile göstermeye &ndash; diğer bir deyişle, özelleştirme yok veya bağlamalarını hata ayıklama gerekli olduğu. 
[Bağlamaları kullanarak meta verileri oluşturma](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) nerede bağlama işlemi tamamen otomatik değildir ve miktar el ile müdahale gerekli olduğuna daha gelişmiş bir senaryo örneği sunar. Java kitaplığı (temel kod örneği ile) genel bağlama genel bakış için bkz: [bir Java kitaplığı bağlama](~/android/platform/binding-java-library/index.md). 

 
## <a name="walkthrough"></a>İzlenecek yol

Aşağıdaki kılavuzda için bağlamaları kitaplığı oluşturacağız [Picasso](http://square.github.io/picasso/), popüler Android. Yükleme ve önbelleğe alma işlevinin görüntü sağlar JAR. Aşağıdaki adımları bağlamak için kullanacağız **picasso 2.x.x.jar** biz kullanabileceğiniz yeni bir .NET derlemesi bir Xamarin.Android projesi oluşturmak için: 

1. Yeni bir Java bağlamaları kitaplık projesi oluşturun.

2. Ekleyin. Projesine JAR dosyasını.

3. Uygun yapı eylemi için ayarlayın. JAR dosyasını.

4. Bir hedef Framework'ü seçin. JAR destekler.

5. Bağlamaları kitaplığı oluşturun.

Biz bağlamaları kitaplığı oluşturduktan sonra biz bağlamaları Kitaplığı'nda API'leri çağırmak yeteneği bizim gösteren küçük bir Android uygulaması geliştireceksiniz. Bu örnekte, yöntemleri erişim istiyoruz **picasso 2.x.x.jar**:

```java
package com.squareup.picasso

public class Picasso
{ 
    ...
    public static Picasso with (Context context) { ... };
    ...
    public RequestCreator load (String path) { ... };
    ...
}

```csharp
After we generate a Bindings Library for **picasso-2.x.x.jar**,
we can call these methods from C#. For example:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```


### <a name="creating-the-bindings-library"></a>Bağlamaları kitaplığı oluşturma

Aşağıdaki adımlarla başlatmadan önce lütfen indirin [picasso 2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar).

İlk olarak, yeni bağlamalar kitaplık projesi oluşturun. Visual Studio'da Mac veya Visual Studio için yeni bir çözüm oluşturun ve seçin *Android bağlamaları Kitaplığı* şablonu. (Bu kılavuzda ekran görüntüleri Visual Studio'yu kullanın, ancak Mac için Visual Studio çok benzer.) Çözüm adı **JarBinding**: 

[![JarBinding kitaplığı projesi oluşturma](binding-a-jar-images/01-new-bindings-library-sml.png)](binding-a-jar-images/01-new-bindings-library.png#lightbox)

Şablon içeren bir **Jar'lar** eklediğiniz klasörü. Bağlamaları kitaplık projesine JAR(s). Sağ **Jar'lar** klasörü ve select **Ekle > varolan öğeyi**: 

[![Varolan öğeyi Ekle](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

Gidin **picasso 2.x.x.jar** dosyası karşıdan daha önce seçin ve **Ekle**: 

[![Jar dosyasını seçin ve Ekle'yi tıklatın](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

Doğrulayın **picasso 2.x.x.jar** dosyası projeye başarıyla eklendi: 

[![Jar projesine eklendi](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Bir Java bağlamaları kitaplığı projesi oluşturduğunuzda, belirtmelisiniz olup olmadığını. JAR bağlamaları Kitaplığı'nda katıştırılmış veya ayrı olarak paketlenmiş sağlamaktır. Bunu yapmak için aşağıdakilerden birini belirtmeniz *Eylemler yapı*: 

-   **EmbeddedJar** &ndash; . JAR bağlamaları Kitaplığı'nda katıştırılır.

-   **InputJar** &ndash; . JAR bağlamaları Kitaplığı'ndan ayrı tutulur.

Genellikle, kullandığınız **EmbeddedJar** derleme eylemi böylece. JAR bağlamaları kitaplığa otomatik olarak paketlenir. En basit seçenek budur &ndash; Java bayt. JAR Dex bayt koduna dönüştürülür ve (yönetilen aranabilir sarmalayıcılar birlikte), APK katıştırılır. Tutmak istiyorsanız. Bağlamaları Kitaplığı'ndan ayrı JAR, kullanabileceğiniz **InputJar** seçeneği; ancak, emin olmanız gerekir. JAR dosyasını uygulamanızın çalıştırdığı cihazda kullanılabilir. 

Yapı eyleminin kümesine **EmbeddedJar**: 

[![EmbeddedJar yapı eylemi seçin](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

Ardından, projeyi yapılandırmak için özellikler açmak *hedef Framework*. Varsa. JAR tüm Android API'lerini kullanır, API düzeyi hedef Framework'ü ayarlayın. JAR bekliyor. Genellikle, geliştiricisi. Hangi API düzeyi (veya düzeyleri) JAR dosyasını gösterilir,. JAR ile uyumludur. (Hedef Framework ayarını ve genel Android API düzeyleri hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).)

Hedef API'den bağlamaları kitaplığınızda düzeyi ayarlayın (Bu örnekte, API düzeyi 19 kullanıyoruz): 

[![API 19 hedef API düzeyini ayarlayın](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)


Son olarak, bağlamaları kitaplığı oluşturun. Bazı uyarı iletilerini görüntülenebilir ancak bağlamaları kitaplığı proje başarılı bir şekilde oluşturmak ve bir çıktı üretir. DLL şu konumda: **JarBinding/bin/Debug/JarBinding.dll**
    


### <a name="using-the-bindings-library"></a>Bağlamaları kitaplığı kullanma

Bu kullanmak için. DLL'si Xamarin.Android uygulamanıza aşağıdakileri yapın:

1.  Bağlamaları kitaplığına bir başvuru ekleyin.

2.  Çağrılar yapın. Yönetilen aranabilir sarmalayıcılar JAR. 

Aşağıdaki adımlarda, indirin ve görüntüyü görüntülemek için bağlamaları kitaplığını kullanan en az bir uygulama oluşturacağız bir `ImageView`; "ağır lifting" bulunan kodu tarafından yapılır. JAR dosyasını. 

İlk olarak, bağlamaları kitaplığı tüketir yeni bir Xamarin.Android uygulaması oluşturma. Çözüme sağ tıklayın ve seçin **Yeni Proje Ekle**; yeni proje adı **BindingTest**. Bu uygulama bağlamaları kitaplığı olarak aynı çözümde bu kılavuzda basitleştirmek için oluşturuyoruz; Ancak, bağlamaları kitaplığı tüketen uygulamayı bunun yerine, farklı bir çözümde bulunan olabilir: 

[![Yeni BindingTest proje ekleyin](binding-a-jar-images/07-add-new-project-sml.png)](binding-a-jar-images/07-add-new-project.png#lightbox)

Sağ **başvuruları** düğümünün **BindingTest** proje ve seçin **Başvuru Ekle...** :

[![Sağ başvuru ekleme](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

Denetleme **JarBinding** daha önce oluşturduğunuz proje ve tıklatın **Tamam**:

[![JarBinding projesini seçin](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

Açık **başvuruları** düğümünün **BindingTest** proje ve doğrulayın **JarBinding** başvuru varsa: 

[![JarBinding başvuruları altında görünür](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

Değiştirme **BindingTest** Düzen (**Main.axml**) tek bir sahip olması `ImageView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/imageView" />
</LinearLayout>
```

Aşağıdakileri ekleyin `using` ifadesine **MainActivity.cs** &ndash; bu Java tabanlı yöntemlerinin kolayca erişmesini mümkün kılar `Picasso` bağlamaları Kitaplığı'nda bulunan sınıfı:

```csharp
using Com.Squareup.Picasso;
```

Değiştirme `OnCreate` onun kullanan yöntemi `Picasso` görüntünün bir URL'den yüklemek ve içinde görüntülemek için sınıf `ImageView`: 

```csharp
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
        ImageView imageView = FindViewById<ImageView>(Resource.Id.imageView);

        // Use the Picasso jar library to load and display this image:
        Picasso.With (this)
            .Load ("http://i.imgur.com/DvpvklR.jpg")
            .Into (imageView);
    }
}
```

Derleme ve çalıştırma **BindingTest** projesi. Uygulama başlatılacak ve (bağlı olarak ağ koşulları) kısa bir gecikmeyle indirin ve aşağıdaki ekran görüntüsüne benzer bir resmi görüntüle:

[![Ekran görüntüsü, BindingTest çalışıyor](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

Tebrikler! Java kitaplığı başarıyla bağladınız. JAR ve Xamarin.Android uygulamanıza kullanılır.
 
 
## <a name="summary"></a>Özet

Bu kılavuzda, üçüncü taraf bağlamaları kitaplığının oluşturduk. En az test uygulaması için eklenen bağlamaları kitaplığı JAR dosyasını ve bizim C# kodu bulunan Java kod çağırabilir doğrulamak üzere uygulamasını çalıştıran. JAR dosyasını. 



## <a name="related-links"></a>İlgili bağlantılar

- [Java bağlamaları kitaplığı (video) oluşturma](https://university.xamarin.com/classes#10090)
- [Java Kitaplığını Bağlama](~/android/platform/binding-java-library/index.md)
