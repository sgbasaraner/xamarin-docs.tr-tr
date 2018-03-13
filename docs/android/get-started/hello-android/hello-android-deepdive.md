---
title: "Merhaba, Android: Derinlemesine bakış"
description: "Bu iki parçalı Kılavuzu'nda ilk Xamarin.Android uygulamanızı oluşturmak ve Xamarin ile Android uygulaması geliştirme ile ilgili temel bilgileri bir anlayış geliştirmek. Yol boyunca araçları, kavramlar ve oluşturmak ve bir Xamarin.Android uygulaması dağıtmak için gerekli adımları görülecektir."
ms.topic: article
ms.prod: xamarin
ms.assetid: EF0E110B-20EA-43F6-9476-1A0F41AFD298
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: e9e554da80218d2e89ff79c6e89886d707b1ed95
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="hello-android-deep-dive"></a>Merhaba, Android: Derinlemesine bakış

_Bu iki parçalı Kılavuzu'nda ilk Xamarin.Android uygulamanızı oluşturmak ve Xamarin ile Android uygulaması geliştirme ile ilgili temel bilgileri bir anlayış geliştirmek. Yol boyunca araçları, kavramlar ve oluşturmak ve bir Xamarin.Android uygulaması dağıtmak için gerekli adımları görülecektir._

## <a name="hello-android-deep-dive"></a>Merhaba, Android derinlemesine bakış

İçinde [Hello, Android Hızlı Başlangıç](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md), yerleşik ve ilk Xamarin.Android uygulamanız çalıştırılmıştır. Şimdi daha karmaşık programları oluşturabilmeleri nasıl Android uygulamaları iş daha derin bir anlayış geliştirmek için zaman yapılır. Bu kılavuz, böylece ne yaptığınız anlamak ve Android uygulaması geliştirme temel bir anlayış geliştirmeye başlamak Hello, Android izlenecek sürdü adımları gözden geçirir.

Bu kılavuzda aşağıdaki konular touch:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Visual Studio giriş** &ndash; giriş Visual Studio ve yeni bir Xamarin.Android uygulaması oluşturma.

-   **Bir Xamarin.Android uygulaması anatomisi** -bir Xamarin.Android uygulaması temel bölümlerini turu.

-   **Uygulama temelleri ve mimari temel** &ndash; etkinlikleri, Android bildirim ve Android geliştirme genel örneğinizin giriş.

-   **Kullanıcı Arabirimi (UI)** &ndash; kullanıcı arabirimleri Android Tasarımcısı ile oluşturma.

-   **Etkinlikleri ve etkinlik yaşam döngüsü** &ndash; etkinlik yaşam döngüsü ve kablolama kodda kullanıcı arabiriminin bir giriş.

-   **Test, dağıtım ve son rötuşları** &ndash; sınama, dağıtım, oluşturma resmi ve daha fazla öneri uygulamanızla tamamlayın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   **Mac için Visual Studio giriş** &ndash; giriş Xamarin Studio ve yeni bir Xamarin.Android uygulaması oluşturma.

-   **Bir Xamarin.Android uygulaması anatomisi** &ndash; bir Xamarin.Android uygulaması temel bölümlerini turu.

-   **Uygulama temelleri ve mimari temel** &ndash; etkinlikleri, Android bildirim ve Android geliştirme genel örneğinizin giriş.

-   **Kullanıcı Arabirimi (UI)** &ndash; kullanıcı arabirimleri Android Tasarımcısı ile oluşturma.

-   **Etkinlikleri ve etkinlik yaşam döngüsü** &ndash; etkinlik yaşam döngüsü ve kablolama kodda kullanıcı arabiriminin bir giriş.

-   **Test, dağıtım ve son rötuşları** &ndash; sınama, dağıtım, oluşturma resmi ve daha fazla öneri uygulamanızla tamamlayın.

-----


Bu kılavuz becerileri ve bir tek ekran Android uygulaması oluşturmak için gerekli bilgileri geliştirmenize yardımcı olur. Üzerinden çalıştıktan sonra bir Xamarin.Android uygulaması ve onların birlikte nasıl uyduğunu farklı bölümlerini anlamanız gerekir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Visual Studio giriş

Visual Studio Microsoft güçlü bir IDE ' dir. Bir tam olarak tümleşik görsel tasarımcı yeniden düzenleme araçları, bir derleme tarayıcı, kaynak kodu tümleştirme ve daha fazla bilgi içeren bir metin Düzenleyicisi özellikleri. Bu kılavuzda eklenti Xamarin ile temel bazı Visual Studio özellikleri kullanmayı öğreneceksiniz.

Visual Studio düzenler koda _çözümleri_ ve _projeleri_. Bir çözümü bir veya daha fazla projeleri tutan bir kapsayıcıdır. Proje (iOS veya Android için olduğu gibi gibi) bir uygulama, bir destek kitaplığı, bir sınama uygulaması ve daha fazla olabilir. İçinde **Phoneword** uygulama, eklediğiniz yeni bir Android projesi kullanarak **Android uygulaması** şablonuna **Phoneword** oluşturulmuş çözümünü [Merhaba, Android](~/android/get-started/hello-android/hello-android-quickstart.md) Kılavuzu. 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Mac için Visual Studio giriş

Mac için Visual Studio bir ücretsiz, açık kaynaklı IDE Visual Studio'ya benzer ' dir. Tam olarak tümleşik bir görsel tasarımcı, yeniden düzenleme araçları ile tam bir metin düzenleyicisi, bir derleme tarayıcı, kaynak kodu tümleştirme ve daha fazla bilgi sunar. Bu kılavuzda, Mac özellikler için temel bazı Visual Studio kullanmayı öğreneceksiniz. Mac için Visual Studio yeniyseniz, daha kapsamlı denetlemek isteyebilirsiniz [Mac için Visual Studio giriş](https://docs.microsoft.com/visualstudio/mac/).

Mac için Visual Studio aşağıdaki koda düzenleme Visual Studio uygulama _çözümleri_ ve _projeleri_. Bir çözümü bir veya daha fazla projeleri tutan bir kapsayıcıdır. Proje (iOS veya Android için olduğu gibi gibi) bir uygulama, bir destek kitaplığı, bir sınama uygulaması ve daha fazla olabilir. İçinde **Phoneword** uygulama, eklediğiniz yeni bir Android projesi kullanarak **Android uygulaması** şablonuna **Phoneword** oluşturulmuş çözümünü [Merhaba, Android](~/android/get-started/hello-android/hello-android-quickstart.md) Kılavuzu.

-----

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinandroid-application"></a>Bir Xamarin.Android uygulaması anatomisi

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aşağıdaki ekran görüntüsünde çözümün içeriği listeler. Bu dizin yapısını ve tüm Çözümle ilişkili dosyaları içeren Çözüm Gezgini oluşur:

[![Çözüm Gezgini](hello-android-deepdive-images/vs/02-solution-structure-sml.png)](hello-android-deepdive-images/vs/02-solution-structure.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Aşağıdaki ekran görüntüsünde çözümün içeriği listeler. Bu çözüm, dizin yapısını ve tüm Çözümle ilişkili dosyaları içeren Pad'i oluşur:

[![Çözüm paneli](hello-android-deepdive-images/xs/02-solution-structure-sml.png)](hello-android-deepdive-images/xs/02-solution-structure.png#lightbox)

-----

Bir çözüm olarak adlandırılan **Phoneword** oluşturuldu ve Android projesi **Phoneword** bunun içinde yerleştirilir.

Öğeleri her klasör ve amacı görmek için proje içinde bakın:

-   **Özellikler** &ndash; içerir [AndroidManifest.xml](~/android/platform/android-manifest.md) tüm ad, sürüm numarasını ve izinler de dahil olmak üzere Xamarin.Android uygulama gereksinimlerini açıklar dosya. **Özellikleri** klasörü de barındırıldığı [AssemblyInfo.cs](http://msdn.microsoft.com/en-us/library/microsoft.visualbasic.applicationservices.assemblyinfo(v=vs.110).aspx), bir .NET derlemesi meta veri dosyası. Bu dosya, uygulamanız hakkında bazı temel bilgileri doldurmak için iyi bir uygulamadır.

-   **Başvuruları** &ndash; oluşturmak ve uygulamayı çalıştırmak için gerekli olan derlemeleri içerir. Başvuruları dizin genişletirseniz, .NET derleme başvurularını gibi görürsünüz [sistem](http://msdn.microsoft.com/en-us/library/system%28v=vs.110%29.aspx), System.Core, ve [System.Xml](http://msdn.microsoft.com/en-us/library/system.xml%28v=vs.110%29.aspx), Xamarin'ın Mono.Android derlemesine başvuru yanı sıra.


-   **Varlıklar** &ndash; uygulama yazı tiplerini, yerel veri dosyaları ve metin dosyaları dahil olmak üzere çalıştırmak için gereken dosyaları içerir. Burada bulunan dosyalar oluşturulan erişilebilir `Assets` sınıfı. Xamarin Android varlıklar hakkında daha fazla bilgi için bkz: [kullanarak Android varlıklar](~/android/app-fundamentals/resources-in-android/android-assets.md) Kılavuzu.

-   **Kaynakları** &ndash; dizeleri, görüntüler ve düzenleri gibi uygulama kaynakları içerir. Bu kaynaklar kodda oluşturulan erişebilirsiniz `Resource` sınıfı. [Android kaynakları](~/android/app-fundamentals/resources-in-android/index.md) Kılavuzu hakkında daha fazla ayrıntı sağlar **kaynakları** dizin. Uygulama şablonu Ayrıca kaynaklara kısa bir kılavuz içerir **AboutResources.txt** dosya.

### <a name="resources"></a>Kaynaklar

**Kaynakları** dizini içeren dört klasörleri **drawable**, **düzeni**, **mipmap** ve **değerleri**, adında bir dosya yanı sıra **Resource.designer.cs**.

Öğeleri aşağıdaki tabloda özetlenmiştir:

-   **drawable** &ndash; drawable dizinleri ev [drawable kaynakları](http://developer.android.com/guide/topics/resources/drawable-resource.html) görüntüler ve bit eşlemler gibi.

-   **Mipmap** &ndash; mipmap dizin simgesi densities farklı Başlatıcısı drawable dosyalarını içerir. Uygulama simgesi dosyası drawable dizini varsayılan şablonu barındırıldığı **Icon.png**.


-   **Düzen** &ndash; Düzen dizinini içeren _Android designer dosyaları_ (.axml) her ekranı veya etkinliği için kullanıcı arabirimi tanımlayın. Adlı bir varsayılan düzen şablonu oluşturur **Main.axml**.

-   **değerleri** &ndash; bu dizin dize, tamsayı ve renkleri gibi basit değerlerini depolayan XML dosyalarını barındırır. Şablon adlı dize değerlerini depolamak için bir dosya oluşturur **Strings.xml**.

-   **Resource.Designer.cs** &ndash; olarak da bilinen `Resource` sınıfı, bu dosya olduğu için her bir kaynağın atanan benzersiz kimlikler tutan bir parçalı sınıf. Xamarin.Android araçları tarafından otomatik olarak oluşturulur ve gerektiği gibi yeniden oluşturulur. Xamarin.Android için yapılan tüm el ile yapılan değişikliklerin üzerine yazar olarak bu dosyayı el ile düzenlenmemelidir.


## <a name="app-fundamentals-and-architecture-basics"></a>Uygulama temelleri ve mimari temelleri

Android uygulamaları tek giriş noktası yoktur; diğer bir deyişle, hiçbir tek satırlık bir uygulamayı başlatmak için işletim sistemi çağırır uygulamasındaki kod yoktur. Bunun yerine, Android hangi sırada Android tüm uygulama işlemi belleğe sınıflarından biri başlattığında bir uygulamayı başlatır.

Android, bu benzersiz özellik son derece yararlı uygulamalar tasarlama karmaşık veya etkileşen Android işletim sistemine sahip olabilir. Ancak, bu seçenekler ayrıca Android gibi basit bir senaryoyla ilgilenirken karmaşık hale **Phoneword** uygulama. Bu nedenle, Keşif Android mimarisinin iki ayrılır. Bu kılavuz bir Android uygulaması için en yaygın giriş noktası kullanan bir uygulamayı dissects: ilk ekran. İçinde [Hello, Android Multiscreen](~/android/get-started/hello-android-multiscreen/index.md), uygulamayı başlatmak için farklı yollar açıklandığı gibi Android mimarisi tam karmaşıklığını incelediniz.


### <a name="phoneword-scenario---starting-with-an-activity"></a>Phoneword senaryo - bir etkinlikle başlatılıyor

Açtığınızda **Phoneword** uygulama ilk kez bir öykünücü veya cihaz işletim sistemi oluşturur ilk *etkinlik*. Tek bir uygulama ekranına karşılık gelen özel bir Android sınıfı bir etkinliktir ve çizim ve kullanıcı arabirimini destekleyen sorumludur. Android uygulamanın ilk etkinlik oluşturduğunda, tüm uygulama yükler:

[![Etkinlik yükleme](hello-android-deepdive-images/01-activity-load-sml.png)](hello-android-deepdive-images/01-activity-load.png#lightbox)

Hiçbir doğrusal progression (birkaç nokta uygulamasından başlatabilirsiniz) bir Android uygulaması olduğundan, Android uygulama ne sınıfları ve dosyaları sunun izlemek için benzersiz bir şekilde sahiptir. İçinde **Phoneword** örnek, uygulamayı oluşturan bölümleri adlı özel bir XML dosyası ile kaydedilen tüm **Android derleme bildirimi**. Rolü **Android derleme bildirimi** uygulama içeriği, özellikler ve izinleri izlemek ve Android işletim sistemine ifşa sağlamaktır. Düşünebilirsiniz **Phoneword** uygulama tek bir etkinlik (ekran) ve Android bildirim dosyası tarafından Aşağıdaki diyagramda gösterildiği gibi birbirine bağlı Yardımcısı ve kaynak dosyaları koleksiyonu olarak:

[![Kaynak Yardımcıları](hello-android-deepdive-images/02-resources-helpers-sml.png)](hello-android-deepdive-images/02-resources-helpers.png#lightbox)

Sonraki birkaç bölümleri, çeşitli kısımlarını ilişkileri keşfetmek **Phoneword** uygulama; bu sağlayacağını, daha iyi yukarıdaki diyagramı anlamak ile. Bu keşif Android Tasarımcısı ve düzeni dosyaları anlatılmaktadır kullanıcı arabirimi ile başlar.


## <a name="user-interface"></a>Kullanıcı Arabirimi

`Main.axml` kullanıcı arabirimi düzeni uygulamanın ilk ekranda dosyasıdır. .axml bu Android Tasarımcısı dosyasına gösterir (AXML anlamına gelir *Android XML*). Adı *ana* Android'ın açısından bakıldığında rastgeledir &ndash; düzeni dosyasını başka bir adlandırılmış. Açtığınızda **Main.axml** adlı Android düzeni dosyaları için görsel Düzenleyicisi yukarı getirir IDE içinde *Android Tasarımcısı*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android Tasarımcısı](hello-android-deepdive-images/vs/03-android-designer-sml.png "Android Tasarımcısı")](hello-android-deepdive-images/vs/03-android-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Android Tasarımcısı](hello-android-deepdive-images/xs/03-android-designer-sml.png)](hello-android-deepdive-images/xs/03-android-designer.png#lightbox)

-----

İçinde **Phoneword** uygulama, **TranslateButton**kullanıcının kimliği ayarlanmış `@+id/TranslateButton`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![TranslateButton kimliği ayarı](hello-android-deepdive-images/vs/04-translatebutton-sml.png "TranslateButton kimliği ayarı")](hello-android-deepdive-images/vs/04-translatebutton.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![TranslateButton kimliği ayarı](hello-android-deepdive-images/xs/04-translatebutton-sml.png)](hello-android-deepdive-images/xs/04-translatebutton.png#lightbox)

-----

Ayarladığınızda `id` özelliği **TranslateButton**, Android Tasarımcısı eşlemeleri **TranslateButton** denetimini `Resource` sınıfı ve atar bir *kaynak Kimliği* , `TranslateButton`. Bu eşleme sınıfına visual denetiminin bulun ve kullanmanız mümkün kılar **TranslateButton** ve diğer denetimlerin uygulama kodu. Denetimleri'nın temelini oluşturan kodu parçalayın olduğunda bu daha ayrıntılı olarak ele alınacaktır. Tüm bilmeniz gereken şu an için olan bir denetim kodu gösterimini Tasarımcısı'nda denetiminin görsel gösterimi bağlantılıdır `id` özelliği.


### <a name="source-view"></a>Kaynak Görünümü

Tasarım yüzeyine tanımlanan her şeyi kullanmak Xamarin.Android için XML veri dönüştürülür. Android Tasarımcısı visual Tasarımcısı'ndan oluşturulan XML içeren bir kaynak görünüm sağlar. Bu XML geçerek görüntüleyebileceğiniz **kaynak** aşağıdaki ekran görüntüsüne gösterildiği gibi sol alt tasarımcı görünümü içinde panel:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tasarımcı kaynağı görünümü](hello-android-deepdive-images/vs/05-source-view-sml.png "Tasarımcısı kaynağı görünümü")](hello-android-deepdive-images/vs/05-source-view.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tasarımcı kaynağı görünümü](hello-android-deepdive-images/xs/05-source-view-sml.png)](hello-android-deepdive-images/xs/05-source-view.png#lightbox)

-----

Bu XML kaynak kodunu içermelidir **metin (büyük)**, **düz metin**ve iki **düğmesini** öğeleri. Xamarin Android Android Tasarımcısı hakkında daha ayrıntılı gezinmek için bkz [Tasarımcısı genel bakış](~/android/user-interface/android-designer/index.md) Kılavuzu.

Kullanıcı arabirimini visual parçası kavramları ve araçları şimdi ele alınmış. Ardından, etkinlikleri ve etkinlik yaşam döngüsü incelediniz gibi kullanıcı arabirimini'nın temelini oluşturan koda atlama zamanı geldi.


## <a name="activities-and-the-activity-lifecycle"></a>Etkinlikleri ve etkinlik yaşam döngüsü

`Activity` Sınıfı, kullanıcı arabirimini destekler kodunu içerir.
Etkinlik için kullanıcı etkileşimi yanıt ve dinamik kullanıcı deneyimi oluşturma sorumludur.
Bu bölüm tanıtır `Activity` sınıfı, etkinlik yaşam döngüsü açıklanır ve kullanıcı arabiriminde'nın temelini oluşturan kodu dissects **Phoneword** uygulama.


### <a name="activity-class"></a>Etkinlik sınıfı

**Phoneword** uygulama yalnızca bir ekran (etkinlik) sahiptir. Ekran'ın temelini oluşturan sınıfı adlı `MainActivity` ve yaşadığı **MainActivity.cs** dosya. Adı `MainActivity` Android özel bir önemi olan &ndash; kuralı uygulamanın ilk etkinliğin adını olsa `MainActivity`, Android değil dikkat edin, başka bir şey olarak adlandırılmışsa.

Açtığınızda **MainActivity.cs**, görebilirsiniz `MainActivity` sınıfı bir *alt* , `Activity` sınıf ve etkinlik'ın ile donatılan [etkinlik](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) özniteliği:

```csharp
[Activity (Label = "Phone Word", MainLauncher = true)]
public class MainActivity : Activity
{
  ...
}
```

`Activity` Özniteliği Android derleme bildirimi ile etkinliğini kaydeder; Bu, bu sınıfın bir parçası olduğunu biliyor Android sağlar **Phoneword** bu bildirimi tarafından yönetilen uygulama. `Label` Özelliği ayarlar ekranın üstünde görüntülenecek metin.

`MainLauncher` Özelliği, uygulama başlatıldığında bu etkinliği görüntülemek için Android söyler. Bu özellik daha fazla etkinlikleri (ekranları) içinde anlatıldığı gibi uygulama eklemek gibi önemli hale [Hello, Android Multiscreen](~/android/get-started/hello-android-multiscreen/index.md) Kılavuzu.

Artık temel bilgileri `MainActivity` silinmiş kapsamında, etkinlik koda daha derin Dalış için sunarak zamanı _etkinlik yaşam döngüsü_.

### <a name="activity-lifecycle"></a>Etkinlik yaşam döngüsü

Android, etkinlikleri yaşam döngüsü kullanıcı ile etkileşimlerini bağlı olarak farklı aşamaları gidin. Başlatılan ve duraklatılmış, sürdürüldü ve yok etme ve vb. etkinlikleri oluşturulabilir. `Activity` Sınıfı ekranının yaşam döngüsü belirli noktalarında sistem çağırır yöntemlerini içerir. Aşağıdaki diyagram tipik bir aktivite ömrünü ve aynı zamanda ilgili yaşam döngüsü yöntemleri gösterir:

[![Etkinlik yaşam döngüsü](hello-android-deepdive-images/04-lifecycle-sml.png)](hello-android-deepdive-images/04-lifecycle.png#lightbox)

Geçersiz kılma tarafından `Activity` yaşam döngüsü yöntemleri nasıl etkinlik, kullanıcıya nasıl tepki verdiğini yükler denetlemek ve hatta aygıt ekranından kaybolur sonra ne olur. Örneğin, bazı önemli görevleri gerçekleştirmek için yukarıdaki diyagramda yaşam döngüsü yöntemleri geçersiz kılabilirsiniz:

-   **OnCreate** &ndash; görünümler oluşturur, değişkenleri başlatır ve kullanıcı etkinliğini görür önce yapılması gereken diğer hazırlığı çalışma gerçekleştirir. Yalnızca etkinliğin belleğe zaman yüklendikten sonra bu yöntem çağrılır. 

-   **OnResume** &ndash; etkinlik her cihaz ekranı çıktığında, gerçekleştirilmesi gerekir görevleri gerçekleştirir. 

-   **OnPause** &ndash; etkinlik cihaz ekranı ayrıldığında her zaman gerçekleştirilmesi gerekir görevleri gerçekleştirir.


Özel kod yaşam döngüsü yönteminde eklediğinizde `Activity`, size *geçersiz kılma* bu yaşam döngüsü yöntemin *temel uygulamayı*. (Zaten kullanıma biraz kod olan) mevcut yaşam döngüsü yöntemiyle dokunun ve bu yöntem ile kendi kodunuzu genişletir. Özgün kod yeni kodunuz önce çalıştığından emin olmak için yönteminize içinde temel uygulamasından çağırın. Buna örnek olarak bir sonraki bölümde gösterilmiştir. 

Etkinlik yaşam döngüsü Android önemli ve karmaşık parçasıdır. Bitirdikten sonra etkinlikleri hakkında daha fazla bilgi edinmek istiyorsanız _Başlarken_ serisi, okuma [etkinlik yaşam döngüsü](~/android/app-fundamentals/activity-lifecycle/index.md) Kılavuzu. Bu kılavuzda, sonraki odak etkinlik yaşam döngüsünün ilk aşamadır `OnCreate`.


### <a name="oncreate"></a>OnCreate

Android çağrıları `Activity`'s `OnCreate` (ekran kullanıcıya sunulan önce) etkinlik oluşturduğunda yöntemi. Geçersiz kılabilirsiniz `OnCreate` yaşam döngüsü yöntemi görünümleri oluşturma ve kullanıcı karşılamak üzere etkinliklerinizi hazırlamak için:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);
    // Additional setup code will go here
}
```

İçinde **Phoneword** uygulama, yapılacağı ilk şey `OnCreate` Android Tasarımcısı'nda oluşturulan kullanıcı arabirimi yük değil. Kullanıcı arabirimini yüklemek için arama `SetContentView` ve bu geçirin *kaynak Düzen adı* Düzen dosyası için: `Main.axml`. Düzen konumundadır `Resource.Layout.Main`:

```csharp
SetContentView (Resource.Layout.Main);
```

Zaman `MainActivity` başlatır yukarı içeriğini temel alarak bir görünüm oluşturur **Main.axml** dosya. Etkinlik adı düzeni dosya adı eşleştirildiği Not &ndash; *ana*.axml olan düzenini *ana*etkinlik. Bu Android'ın açısından bakıldığında gerekli değildir, ancak daha fazla ekranlar uygulama eklemek başladığınızda, bu adlandırma kuralını düzeni dosyasını kod dosyasına eşleşecek şekilde kolaylaştırır olduğunu bulabilirsiniz.

Düzen dosyasını hazırladıktan sonra denetimleri arayan başlatabilirsiniz.
Bir kontrolü aramak için arama `FindViewById` ve denetiminin kaynak Kimliğini geçirin:

```csharp
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
```

Düzen dosyasında başvuruları denetimlerine sahip olduğunuza göre bunları kullanıcı etkileşimi yanıt programlama başlatabilirsiniz.


### <a name="responding-to-user-interaction"></a>Kullanıcı etkileşimine yanıt verme

Android içinde `Click` olayı için kullanıcının dokunma dinler. Bu uygulamadaki `Click` olay ile bir lambda gerçekleştirilir, ancak bunun yerine bir temsilci veya bir adlandırılmış olay işleyicisi kullanılabilir. En son **TranslateButton** kod benzeyen aşağıdaki: 

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

## <a name="testing-deployment-and-finishing-touches"></a>Test, dağıtım ve son rötuşları

Mac için Visual Studio ve Visual Studio Test ve bir uygulamayı dağıtmak için birçok seçenek sağlar. Bu bölümde, hata ayıklama seçenekleri kapsamaktadır, bir cihazda test uygulamalar gösterir ve farklı ekran densities için özel uygulama simgeleri oluşturmak için araçlar sunar.


### <a name="debugging-tools"></a>Hata Ayıklama Araçları

Uygulama kodundaki sorunları tanılamak zor olabilir. Karmaşık kod sorunları tanılamak için [bir kesme noktası belirleyerek](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [adım aracılığıyla kodu](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), veya [Günlük penceresine çıkış bilgileri](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).


### <a name="deploy-to-a-device"></a>Bir aygıta dağıtmak

Öykünücü dağıtmak ve bir uygulamayı test etmek için iyi bir başlangıç olmakla birlikte, kullanıcıların bir öykünücü son uygulamada tüketir değil. Erken ve genellikle gerçek bir cihazdaki uygulamaları test etmek için iyi bir uygulamadır.

Bir Android cihaz uygulamaları test etmek için kullanılabilmesi için geliştirme yapılandırılması gerekiyor. [Ayarlamak yukarı geliştirme için cihazı](~/android/get-started/installation/set-up-device-for-development.md) Kılavuzu, bir cihaz geliştirme için hazır hale kapsamlı yönergeler sağlar.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Cihaz yapılandırıldıktan sonra kendisine ondan seçerek, takarak dağıtabilirsiniz **aygıtı Seç** iletişim ve uygulama başlatma:

![Select hata ayıklama aygıt](hello-android-deepdive-images/vs/06-select-device.png "Select hata ayıklama cihaz")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Cihaz yapılandırıldıktan sonra kendisine tuşlarına basarak, takarak dağıtabilirsiniz **başlangıç (Çalıştır)**, ondan seçme **aygıtı Seç** iletişim ve tuşlarına basarak **Tamam**:

[![Select hata ayıklama cihaz](hello-android-deepdive-images/xs/06-select-device-sml.png)](hello-android-deepdive-images/xs/06-select-device.png#lightbox)

-----

Bu aygıttaki uygulama başlatır:

[![Phoneword girin](hello-android-deepdive-images/05-enter-phoneword-sml.png)](hello-android-deepdive-images/05-enter-phoneword.png#lightbox)


### <a name="set-icons-for-different-screen-densities"></a>Farklı ekran Densities için simgeler ayarlama

Android aygıtlar farklı ekran boyutlarına ve çözümlemeleri gelir ve tüm görüntüleri tüm ekranlarda iyi bakın. Örneğin, bir ekran görüntüsünü yüksek yoğunluklu bir Nexus 5 düşük yoğunluklu simgesinde aşağıdadır. Nasıl bulanık onu çevresindeki simgeleri karşılaştırılır dikkat edin:

[![Bulanık simgesi](hello-android-deepdive-images/06-blurry-icon-sml.png)](hello-android-deepdive-images/06-blurry-icon.png#lightbox)

Bu hesap için farklı çözümler simgeleri eklemek için iyi bir uygulamadır **kaynakları** klasör. Android sağlar farklı sürümlerini **mipmap** farklı densities Başlatıcısı simgeleri işlemek için klasör *mdpi* Orta, *hdpi* yüksek için ve  *xhdpi*, *xxhdpi*, *xxxhdpi* çok yüksek yoğunluk ekranlar için. Farklı boyutlarda simgeler uygun depolanır **mipmap -** klasörler:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Mipmap klasörlerinde](hello-android-deepdive-images/vs/07-mipmap-folders.png "mipmap klasörlerinde")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Mipmap klasörlerinde](hello-android-deepdive-images/xs/07-mipmap-folders-sml.png)](hello-android-deepdive-images/xs/07-mipmap-folders.png#lightbox)

-----

Android uygun yoğunluğu simgesiyle seçer:

[![Uygun yoğunluğu simgeleri](hello-android-deepdive-images/07-appropriate-density-sml.png)](hello-android-deepdive-images/07-appropriate-density.png#lightbox)

### <a name="generate-custom-icons"></a>Özel simge oluşturma

Herkes özel simge oluşturmak ve uygulama göze gerekiyor görüntüleri başlatmak için kullanılabilecek bir tasarımcı sahiptir. Özel uygulama resmi oluşturmak için birkaç alternatif yaklaşımlar şunlardır:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   [Android varlık Studio](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; Android simgeler, diğer yararlı topluluk araçları için bağlantılar ile birlikte tüm türleri için web tabanlı, tarayıcı içi bir üreteci. Ayrıca Google Chrome en iyi şekilde çalışır.

-   Visual Studio &ndash; uygulamanızda doğrudan IDE için ayarlama basit bir simge oluşturmak için bunu kullanabilirsiniz.

-   [Glyphish](http://www.glyphish.com/) &ndash; yüksek kaliteli önceden oluşturulmuş simgesi ayarlar ücretsiz indirme ve satın alma.

-   [Fiverr](http://www.fiverr.com/) &ndash; , 5'Başlangıç için ayarlanmış bir simge oluşturulmaya tasarımcıları çeşitli seçin. Miss hit veya olabilir ancak simgeler gerekiyorsa iyi bir kaynak üzerinde kolay bir şekilde tasarlanmış.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   [Android varlık Studio](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; Android simgeler, diğer yararlı topluluk araçları için bağlantılar ile birlikte tüm türleri için web tabanlı, tarayıcı içi bir üreteci. Ayrıca Google Chrome en iyi şekilde çalışır.

-   [Taslak 3](https://itunes.apple.com/us/app/sketch/id852320343?mt=12) &ndash; taslak olan kullanıcı arabirimleri, simgeler ve daha fazlasını tasarlamak için bir Mac uygulaması. Bu, Xamarin uygulama simgeleri ve başlatma görüntüleri kümesi tasarlamak için kullanılan uygulamadır. Taslak 3 hakkında $80 maliyetleri ve App Store'da kullanılabilir. Ücretsiz deneyebilirsiniz [taslak aracı](http://bohemiancoding.com/sketch/tool/) de.

-   [Pixelmator](http://www.pixelmator.com/) &ndash; uygulama hakkında $30 maliyetleri Mac için düzenleme çok yönlü bir görüntü.

-   [Glyphish](http://www.glyphish.com/) &ndash; yüksek kaliteli önceden oluşturulmuş simgesi ayarlar ücretsiz indirme ve satın alma.

-   [Fiverr](http://www.fiverr.com/) &ndash; , 5'Başlangıç için ayarlanmış bir simge oluşturulmaya tasarımcıları çeşitli seçin. Miss hit veya olabilir ancak simgeler gerekiyorsa iyi bir kaynak üzerinde kolay bir şekilde tasarlanmış.

-----

Simge boyutu ve gereksinimleri hakkında daha fazla bilgi için başvurmak [Android kaynakları](~/android/app-fundamentals/resources-in-android/index.md) Kılavuzu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

### <a name="adding-google-play-services-packages"></a>Hizmet paketleri Google Play ekleme

_Google Play Hizmetleri_ Google sayfası Google haritalar, Google Cloud Messaging ve uygulama içi faturalama gibi en son özellikleri avantajlarından yararlanmak Android geliştiricileri izin veren bir dizi eklenti kitaplıktır.
Tüm Google Play Hizmetleri'nin kitaplıklarına bağlamaları tek bir paket biçiminde Xamarin tarafından daha önce sağlanan &ndash; Mac için Visual Studio ile başlayarak, yeni proje iletişim kutusu, dahil etmek için Google Play Hizmetleri'nin paketleri seçmek için kullanılabilir Uygulamanızı.

Bir veya daha fazla Google Play Hizmeti'ni kitaplıkları eklemek için sağ tıklatın **paketleri** tıklayın ve proje ağacı düğümünde **Google Play Hizmeti'ni Ekle...** :

[![Add Google Play Service](hello-android-deepdive-images/xs/08-add-google-play-services-sml.png)](hello-android-deepdive-images/xs/08-add-google-play-services.png#lightbox)

Zaman **Google Play Hizmetleri Ekle** iletişim sunulur, projenize eklemek istediğiniz paketleri (nugets) seçin:

[![Paketleri seçin](hello-android-deepdive-images/xs/09-add-dialog-sml.png)](hello-android-deepdive-images/xs/09-add-dialog.png#lightbox)

Ne zaman bir hizmeti seçin ve tıklatın **Paketi Ekle**, Mac için Visual Studio indirir ve bunu gerektiren tüm bağımlı Google Play Hizmetleri'nin paketler yanı sıra seçtiğiniz paketi yükler. Bazı durumlarda, görebileceğiniz bir **lisans kabulünü** tıklatın gerektirir iletişim **kabul** paketler yüklenmeden önce:

[![Lisans Kabulünü](hello-android-deepdive-images/xs/10-license-acceptance-sml.png)](hello-android-deepdive-images/xs/10-license-acceptance.png#lightbox)

-----

## <a name="summary"></a>Özet

Tebrikler! Şimdi oluşturmak için gereken araçları yanı sıra, bir Xamarin.Android uygulaması bileşenlerinin düz bir anlayış olmalıdır.

Sonraki öğreticide _Başlarken_ serisi, daha gelişmiş Android mimarisi ve kavramları keşfetmenizde birden çok ekran işlemek için uygulamanızın genişletmek.
