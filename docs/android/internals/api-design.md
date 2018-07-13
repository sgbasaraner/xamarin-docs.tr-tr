---
title: Xamarin.Android API tasarım ilkeleri
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 8abb78f335b159223e9394b7845eccbba8d124da
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996353"
---
# <a name="xamarinandroid-api-design-principles"></a>Xamarin.Android API tasarım ilkeleri


## <a name="overview"></a>Genel Bakış

Mono parçası olan bir temel sınıf kitaplıkları çekirdeğin yanı sıra Xamarin.Android çeşitli Android geliştiricileri, Mono ile yerel Android uygulamaları oluşturmak izin vermek API'ler için bağlamaları ile birlikte gelir.

Çekirdek xamarin.Android var. bir birlikte çalışma altyapısı, köprüleri C# Dünyası Java dünya çapında ve geliştiriciler C# veya diğer .NET dilleri ile erişim için Java API sağlar.


## <a name="design-principles"></a>Tasarım ilkeleri

Xamarin.Android bağlama için tasarım ilkelerimizde bazıları şunlardır

-  Uygun [.NET Framework tasarım yönergeleri](https://docs.microsoft.com/dotnet/standard/design-guidelines/).

-  Alt Java sınıfları geliştiricilerin olanak tanır.

-  Öğesinin alt sınıfı, C# Standart yapıları ile çalışması gerekir.

-  Varolan bir sınıftan türetilen.

-  Çağrı zincirinin için temel oluşturucu.

-  C# ' nin geçersiz kılma sistemiyle yöntemleri geçersiz yapılmalıdır.

-  Sık kullanılan Java görevleri kolay ve sabit Java görevleri mümkün kılar.

-  JavaBean özellikleri, C# özellikleri olarak kullanıma sunar.

-  Türü kesin belirlenmiş bir API kullanıma sunar:

    - Tür güvenliği artırın.

    - Çalışma zamanı hataları en aza indirin.

    - IDE IntelliSense dönüş türlerinde alın.

    - IDE açılan belgelerine sağlar.

-  IDE içinde araştırma API'leri önerilir:

    - Simge Durumuna Küçült Java Classlib Etkilenme Framework alternatifleri kullanın.

    - C# temsilciler (lambda ifadeleri, anonim metotlar ve System.Delegate), uygun ve uygun olduğunda, yerine tek yöntemi arabirimleri kullanıma sunar.

    - İsteğe bağlı Java kitaplıkları çağırmak için bir mekanizma sağlar ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).


## <a name="assemblies"></a>Derlemeleri

Xamarin.Android oluşturan derlemeleri içeren *MonoMobile profili*. [Derlemeleri](~/cross-platform/internals/available-assemblies.md) sayfası, daha fazla bilgi içerir.

Android platformu için bağlamaları bulunan `Mono.Android.dll` derleme. Bu derleme tüm bağlamayı kullanan Android API'leri ve Android çalışma zamanı VM ile iletişim kurulurken içerir.


## <a name="binding-design"></a>Tasarım bağlama


### <a name="collections"></a>Koleksiyonlar

Android API'lerine java.util koleksiyonları listeler, ayarlar ve eşlemeler kapsamlı bir şekilde sağlamak için kullanır. Kullanarak bu öğeleri kullanıma sunuyoruz [System.Collections.Generic](xref:System.Collections.Generic) bizim bağlama arabirimleri. Temel eşlemeleri şunlardır:

-   [java.util.Set<E> ](http://developer.android.com/reference/java/util/Set.html) eşler sistem türü [ICollection<T>](xref:System.Collections.Generic.ICollection`1), yardımcı sınıf [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/).

-   [java.util.List<E> ](http://developer.android.com/reference/java/util/List.html) eşler sistem türü [IList<T>](xref:System.Collections.Generic.IList`1), yardımcı sınıf [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/).

-   [< K, V > java.util.Map](http://developer.android.com/reference/java/util/Map.html) eşler sistem türü [IDictionary < TKey, TValue >](xref:System.Collections.Generic.IDictionary`2), yardımcı sınıf [Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/).

-   [java.util.Collection<E> ](http://developer.android.com/reference/java/util/Collection.html) eşler sistem türü [ICollection<T>](xref:System.Collections.Generic.ICollection`1), yardımcı sınıf [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/).

Bu tür hazırlama daha hızlı copyless kolaylaştırmak için yardımcı sınıfları sağladık. Mümkün olduğunda, bu koleksiyon sağlanan framework uygulamasını yerine sağlanan kullanmanızı öneririz, ister [ `List<T>` ](xref:System.Collections.Generic.List`1) veya [ `Dictionary<TKey, TValue>` ](xref:System.Collections.Generic.Dictionary`2). [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) uygulamaları dahili olarak yerel bir Java koleksiyon yazılımınız ve bu nedenle ve yerel bir koleksiyondan bir Android API üyesine geçerken kopyalama gerektirmez.

Herhangi bir arabirim uygulaması arabirimi kabul eden bir Android yönteme geçirin, örneğin başarılı bir `List<int>` için [ArrayAdapter&lt;int&gt;(bağlam, int, IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)Oluşturucusu. *Ancak*, tüm uygulamalar için *dışında* Android.Runtime uygulamaları için bu içerir *kopyalama* Android çalışma zamanı VM Mono VM'ye listeden. Liste sonraki, Android çalışma zamanı içinde değiştirildi (çağırarak örn [ArrayAdapter&lt;T&gt;. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) yöntemi), bu değişiklikleri *yapmamayı* yönetilen kodda görülebilir. Varsa bir `JavaList<int>` olan kullanıldığında, bu değişiklikleri görünür olur.

İşlemindeki, koleksiyonları arabirim olan uygulamaları *değil* Yukarıdakilerden biri listelenen **yardımcı sınıf**es yalnızca Hazırlama [In]:

```csharp
// This fails:
var badSource  = new List<int> { 1, 2, 3 };
var badAdapter = new ArrayAdapter<int>(context, textViewResourceId, badSource);
badAdapter.Add (4);
if (badSource.Count != 4) // true
    throw new InvalidOperationException ("this is thrown");

// this works:
var goodSource  = new JavaList<int> { 1, 2, 3 };
var goodAdapter = new ArrayAdapter<int> (context, textViewResourceId, goodSource);
goodAdapter.Add (4);
if (goodSource.Count != 4) // false
    throw new InvalidOperationException ("should not be reached.");
```


### <a name="properties"></a>Özellikler

Java metotları, özellikleri, uygun olduğunda dönüştürülür:

-  Java yöntemi çifti `T getFoo()` ve `void setFoo(T)` olarak dönüştürülür `Foo` özelliği. Örnek: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/).

-  Java yöntemi `getFoo()` salt okunur Foo özelliğe dönüştürülür. Örnek: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/).

-  Yalnızca küme özellikleri oluşturulmaz.

-  Özellikleri *değil* olursa özellik türü bir dizi oluşturulur.



### <a name="events-and-listeners"></a>Olayları ve dinleyicileri

Android API'ları üzerinde Java yerleşiktir ve bileşenleri olay dinleyicileri ' takma için Java desenini izler. Bu düzen, kullanıcının anonim bir sınıf oluşturun ve yöntemleri geçersiz kılmak için bildirmek gerektirdiği uğraşmayı eğilimindedir, örneğin, bu Java ile Android şeyleri nasıl uygulanır:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

C# olayları kullanarak eşdeğer kod şu şekilde olur:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Her ikisi de yukarıdaki mekanizmaları ile Xamarin.Android kullanılabilir olduğunu unutmayın. Dinleyici arabirimini uygulayan ve View.SetOnClickListener ile eklemek veya bir temsilci yoluyla herhangi bir normal C# paradigmalarını tıklama olayı için oluşturulan ekleyebilirsiniz.

Dinleyici geri çağırma yöntemi void dönüş olduğunda, temel API öğeleri oluştururuz. bir [EventHandler&lt;TEventArgs&gt; ](xref:System.EventHandler`1) temsilci. Biz bu dinleyici türleri için yukarıdaki örnekte olduğu gibi bir olay oluşturur. Ancak, dinleyici geri çağırma void olmayan ve dışı döndürürse- **Boole** değer, olayları ve EventHandlers kullanılmaz. Biz bunun yerine geri çağırma imzası için belirli bir temsilci oluşturmak ve olayları yerine özellikleri ekleyin. Temsilci çağırma siparişle işlem ve işleme döndürmek için kullanılan nedenidir. Bu yaklaşım, Xamarin.iOS API ile yapılır yansıtır.

C# olaylar veya özellikler yalnızca otomatik olarak oluşturulur, Android olay kaydı yöntemi:

1. Sahip bir `set` önek, örneğin [ *ayarlamak*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/).

1. Sahip bir `void` dönüş türü.

1. Yalnızca bir parametreyi kabul eden parametre türü bir arabirim olduğundan, yöntemi yalnızca bir arabirimi vardır ve arabirim adı ile biten `Listener` , örneğin [View.OnClick *dinleyici*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/).


Dinleyici arabirim yöntemi, ayrıca, bir dönüş türü olan **Boole** yerine **void**, ardından oluşturulan *EventArgs* öğesinin alt sınıfı bir içerecek*İşlenmiş* özelliği. Değerini *işlenmiş* özelliği, dönüş değeri olarak kullanılır *dinleyici* yöntemi varsayılan olarak `true`.

Örneğin, Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) yöntemi kabul [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) arabirimi ve [View.OnKeyListener.onKey (Görünüm, int, KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) yöntemi, bir boolean dönüş türüne sahip. Xamarin.Android oluşturur karşılık gelen [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) olan olay bir [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/).
*KeyEventArgs* sırayla sınıfında bir [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) dönüş değeri olarak kullanılan özellik *View.OnKeyListener.onKey()* yöntemi.

Aşırı yüklemeler için diğer yöntemleri ve oluşturucuları temsilci tabanlı bağlantı kullanıma sunmak için eklemek istediğiniz. Ayrıca, birden çok geri çağırmaları dinleyiciyle tanımlandıkları gibi bu dönüştürme için tek tek geri çağırmaları uygulama makul, olup olmadığını belirlemek için bazı ek incelemesi gerekir. Karşılık gelen bir olay varsa, dinleyicileri C# ' de kullanılmalıdır, ancak lütfen uygulamamızla temsilci kullanımına sahip düşündüğünüz herhangi getirin. Temsilci alternatif bir avantaj elde edecektir açık olduğunda bazı dönüştürmeleri "Dinleyici" soneki olmadan arabirimlerin uyguladığımız da.

Tüm dinleyici arabirimleri uygulayan [ `Android.Runtime.IJavaObject` ](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) dinleyici sınıfları bu arabirimi uygulamalıdır. Bu nedenle bağlama uygulama ayrıntıları nedeniyle arabirimi. Bu dinleyici arabirimi öğesinin üzerinde uygulayarak yapılabilir [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) veya diğer sarmalanmış Android etkinliği gibi Java nesne.


### <a name="runnables"></a>Runnables

Java kullanan [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) bir temsilci seçme düzeneğini sağlamak için arabirim. [Java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) sınıfı, bu arabirimin önemli bir tüketici. Android API de arabiriminde işe.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) ve [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) önemli verilebilir.

`Runnable` Arabirimi içeren tek bir void yöntemi [uygulamasındaki run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/). Bu nedenle kendi C# bağlamaya uygundur bir [System.Action](xref:System.Action) temsilci. Kabul aşırı bağlamasında sağladık bir `Action` parametresi, tükettikleri tüm API üyeleri için bir `Runnable` yerel API örn [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) ve [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

Kaldığımız [IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) aşırı birden fazla arabirim uygular ve bu nedenle için olduğundan, bunları değiştirme yerine yerinde geçirilecek runnables doğrudan.


### <a name="inner-classes"></a>İç sınıflar

Java sahip iki farklı türde [iç içe sınıflar](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): statik sınıflar ve statik olmayan sınıf iç içe geçmiş.

Java statik iç içe geçmiş sınıflar için C# iç içe geçmiş türleri aynıdır.

Statik olmayan iç içe sınıfları olarak da bilinir, *iç sınıfları*, önemli ölçüde farklıdır. Bunlar, kendilerini kapsayan türle örneğini örtük bir başvuru içerir ve statik üyeleri (arasında başka farklılıklar bu genel bakışta kapsamı dışında) içeremez.

Statik iç içe geçmiş sınıflar, bağlama ve C# kullanımı söz konusu olduğunda, normal bir iç içe geçmiş türler kabul edilir. İç sınıflar, bu arada, iki önemli farklılıkları da vardır:

1. Örtük bir başvuru içeren türe açıkça bir oluşturucu parametresi sağlanmalıdır.

1. Bir iç sınıftan ise iç sınıfa devralınırken *gerekir* iç içe geçmiş bir tür içinde olabilir içeren temel bir iç sınıf türünden devralan ve türünü içeren bir oluşturucu olarak C# aynı türden türetilmiş bir tür sağlamanız gerekir.


Örneğin, düşünün [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) iç sınıf. Bir iç sınıf olduğundan [WallpaperService.Engine() Oluşturucusu](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) başvuru alan bir [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) örneği (karşılaştırın ve Java'yı [WallpaperService.Engine ( ) Oluşturucusu](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) herhangi bir parametre alır).

Bir iç sınıfın bir örneği türetme CubeWallpaper.CubeEngine şöyledir:

```csharp
class CubeWallpaper : WallpaperService {
        public override WallpaperService.Engine OnCreateEngine ()
        {
                return new CubeEngine (this);
        }

        class CubeEngine : WallpaperService.Engine {
                public CubeEngine (CubeWallpaper s)
                        : base (s)
                {
                }
        }
}
```

Not nasıl `CubeWallpaper.CubeEngine` içinde iç içe `CubeWallpaper`, `CubeWallpaper` içeren sınıfından devralan `WallpaperService.Engine`, ve `CubeWallpaper.CubeEngine` bildirim türü--alan bir oluşturucu sahip `CubeWallpaper` bu durumda--olarak yukarıda belirtilen tüm.


### <a name="interfaces"></a>Arabirimler

Java arabirimleri üç adet ikisi C# ' tan sorunlara üyeleri içerebilir:

1. Yöntemler

1. Türler

1. Alanlar


Java arabirimleri, iki tür olarak çevrilir:

1. Yöntem bildirimleri içeren (isteğe bağlı) bir arabirim. Bu arabirim, Java arabirimi olarak aynı ada sahip *dışında* de sahip bir ' *miyim* ' öneki.

1. Herhangi bir alan içeren bir (isteğe bağlı) statik sınıf içinde Java arabirimini bildirilir.

İç içe türler "öneki olarak kapsayan arabirimi ada sahip iç içe geçmiş türler yerine kapsayan arabirimi eşdüzeyi olarak konumlandırılır".

Örneğin, düşünün [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) arabirimi.
*Parcelable* arabirim yöntemleri, iç içe geçmiş türler ve sabitler içerir. *Parcelable* arabirim yöntemleri içine yerleştirilir [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) arabirimi.
*Parcelable* arabirimi sabitleri içine yerleştirilir [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) türü. İç içe [android.os.Parcelable.ClassLoaderCreator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) ve [android.os.Parcelable.Creator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.Creator.html) türleridir şu anda değil genel türler desteğimiz; sınırlamaları nedeniyle bağlı Bunlar desteklendi, bunlar gibi olur *Android.OS.IParcelableClassLoaderCreator* ve *Android.OS.IParcelableCreator* arabirimleri. Örneğin, iç içe geçmiş [android.os.IBinder.DeathRecpient](http://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) arabirimi bağlı olarak [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) arabirimi.


> [!NOTE]
> Xamarin.Android 1.9 ile başlayarak, Java arabirimi sabittir <em>yinelenen</em> Java taşıma basitleştirmek amacıyla kod. Bu dayanan taşıma Java kod artırmaya yardımcı olur [android sağlayıcısı](http://developer.android.com/reference/android/provider/package-summary.html) arabirim sabitler.

Yukarıdaki türlerine ek olarak, dört daha fazla değişiklik vardır:

1. Java arabirimi aynı ada sahip bir türü sabitleri içerecek şekilde oluşturulur.

1. Arabirimi sabitleri de içeren türler uygulanan Java arabirimlerden gelen tüm sabit bulunur.

1. Sabitler içeren bir Java arabirimi uygulayan tüm sınıflar sabitlerinden tüm uygulanan arabirimleri içeren yeni bir iç içe geçmiş InterfaceConsts türü alın.

1. *Maliyet* türü artık kullanılmayan sunuldu.


İçin *android.os.Parcelable* arabirimi, yani artık olacağına dair bir [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) türü sabitleri içerir. Örneğin, [Parcelable.CONTENTS_FILE_DESCRIPTOR](http://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) sabiti bağlı olarak [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) sabit yerine olarak *ParcelableConsts.ContentsFileDescriptor* sabit.

İçeren diğer arabirimleri uygulayan sabitleri henüz sabit içeren arabirimleri için tüm sabit birleşimi artık oluşturulur. Örneğin, [android.provider.MediaStore.Video.VideoColumns](http://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) arabirimi uygulayan [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) arabirimi. 1.9, ancak önce [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) türünde bildirilmiş sabitleri erişmenin bir yolu yoktur [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/).
Sonuç olarak, Java ifade *MediaStore.Video.VideoColumns.TITLE* C# ifadesi bağlanması gereken *MediaStore.Video.MediaColumnsConsts.Title* okuma olmadan bulmak zor olduğu Java belgeleri çok sayıda. 1.9 içinde eşdeğer C# ifadesi olacaktır [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/).

Ayrıca, göz önünde bulundurun [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) Java uygulayan türü *Parcelable* arabirimi. Arabirimi uygulayan olduğundan, bu arabirimdeki tüm sabit örn paket türü "ile" erişilebilir *Bundle.CONTENTS_FILE_DESCRIPTOR* mükemmel geçerli bir Java ifadesidir.
Daha önce bağlantı noktası şu ifade olarak C#, hangi türünden görmek için uygulanan tüm arabirimleri bakmak gerekir *CONTENTS_FILE_DESCRIPTOR* geldi. Xamarin.Android 1.9 içinde itibaren sabitleri içeren Java arabirimleri uygulayan sınıflar iç içe bir olacaktır *InterfaceConsts* türü, devralınan arabirimi sabitleri içerir. Bu çevirme sağlayacak *Bundle.CONTENTS_FILE_DESCRIPTOR* için [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/).

Son olarak, ile türleri bir *maliyet* gibi soneki *Android.OS.ParcelableConsts* eski, yeni tanıtılan InterfaceConsts dışındaki türleri iç içe olan. Xamarin.Android 3. 0'kaldırılacak.


## <a name="resources"></a>Kaynaklar

Görüntü, Düzen açıklamaları, ikili BLOB'ları ve dize sözlükleri dahil edilebilir olarak [kaynak dosyaları](http://developer.android.com/guide/topics/resources/providing-resources.html).
Çeşitli Android API için tasarlanmış [kaynak kimliklerini çalışması](http://developer.android.com/guide/topics/resources/accessing-resources.html) görüntüleri uğraşmanızı yerine, dize veya ikili blobları doğrudan.

Örneğin, bir kullanıcı arabirimi düzeni içeren örnek Android uygulamasını ( `main.axml`), uluslararası duruma getirme tablo dizisi ( `strings.xml`) ve bazı simgeleri ( `drawable-*/icon.png`) kaynaklarıyla uygulamasının "Kaynaklar" dizininde engelleneceği:

    Resources/
        drawable-hdpi/
            icon.png

        drawable-ldpi/
            icon.png

        drawable-mdpi/
            icon.png

        layout/
            main.axml

        values/
            strings.xml

Yerel Android API'lerini doğrudan dosya adları ile çalıştırmayın, ancak bunun yerine kaynak kimliklerini üzerinde çalışır. Kaynakları kullanan bir Android uygulaması derlerken, derleme sistemi kaynaklar için dağıtım paketini ve adlı bir sınıf oluşturmak `Resource` bulunan kaynakların her biri için belirteçleri içerir. Örneğin, yukarıdaki kaynakları düzeni için bu R sınıfı kullanıma.

```csharp
public class Resource {
    public class Drawable {
        public const int icon = 0x123;
    }

    public class Layout {
        public const int main = 0x456;
    }

    public class String {
        public const int first_string = 0xabc;
        public const int second_string = 0xbcd;
    }
}
```

Daha sonra kullanacağınız `Resource.Drawable.icon` başvurmak için `drawable/icon.png` dosyası veya `Resource.Layout.main` başvuru `layout/main.xml` dosyası veya `Resource.String.first_string` sözlüğü dosyasında ilk dize başvurmak için `values/strings.xml`.


## <a name="constants-and-enumerations"></a>Sabitler ve Numaralandırmalar

Yerel Android API'ları almak veya int anlamını belirlemek için sabit bir alana eşlenmiş bir int döndüren birçok yöntem vardır. Bu yöntemleri kullanmak için kullanıcının hangi sabitleri küçüktür ideal olduğu uygun değerler olduğunu görmek için belgelere için gereklidir.

Örneğin, düşünün [Activity.requestWindowFeature (int Featureıd)](http://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

Bu durumlarda, biz ilgili sabitlerinin bir .NET numaralandırma gruplamak ve bunun yerine sabit listesi almak için yöntemi yeniden eşlemek çaba.
Bunu yaparak olası değerlerin IntelliSense seçimi hizmetlerimiz duyuyoruz.

Yukarıdaki örnek olur: [Activity.RequestWindowFeature (WindowFeatures Featureıd)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)).

Hangi sabitleri birlikte ait kullanıma bulmak için oldukça manuel bir işlem olduğunu unutmayın ve hangi API'ler bu sabiti kullanma. Lütfen, daha iyi bir sabit olarak ifade edilen olacak API'sindeki hatalar sabitleri kullanım için dosya.
