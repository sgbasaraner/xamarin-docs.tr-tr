---
title: "Platformlar arası performansı"
description: "Xamarin platformuyla oluşturulan uygulamaların performansını artırmak için birçok tekniği vardır. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir. Bu makalede ve bu teknikler anlatılmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9ce61f18-22ac-4b93-91be-5b499677d661
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: 287f564ba74050aa8a06e5a582ae8db6657e440e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="cross-platform-performance"></a>Platformlar arası performansı

_Xamarin platformuyla oluşturulan uygulamaların performansını artırmak için birçok tekniği vardır. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir. Bu makalede ve bu teknikler anlatılmaktadır._

Zayıf uygulama performans kendisini birçok yolla gösterir. Bir uygulama yapabilirsiniz yanıt vermeyen gibi görünebilir, yavaş kaydırma neden olabilir ve pil ömrünün azaltabilir. Ancak, performansı en iyi duruma getirme daha fazlasını verimli kod uygulama içerir. Uygulama performansı kullanıcı deneyimi de dikkate alınmalıdır. Örneğin, diğer etkinlikler gerçekleştirmeyi kullanıcı engellenmeden işlemlerini yürütmek sağlayarak kullanıcı deneyimini geliştirmek için yardımcı olabilir.


<a name="profiler" />

## <a name="use-the-profiler"></a>Profil Oluşturucu kullanın

Bir uygulama geliştirirken, yalnızca bu profili sonra kodu en iyi duruma getirme girişimi önemlidir. Profil oluşturma, burada kodu en iyi duruma getirme performans sorunları azaltmak amacıyla, en yüksek etkisi belirlemek için bir tekniktir. Profil Oluşturucu uygulamanın bellek kullanımını izler ve uygulamada yöntemleri, çalışma süresini kaydeder. Böylece en iyi duruma getirme için en iyi fırsatları bulunan bu veriler uygulama yürütme yollarını ve kod yürütme maliyetini arasında gezinmek için yardımcı olur.

Xamarin profil oluşturucu ölçmek, değerlendirmek ve uygulamada performans ile ilgili sorunları çözmeye yardımcı olmak. Profil Xamarin.iOS ve Xamarin.Android uygulamalar için kullanılabilir Mac veya Visual Studio için Visual Studio içinde. Xamarin profil oluşturucu hakkında daha fazla bilgi için bkz: [Xamarin profil oluşturucu giriş](~/tools/profiler/index.md).

Aşağıdaki en iyi yöntemler, bir uygulama profili oluşturma zaman önerilir:

- Simulator uygulama performansı bozabilir gibi bir simulator uygulamada profil kaçının.
- Bir cihazda performans ölçümleri alma diğer cihazların performans özellikleri her zaman göstermeyecektir gibi ideal olarak, profil çeşitli aygıtlardan gerçekleştirilmesi gerekir. Ancak, en azından, profil düşük beklenen belirtimi içeriyor bir aygıtta gerçekleştirilmesi gerekir.
- Profili oluşturuluyor uygulama tam etkisini, diğer uygulamalar yerine ölçülecek emin olmak için tüm diğer uygulamaları kapatın.

<a name="idisposable" />

## <a name="release-idisposable-resources"></a>IDisposable kaynakları serbest bırakmak

`IDisposable` Arabirimi kaynakları serbest bırakmak için bir mekanizma sağlar. Sağladığı bir `Dispose` açıkça kaynakları serbest bırakmak için uygulanması yöntemi. `IDisposable` bir yıkıcı değildir ve yalnızca aşağıdaki durumlarda uygulanması gerekir:

- Ne zaman sınıf yönetilmeyen kaynakları sahip olur. Serbest bırakma gerektiren tipik yönetilmeyen kaynaklar dosyaları, akış ve ağ bağlantıları içerir.
- Yönetilen sınıf sahip olduğunda `IDisposable` kaynakları.

Türü tüketicileri sonra çağırabilir `IDisposable.Dispose` örneği artık gerekli olmadığında kaynakları serbest uygulama. Bunu elde etmek için iki yaklaşım vardır:

- Kaydırma tarafından `IDisposable` nesnesinde bir `using` deyimi.
- Çağrı kaydırma tarafından `IDisposable.Dispose` içinde bir `try` / `finally` bloğu.

### <a name="wrapping-the-idisposable-object-in-a-using-statement"></a>Kullanarak bir IDisposable nesne kaydırma deyimi

Aşağıdaki kod örneğinde nasıl kaydırılacağını gösterir bir `IDisposable` nesnesinde bir `using` deyimi:

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  using (StreamReader reader = new StreamReader (filename)) {
    text = reader.ReadToEnd ();
  }
  ...
}
```

`StreamReader` Uygulayan sınıf `IDisposable`ve `using` deyimi çağırır kullanışlı bir sözdizimi sağlar `StreamReader.Dispose` yöntemi `StreamReader` , kapsam dışındadır geçmeden önce nesne. İçinde `using` bloğu `StreamReader` nesnesi salt okunurdur ve atanamaz. `using` Deyimi, aynı zamanda sağlar, `Dispose` yöntemi çağrıldığında bir özel durum oluştuğunda dahi derleyici Ara dile (IL) uygulayan gibi için bir `try` / `finally` bloğu.

### <a name="wrapping-the-call-to-idisposabledispose-in-a-tryfinally-block"></a>Bir Try/Finally bloğu içinde IDisposable.Dispose çağrısı sarmalama

Aşağıdaki kod örneğinde çağrısı sarmalama gösterir `IDisposable.Dispose` içinde bir `try` / `finally` engelle:

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  StreamReader reader = null;
  try {
    reader = new StreamReader (filename);
    text = reader.ReadToEnd ();
  } finally {
    if (reader != null) {
      reader.Dispose ();
    }
  }
  ...
}
```

`StreamReader` Uygulayan sınıf `IDisposable`ve `finally` engelleme çağrıları `StreamReader.Dispose` kaynak yayımlamayı yöntemi.

Daha fazla bilgi için bkz: [IDisposable arabirimi](https://developer.xamarin.com/api/type/System.IDisposable/).

<a name="events" />

## <a name="unsubscribe-from-events"></a>Olay aboneliği

Abonelik nesnesinin elden önce bellek sızıntıları önlemek için olayları gelen aboneliği olmalıdır. Olay gelen aboneliği kaldırılan kadar yayımlama nesnesindeki olayı için temsilci abonenin olay işleyicisi yalıtır temsilci bir başvuru içeriyor. Bu başvuru yayımlama nesnesi tutar sürece, atık toplama, abone nesne belleği geri değil.

Aşağıdaki kod örneği, bir olay aboneliği gösterilmektedir:

```csharp
public class Publisher
{
  public event EventHandler MyEvent;

  public void OnMyEventFires ()
  {
    if (MyEvent != null) {
      MyEvent (this, EventArgs.Empty);
    }
  }
}

public class Subscriber : IDisposable
{
  readonly Publisher publisher;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    publisher.MyEvent += OnMyEventFires;
  }

  void OnMyEventFires (object sender, EventArgs e)
  {
    Debug.WriteLine ("The publisher notified the subscriber of an event");
  }

  public void Dispose ()
  {
    publisher.MyEvent -= OnMyEventFires;
  }
}
```

`Subscriber` Sınıfı işlemleri içinde olaydan kendi `Dispose` yöntemi.

Başvuru döngüleri da lambda ifadeleri başvuru ve nesneleri canlı olay işleyicileri ve lambda sözdizimi kullanılırken oluşabilir. Bu nedenle, bir başvuru anonim yöntemi bir alana depolanabilir ve aşağıdaki kod örneğinde gösterildiği gibi olayından aboneliği için kullanılan:

```csharp
public class Subscriber : IDisposable
{
  readonly Publisher publisher;
  EventHandler handler;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    handler = (sender, e) => {
      Debug.WriteLine ("The publisher notified the subscriber of an event");
    };
    publisher.MyEvent += handler;
  }

  public void Dispose ()
  {
    publisher.MyEvent -= handler;
  }
}
```

`handler` Alan anonim yöntemi referansı korur ve olay aboneliği için kullanılır ve aboneliği.

<a name="weakreferences" />

## <a name="use-weak-references-to-prevent-immortal-objects"></a>Zayıf başvurular Immortal nesneleri engellemek için kullanın

> [!NOTE]
> iOS geliştiricileri gözden geçirmeniz gereken belge üzerinde [iOS içinde döngüsel başvurulara önleme](~/ios/deploy-test/performance.md#avoidcircularreferences) uygulamalarını kullanmak bellek verimli bir şekilde sağlamak için.

<a name="lazy" />

## <a name="delay-the-cost-of-creating-objects"></a>Nesne oluşturma maliyetini gecikme

Geç Başlatma ilk kullanılan kadar bir nesnesinin oluşturulmasını ertelemek için kullanılabilir. Bu teknik öncelikle performansı, hesaplama önlemek ve bellek gereksinimlerini azaltmak için kullanılır.


Bu iki senaryoda pahalıdır nesneler için geç başlatma kullanarak göz önünde bulundurun:

- Uygulama nesnesi kullanamayabilir.
- Nesne oluşturulmadan önce pahalı diğer işlemleri tamamlamanız gerekir.

`Lazy<T>` Sınıfı bir başlatılacağı yavaş türünü tanımlamak için kullanılan aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
void ProcessData(bool dataRequired = false)
{
  Lazy<double> data = new Lazy<double>(() =>
  {
    return ParallelEnumerable.Range(0, 1000)
                 .Select(d => Compute(d))
                 .Aggregate((x, y) => x + y);
  });

  if (dataRequired)
  {
    if (data.Value > 90)
    {
      ...
    }
  }
}

double Compute(double x)
{
  ...
}
```

Geç Başlatma ilk kez oluşur `Lazy<T>.Value` özelliği erişilir. Sarmalanan türü oluşturulur ve ilk erişimde döndürülen ve gelecekteki tüm erişimi için depolanan.

Geç başlatma hakkında daha fazla bilgi için bkz: [geç başlatma](https://msdn.microsoft.com/en-us/library/dd997286(v=vs.110).aspx).

<a name="async" />

## <a name="implement-asynchronous-operations"></a>Zaman uyumsuz işlemleri uygulama

.NET API'lerini çoğunu zaman uyumsuz sürümlerini sağlar. Zaman uyumlu API'ları farklı olarak, etkin yürütme iş parçacığı önemli miktarda zaman için hiçbir zaman çağıran iş parçacığı engeller zaman uyumsuz API'leri emin olun. Bu nedenle, kullanılabilir durumdaysa bir API kullanıcı Arabirimi iş parçacığından çağrılırken, zaman uyumsuz API kullanın. Bu uygulama ile kullanıcı deneyimini geliştirmek için yardımcı olacak engeli kaldırılmış, kullanıcı Arabirimi iş parçacığı tutar.

Ayrıca, uzun süre çalışan işlemleri kullanıcı Arabirimi iş parçacığı engelleme önlemek için bir arka plan iş parçacığında yürütülmelidir. .NET sağlar `async` ve `await` arka plan iş parçacığında uzun süre çalışan işlemlerini yürütür ve tamamlanma sonuçlarına eriştiğinde zaman uyumsuz kod yazmayı etkinleştirmek anahtar sözcükler. Ancak, uzun süreli işlemler yürütülebilir zaman uyumsuz olarak ile `await` anahtar sözcüğü, bu garanti etmez işlemi arka plan iş parçacığı üzerinde çalışır. Bunun yerine, bu uzun süre çalışan işlemin geçirerek gerçekleştirilebilir `Task.Run`aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public class FaceDetection
{
  ...
  async void RecognizeFaceButtonClick(object sender, EventArgs e)
  {
    await Task.Run(() => RecognizeFace ());
    ...
  }

  async Task RecognizeFace()
  {
    ...
  }
}
```

`RecognizeFace` Yöntemi ile bir arka plan iş parçacığında yürütür `RecognizeFaceButtonClick` kadar bekleyen yöntemi `RecognizeFace` yöntemi tamamlayan devam etmeden önce.

Uzun süreli işlemler iptal desteklemesi gerekir. Örneğin, uzun süren bir işlem devam kullanıcı uygulama içerisinden giderse gereksiz olabilir. İptal uygulamak için desen aşağıdaki gibidir:

- Oluşturma bir `CancellationTokenSource` örneği. Bu örnek, yönetme ve iptal bildirimleri göndermek.
- Geçirmek `CancellationTokenSource.Token` iptal edilebilen olması gereken her görev için özellik değeri.
- Her görev için İptali yanıt için bir mekanizma sağlar.
- Çağrı `CancellationTokenSource.Cancel` iptal bildirim sağlamak için yöntem.

> [!IMPORTANT]
> `CancellationTokenSource` Sınıfını uygular `IDisposable` arabirimi ve bu nedenle `CancellationTokenSource.Dispose` yöntemi çağrılabilir kez `CancellationTokenSource` örneği ile tamamlandı.



Daha fazla bilgi için bkz: [zaman uyumsuz desteğine genel bakış](~/cross-platform/platform/async.md).

<a name="sgen" />

## <a name="use-the-sgen-garbage-collector"></a>SGen atık toplayıcısını kullanın

Artık kullanımda olmayan nesneler için ayrılan bellek geri kazanmak için C# kullanın çöp toplama gibi dilleri yönetilen. Xamarin platform tarafından kullanılan iki atık toplayıcıları şunlardır:

- [**SGen** ](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) – bu kişinin atık toplayıcısını ve Xamarin platformunda varsayılan atık toplayıcı.
- [**Boehm** ](http://www.hboehm.info/gc/) – koruyucu, kişinin olmayan atık toplayıcı budur. Klasik API kullanan Xamarin.iOS uygulamaları için kullanılan varsayılan atık toplayıcı olur.

SGen nesneler için alan ayırmak için üç yığınlara birini kullanır:

-  **Çocuk odası** – yeni küçük nesneleri burada ayrılan budur. Çocuk odası alana çalıştığında, küçük çöp toplama meydana gelir. Herhangi bir dinamik Nesne ana yığın taşınır.
-  **Ana yığın** – uzun süre çalışan nesneleri saklandığı budur. Ana yığınında yeterli bellek yoksa, ana çöp toplama meydana gelir. Yeterli belleği boşaltmak ana çöp toplama başarısız olursa SGen için daha fazla bellek sistem sorar.
-  **Büyük nesne alanı** – 8000 bayttan fazla gerektiren nesnelerini saklandığı budur. Büyük nesneler çocuk odası başlatılmaz, ancak bunun yerine bu yığın ayrılır.

SGen avantajlarından biri, küçük çöp toplama gerçekleştirmek için gereken süreyi son küçük çöp toplamadan beri oluşturulan yeni dinamik nesneler sayısıyla orantılı olmasıdır. Bu ikincil çöp koleksiyonları büyük çöp toplama daha az zaman alır gibi bu atık toplama bir uygulama performans üzerindeki etkisini azaltır. Hala ana çöp koleksiyonları gerçekleşir, ancak daha az sıklıkta.

### <a name="reducing-pressure-on-the-garbage-collector"></a>Çöp toplayıcı Basıncı azaltma

SGen çöp toplama başladığında, bellek kaldırsa durumdayken uygulamanın iş parçacığı durdurur. Bellek iadesi olsa da, uygulama kısa bir duraklama deneyimi veya kullanıcı Arabiriminde titremesine. Bu Duraklat nasıl algılanabilir olan iki etkenlere bağlıdır:

1. **Sıklık** – ne sıklıkta çöp toplama oluşur. Daha fazla bellek koleksiyonları arasında ayrılmış olarak çöp koleksiyonları artacaktır.
1. **Süre** – her bireysel çöp toplama ne kadar sürer. Bu kabaca toplanmakta olan dinamik Nesne sayısı için doğru orantılıdır.

Topluca bu anlamına birçok nesne ayrılan ancak Canlı kalmak değil, olacağını birçok kısa çöp koleksiyonu. Buna karşılık, yavaş ayrılmış yeni nesneleri ve nesnelerin Canlı kalır, olacaktır daha az ancak uzun çöp koleksiyonları.

Çöp toplayıcı Basıncı azaltmak için aşağıdaki yönergeleri izleyin:

- Çöp toplama sıkı döngüler nesne havuzları kullanarak kaçının. Bu, özellikle nesnelerine çoğunluğu önceden oluşturmak için gereken oyunlar için geçerlidir.
- Artık gerekli olmayan sonra açıkça akışlar, ağ bağlantıları, bellek ve dosyaları büyük bloklarını gibi kaynakları serbest bırakır. Daha fazla bilgi için bkz: [yayın IDisposable kaynakları](#idisposable).
- Olay işleyicileri XML'deki artık nesneleri toplanabilir yapmak için gerekli olan sonra kaydedin. Daha fazla bilgi için bkz: [olaylardan abonelikten](#events).

<a name="linker" />

## <a name="reduce-the-size-of-the-application"></a>Uygulama boyutunu azaltın

Derleme işlemi uygulamaları yürütülebilir boyutu nereden geldiğini anlamak için her platformda anlamak önemlidir:

- iOS, tamamlanan,-ARM derleme dili için derlenmiş zamanı (Uygulama Nesne AĞACI) uygulamalardır. .NET framework ile yalnızca uygun bağlayıcı seçeneği etkinse çıkarılır kullanılmayan sınıfları içerir.
- Android uygulamaları Ara dile (IL) derlenir ve MonoVM ve tam zamanında (JIT) derleme paketlenir. Yalnızca uygun bağlayıcı seçeneği etkinse kullanılmayan framework sınıfları çıkarılır.
- Windows Phone uygulamaları için IL derlenir ve yerleşik çalışma zamanı tarafından yürütülür.

Ayrıca, yerel olarak içerecektir bu yana en son yürütülebilir boyutuna daha fazla artıracaktır sonra uygulamanın genel türler kapsamlı kullanımını yaparsa genel olasılıklar sürümleri derlenmiş.

Uygulamaları boyutunu azaltmak için Xamarin platform derleme araçları bir parçası olarak bir bağlayıcı içerir. Bağlayıcı, varsayılan olarak devre dışıdır ve proje seçenekleri uygulama için etkinleştirilmiş olması gerekir. Derleme zamanında hangi türleri ve üyeleri, uygulama tarafından gerçekten kullanılacağını belirlemek için uygulamanın statik çözümleme gerçekleştirir. Kullanılmayan türleri ve yöntemleri'nı, ardından uygulamadan kaldırır.

Aşağıdaki ekran görüntüsünde bağlayıcı seçenekleri Visual Studio'da Mac için bir Xamarin.iOS projesi için gösterir:

![](memory-perf-best-practices-images/linker-options-ios.png)

Aşağıdaki ekran görüntüsünde bağlayıcı seçenekleri Visual Studio'da Mac için bir Xamarin.Android projesi için gösterir:

![](memory-perf-best-practices-images/linker-options-droid.png)

Bağlayıcı davranışını denetlemek için üç farklı ayardan sağlar:

-  **Bağlantı** – bağlayıcı tarafından hiç kullanılmamış türleri ve yöntemleri kaldırılacak. Performans nedenleriyle hata ayıklama derlemeleri için varsayılan ayar budur.
-  **Bağlama çerçevesi SDK'lar/SDK derlemeleri yalnızca** – Bu ayar yalnızca Xamarin tarafından gönderilen bu derlemeler boyutunu azaltır. Kullanıcı kodu etkilenmeyecek.
-  **Bağlantı tüm derlemelerde** – SDK derlemeleri ve kullanıcı kodu hedeflediğini daha agresif bir iyileştirme budur. Bağlamaları için bu kullanılmayan yedekleme alanlarını kaldırın ve her örneği (veya bağımlı nesneler) yapar daha az bellek tükettikten açık.

*Bağlantı tüm derlemelerde* beklenmedik bir şekilde uygulama kesilebilir gibi dikkatli kullanılmalıdır. Bağlayıcı tarafından gerçekleştirilen statik çözümleme doğru derlenmiş uygulamadan kaldırılmakta olan çok fazla kod bunun sonucunda, gerekli olan tüm kodları tanımlayabilir değil. Uygulama kilitlendiğinde bu durum kendisi yalnızca çalışma zamanında bildirimi. Bu nedenle baştan sona bağlayıcı davranışı değiştirdikten sonra bir uygulamayı test etmek önemlidir.

Sınama bağlayıcı yanlış olduğunu ortaya çıkıyorsa bir sınıf veya yöntemini türleri işaretlemek mümkündür veya statik olarak başvurulan değil, ancak aşağıdaki öznitelikler birini kullanarak uygulama tarafından gerekli yöntemlerini kaldırıldı:

-  `Xamarin.iOS.Foundation.PreserveAttribute` – Bu öznitelik Xamarin.iOS projelerde bağlıdır.
-  `Android.Runtime.PreserveAttribute` – Bu öznitelik Xamarin.Android projelerde bağlıdır.

Örneğin, dinamik olarak örneği türlerinin varsayılan oluşturucular korumak gerekli olabilir. Ayrıca, XML serileştirme kullanımını türlerin özellikleri korunur gerektirebilir.

Daha fazla bilgi için bkz: [iOS için bağlayıcı](~/ios/deploy-test/linker.md) ve [Android için bağlayıcı](~/android/deploy-test/linker.md).

### <a name="additional-size-reduction-techniques"></a>Ek boyutunu azaltma teknikleri

Bu güç mobil aygıtları çok çeşitli CPU mimarileri vardır. Bu nedenle, Xamarin.iOS ve Xamarin.Android üretmek *fat ikili dosyaları* uygulama her CPU mimarisi için derlenmiş bir sürümünü içerir. Bu, bir mobil uygulama bir cihazda CPU mimarisi bağımsız olarak çalıştırabilirsiniz sağlar.

Daha fazla uygulama yürütülebilir boyutunu azaltmak için aşağıdaki adımları kullanılabilir:

- Yayın derlemesi üretilen emin olun.
- Üretilen FAT bir ikili önlemek için uygulamanın yerleşik mimarileri sayısını azaltın.
- LLVM derleyici, daha fazla en iyi duruma getirilmiş bir yürütülebilir dosyayı oluşturmak için kullanıldığından emin olun.
- Uygulamanın yönetilen kod boyutunu küçültün. Bu bağlayıcı her derleme üzerinde etkinleştirerek gerçekleştirilebilir (*bağlantı tüm* iOS projelerde ve *tüm derlemelerde bağlantı* Android projeleri için).

Android uygulamaları, her ABI ("mimari") ayrı APK da bölünebilir.
Bu Web günlüğü gönderisinde daha fazla bilgi edinin: [nasıl için tutmak bilgisayarınızı Android uygulama boyutu aşağı](http://motzcod.es/post/112072508362/how-to-keep-your-android-app-size-down).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Görüntü kaynakları en iyi duruma getirme

Görüntüleri uygulamaları kullanan, en pahalı kaynakları bazıları verilmiştir ve genellikle yüksek çözünürlüklerde yakalanır. Bu Canlı görüntüleri ayrıntılarını tam oluştururken, bu tür görüntüleri genellikle göstermek uygulamalar görüntü kodunu çözmek için daha fazla CPU kullanımı ve kodu çözülmüş görüntüyü depolamak için daha fazla bellek gerektirir. Bu görüntü için daha küçük bir boyuta ölçeklendirilir olduğunda yüksek çözünürlüklü görüntü bellekte çözecek kayıp. Bunun yerine, yakın tahmin edilen görüntü boyutu depolanan görüntüleri birden fazla çözümleme sürümünü oluşturarak CPU kullanımı ve bellek alanını azaltır. Örneğin, bir liste görünümünde görüntülenen bir resimdir büyük olasılıkla tam ekranında görüntülenen bir resimdir'den daha düşük çözünürlüğü olmalıdır. Ayrıca, yüksek çözünürlüklü görüntülerinin sürümlerin ölçeklendirilmiş ile en az bellek etkisi bunları verimli bir şekilde görüntülenecek yüklenebilir. Daha fazla bilgi için bkz: [yük büyük bit eşlemler verimli bir şekilde](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently/).

Resim çözünürlüğü bağımsız olarak, görüntü kaynakları görüntüleme uygulamanın bellek alanını önemli ölçüde artırabilir. Bu nedenle, bunlar yalnızca gerekli ve uygulama artık gerektirmesi hemen serbest bırakılacak oluşturulmalıdır.

<a name="activationperiod" />

## <a name="reduce-the-application-activation-period"></a>Uygulama etkinleştirme süresini azaltın

Tüm uygulamaların bir *etkinleştirme süresi*, uygulama başlatıldığında ve uygulama kullanıma hazır olduğunda arasındaki süreyi olduğu. Bu etkinleştirme süresi, uygulama kendi ilk izlenim kullanıcılara sağlar ve bu nedenle etkinleştirme süresi ve kullanıcıların algısı, uygulamanın olumlu bir ilk izlenim elde etmesine sırayla azaltmak önemlidir.

İlk kullanıcı Arabiriminde bir uygulama görüntülemeden önce kullanıcıya uygulama başlayarak olduğunu belirtmek için bir giriş ekranı sağlamalıdır. Uygulama Hızlı Başlangıç kullanıcı Arabiriminde görüntüleyemiyor, giriş ekranı etkinleştirme süresi ilerlemeyi uygulama askıda kurmadı güvenceyi teklif kullanıcıya bildirmek için kullanılmalıdır. Bu güvenceyi bir ilerleme çubuğu veya benzer denetim olabilir.

Etkinleştirme süresi boyunca uygulamalar genellikle yükleme ve kaynakları işlenmesini içerir etkinleştirme mantığı yürütün. Etkinleştirme süresi, gerekli kaynakları uzaktan alınan yerine uygulama içinde paketlenir sağlayarak azaltılabilir. Örneğin, bazı durumlarda, yerel olarak depolanmış yer tutucu veri yüklemek için etkinleştirme süresi boyunca uygun olabilir. Sonra Başlangıçtaki kullanıcı Arabirimi görüntülenir ve kullanıcının uygulamayla etkileşemeyebilirsiniz sonra yer tutucu veri aşamalı olarak uzak kaynaktan değiştirilebilir. Ayrıca, uygulamanın etkinleştirme mantığı yalnızca uygulamayı kullanmaya başlayın kullanıcı izin vermek için gerekli olan iş gerçekleştirmeniz gerekir. Yükleme ek derlemeler gecikmeler varsa, derlemeler kullanıldıkları ilk kez yüklü olduğundan bu yardımcı olabilir.

<a name="webservicecommunication" />

## <a name="reduce-web-service-communication"></a>Web hizmeti iletişimi azaltmak için

Bir uygulama bir web hizmetine bağlanma uygulama performansı üzerinde bir etkisi olabilir. Örneğin, ağ bant genişliği kullanımının yüksek bir cihazın pil artan kullanım neden olur. Ayrıca, kullanıcılar uygulama bant genişliği sınırlı bir ortamda kullanıyor olabilir. Bu nedenle, bir uygulama ile bir web hizmeti arasında bant genişliği kullanımını sınırlama duyarlı.

Bir uygulamanın bant genişliği kullanımını azaltmak için tek bir ağ üzerinden aktarmadan önce verileri sıkıştırmak için yaklaşımdır. Ancak, ek CPU kullanımı sıkıştırma işlemi aynı zamanda bir artan pil kullanılmasına neden olabilir. Bu nedenle, bu kolaylığını dikkatle sıkıştırılmış verileri bir ağ üzerinden taşınıp taşınmayacağını karar vermeden önce değerlendirilmelidir.

Dikkate alınması gereken başka bir sorun, bir uygulama ile bir web hizmeti arasında taşır veri biçimidir. İki birincil biçimler şunlardır: Genişletilebilir İşaretleme Dili (XML) ve JavaScript nesne gösterimi (JSON). Çok sayıda biçimlendirme karakterlerini içerdiğinden XML görece büyük veri yüklerini üreten bir metin tabanlı veri değişim biçimidir. JSON veri gönderilirken hangi sonuçlarında azaltılmış bant genişliği gereksinimlerini compact veri yüklerini üreten bir metin tabanlı veri değişim biçimi olan ve veri alma. Bu nedenle, JSON mobil uygulamalar için tercih edilen biçim görülür.

Bu uygulama ve bir web hizmeti arasında veri aktarımı yaparken veri aktarımı nesneleri (DTOs) kullanılması önerilir. Bir DTO ağ üzerinden aktarılması için veri kümesini içerir. DTOs yararlanarak daha fazla veri uygulama tarafından yapılan uzak çağrı sayısı azaltılmasına yardımcı olabilir tek bir uzak çağrıda iletilebilir. Genellikle, daha büyük bir veri yükünü taşıyan bir uzak çağrısı bir benzer süreyi yalnızca küçük veri yükünü taşıyan bir çağrı olarak alır.

Web hizmetinden alınan verileri kullanılan yerine sürekli web hizmetinden alınan önbellekteki veriler ile yerel olarak önbelleğe. Ancak, bu yaklaşım benimsenmesi zaman önbelleğe alma için uygun bir strateji de web hizmetinde değişirse yerel önbellekteki verileri güncelleştirmek için uygulanmalıdır.

## <a name="summary"></a>Özet

Bu makalede açıklanan ve Xamarin platformu kullanılarak oluşturulan uygulamaların performansını artırmak için teknikleri ele alınan. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.iOS Performance](~/ios/deploy-test/performance.md)
- [Xamarin.Android Performance](~/android/deploy-test/performance.md)
- [Xamarin profil oluşturucu giriş](~/tools/profiler/index.md)
- [Xamarin.Forms performans](~/xamarin-forms/deploy-test/performance.md)
- [Zaman Uyumsuz Desteğe Genel Bakış](~/cross-platform/platform/async.md)
- [IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/)
- [Xamarin uygulamaları (video) ortak Tuzaklar önleme](https://university.xamarin.com/guestlectures/avoiding-common-pitfalls-in-xamarin-apps)
