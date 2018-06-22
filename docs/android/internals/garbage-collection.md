---
title: Çöp Toplama
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 49bb340edcad0c5ce39a2d9db6da72d488a114b1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772974"
---
# <a name="garbage-collection"></a>Çöp Toplama

Xamarin.Android Mono'nın kullandığı [basit kişinin atık toplayıcı](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/). İki nesil ile işareti tarama atıktoplayıcı budur ve *büyük nesne alanı*, iki tür koleksiyonların ile: 

-   Alt koleksiyonlar (toplar Gen0 yığın) 
-   Ana koleksiyonları (Gen1 toplar ve büyük nesne yığın alanı). 

> [!NOTE]
> Bir açık koleksiyonuyla olmadığında [GC. Collect()](xref:System.GC.Collect) koleksiyonlarıdır *isteğe bağlı*, yığın ayırmaları göre. *Bu başvuru sistemi sayım değil*; nesneleri *bekleyen başvuru vardır hemen toplanmaz*, veya ne zaman bir kapsam çıkış yaptı. Yeni Ayırma için bellek yetersiz küçük yığın çalıştırıldığında GC çalıştırın. Hiçbir ayırmaları varsa, çalışmaz.


Alt koleksiyonlar ucuz ve sık ve son ayrılmış ve kullanılmayan nesneler toplamak için kullanılır. Alt koleksiyonlar ayrılmış nesneleri sonra her birkaç MB gerçekleştirilir. Alt koleksiyonlar el ile yapılmalıdır çağırarak [GC. TOPLA (0)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

Ana koleksiyonları pahalı ve daha az sıklıkta ve tüm ölü nesneleri geri kazanmak için kullanılır. Bellek için geçerli yığın boyutu (öbek yeniden boyutlandırma önce) bitti sonra ana koleksiyonları gerçekleştirilir. Ana koleksiyonlar el ile yapılmalıdır çağırarak [GC. TOPLA ()](xref:System.GC.Collect) veya çağırarak [GC. (İnt) toplamak](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) bağımsız değişkeniyle [GC. MaxGeneration](xref:System.GC.MaxGeneration). 



## <a name="cross-vm-object-collections"></a>Çapraz VM nesne koleksiyonları

Nesne türleri üç kategoriye ayrılır.

-   **Yönetilen nesneler**: yapmak türlerini *değil* devralınmalıdır [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) , örneğin [System.String](xref:System.String). 
    Bu, normal olarak GC tarafından toplanır. 

-   **Java nesnelerini**: Android çalışma zamanı VM içinde var ancak Mono VM'ye gösterilmeyen Java türü. Bunlar sıkıcı ve daha ayrıntılı ele olmaz. Bu, normal olarak Android çalışma zamanı VM tarafından toplanır. 

-   **Eş nesneleri**: türleri uygulayan [IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) , örneğin tüm [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) ve [Java.Lang.Throwable](https://developer.xamarin.com/api/type/Java.Lang.Throwable/) alt sınıflar. Bu tür örnekleri sahip iki "halfs" bir *yönetilen eş* ve *yerel eş*. Yönetilen eş C# sınıfının bir örneğidir. Yerel eş Android çalışma zamanı VM içinde Java sınıfı ve C# örneği olan [IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) özelliği yerel eş JNI genel başvuru içeriyor. 


Yerel eş iki tür vardır:

-   **Framework eş** : hiçbir şey Xamarin.Android, örneğin bilmek "Normal" Java türleri [android.content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/).

-   **Kullanıcı eş** : [Android aranabilir sarmalayıcılar](~/android/platform/java-integration/working-with-jni.md) , uygulama içinde mevcut her bir Java.Lang.Object alt derleme zamanında oluşturulur.


Xamarin.Android işlemi içinde iki VM olarak çöp koleksiyonları iki tür vardır:

-   Android çalışma zamanı koleksiyonları 
-   Mono koleksiyonları 

Android çalışma zamanı koleksiyonları çalışması genellikle ancak bir uyarı ile: JNI genel başvuru GC kök olarak kabul edilir. Genel bir JNI ise sonuç olarak, VM nesnesini Android bir çalışma zamanı bulunduran başvuru *olamaz* toplanmasını, aksi takdirde koleksiyonu için uygun olsa bile.

Burada fun olur Mono koleksiyonlarıdır. Yönetilen nesneler normal olarak toplanır. Eş nesneleri, aşağıdaki işlemi gerçekleştirerek toplanır:

1.  Tüm eş nesneleri Mono koleksiyonu için uygun bir JNI zayıf genel başvurusuyla değiştirilir kendi JNI genel başvuru yapıyor. 

2.  Bir Android çalışma zamanı VM GC çağrılır. Herhangi bir yerel eş örneği toplanabilir. 

3.  (1) oluşturduğunuz JNI zayıf genel başvurular denetlenir. Zayıf başvuru toplanan, eş nesne toplanır. Zayıf başvuru varsa *değil* toplanan, ardından zayıf başvuru JNI genel başvurusuyla değiştirilir ve eş nesne alınamadı. Not: Üzerinde API 14+, döndürülen değer buna `IJavaObject.Handle` sonra bir GC değişebilir. 

Tüm budur tarafından başvuruluyor sürece eş nesnesinin örneği Canlı nihai sonucu yönetilen kod (örn. depolanan bir `static` değişkeni) veya Java kodu tarafından başvurulan. Ayrıca, yerel eş ömrü ne aksi yaptıkları ötesinde uzatılır Canlı yerel eş ve yönetilen eş collectible kadar yerel eş collectible olmayacak şekilde.


## <a name="object-cycles"></a>Nesne döngüsü

Eş nesneleri ve Android çalışma zamanı ve Mono VM'in içinde mantıksal olarak mevcuttur. Örneğin, bir [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) yönetilen eş örneği, karşılık gelen olacaktır [android.app.Activity](http://developer.android.com/reference/android/app/Activity.html) framework eş Java örneği. Öğesinden devralan tüm nesneler [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) Beyanları her iki VM içinde olması bekleniyor. 

Her iki VM gösterimi olan tüm nesneler yalnızca tek bir VM içinde mevcut olan nesneler karşılaştırılan genişletilmiş yaşam süresi vardır (aşağıdaki gibi bir [ `System.Collections.Generic.List<int>` ](xref:System.Collections.Generic.List%601)). Çağırma [GC. Toplama](xref:System.GC.Collect) toplama önce ya da VM tarafından nesne başvurulan olduğunu değil emin olmak Xamarin.Android GC gereksinimleriniz değiştikçe bu nesneler mutlaka toplamaz. 

Nesne ömrü kısaltmak için [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) çağrılmalıdır. Bu el ile "böylece daha hızlı toplanacak nesneleri sağlar genel başvuru boşaltma tarafından iki VM'ler arasında nesne bağlantısında sever". 


## <a name="automatic-collections"></a>Otomatik koleksiyonları

İle başlayarak [sürüm 4.1.0'da](https://developer.xamarin.com/releases/android/mono_for_android_4/mono_for_android_4.1.0), Xamarin.Android gref eşiği aşıldığında, tam bir GC otomatik olarak gerçekleştirir. Bu eşik % 90 ' platform için bilinen en fazla grefs ise: 1800 grefs (en fazla 2000) öykünücüsü üzerinde ve 46800 grefs donanımda (en fazla 52000). *Not:* Xamarin.Android yalnızca sayar tarafından oluşturulan grefs [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)ve işlem içinde oluşturulduğu grefs hakkında bilmez. Bir buluşsal yöntem budur *yalnızca*. 

Bir otomatik olarak toplanmasını gerçekleştirildiğinde, aşağıdakine benzer bir ileti hata ayıklama günlüğü basılır:

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

Bu örneğini belirleyici değildir ve (örneğin ortasında grafik işleme) çıkarsanız zamanlarda ortaya çıkabilir. Bu iletiyi görmesini, başka bir yerde açık bir koleksiyonu gerçekleştirmek istediğinizi düşünelim ya da denemek istiyor [eş nesnelerin ömrü azaltmak](#Helping_the_GC). 

## <a name="gc-bridge-options"></a>GC köprüsü seçenekleri

Xamarin.Android Android ve Android çalışma zamanı ile saydam bellek yönetimi sunar. Adlı Mono atık toplayıcı bir uzantısı olarak uygulanan *GC köprüsü*. 

Mono çöp toplama ve hangi eş nesneleri gerekir "Android çalışma zamanı yığınla doğrulandı kendi liveness" rakamları sırasında GC köprüsü çalışır. GC Köprüsü (sırayla) aşağıdakileri yaparak bu belirlemeyi yapar:

1.  Mono başvuru grafiğin ulaşılamaz eş nesnelerin Java nesnelerini temsil ettikleri anlamına. 

2.  Java GC gerçekleştirin.

3.  Hangi nesnelerin gerçekten ölü olduğundan emin olun. 

Bu karmaşık ne alt sınıflarının sağlayan işlemdir `Java.Lang.Object` serbestçe referansı nesnelerin; üzerinde Java nesnelerini bağlanabilir C# için herhangi bir kısıtlamanın kaldırır. Bu karmaşıklığı nedeniyle köprüsü işlemi çok pahalı olabilir ve bir uygulamada belirgin duraklatır neden olabilir. Uygulama önemli duraklatır yaşanıyorsa, aşağıdaki üç GC köprüsü uygulamaları birini araştırma değerinde şöyledir: 

-   **Tarjan** -GC köprüsü tamamen yeni bir tasarım dayalı [Robert Tarjan'ın algoritması ve geriye doğru yayma başvuru](http://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm).
    Bizim sanal iş yüklerinin altında en iyi performansı sahiptir, ancak Deneysel kodunun büyük paylaşım da sahiptir. 

-   **Yeni** -ikinci derece davranışı iki örneğini düzelttikten ancak çekirdek algoritması koruyarak özgün kodun önemli onarımı (temel [Kosaraju'nın algoritması](http://en.wikipedia.org/wiki/Kosaraju's_algorithm) kesinlikle bulma bağlı bileşenler için). 

-   **Eski** -(en çok üç kararlı kabul) özgün uygulaması. Bir uygulama kullanmalısınız köprüsü budur `GC_BRIDGE` duraklatır kabul edilebilir. 


Hangi GC köprüsü çalışır en iyi şekilde tek bir uygulamada denemek ve çıktı çözümleme yoludur. Değerlendirmesi için veri toplamak üzere iki yolu vardır: 

-   **Günlük kaydını etkinleştir** -günlük kaydını etkinleştir (içinde açıklanan şekilde [yapılandırma](~/android/internals/garbage-collection.md) bölüm) her GC köprüsü seçeneği için sonra yakalamak ve günlük çıktısı her ayarından karşılaştırın. İnceleme `GC` iletileri her seçeneği; özellikle, `GC_BRIDGE` iletileri. Etkileşimli olmayan uygulamalar tolerable, ancak duraklatır 60ms çok etkileşimli uygulamalar (örneğin, oyunlar) için yukarıda bir sorun için en fazla 150ms duraklatır. 

-   **Köprü hesap etkinleştir** -köprüsü hesap köprüsü işleminde yer alan her bir nesne gösterdiği nesnelerin ortalama maliyetini görüntüler. Bu bilgiler boyutuna göre sıralama ek nesneleri en fazla basılı tutarak konusunda ipuçları sağlar. 


Belirtmek için `GC_BRIDGE` seçeneği uygulamanın bize gerekir, geçirin `bridge-implementation=old`, `bridge-implementation=new` veya `bridge-implementation=tarjan` için `MONO_GC_PARAMS` ortamı değişkeni, örneğin: 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

Varsayılan ayar **Tarjan**. Bir gerileme bulursanız, onu bu seçeneği ayarlamak için gerekli bulabilirsiniz **eski**. Ayrıca, daha fazla kararlı kullanmayı seçebilirsiniz **eski** varsa seçeneği **Tarjan** bir geliştirme performans üretmez. 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>GC yardımcı olma

Bellek kullanım ve toplama zamanı azaltmak için GC yardımcı olmak için birden çok yolu vardır.



### <a name="disposing-of-peer-instances"></a>Eş örneklerini atma

GC tamamlanmamış bir görünüme sahiptir çalışmıyor olabilir ve işlem GC belleğin bilmiyor olduğundan bellek düşük olduğunda düşük olur. 

Örneğin, bir örneğini bir [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) türü veya türetilmiş bir tür ise en az 20 bayt boyutunda (dikkat edin, vb. olmadan değiştirilebilir vs.). 
[Aranabilir sarmalayıcılar yönetilen](~/android/internals/architecture.md) ek örnek üyeler, bu nedenle eklemeyin olduğunda bir [Android.Graphics.Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) için bellek, 10 MB blob başvuran örnek Xamarin.Android'ın GC, bilmeniz olmaz &ndash; GC 20 baytlık nesne görür ve 10 MB bellek canlı tutma Android çalışma zamanı ayrılan nesnelere bağlı olduğunu belirlemek mümkün olmayacaktır. 

GC yardımcı olmak sık gereklidir. Ne yazık ki, *GC. AddMemoryPressure()* ve *GC. RemoveMemoryPressure()* desteklenmez, dolayısıyla, *bilmeniz* ihtiyacınız olabilecek el ile çağırmak için bir Java ayrılmış büyük nesne grafiği yalnızca serbest [GC. Collect()](xref:System.GC.Collect) istemi Java tarafı serbest bırakmak için bir GC bellek veya açıkça elden *Java.Lang.Object* alt sınıfların, yönetilen aranabilir sarmalayıcısı Java örneği arasında eşleme kesiliyor. Örneğin, [hata 1084](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6). 


> [!NOTE]
> Olmalıdır *son derece* atma zaman dikkatli `Java.Lang.Object` alt sınıf örnekleri.

Bellek Bozulması olasılığını en aza indirmek için aşağıdaki kılavuzları çağrılırken gözlemlemek `Dispose()`.


#### <a name="sharing-between-multiple-threads"></a>Çoklu iş parçacıkları arasında paylaşma

Varsa *Java veya yönetilen* örneği birden çok iş parçacıkları arasında paylaşılan *olmamalıdır `Dispose()`d*, **hiç**. Örneğin, [ `Typeface.Create()` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/(System.String%2cAndroid.Graphics.TypefaceStyle)) döndürebilir bir *önbelleğe alınmış örneğini*. Birden çok iş parçacığı aynı bağımsız değişkenlere sağlarsanız, alacak *aynı* örneği. Sonuç olarak, `Dispose()`, lık `Typeface` örnek bir iş parçacığından diğerine sonuçlanabilir diğer iş parçacığı geçersiz `ArgumentException`s'den `JNIEnv.CallVoidMethod()` (diğerlerinin yanı sıra) örneği başka bir iş parçacığından bırakıldı çünkü. 


#### <a name="disposing-bound-java-types"></a>İlişkili Java türleri atma

İlişkili bir Java türü örneğiyse örneği, dönüşüme gönderilebilir *sürece* örneği yönetilen koddan yeniden olmaz *ve* Java örneği (bkz. önceki işparçacıklarıarasındapaylaşılamaz`Typeface.Create()` tartışma). (Bu belirlemeyi yapmak zor olabilir.) Java örneği girer sonraki zaman yönetilen kod, bir *yeni* için sarmalayıcı oluşturulur. 

Bu, genellikle Drawables ve diğer kaynak ağır örnekleri söz konusu olduğunda yararlıdır:

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

Yukarıdaki güvenlidir çünkü eş, [Drawable.CreateFromPath()](https://developer.xamarin.com/api/member/Android.Graphics.Drawables.Drawable.CreateFromPath/) döndürür Framework eşler arası başvurmak *değil* kullanıcı eş. `Dispose()` Çağrısı sonunda `using` engelleme, yönetilen arasındaki ilişkiyi bozar [Drawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.Drawable/) ve framework [Drawable](http://developer.android.com/reference/android/graphics/drawable/Drawable.html) örnekleri, Java örneği olması izin verme Android çalışma zamanı gerekir hemen toplanır. Bu misiniz *değil* kullanıcı eşler arası eş örneği başvurulan güvenli; burada biz "dış" bilgi kullanmakta olduğunuz *bilmeniz* , `Drawable` kullanıcı eşler arası başvuruda bulunamaz ve bu nedenle `Dispose()` çağırın güvenli değildir. 


#### <a name="disposing-other-types"></a>Diğer türleri atma 

Örnek bir Java türündeki bağlama olmayan bir türe başvuruda bulunuyorsa (gibi özel bir `Activity`), **yok** çağrısı `Dispose()` sürece, *bilmeniz* Java kod geçersiz kılınan yöntemleri üzerinde çağıracaktır örneği. Bunun Sağlanamaması sonuçlanıyor [ `NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls). 

Örneğin, varsa dinleyicisi özel tıklatın:

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

*Vermemelisiniz* Java gelecekte dayanarak yöntemleri çağırmak çalışacak şekilde bu örneği dispose:

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>Özel durumlar önlemek için açık denetimlerini kullanma

Uygulanırsa bir [Java.Lang.Object.Dispose](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose(System.Boolean)/) yöntemi aşırı yükleme, JNI ile ilgili nesneleri temas kaçının. Bunun yapılması oluşturabilir bir *çift dispose* (fatally) kodunuzu için mümkün kılar durum çöpünün toplanma zaten yapılmış bir Java nesnesini erişim dener. Bunun yapılması, aşağıdakine benzer bir özel durum oluşturur: 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

Bu durum genellikle bir nesnenin ilk dispose üyesi null hale gelmesine neden olur ve bir özel durum bu null üye sonraki erişim denemede neden olan oluşur. Özellikle, nesnenin `Handle` (hangi yönetilen bir örneği temel Java örneğine bağlar) ilk silinmek üzere geçersiz ancak yönetilen kod dakikasında artık kullanılabilir (bakın olmasınarağmenbutemelJavaörneğineerişmek[ Yönetilen aranabilir sarmalayıcılar](~/android/internals/architecture.md#Managed_Callable_Wrappers) Java örnekleri ve yönetilen örnekleri arasında eşleme hakkında daha fazla bilgi için). 

Açıkça doğrulamak için bu özel durumun önlemek için en iyi yolu olduğundan, `Dispose` yönetilen örneğiyle temel Java örneği arasında eşleme olan; geçerliliğinin yöntemi, olmadığını görmek için onay nesnenin `Handle` null (`IntPtr.Zero`) üyelerine erişmeden önce. Örneğin, aşağıdaki `Dispose` yöntemi erişimleri bir `childViews` nesnesi: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);
        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

İlk dispose nedenler geçirirseniz `childViews` geçersiz olmasını `Handle`, `for` döngü erişim throw bir `ArgumentException`. Açık bir ekleyerek `Handle` null ilk önce onay `childViews` erişim, aşağıdaki `Dispose` yöntemi özel durum gerçekleşmesini engeller: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);

        // Check for a null handle:
        if (this.childViews.Handle == IntPtr.Zero)
            return;

        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```


### <a name="reduce-referenced-instances"></a>Başvurulan örnekleri azaltın

Örneği her bir `Java.Lang.Object` türü veya alt sınıfın tüm GC sırasında taranan *Nesne grafiği* örneğinin başvurduğu de taranan gerekir. Nesne grafiği "kök örneğine" başvurduğu nesne örneklerini kümesidir *artı* hangi kök örneği tarafından başvurulan her şeyi, özyinelemeli başvuruyor. 

Aşağıdaki sınıf göz önünde bulundurun:

```csharp
class BadActivity : Activity {

    private List<string> strings;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

Zaman `BadActivity` olan oluşturulan, Nesne grafiği 10004 örnekleri içerecek (1 x `BadActivity`, 1 x `strings`, 1 x `string[]` tutulan `strings`, string örneklerini x 10000), *tüm* hangi olması gerekir, Her taranan `BadActivity` örneği taranan. 

Bu detrimental etkileri olabilir, koleksiyon saatlerinin GC Duraklat kez artan sonuçlanır. 

GC tarafından Yardım *azaltma* kullanıcı eş örnekleri tarafından kökü nesne grafiklerinin boyutu. Yukarıdaki örnekte, bu taşıyarak yapılabilir `BadActivity.strings` Java.Lang.Object devralmaz ayrı bir sınıf içine: 

```csharp
class HiddenReference<T> {

    static Dictionary<int, T> table = new Dictionary<int, T> ();
    static int idgen = 0;

    int id;

    public HiddenReference ()
    {
        lock (table) {
            id = idgen ++;
        }
    }

    ~HiddenReference ()
    {
        lock (table) {
            table.Remove (id);
        }
    }

    public T Value {
        get { lock (table) { return table [id]; } }
        set { lock (table) { table [id] = value; } }
    }
}

class BetterActivity : Activity {

    HiddenReference<List<string>> strings = new HiddenReference<List<string>>();

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```


## <a name="minor-collections"></a>Alt koleksiyonlar

Alt koleksiyonlar el ile yapılmalıdır çağırarak [GC. Collect(0)](xref:System.GC.Collect). Alt koleksiyonlar (ana koleksiyonlarına kıyasla) ucuz, ancak önemli bir maliyet, çok sık tetiklemek istemediğiniz için sabit olması ve birkaç milisaniye bir duraklatma süresi olması gerekir. 

"Aynı şeyi tekrar tekrar yapılır çevrimi" uygulamanız varsa, iş hacmi sona erdikten sonra el ile SPN'i küçük bir koleksiyon önerilir olabilir. Örnek vergi döngüleri şunları içerir: 

-  Tek bir oyun çerçeve işleme döngüsü.
-  (Açma, kapatma doldurma) belirli bir uygulamanın iletişim tüm etkileşim 
-  Yenileme/uygulama verilerini eşitleme için ağ isteklerini grubudur.



## <a name="major-collections"></a>Ana koleksiyonları

Ana koleksiyonlar el ile yapılmalıdır çağırarak [GC. Collect()](xref:System.GC.Collect) veya `GC.Collect(GC.MaxGeneration)`. 

Bunlar nadiren gerçekleştirilmelidir ve ikinci bir duraklatma süresi, 512 MB öbek toplarken bir stil Android cihazına sahip olabilir. 

Ana koleksiyonlar yalnızca el ile varsa, her zamankinden çağrılması: 

-   Uzun vergi sonunda döngüleri ve ne zaman bir uzun duraklatmak bir sorun kullanıcıya sunmak olmaz. 

-   Geçersiz kılınan bir içinde [Android.App.Activity.OnLowMemory()](https://developer.xamarin.com/api/member/Android.App.Activity.OnLowMemory/) yöntemi. 



## <a name="diagnostics"></a>Tanılamalar

Genel başvuru oluşturduğunuzda ve yok izlemek için ayarlayabileceğiniz [debug.mono.log](~/android/troubleshooting/index.md) sistem özelliğini içerecek şekilde [ *gref* ](~/android/troubleshooting/index.md) ve/veya [ *gc*](~/android/troubleshooting/index.md). 



## <a name="configuration"></a>Yapılandırma

Xamarin.Android atık toplayıcı ayarlanarak yapılandırılabilir `MONO_GC_PARAMS` ortam değişkeni. Ortam değişkenlerini yapı eylemi ayarlama [AndroidEnvironment](~/android/deploy-test/environment.md).

`MONO_GC_PARAMS` Ortam değişkeni aşağıdaki parametreleri virgülle ayrılmış bir listesi verilmiştir: 

-   `nursery-size` = *boyutu* : çocuk odası boyutunu ayarlar. Boyutunu bayt cinsinden belirtilir ve iki gücünü olması gerekir. Sonekleri `k` , `m` ve `g` kilo - mega - ve gigabayt cinsinden sırasıyla belirtmek için kullanılır. Çocuk odası birinci nesil (iki) olur. Daha büyük bir çocuk odası programı genellikle hızlandırır ancak tabii daha fazla bellek kullanır. Varsayılan çocuk odası boyutu 512 kb. 

-   `soft-heap-limit` = *boyutu* : hedef en fazla yönetilen uygulama için bellek tüketimi. Bellek kullanımını belirtilen değerin altında olduğunda, GC yürütme süresi (daha az koleksiyonu) için optimize edilmiştir. 
    Bu sınır GC bellek kullanımı (daha fazla koleksiyonu) için optimize edilmiştir. 

-   `evacuation-threshold` = *Eşik* : Tahliye eşiğini yüzde cinsinden ayarlar. Değeri 0-100 aralığında bir tamsayı olmalıdır. Varsayılan değer 66 ' dir. Koleksiyon tarama aşaması belirli yığın blok türü doluluğu bu yüzdeden küçük olduğunu bulursa, böylece doluluk için yüzde 100'e yakın geri yükleme, blok türü sonraki ana koleksiyonundaki kopyalama toplamalarında yapın. 0 değeri Tahliye devre dışı bırakır. 

-   `bridge-implementation` = *Uygulama köprü* : Bu işlem performans sorunlarını gidermek GC yardımcı olmak için GC köprüsü seçeneği ayarlar. Üç olası değerler: *eski* , *yeni* , *tarjan*.

-   `bridge-require-precise-merge`: İlk çöp hale geldikten sonra köprüsü nadir durumlarda, bir nesne olarak neden olabilecek bir iyileştirme içerir Tarjan bir GC toplanır. Bu seçenek de dahil olmak üzere GC'ler potansiyel olarak daha yavaş ancak daha tahmin edilebilir yapma, en iyi duruma getirme, devre dışı bırakır.

Örneğin, bir yığın boyut sınırı 128 MB olmasını GC yapılandırmak için yeni bir dosya ile projenize ekleyin bir **yapı eylemi** , `AndroidEnvironment` içeriğiyle: 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
