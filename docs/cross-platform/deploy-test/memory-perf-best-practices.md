---
title: Platformlar arası performans
description: Bu belgede, mobil uygulama performansını artırmak için kullanılan çeşitli teknikler açıklanır. Profiler IDisposable kaynak, zayıf başvurular, SGen atık toplayıcı, boyutu azaltma teknikleri ve daha fazla ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 9ce61f18-22ac-4b93-91be-5b499677d661
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: c529d1d42d582cb49a906ad6fc39a191a7389f58
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997445"
---
# <a name="cross-platform-performance"></a>Platformlar arası performans

Kötü uygulama performansı kendisini birçok şekilde gösterir. Bir uygulamayı yapabilirsiniz yanıt vermiyor, yavaş kaydırma neden olabilir ve pil ömrü azaltabilir. Ancak, performansı en iyi duruma getirme verimli kod daha fazlasını uygulama içerir. Uygulama performansı kullanıcı deneyimi de dikkate alınmalıdır. Örneğin, kullanıcının diğer etkinliklerini gerçekleştirmesi engellemeden işlemleri yürütmek sağlayarak kullanıcı deneyimini geliştirmek için yardımcı olabilir.

<a name="profiler" />

## <a name="use-the-profiler"></a>Profiler'ı kullanın

Bir uygulama geliştirirken, yalnızca bu profili sonra kodu En İyileştir denemek önemlidir. Profil oluşturma, burada kod iyileştirmeleri performans sorunları azaltmak amacıyla, en büyük etkisi olmayacağını belirlemek için bir tekniktir. Profil Oluşturucu uygulamanın bellek kullanımı izler ve uygulama çalışma süresi yöntemleri kaydeder. Böylece en iyi duruma getirme için iyi fırsatlar bulunan uygulama yürütme yollarını ve kodu yürütme maliyeti gitmek için bu verileri yardımcı olur.

Xamarin Profiler ölçmesine, değerlendirin ve uygulamada performans ile ilgili sorunları çözmeye yardımcı olmak. Profili Xamarin.iOS ve Xamarin.Android uygulamaları için kullanılabilir Visual Studio veya Mac için Visual Studio içinde. Xamarin Profiler hakkında daha fazla bilgi için bkz: [Xamarin Profiler giriş](~/tools/profiler/index.md).

Aşağıdaki en iyi bir uygulama profili oluşturulurken önerilir:

- Simülatör uygulamanın performansını bozabilir gibi bir uygulamada bir simülatör profil oluşturma kaçının.
- Performans ölçümleri bir cihazda alan diğer cihazlar performans özellikleri her zaman gösterilmez gibi ideal olarak, profil oluşturma çeşitli cihazlar üzerinde gerçekleştirilmelidir. Ancak, en azından, profil oluşturma beklenen en düşük belirtimi var olan bir cihazda gerçekleştirilmelidir.
- Tam profillenen uygulamanın etkisini, diğer uygulamaların yerine Ölçülmekte olan emin olmak için tüm diğer uygulamaları kapatın.

<a name="idisposable" />

## <a name="release-idisposable-resources"></a>IDisposable kaynakları serbest bırakmak

`IDisposable` Arabirimi kaynakları serbest bırakmak için bir mekanizma sağlar. Sağladığı bir `Dispose` kaynakları açıkça serbest bırakmak için uygulanması gereken yöntemini. `IDisposable` bir yok edici değildir ve yalnızca aşağıdaki durumlarda uygulanması:

- Ne zaman sınıf, yönetilmeyen kaynakları sahip olur. Serbest gerektiren tipik yönetilmeyen kaynaklar, dosyalar, akışları ve ağ bağlantıları içerir.
- Yönetilen sınıf sahip olduğunda `IDisposable` kaynakları.

Tür tüketicileri daha sonra çağırabilir `IDisposable.Dispose` örneği artık gerekli olmadığında kaynakları serbest bırakmak. Bunu elde etmek için iki yaklaşım vardır:

- Sarmalama tarafından `IDisposable` nesnesine bir `using` deyimi.
- Sarmalama çağrısı tarafından `IDisposable.Dispose` içinde bir `try` / `finally` blok.

### <a name="wrapping-the-idisposable-object-in-a-using-statement"></a>Kullanarak bir IDisposable nesnesi sarmalama deyimi

Aşağıdaki kod örneği nasıl kaydırılacağını gösteren bir `IDisposable` nesnesine bir `using` deyimi:

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

`StreamReader` Sınıfının Implements `IDisposable`ve `using` deyimi çağıran bir sözdizimindeki sağlar `StreamReader.Dispose` metodunda `StreamReader` , kapsam dışına geçmeden önce nesne. İçinde `using` bloğu `StreamReader` nesne salt okunur ve yeniden atanamaz. `using` Bildirimi ayrıca sağlar `Dispose` yöntemi çağrıldığında bir özel durum oluşursa bile derleyici Ara dil (IL) uyguladığı için bir `try` / `finally` blok.

### <a name="wrapping-the-call-to-idisposabledispose-in-a-tryfinally-block"></a>Bir Try/Finally bloğu içinde IDisposable.Dispose çağrısı sarmalama

Aşağıdaki kod örneği, çağrısını sarmalamak gösterilmektedir `IDisposable.Dispose` içinde bir `try` / `finally` engelle:

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

`StreamReader` Sınıfının Implements `IDisposable`ve `finally` block çağrıları `StreamReader.Dispose` kaynağı bırakmak için yöntemi.

Daha fazla bilgi için [IDisposable arayüzünü](xref:System.IDisposable).

<a name="events" />

## <a name="unsubscribe-from-events"></a>Olay aboneliği iptal et

Abonelik nesnesinin elden önce bellek sızıntılarını önlemek için olayları gelen aboneliği olmalıdır. Gelen olay aboneliğiniz kadar yayımlama nesnede bir olay için temsilci abonenin olay işleyicisi yalıtan temsilci bir başvuru içeriyor. Yayımlama nesne bu başvuru tutan sürece, atık toplama, abone nesne belleği geri değil.

Aşağıdaki kod örneği, bir olayın aboneliğini kaldırmak gösterilmektedir:

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

`Subscriber` Sınıfı abonelikten çıkma olay kendi `Dispose` yöntemi.

Başvuru döngülerini, lambda ifadeleri, başvuru ve nesneleri canlı olarak olay işleyicileri ve lambda sözdizimi kullandığınızda da meydana gelebilir. Bu nedenle, anonim bir yönteme başvuru bir alanda depolanabilir ve olayın aboneliğini kaldırmak için aşağıdaki kod örneğinde gösterildiği gibi kullanılır:

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

`handler` Alan anonim bir yönteme başvuru tutan ve olay aboneliği için kullanılır ve aboneliği kaldırma.

<a name="weakreferences" />

## <a name="use-weak-references-to-prevent-immortal-objects"></a>Zayıf başvurular Immortal nesneleri engellemek için kullanın

> [!NOTE]
> iOS geliştiricileri, şirket belgelerine incelemelidir [iOS bulunan döngüsel başvurular önleme](~/ios/deploy-test/performance.md#avoid-strong-circular-references) uygulamalarını kullanan bellek verimli bir şekilde sağlamak için.

<a name="lazy" />

## <a name="delay-the-cost-of-creating-objects"></a>Gecikme maliyeti nesneleri oluşturma

Yavaş başlatma, ilk kez kullanılana kadar bir nesne oluşturma ertelemek için kullanılabilir. Bu teknik, öncelikle performansı, hesaplama önlemek ve bellek gereksinimlerini azaltmak için kullanılır.


Bu iki senaryoda oluşturması pahalıya nesnelerin geç başlatma kullanarak göz önünde bulundurun:

- Uygulama, nesne kullanamayabilir.
- Nesne oluşturulmadan önce diğer pahalı işlemlerini tamamlamanız gerekir.

`Lazy<T>` Sınıfı geç başlatılan bir tür tanımlamak için kullanılır aşağıdaki kod örneğinde gösterildiği gibi:

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

Yavaş başlatma gerçekleşir ilk kez `Lazy<T>.Value` özelliğine erişilirse. Kaydırılmış tür oluşturulur ve ilk erişimde döndürdü ve herhangi bir gelecek erişimi için depolanır.

Yavaş başlatma hakkında daha fazla bilgi için bkz: [yavaş başlatma](https://msdn.microsoft.com/library/dd997286(v=vs.110).aspx).

<a name="async" />

## <a name="implement-asynchronous-operations"></a>Zaman uyumsuz işlemleri uygulama

.NET API'lerini çoğu zaman uyumsuz sürümlerini sağlar. Zaman uyumlu API'ları farklı olarak, zaman uyumsuz API'leri active yürütme iş parçacığı önemli bir süre için hiçbir zaman çağıran iş parçacığını engeller emin olun. Bu nedenle, varsa UI iş parçacığından bir API'nin çağrılması durumunda, zaman uyumsuz API kullanın. Bu uygulama ile kullanıcı deneyimini geliştirmeye yardımcı olan engeli kaldırılmış, UI iş parçacığı tutar.

Ayrıca, uzun süren işlemlere UI iş parçacığı engellenmesini önlemek için bir arka plan iş parçacığı üzerinde yapılmalıdır. .NET sağlar `async` ve `await` uzun süre çalışan işlemleri arka plan iş parçacığında çalışır ve tamamlandığında sonuçları eriştiğinde, zaman uyumsuz kod yazmayı sağlayan anahtar sözcükler. Ancak, uzun süren işlemlere yürütülebilir zaman uyumsuz olarak ile `await` anahtar sözcüğü, bu garanti etmez işlemi bir arka plan iş parçacığında çalışır. Bunun yerine, bu için uzun süre çalışan işlemin geçirerek gerçekleştirilebilir `Task.Run`aşağıdaki kod örneğinde gösterildiği gibi:

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

`RecognizeFace` Yöntemi ile bir arka plan iş parçacığında yürütülür `RecognizeFaceButtonClick` kadar bekleyen yöntemi `RecognizeFace` yöntemi, devam etmeden önce tamamlanır.

Uzun süren işlemlere iptal desteklemelidir. Örneğin, uzun süre çalışan bir işleme devam ederseniz uygulama içinde kullanıcı giderse gereksiz olabilir. İptal uygulamada Düzen aşağıdaki gibidir:

- Oluşturma bir `CancellationTokenSource` örneği. Bu örnek, yönetmek ve iptal bildirimleri gönderin.
- Geçirmek `CancellationTokenSource.Token` iptal edilebilir olması gereken her görev için özellik değeri.
- Her görev, iptal için yanıt vermesi için bir mekanizma sağlar.
- Çağrı `CancellationTokenSource.Cancel` iptal bildirimi sağlamak için yöntemi.

> [!IMPORTANT]
> `CancellationTokenSource` Sınıfının Implements `IDisposable` arabirimi ve bu nedenle `CancellationTokenSource.Dispose` yöntemi çağrılacak bir kez `CancellationTokenSource` örneği ile tamamlandı.



Daha fazla bilgi için [zaman uyumsuz desteğe genel bakış](~/cross-platform/platform/async.md).

<a name="sgen" />

## <a name="use-the-sgen-garbage-collector"></a>SGen atık toplayıcıyı kullanın

Artık kullanılmayan nesnelere ayrılan belleği geri kazanmak için C# kullanımı çöp toplama gibi dilleri yönetilen. Xamarin platformu tarafından kullanılan iki atık toplayıcıları şunlardır:

- [**SGen** ](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) – bu soy tabanlı atık Toplayıcıya ve varsayılan atık toplayıcı Xamarin platformu üzerinde.
- [**Boehm** ](http://www.hboehm.info/gc/) – koruyucu, olmayan-soy tabanlı atık Toplayıcıya budur. Bu, Klasik API kullanan Xamarin.iOS uygulamaları için kullanılan varsayılan çöp toplayıcı olur.

SGen nesneler için alan ayırmak için üç yığınlar birini kullanır:

-  **Çocuk odası** – küçük yeni nesnelerin ayrıldığı budur. Çocuk odası alan tükendiğinde küçük çöp toplama meydana gelir. Tüm Canlı nesnelerin büyük öbek için taşınır.
-  **Büyük yığın** – uzun süre çalışan nesneleri saklandığı budur. Ana yığınında yeterli bellek yoksa, bir ana atık toplama meydana gelir. Yeterli bellek boşaltmak bir ana atık toplama işlemi başarısız olursa SGen sistem için daha fazla bellek sorar.
-  **Büyük nesne alanı** – 8000 bayttan fazlasını gerektiren nesnelerini saklandığı budur. Büyük nesneler çocuk odası başlatılmaz, ancak bunun yerine bu yığında ayrılır.

SGen avantajlarından biri, bir küçük atık toplama işini gerçekleştirmek için gereken süreyi son küçük çöp toplama işleminden itibaren oluşturulan yeni Canlı nesne sayısını orantılı olmasıdır. Bu küçük çöp koleksiyonları ana atık toplama süreden daha kısa zamanda olacağından bu çöp toplama bir uygulama performansı üzerindeki etkisini azaltır. Ana atık toplamaları yine de gerçekleşir, ancak daha az sıklıkta.

### <a name="reducing-pressure-on-the-garbage-collector"></a>Çöp toplayıcı Basıncı azaltma

SGen atık toplama başladığında, belleği geri kazanır durumdayken uygulamanın iş parçacıkları durdurur. Bellek iadesi sırasında uygulama kısa duraklatma deneyimi veya kullanıcı Arabiriminde titremesine. Bu duraklatma nasıl algılanabilir olan iki etkenlere bağlıdır:

1. **Sıklık** – ne sıklıkta çöp toplama oluşur. Koleksiyonları arasında daha fazla bellek ayırma işlemi yapılırken atık Toplamaların sıklığını artacaktır.
1. **Süre** – her ayrı bir çöp toplama ne kadar sürer. Bu kabaca toplanmakta olan Canlı nesneleri sayısı için doğru orantılıdır.

Topluca bu anlamına birçok nesne ayrılır, ancak Canlı kalmasını değil, olacağını çok kısa bir atık toplama. Buna karşılık, yeni nesneler yavaş ayrılır ve nesneleri canlı kalmasını olacaktır daha az sayıda ancak daha uzun bir atık toplama.

Çöp toplayıcı Basıncı azaltmak için aşağıdaki yönergeleri izleyin:

- Çöp toplama sıkı Döngülerde, nesne havuzlarını kullanarak kaçının. Bu, özellikle kendi nesnelerin çoğu önceden oluşturmak için gereken oyunlar için geçerlidir.
- Artık gerekli olmayan akışlar, ağ bağlantıları, bellek ve dosyaları büyük blokları gibi kaynakları açıkça serbest bırakın. Daha fazla bilgi için [yayın IDisposable kaynakları](#idisposable).
- Artık toplanabilir nesneler yapmak için gerekli olmayan olay işleyicileri sildirebilir. Daha fazla bilgi için [olaylardan Unsubscribe](#events).

<a name="linker" />

## <a name="reduce-the-size-of-the-application"></a>Uygulamanın boyutunu azaltın

Bir uygulamanın yürütülebilir boyutu nereden geldiğini anlamak için her platformda derleme süreci anlamak önemlidir:

- iOS, tamamlanan,-ARM için derleme dili derlenmiş zamanı (AOT) uygulamalardır. .NET framework yalnızca uygun bağlayıcı seçeneği etkinse çıkarılır kullanılmayan sınıflarıyla içindedir.
- Android uygulamaları Ara dil (IL) derlenir ve MonoVM just-in-time (JIT) derleme ile paketlenir. Yalnızca uygun bağlayıcı seçeneği etkinse kullanılmayan framework sınıfları çıkarılır.
- Windows Phone uygulamaları için IL derlenir ve yerleşik çalışma zamanı tarafından yürütülür.

Ayrıca, bir uygulamanın yerel olarak içerecektir sonra son yürütülebilir boyutu daha fazla artırmak sonra genel türler kapsamlı kullanımını yaparsa sürümleri genel olasılık derlenmiş.

Uygulamaları boyutunu azaltmak için derleme Araçları'nın bir parçası bir bağlayıcı Xamarin platformunu içerir. Bağlayıcı varsayılan olarak devre dışıdır ve uygulama için proje seçeneklerinde etkinleştirilmesi gerekir. Derleme sırasında uygulama tarafından hangi türler ve üyeler, gerçekte kullanıldığını belirlemek için uygulamanın statik çözümleme gerçekleştirir. Tüm kullanılmayan türlerin ve yöntemlerin'nı, ardından uygulamadan kaldırır.

Aşağıdaki ekran görüntüsünde bağlayıcı seçenekleri Visual Studio için Mac için bir Xamarin.iOS projesi gösterir:

![](memory-perf-best-practices-images/linker-options-ios.png)

Aşağıdaki ekran görüntüsünde bağlayıcı seçenekleri Visual Studio'da Mac için bir Xamarin.Android projesi için gösterir:

![](memory-perf-best-practices-images/linker-options-droid.png)

Bağlayıcı davranışını denetlemek için üç farklı ayarları sağlar:

-  **Bağlama** – hiç kullanılmayan türlerin ve yöntemlerin bağlayıcı tarafından kaldırılır. Performansla ilgili nedenlerden dolayı hata ayıklama yapıları için varsayılan ayar budur.
-  **Bağlantı yalnızca çerçeve SDK'ları / SDK derlemeleri** – Bu ayar yalnızca Xamarin tarafından gönderilir ve bu derlemeler boyutunu azaltır. Kullanıcı kodu etkilenmeyecektir.
-  **Bağlantı tüm derlemelerin** – SDK bütünleştirilmiş kodları ve kullanıcı kodu hedeflediğini daha agresif bir iyileştirme budur. Bağlamalarda bu kullanılmayan destekleyen alanlar kaldırır ve her örneği (veya ilişkili nesneleri) yapmak daha açık, daha az bellek kullanma.

*Bağlantı tüm derlemelerin* beklenmedik bir şekilde uygulama çökebilir dikkatli kullanılmalıdır. Derlenmiş uygulamadan kaldırılmasını çok fazla kod bunun sonucunda, gerekli olan tüm kodları bağlayıcı tarafından gerçekleştirilen statik analiz doğru tanımlayamayabilir. Bu durum uygulama kilitlendiğinde kendisi yalnızca çalışma zamanında bildirilmez. Bu nedenle bağlayıcı davranışı değiştirdikten sonra bir uygulamayı sınamanız önemlidir.

Test bağlayıcı yanlış olduğunu çıkıyorsa bir sınıf veya türleri işaretlemek mümkündür yöntemi veya statik olarak başvurulan değil, ancak aşağıdaki özniteliklerden birini kullanarak uygulama için gereken yöntemleri kaldırıldı:

-  `Xamarin.iOS.Foundation.PreserveAttribute` – Xamarin.iOS projeleri için bu özniteliğidir.
-  `Android.Runtime.PreserveAttribute` – Xamarin.Android projeleri için bu özniteliğidir.

Örneğin, dinamik olarak oluşturulan tür varsayılan oluşturucular korumak gerekli olabilir. Ayrıca, XML serileştirme kullanımını türlerin özelliklerine korunur gerektirebilir.

Daha fazla bilgi için [iOS için bağlayıcı](~/ios/deploy-test/linker.md) ve [Android için bağlayıcı](~/android/deploy-test/linker.md).

### <a name="additional-size-reduction-techniques"></a>Ek boyutu azaltma teknikleri

Güç Mobil cihazların çok çeşitli CPU mimarileri vardır. Bu nedenle, Xamarin.iOS ve Xamarin.Android üretmek *fat ikili dosyaları* uygulama her CPU mimarisi için derlenmiş bir sürümünü içerir. Bu, bir mobil uygulama bir cihazda CPU mimarisi ne olursa olsun çalışabilmesini sağlar.

Aşağıdaki adımlar, daha fazla uygulama yürütülebilir boyutunu azaltmak için kullanılabilir:

- Yayın derlemesi üretilir emin olun.
- Üretilen FAT bir ikili önlemek için uygulama yerleşik mimarileri sayısını azaltın.
- LLVM derleyicisi, daha en iyi duruma getirilmiş bir yürütülebilir dosyayı oluşturmak için kullanıldığından emin olun.
- Uygulamanın yönetilen kod boyutunu küçültün. Bu bağlayıcı her derleme üzerinde etkinleştirerek gerçekleştirilebilir (*bağlantı tüm* iOS projeleri için ve *tüm bütünleştirilmiş kodları Bağla* Android projeleri için).

Android uygulamaları, her ABI için ("mimari") ayrı bir APK da bölünebilir.
Bu Web günlüğü gönderisinde daha fazla bilgi edinin: [nasıl için tutmak uygulamanızın Android uygulama boyutu aşağı](http://motzcod.es/post/112072508362/how-to-keep-your-android-app-size-down).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Resim kaynakları en iyi duruma getirme

Görüntüleri olan bazı uygulamaları kullanan en pahalı kaynaklara ve genellikle yüksek çözünürlüklerde yakalanır. Bu benzerleriyle ayrıntılarını tam oluşturur, ancak böyle görüntüler genellikle uygulama görüntü kodunu çözmek için daha fazla CPU kullanımı ve kodu çözülmüş görüntünün depolanması için daha fazla bellek gerektirir. Bunu görüntülemek için daha küçük bir boyuta ölçeğinin azaltılıp azaltılmayacağını bellekte yüksek çözünürlüklü bir görüntü çözülecek kısıp. Bunun yerine, CPU kullanımı ve bellek Ayak izi versiyonlarını çözümleme yakın tahmin edilen görüntü boyutu depolanan görüntüler oluşturarak azaltın. Örneğin, bir liste görünümünde görüntülenen görüntünün büyük olasılıkla tam ekranında görüntülenen bir görüntü daha düşük bir çözünürlüğe olmalıdır. Ayrıca, yüksek çözünürlüklü görüntülerin sürümlerin ölçeği en az bellek etkileyerek görüntüler verimli bir şekilde yüklenebilir. Daha fazla bilgi için [yük büyük bit eşlemler verimli](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently/).

Görüntü çözünürlüğü ne olursa olsun, resim kaynakları görüntüleme uygulamanın bellek Ayak izi önemli ölçüde artırabilirsiniz. Bu nedenle, bunlar yalnızca gerekli ve uygulama artık gerektirdiği hemen sonra serbest bırakılması oluşturulmalıdır.

<a name="activationperiod" />

## <a name="reduce-the-application-activation-period"></a>Uygulama etkinleştirme süresini kısaltmaya

Tüm uygulamaların bir *etkinleştirme süresi*, uygulama başlatıldığında ve uygulama kullanıma hazır olduğunda arasındaki süreyi olduğu. Uygulamanın kendi ilk izlenim kullanıcılarla bu etkinleştirme süresi sağlar ve böylece etkinleştirme süresi ve uygulamanın olumlu bir ilk izlenim elde etmek bunları sırayla, bir kullanıcının algılamasını azaltmak önemlidir.

Uygulamanın ilk kullanıcı arabirimini görüntülemeden önce uygulamanın başlatıyor kullanıcıya göstermek için giriş ekranı sağlamanız gerekir. Uygulama Hızlı Başlangıç kullanıcı arabirimini görüntüleyemiyorsanız Karşılama ekranında uygulama askıda taşınmadığından güvenceyi sunmak için kullanıcı etkinleştirme süresi boyunca ilerleme bilgilendirmek için kullanılmalıdır. Bu güvenceyi bir ilerleme çubuğu veya benzer denetimi olabilir.

Etkinleştirme süresi boyunca, uygulamaları yükleme ve kaynakların işleme genellikle içeren etkinleştirme mantığını yürütün. Etkinleştirme süresi, gerekli kaynaklara uzaktan alınmasını yerine uygulama içinde paketlenir sağlayarak azaltılabilir. Örneğin, bazı durumlarda, yerel olarak saklanan bir yer tutucu verileri yüklemek için etkinleştirme süresi sırasında uygun olabilir. Ardından, ilk kullanıcı Arabirimi görüntülenir ve kullanıcının uygulamayla etkileşim kuramayacak sonra yer tutucu veriler aşamalı bir uzak kaynaktan değiştirilebilir. Ayrıca, uygulamanın etkinleştirme mantığının yalnızca uygulama kullanmaya izin vermek için gereken iş gerçekleştirmeniz gerekir. Ek derlemeler yüklenmesi gecikmeler varsa bu kullanıldıkları ilk kez yüklenen derlemeler gibi yardımcı olabilir.

<a name="webservicecommunication" />

## <a name="reduce-web-service-communication"></a>Web hizmeti iletişimi azaltmak

Bir uygulamaya ait bir web hizmetine bağlanma, uygulama performansı üzerinde bir etkisi olabilir. Örneğin, ağ bant genişliği kullanımının yüksek artan bir cihazın pil kullanımını neden olur. Ayrıca, kullanıcılar uygulama bant genişliği sınırlı bir ortamda kullanıyor olabilir. Bu nedenle, bir uygulama ile bir web hizmeti arasında bant genişliği kullanımını sınırlamak mantıklı olur.

Bir uygulamanın bant genişliği kullanımını azaltmak için tek bir ağ üzerinden aktarılması önce verileri sıkıştırmak için bir yaklaşımdır. Ancak, ek CPU kullanımı sıkıştırma işlemi bir artan pil kullanımını da neden olabilir. Bu nedenle, bu bir tradeoff dikkatli bir şekilde bir ağ üzerinden sıkıştırılmış veri taşınıp taşınmayacağını karar vermeden önce değerlendirilmelidir.

Dikkate alınması gereken başka bir sorun, bir uygulama ve web hizmeti arasında hareket veri biçimidir. İki birincil biçimler şunlardır: Genişletilebilir Biçimlendirme Dili (XML) ve JavaScript nesne gösterimi (JSON). Çok sayıda biçimlendirme karakterleri içerdiğinden XML görece büyük veri yüklerini üreten bir metin tabanlı veri değişimi biçimidir. JSON biçimindedir kullanılmıştı azaltılmış bant genişliği gereksinimlerini veri gönderilirken sıkıştırılmış veri yüklerini üreten bir metin tabanlı veri değişimi ve veri alma. Bu nedenle, JSON, genellikle tercih edilen mobil uygulamaları biçimi değil.

Bir uygulama ve web hizmeti arasında veri aktarımı sırasında veri aktarımı nesneleri (Dto) kullanmak için önerilir. Bir ağ üzerinden aktarılması veri kümesi bir DTO içerir. Dto'lar yararlanarak daha fazla veri uygulama tarafından yapılan uzak çağrıların sayısını azaltmaya yardımcı olabilir tek bir uzak çağrıda iletilebilir. Genellikle, bir büyük veri yükünü taşıyan bir uzak çağrı benzer bir miktar süre yalnızca küçük veri yükü yürüten bir çağrı olarak alır.

Web hizmetinden alınan verileri kullanılan yerine sürekli web hizmetinden alınan önbelleğe alınmış verileri yerel olarak önbelleğe alınmalıdır. Ancak, bu yaklaşımın benimsenmesi, uygun bir önbelleğe alma stratejisi Ayrıca web hizmetinde değişirse, yerel önbellekteki verileri güncelleştirmek için uygulanmalıdır.

## <a name="summary"></a>Özet

Bu makalede açıklanan ve Xamarin platformu kullanılarak oluşturulan uygulamaların performansını artırmaya yönelik teknikleri ele alınan. Topluca bu tekniklerin bir CPU ve bir uygulama tarafından kullanılan bellek miktarı tarafından gerçekleştirilen iş miktarını önemli ölçüde azaltabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.iOS performans](~/ios/deploy-test/performance.md)
- [Xamarin.Android performans](~/android/deploy-test/performance.md)
- [Xamarin Profiler giriş](~/tools/profiler/index.md)
- [Xamarin.Forms performans](~/xamarin-forms/deploy-test/performance.md)
- [Zaman Uyumsuz Desteğe Genel Bakış](~/cross-platform/platform/async.md)
- [IDisposable](xref:System.IDisposable)
- [Xamarin uygulamaları (video) planlarken düşebileceğiniz yaygın tuzaklardan kaçınma](https://university.xamarin.com/guestlectures/avoiding-common-pitfalls-in-xamarin-apps)
