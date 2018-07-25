---
title: 'Hello, Android: Derinlemesine bakış'
description: Bu iki bölümden Kılavuzu'nda, ilk Xamarin.Android uygulamanızı oluşturmak ve Xamarin ile Android uygulama geliştirme temelleri bir anlayış geliştirmek. Bu doğrultuda, Araçlar, kavramlar ve bir Xamarin.Android uygulaması derleme ve dağıtma için gerekli adımları sunulacaktır.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: EF0E110B-20EA-43F6-9476-1A0F41AFD298
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 3aa70469c5916a22a22d7857c62a4b46c1637124
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242426"
---
# <a name="hello-android-deep-dive"></a>Hello, Android: Derinlemesine bakış

_Bu iki bölümden Kılavuzu'nda, ilk Xamarin.Android uygulamanızı oluşturmak ve Xamarin ile Android uygulama geliştirme temelleri bir anlayış geliştirmek. Bu doğrultuda, Araçlar, kavramlar ve bir Xamarin.Android uygulaması derleme ve dağıtma için gerekli adımları sunulacaktır._

## <a name="hello-android-deep-dive"></a>Hello, Android derinlemesine bakış

İçinde [Hello, Android Hızlı Başlangıç](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md), oluşturulan ve ilk Xamarin.Android uygulamanız çalıştı. Artık daha karmaşık programlar oluşturabilmeleri nasıl Android uygulamaları iş daha derin bir anlayış geliştirmek için zamanı geldi. Bu kılavuz, böylece ne yaptığını anlamanıza ve Android uygulaması geliştirme temel bir anlayış geliştirmek başlamak Hello, Android izlenecek gerçekleştirdiğiniz adımları gözden geçirir.

Bu kılavuzda aşağıdaki konular dokunma:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Visual Studio'ya giriş** &ndash; Visual Studio ve yeni bir Xamarin.Android uygulaması oluşturmaya giriş.

-   **Bir Xamarin.Android uygulaması anatomisi** -Xamarin.Android uygulamasının temel parçalarından turu.

-   **Uygulama temelleri ve mimarisi temellerini** &ndash; etkinlikleri, Android bildirim ve Android geliştirme genel örneğinizin giriş.

-   **Kullanıcı Arabirimi (UI)** &ndash; kullanıcı arabirimleri ile Android Designer oluşturma.

-   **Etkinlikleri ve etkinlik yaşam döngüsü** &ndash; etkinlik yaşam döngüsü ve kablolama kodda kullanıcı arabiriminin bir giriş.

-   **Test, dağıtım ve Son dokunuşları** &ndash; test, dağıtım, oluşturma resmi ve daha fazla öneri uygulamanızla tamamlayın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   **Mac için Visual Studio giriş** &ndash; Xamarin Studio ve yeni bir Xamarin.Android uygulaması oluşturmaya giriş.

-   **Bir Xamarin.Android uygulaması anatomisi** &ndash; Xamarin.Android uygulamasının temel parçalarından turu.

-   **Uygulama temelleri ve mimarisi temellerini** &ndash; etkinlikleri, Android bildirim ve Android geliştirme genel örneğinizin giriş.

-   **Kullanıcı Arabirimi (UI)** &ndash; kullanıcı arabirimleri ile Android Designer oluşturma.

-   **Etkinlikleri ve etkinlik yaşam döngüsü** &ndash; etkinlik yaşam döngüsü ve kablolama kodda kullanıcı arabiriminin bir giriş.

-   **Test, dağıtım ve Son dokunuşları** &ndash; test, dağıtım, oluşturma resmi ve daha fazla öneri uygulamanızla tamamlayın.

-----


Bu kılavuz, bir tek ekranlı Android uygulaması oluşturmak için gereken bilgi ve becerilerinizi geliştirmenize yardımcı olur. Üzerinden çalıştıktan sonra farklı bölümlerini bir Xamarin.Android uygulaması ve bunların birlikte nasıl getireceğinizi anlamanız gerekir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Visual Studio'ya giriş

Visual Studio, Microsoft güçlü bir ıde'dir. Bu, tamamen tümleşik bir görsel tasarımcı, yeniden düzenleme araçları, bir bütünleştirilmiş kod tarayıcı, kaynak kodu tümleştirmesi ve daha fazlasını içeren bir metin düzenleyicisi sunar. Bu kılavuzda eklenti Xamarin ile bazı temel Visual Studio özellikleri kullanmayı öğreneceksiniz.

Visual Studio kod halinde düzenler _çözümleri_ ve _projeleri_. Bir çözüm bir veya daha fazla proje tutan bir kapsayıcıdır. Bir proje, bir uygulama (iOS veya Android olduğu gibi), destek kitaplığı, bir test uygulaması ve daha fazla olabilir. İçinde **Phoneword** uygulama, eklediğiniz yeni bir Android projesi kullanarak **Android uygulaması** şablona **Phoneword** oluşturulmuş çözümünü [Merhaba, Android](~/android/get-started/hello-android/hello-android-quickstart.md) Kılavuzu. 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Mac için Visual Studio'ya giriş

Mac için Visual Studio bir ücretsiz, açık kaynaklı Visual Studio'ya benzer bir ıde'dir. Bu tam olarak tümleşik bir görsel tasarımcı, yeniden düzenleme araçları ile tam bir metin düzenleyicisi, bir bütünleştirilmiş kod tarayıcı, kaynak kodu tümleştirmesi ve diğer özellikleri. Bu kılavuzda, bazı temel Visual Studio Mac özelliklerini kullanmayı öğreneceksiniz. Mac için Visual Studio yeniyseniz, daha ayrıntılı denetlemek isteyebilirsiniz [Mac için Visual Studio giriş](https://docs.microsoft.com/visualstudio/mac/).

Mac için Visual Studio aşağıdaki koda düzenleme Visual Studio uygulaması _çözümleri_ ve _projeleri_. Bir çözüm bir veya daha fazla proje tutan bir kapsayıcıdır. Bir proje, bir uygulama (iOS veya Android olduğu gibi), destek kitaplığı, bir test uygulaması ve daha fazla olabilir. İçinde **Phoneword** uygulama, eklediğiniz yeni bir Android projesi kullanarak **Android uygulaması** şablona **Phoneword** oluşturulmuş çözümünü [Merhaba, Android](~/android/get-started/hello-android/hello-android-quickstart.md) Kılavuzu.

-----

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinandroid-application"></a>Bir Xamarin.Android uygulaması anatomisi

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aşağıdaki ekran görüntüsünde, çözümün içeriğini listeler. Bu dizin yapısını ve çözümle ilişkili dosyaların tümünü içeren Çözüm Gezgini.

[![Çözüm Gezgini](hello-android-deepdive-images/vs/02-solution-structure-sml.png)](hello-android-deepdive-images/vs/02-solution-structure.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Aşağıdaki ekran görüntüsünde, çözümün içeriğini listeler. Bu dizin yapısını ve çözümle ilişkili dosyaların tümünü içeren çözüm bölmesi.

[![Çözüm bölmesi](hello-android-deepdive-images/xs/02-solution-structure-sml.png)](hello-android-deepdive-images/xs/02-solution-structure.png#lightbox)

-----

Bir çözüm olarak **Phoneword** oluşturuldu ve Android proje **Phoneword** bunun içinde yerleştirilir.

İçindeki her klasör ve amacını görmek için proje öğeleri bakın:

-   **Özellikleri** &ndash; içerir [AndroidManifest.xml](~/android/platform/android-manifest.md) tüm gereksinimleri için ad, sürüm numarasını ve izinleri de dahil olmak üzere, bir Xamarin.Android uygulaması tanımlayan dosyası. **Özellikleri** klasör de barındırıldığı [AssemblyInfo.cs](xref:Microsoft.VisualBasic.ApplicationServices.AssemblyInfo), bir .NET bütünleştirilmiş kod meta veri dosyası. Bu dosya uygulama ile ilgili bazı temel bilgileri doldurmak için iyi bir uygulamadır.

-   **Başvuruları** &ndash; derlemek ve uygulamayı çalıştırmak için gerekli derlemelerini içerir. Başvuruları dizini genişletirseniz, .NET derlemesine ilişkin başvurular gibi görürsünüz [sistem](xref:System), System.Core, ve [System.Xml](xref:System.Xml), Xamarin'in Mono.Android derlemesine bir başvuru yanı sıra.


-   **Varlıklar** &ndash; yazı tipleri, yerel veri dosyaları ve metin dosyaları gibi çalışması için uygulaması gereken dosyaları içerir. Buraya eklenen dosyaları oluşturulan erişilebilir `Assets` sınıfı. Xamarin Android varlıklar hakkında daha fazla bilgi için bkz. [kullanarak Android varlıklarını](~/android/app-fundamentals/resources-in-android/android-assets.md) Kılavuzu.

-   **Kaynakları** &ndash; dizeleri, resimler ve düzenleri gibi uygulama kaynakları içerir. Oluşturulan kod bu kaynaklara erişebilir `Resource` sınıfı. [Android kaynakları](~/android/app-fundamentals/resources-in-android/index.md) Kılavuzu hakkında daha fazla ayrıntı sağlar **kaynakları** dizin. Uygulama şablonu, ayrıca kaynaklara kısa bir kılavuz içerir **AboutResources.txt** dosya.

### <a name="resources"></a>Kaynaklar

**Kaynakları** dizinini içeren dört klasörleri **drawable**, **Düzen**, **mipmap** ve **değerleri**, adlı bir dosya yanı sıra **Resource.designer.cs**.

Öğeler, aşağıdaki tabloda özetlenmiştir:

-   **drawable** &ndash; drawable dizinleri evi [drawable kaynakları](http://developer.android.com/guide/topics/resources/drawable-resource.html) görüntüler ve bit eşlemler gibi.

-   **Mipmap** &ndash; mipmap dizini drawable dosyaları için farklı bir Başlatıcısı simgesini densities tutar. Varsayılan şablonda uygulama simge dosyasını drawable dizini barındırır **Icon.png**.


-   **Düzen** &ndash; Düzen dizini içeren _Android designer dosyaları_ (.axml) her ekranı veya etkinliği için kullanıcı arabirimini tanımlar. Şablon, varsayılan olarak adlandırılan bir düzeni oluşturur. **Main.axml**.

-   **değerleri** &ndash; bu dizin, dizeler ve tamsayılar renkleri gibi basit değerler depolayan XML dosyalarını barındırır. Şablon olarak adlandırılan dize değerleri depolamak için bir dosya oluşturur **Strings.xml**.

-   **Resource.Designer.cs** &ndash; olarak da bilinen `Resource` sınıfı, bu dosya olduğu her kaynağa atanmış benzersiz kimlikler tutan bir parçalı sınıf. Xamarin.Android araçları tarafından otomatik olarak oluşturulur ve gerektiği gibi yeniden oluşturulur. Xamarin.Android için el ile yapılan değişiklikleri üzerine yazılacağından bu dosyayı el ile düzenlenmemelidir.


## <a name="app-fundamentals-and-architecture-basics"></a>Uygulama temelleri ve mimarisinin temelleri

Android uygulamaları tek giriş noktası yoktur; diğer bir deyişle, hiçbir tek satırlık bir kod uygulamayı başlatmak için işletim sistemini çağıran uygulamada yoktur. Android hangi sırada Android tüm uygulama işlemi belleğine yükler, kendi sınıfları başlattığında, bunun yerine, bir uygulamayı başlatır.

Android benzersiz bu özellik, son derece yararlı uygulamalar tasarlama karmaşık veya Android işletim sistemi ile etkileşim kuran olabilir. Ancak, bu seçenekler ayrıca Android gibi basit bir senaryoyla ilgilenirken karmaşık hale **Phoneword** uygulama. Bu nedenle, Keşif Android mimarisinin iki ayrılır. Bu kılavuzda bir Android uygulaması için en yaygın giriş noktası kullanan bir uygulamayı dissects: ilk ekran. İçinde [Hello, Android çoklu ekranı](~/android/get-started/hello-android-multiscreen/index.md), uygulamayı başlatmak için farklı bir şekilde açıklandığı gibi Android mimari tam karmaşıklığını incelenmektedir.


### <a name="phoneword-scenario---starting-with-an-activity"></a>Phoneword senaryo - bir etkinlikle başlatılıyor

Açtığınızda **Phoneword** uygulama ilk kez bir öykünücü veya cihaz, işletim sistemi ilk oluşturur *etkinlik*. Tek bir uygulama ekrana karşılık gelen özel bir Android sınıf bir etkinliktir ve çizim ve kullanıcı arabirimini destekleyen sorumludur. Android uygulamanın ilk etkinliği oluşturduğunda, uygulamanın tamamını yükler:

[![Etkinlik yük](hello-android-deepdive-images/01-activity-load-sml.png)](hello-android-deepdive-images/01-activity-load.png#lightbox)

Bir Android uygulaması (birkaç nokta uygulamadan başlatabilir) aracılığıyla hiçbir doğrusal ilerlemeyi olduğundan, Android uygulama sınıf ve dosyaları ne yapmak izlemek için benzersiz bir şekilde sahiptir. İçinde **Phoneword** örnek, uygulamayı oluşturan parçaları olarak adlandırılan özel bir XML dosyasıyla kaydedilen tüm **Android bildirim**. Rolü **Android bildirim** uygulamanın içeriği, özellikler ve izinleri izlemenize ve Android işletim sistemi için bunları ifşa etmek. Düşünebilirsiniz **Phoneword** uygulamayı tek bir etkinlik (ekran) ve birlikte Android bildirim dosyasında, aşağıdaki diyagramda gösterildiği gibi bağlı kaynak ve yardımcı dosyaları koleksiyonu olarak:

[![Kaynak Yardımcıları](hello-android-deepdive-images/02-resources-helpers-sml.png)](hello-android-deepdive-images/02-resources-helpers.png#lightbox)

Sonraki birkaç bölümler çeşitli bölümlerini ilişkileri keşfedin **Phoneword** uygulama; bu sağlamalıdır, daha iyi bir Yukarıdaki diyagramda'nın anlayış ile. Bu araştırma kullanıcı arabirimi ile başlar Android designer ve Düzen dosyalarını açıklar.


## <a name="user-interface"></a>Kullanıcı Arabirimi

`Main.axml` uygulamadaki ilk ekran için kullanıcı arabirimi Düzen dosyası olan. Bu Android bir tasarımcı dosyası olduğunu .axml gösterir (AXML anlamına gelen *Android XML*). Adı *ana* Android açısından bakıldığında rastgeledir &ndash; Düzen dosyası başka bir adlandırılmış. Açtığınızda **Main.axml** IDE'de, bunu visual düzenleyici olarak adlandırılan Android Düzen dosyalarını için getirir *Android Designer*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android Designer](hello-android-deepdive-images/vs/03-android-designer-sml.png "Android Tasarımcısı")](hello-android-deepdive-images/vs/03-android-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Android Designer](hello-android-deepdive-images/xs/03-android-designer-sml.png)](hello-android-deepdive-images/xs/03-android-designer.png#lightbox)

-----

İçinde **Phoneword** uygulamayı **TranslateButton**kişinin kimliği ayarlanır `@+id/TranslateButton`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![TranslateButton kimliği ayarı](hello-android-deepdive-images/vs/04-translatebutton-sml.png "TranslateButton kimliği ayarı")](hello-android-deepdive-images/vs/04-translatebutton.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![TranslateButton kimliği ayarı](hello-android-deepdive-images/xs/04-translatebutton-sml.png)](hello-android-deepdive-images/xs/04-translatebutton.png#lightbox)

-----

Ayarladığınızda `id` özelliği **TranslateButton**, Android Designer eşler **TranslateButton** denetimini `Resource` sınıfı ve atar bir *kaynak Kimliği* , `TranslateButton`. Bu eşleme visual denetiminin sınıfı bulun ve kullanmak mümkün kılar **TranslateButton** ve diğer denetimleri uygulama kodunda. Denetimleri güç katan kod parçalayın olduğunda bu daha ayrıntılı olarak ele alınacaktır. Bilmeniz gereken her şeyi şu an için olan kod gösterimini bir denetimin görsel temsilini tasarımcıda bir denetime bağlı olduğundan emin `id` özelliği.


### <a name="source-view"></a>Kaynak Görünümü

Tasarım yüzeyinde tanımlanan her şey, XML için kullanılacak bir Xamarin.Android uygulamasına çevrilir. Android Designer görsel tasarımcıdan üretilen XML içeren kaynak görünümü sağlar. Bu XML geçerek görüntüleyebileceğiniz **kaynak** ekran aşağıda gösterildiği gibi sol alt tasarımcı görünümü içinde panel:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tasarımcı kaynağı görünümü](hello-android-deepdive-images/vs/05-source-view-sml.png "kaynağı görünümü Tasarımcısı")](hello-android-deepdive-images/vs/05-source-view.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tasarımcı kaynak görünümü](hello-android-deepdive-images/xs/05-source-view-sml.png)](hello-android-deepdive-images/xs/05-source-view.png#lightbox)

-----

Bu XML kaynak kodunu içermesi gereken **metin (büyük)**, **düz metin**, ve iki **düğmesi** öğeleri. Android Designer'ın daha ayrıntılı bir tur başvurmak için Xamarin Android [Tasarımcısı genel bakış](~/android/user-interface/android-designer/index.md) Kılavuzu.

Görsel bölümü kullanıcı arabiriminin kavramları ve araçları artık ele alınmış. Ardından, etkinlikleri ve etkinlik yaşam döngüsü keşfedilmemiş olan kullanıcı arabirimi güç veren koda atlama zamanı gelmiş demektir.


## <a name="activities-and-the-activity-lifecycle"></a>Etkinlikleri ve etkinlik yaşam döngüsü

`Activity` Sınıf kullanıcı arabirimi güç katan kodunu içerir.
Etkinlik kullanıcı etkileşimine yanıt verme ve dinamik kullanıcı deneyimi oluşturmaya sorumludur.
Bu bölüm tanıtır `Activity` sınıfı, etkinlik yaşam döngüsü ele alır ve kullanıcı arabiriminde güç katan kod dissects **Phoneword** uygulama.


### <a name="activity-class"></a>Etkinlik sınıfı

**Phoneword** uygulama yalnızca bir ekran (etkinlik) sahiptir. Ekran güç katan bir sınıfa `MainActivity` ve kendini **MainActivity.cs** dosya. Adı `MainActivity` Android özel bir önemi yoktur &ndash; kuralı bir uygulamada ilk etkinliği ad olmasına rağmen `MainActivity`, Android değil dikkatli olun, başka bir şey ise.

Açtığınızda **MainActivity.cs**, gördüğünüz gibi `MainActivity` sınıfı bir *alt* , `Activity` sınıf ve etkinlik ile donatılmış [etkinliği](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) özniteliği:

```csharp
[Activity (Label = "Phone Word", MainLauncher = true)]
public class MainActivity : Activity
{
  ...
}
```

`Activity` Özniteliği, Android bildirim ile etkinliğini kaydeder; bu Android Bu sınıf bir parçası olduğunu bilmesini sağlar **Phoneword** bu bildirimi tarafından yönetilen bir uygulama. `Label` Özelliğini ayarlar ekranının üstünde görüntülenecek metin.

`MainLauncher` Özelliği, uygulama başlatıldığında, bu etkinliği görüntülemek için Android söyler. Bu özellik daha fazla etkinlik (ekranları) açıklandığı gibi bir uygulamaya ekleme gibi önemli hale gelir [Hello, Android çoklu ekranı](~/android/get-started/hello-android-multiscreen/index.md) Kılavuzu.

Şimdi Temelleri `MainActivity` silinmiş Kapsandı, etkinlik koda daha derin Dalış için sunarak zamanı _etkinlik yaşam döngüsü_.

### <a name="activity-lifecycle"></a>Etkinlik yaşam döngüsü

Android'de, bir yaşam döngüsü, kullanıcının etkileşim bağlı olarak farklı aşamalarında etkinlikleri inceleyin. Başlatılan ve duraklatılmış, devam ettirildi ve yok ve vb. etkinlikleri oluşturulabilir. `Activity` Sınıfı, bazı noktalarda ekranın yaşam döngüsünde sistem çağırdığı yöntemleri içerir. Aşağıdaki diyagramda tipik geçerlilik etkinliğin yanı sıra ilgili yaşam döngüsü yöntemlerden bazıları gösterilmektedir:

[![Etkinlik yaşam döngüsü](hello-android-deepdive-images/04-lifecycle-sml.png)](hello-android-deepdive-images/04-lifecycle.png#lightbox)

Geçersiz kılma tarafından `Activity` yaşam döngüsü yöntemleri nasıl etkinlik, kullanıcıya nasıl tepki verdiğini yükleyeceğini denetlemek ve hatta cihaz ekranından kaybolduktan sonra ne olur. Örneğin, bazı önemli görevleri gerçekleştirmek için yukarıdaki diyagramda yaşam döngüsü yöntemlerini geçersiz kılabilirsiniz:

-   **OnCreate** &ndash; görünümler oluşturur, değişkenleri başlatır ve kullanıcı etkinliğini görür önce yapılmalıdır hazırlık diğer işleri yapar. Bu yöntem, yalnızca zaman etkinlik belleğe yüklendiğinde çağrılır. 

-   **OnResume** &ndash; cihaz ekranı etkinlik tarafından döndürülen her zaman gerçekleştirilmesi gerekir herhangi bir görev gerçekleştirir. 

-   **OnPause** &ndash; etkinlik cihaz ekranı ayrıldığında her zaman gerçekleştirilmesi gerekir herhangi bir görev gerçekleştirir.


Bir yaşam döngüsü yöntemine özel kod eklediğinizde `Activity`, size *geçersiz kılma* yaşam döngüsü yöntemin *taban uygulamasını*. (Bazı kod zaten bağlı olan) varolan bir yaşam döngüsü yöntemi uygulamasına dokunun ve bu yöntem kendi kod ile genişletin. Özgün koda yeni kodunuz önce çalıştığından emin olmak için yöntemin içindeki temel uygulamasından çağırın. Buna örnek olarak, sonraki bölümde gösterilmiştir. 

Etkinlik yaşam döngüsü Android önemli ve karmaşık bir parçasıdır. Bitirdikten sonra etkinlikleri hakkında daha fazla bilgi edinmek istiyorsanız, _Başlarken_ okuma serisi [etkinlik yaşam döngüsü](~/android/app-fundamentals/activity-lifecycle/index.md) Kılavuzu. Bu kılavuzda, sonraki odağı etkinlik yaşam döngüsünün ilk aşamasıdır `OnCreate`.


### <a name="oncreate"></a>OnCreate

Android çağrıları `Activity`'s `OnCreate` yöntemi (ekran kullanıcıya sunulan önce) etkinliği oluşturur. Geçersiz kılabilirsiniz `OnCreate` görünümler oluşturabilir ve kullanıcı karşılamak için etkinlik hazırlamak için yaşam döngüsü yöntemi:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);
    // Additional setup code will go here
}
```

İçinde **Phoneword** uygulama, yapılacak ilk şey `OnCreate` Android Tasarımcısı'nda oluşturulan kullanıcı arabirimi olan. Kullanıcı arabirimini yüklemek için çağrı `SetContentView` ve geçirin *kaynak Düzen adı* için Düzen dosyası: `Main.axml`. Düzen şu konumdadır `Resource.Layout.Main`:

```csharp
SetContentView (Resource.Layout.Main);
```

Zaman `MainActivity` başladığında yukarı içeriklerine dayanan bir görünüm oluşturur **Main.axml** dosya. Düzen dosyası adı için etkinlik adı eşleştirilir unutmayın &ndash; *ana*.axml olan düzenini *ana*etkinlik. Bu Android açısından bakıldığında gerekli değildir, ancak uygulamaya daha fazla ekran ekleme başladığınızda, bu adlandırma kuralı, Düzen dosyası kod dosyasına eşleşen kolaylaştırır olduğunu göreceksiniz.

Düzen dosyası hazırladıktan sonra denetimleri bakarak başlayabilirsiniz.
Bir denetimi aramak için arama `FindViewById` ve denetimin kaynak kimliği geçirin:

```csharp
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
```

Denetimlere yapılan başvuruları Düzen dosyasına sahip olduğunuza göre bunları kullanıcı etkileşimine yanıt vermek için programlama başlayabilirsiniz.


### <a name="responding-to-user-interaction"></a>Kullanıcı etkileşimine yanıt verme

Android, `Click` olay için kullanıcının touch dinler. Bu uygulamada `Click` olay, bir lambda ile gerçekleştirilir, ancak bunun yerine bir temsilci veya bir adlandırılmış olayı işleyicisi kullanılabilir. En son **TranslateButton** kod aşağıdaki benzeyen: 

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

## <a name="testing-deployment-and-finishing-touches"></a>Test, dağıtım ve Son dokunuşları

Mac için Visual Studio hem de Visual Studio test etmek ve bir uygulamayı dağıtmak için pek çok seçenek sağlar. Bu bölümde, hata ayıklama seçeneklerini kapsar, bir cihaz üzerinde test uygulamaları gösterir ve özel uygulama simgeleri için farklı bir ekrana densities oluşturmaya yönelik araçlar sunar.


### <a name="debugging-tools"></a>Hata Ayıklama Araçları

Uygulama kodunda sorunları tanılamak zor olabilir. Karmaşık kod sorunlarının tanılanmasına yardımcı olmak için [bir kesme noktası ayarlamak](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [adım kod aracılığıyla](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code), veya [günlük penceresini çıkış bilgileri](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).


### <a name="deploy-to-a-device"></a>Bir cihaza dağıtın

Öykünücü dağıtmak ve bir uygulamayı test etmek için iyi bir başlangıç noktasıdır, ancak kullanıcılar, bir öykünücü son uygulamada tüketecektir değil. Erken ve sıkça gerçek bir cihaz uygulamaları test etmek için iyi bir uygulamadır.

Bir Android cihaz uygulamaları test etmek için kullanılabilmesi için geliştirme yapılandırılması gerekiyor. [Cihaz geliştirme için ayarlanmış yukarı](~/android/get-started/installation/set-up-device-for-development.md) Kılavuzu, bir cihaz geliştirme sürecine hazır alma kapsamlı yönergeler sunar.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Cihaz yapılandırdıktan sonra ona menüsünden seçim, takarak dağıtabileceğiniz **cihaz seçin** iletişim ve uygulama başlatılıyor:

![Select hata ayıklama cihazı](hello-android-deepdive-images/vs/06-select-device.png "Select hata ayıklama cihazı")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Cihaz yapılandırdıktan sonra ona tuşlarına basarak, takarak dağıtabileceğiniz **başlangıç (Çalıştır)**, menüsünden seçim **cihaz seçin** iletişim ve tuşlarına basarak **Tamam**:

[![Select hata ayıklama cihazı](hello-android-deepdive-images/xs/06-select-device-sml.png)](hello-android-deepdive-images/xs/06-select-device.png#lightbox)

-----

Bu, uygulama cihaz üzerinde çalıştırır:

[![Phoneword girin](hello-android-deepdive-images/05-enter-phoneword-sml.png)](hello-android-deepdive-images/05-enter-phoneword.png#lightbox)


### <a name="set-icons-for-different-screen-densities"></a>Farklı ekran Densities için simgeler ayarlama

Android cihazlar farklı ekran boyutları ve çözümleri gelir ve tüm görüntüleri tüm ekranlarda iyi görünür. Örneğin, ekran üzerindeki bir yüksek yoğunluklu Nexus 5 düşük yoğunluklu bir simge görüntüsü aşağıdadır. Nasıl bulanık, çevreleyen simgeleri karşılaştırılır dikkat edin:

[![Bulanık simgesi](hello-android-deepdive-images/06-blurry-icon-sml.png)](hello-android-deepdive-images/06-blurry-icon.png#lightbox)

Bu hesap için farklı çözümler simgeleri eklemek için iyi bir uygulamadır **kaynakları** klasör. Android tarafından sağlanan farklı sürümlerini **mipmap** farklı densities Başlatıcısı simgeleri işlemek için klasör *mdpi* Orta, *hdpı* için yüksek ve  *xhdpi*, *xxhdpi*, *xxxhdpi* çok yüksek yoğunluk ekranlar için. Çeşitli boyutlardaki simgeler uygun depolanan **mipmap -** klasörler:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Mipmap klasörlerinde](hello-android-deepdive-images/vs/07-mipmap-folders.png "mipmap klasörlerinde")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Mipmap klasörlerinde](hello-android-deepdive-images/xs/07-mipmap-folders-sml.png)](hello-android-deepdive-images/xs/07-mipmap-folders.png#lightbox)

-----

Android uygun yoğunluklu içeren simge çeker:

[![En uygun yoğunluklu simgeleri](hello-android-deepdive-images/07-appropriate-density-sml.png)](hello-android-deepdive-images/07-appropriate-density.png#lightbox)

### <a name="generate-custom-icons"></a>Özel simgeleri oluştur

Herkes özel simgeleri oluşturma ve başlatma görüntüleri Rekabetin gerektiren bir uygulama kullanılabilir bir tasarımcı sahiptir. Özel uygulama resmi oluşturmanın birkaç alternatif yaklaşımlar aşağıda verilmiştir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   [Varlık Android Studio](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; diğer yararlı topluluk araçları için bağlantılarla birlikte Android simgeler her tür için bir web tabanlı, tarayıcı içi Oluşturucu. Google Chrome'da en iyi şekilde çalışır.

-   Visual Studio &ndash; doğrudan IDE'de uygulamanız için basit bir simge oluşturmak için bunu kullanabilirsiniz.

-   [Glyphish](http://www.glyphish.com/) &ndash; yüksek kaliteli önceden oluşturulmuş simgesi ayarlar ücretsiz indirme ve satın alma.

-   [Fiverr](http://www.fiverr.com/) &ndash; tasarımcılar, 5 Başlangıç için bir simge oluşturmak için çeşitli seçin. Miss hit veya olabilir ancak simgeler gerekiyorsa bir makaleden anında tasarlanmış.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   [Varlık Android Studio](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; diğer yararlı topluluk araçları için bağlantılarla birlikte Android simgeler her tür için bir web tabanlı, tarayıcı içi Oluşturucu. Google Chrome'da en iyi şekilde çalışır.

-   [Taslak 3](https://itunes.apple.com/us/app/sketch/id852320343?mt=12) &ndash; taslak olan kullanıcı arabirimleri, simgeler ve diğer tasarlamak için bir Mac uygulaması. Bu, Xamarin uygulama simgeleri ve başlatma resimleri kümesi tasarlamak için kullanılan uygulamadır. Taslak 3 App Store üzerinde kullanılabilir ve yaklaşık 80 $ maliyetlerini. Ücretsiz deneyebilirsiniz [taslak aracı](http://bohemiancoding.com/sketch/tool/) de.

-   [Pixelmator](http://www.pixelmator.com/) &ndash; uygulaması yaklaşık 30 ABD Doları değerindedir Mac için düzenleme, çok yönlü bir görüntüsü.

-   [Glyphish](http://www.glyphish.com/) &ndash; yüksek kaliteli önceden oluşturulmuş simgesi ayarlar ücretsiz indirme ve satın alma.

-   [Fiverr](http://www.fiverr.com/) &ndash; tasarımcılar, 5 Başlangıç için bir simge oluşturmak için çeşitli seçin. Miss hit veya olabilir ancak simgeler gerekiyorsa bir makaleden anında tasarlanmış.

-----

Simge boyutunu ve gereksinimleri hakkında daha fazla bilgi için başvurmak [Android kaynakları](~/android/app-fundamentals/resources-in-android/index.md) Kılavuzu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

### <a name="adding-google-play-services-packages"></a>Ekleme Google Play Hizmetleri paketleri

_Google Play Hizmetleri_ google'dan Google haritalar, Google bulut Mesajlaşma ve uygulama içi faturalama gibi en son özelliklerden yararlanmak Android geliştiricileri izin veren bir eklenti kitaplığı kümesidir.
Tüm Google Play Hizmetleri kitaplıkları bağlamaları tek bir paket biçiminde Xamarin tarafından daha önce sağlanan &ndash; Mac için Visual Studio ile başlayarak, yeni proje iletişim kutusu, dahil etmek için Google Play Hizmetleri paketleri seçmek için kullanılabilir Uygulamanızı.

Bir veya daha fazla Google Play Hizmeti'ni kitaplıkları eklemek için sağ **paketleri** tıklayın ve proje ağacı düğümünde **Google Play hizmeti Ekle...** :

[![Google Play hizmeti Ekle](hello-android-deepdive-images/xs/08-add-google-play-services-sml.png)](hello-android-deepdive-images/xs/08-add-google-play-services.png#lightbox)

Zaman **Google Play Hizmetleri ekleme** iletişim sunulur, projenize eklemek istediğiniz paketleri (nuget'i) seçin:

[![Paketleri seçin](hello-android-deepdive-images/xs/09-add-dialog-sml.png)](hello-android-deepdive-images/xs/09-add-dialog.png#lightbox)

Ne zaman bir hizmeti seçin ve tıklayın **Paketi Ekle**, Mac için Visual Studio indirmeleri ve bunu gerektiren tüm bağımlı Google Play Hizmetleri paketleri yanı sıra seçtiğiniz paketi yükler. Bazı durumlarda, görebileceğiniz bir **lisans kabulü** tıklayın gerektiren iletişim **kabul** paketleri yüklemeden önce:

[![Lisans kabulü](hello-android-deepdive-images/xs/10-license-acceptance-sml.png)](hello-android-deepdive-images/xs/10-license-acceptance.png#lightbox)

-----

## <a name="summary"></a>Özet

Tebrikler! Şimdi, düz bir anlayış oluşturmak için gereken araçları yanı sıra, bir Xamarin.Android uygulaması bileşenlerinin olmalıdır.

Sonraki öğreticide _Başlarken_ serisi, uygulamanızın daha gelişmiş Android mimarisi ve kavramları keşfederken birden fazla ekran işlemesini genişletmek.
