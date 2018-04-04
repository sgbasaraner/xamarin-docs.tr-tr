---
title: API tasarım
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a9c0b02457f006f75dc5b6f0a52e68865d620f67
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="api-design"></a>API tasarım


## <a name="overview"></a>Genel Bakış

Mono parçası olan bir temel sınıf kitaplıkları çekirdek yanı sıra Xamarin.Android çeşitli Android Mono ile yerel Android uygulamaları oluşturmak geliştiriciler izin vermek API'ler için bağlamaları ile birlikte gelir.

Çekirdeği Xamarin.Android vardır birlikte çalışma altyapısı bu Java world köprüleri C# dünyayla ve C# veya diğer .NET dilleri ile erişim geliştiriciler Java API sağlar.


## <a name="design-principles"></a>Tasarım ilkeleri

Bu bizim tasarım ilkeleri Xamarin.Android bağlama için bazıları

-  Uygun [.NET Framework tasarım yönergeleri](http://msdn.microsoft.com/en-us/library/ms229042.aspx).

-  Bir alt kümesi Java sınıflarını geliştiricilerin olanak sağlar.

-  Alt sınıfı, C# Standart yapıları ile çalışması gerekir.

-  Varolan bir sınıftan türetilir.

-  Zincir temel oluşturucuya çağırın.

-  Yöntemleri geçersiz kılma ile geçersiz kılma sistem C# ' ın yapılması gerekir.

-  Genel Java görevleri kolay ve sabit Java görevleri mümkün kılar.

-  JavaBean özellikleri C# özellikleri olarak kullanıma sunar.

-  Kesin türü belirtilmiş bir API ortaya çıkarır:

    - Tür güvenliği artırın.

    - Çalışma zamanı hataları en aza indirin.

    - IDE IntelliSense dönüş türlerinde alın.

    - IDE açılan belgelerine sağlar.

-  IDE araştırması API'leri teşvik edin:

    - Simge Durumuna Küçült Java Classlib Etkilenme Framework alternatifleri kullanın.

    - C# temsilciler (Lambda'lar, anonim yöntemler ve System.Delegate), uygun ve geçerli olduğunda, yerine tek yöntemi arabirimleri kullanıma sunar.

    - Rastgele Java kitaplıkları çağırmak için bir mekanizma sağlar ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).



## <a name="assemblies"></a>Derlemeler

Xamarin.Android oluşturan derlemeler çeşitli içerir *MonoMobile profil*. [Derlemeleri](~/cross-platform/internals/available-assemblies.md) sayfası, daha fazla bilgi içerir.

Android platformu için bağlamaları bulunan `Mono.Android.dll` derleme. Bu derleme Süren Android API için tüm bağlama ve Android çalışma zamanı VM ile iletişim kurarken içerir.


## <a name="binding-design"></a>Tasarım bağlama


### <a name="collections"></a>Koleksiyonlar

Android API'ları listeler, ayarlar ve haritalar kapsamlı bir şekilde sağlamak için java.util koleksiyonları kullanın. Biz kullanarak bu öğeleri kullanıma [System.Collections.Generic](https://developer.xamarin.com/api/namespace/System.Collections.Generic/) bizim bağlama arabirimleri. Temel eşlemeleri şunlardır:

-   [java.util.Set<E> ](http://developer.android.com/reference/java/util/Set.html) eşlemeleri sistem türü [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), yardımcı sınıfı [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/).

-   [java.util.List<E> ](http://developer.android.com/reference/java/util/List.html) eşlemeleri sistem türü [IList<T>](http://msdn.microsoft.com/en-us/library/5y536ey6.aspx), yardımcı sınıfı [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/).

-   [java.util.Map < K, V >](http://developer.android.com/reference/java/util/Map.html) eşlemeleri sistem türü [IDictionary < TKey, TValue >](http://msdn.microsoft.com/en-us/library/s4ys34ea.aspx), yardımcı sınıfı [Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/).

-   [java.util.Collection<E> ](http://developer.android.com/reference/java/util/Collection.html) eşlemeleri sistem türü [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), yardımcı sınıfı [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/).

Bu tür hazırlama daha hızlı copyless kolaylaştırmak için yardımcı sınıfları sağladık. Mümkün olduğunda, bu koleksiyon sağlanan framework uygulamasını yerine sağlanan kullanmanızı öneririz, ister [ `List<T>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.List%601/) veya [ `Dictionary<TKey, TValue>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%602/). [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) uygulamalarında yerel bir Java koleksiyonunu dahili olarak kullanmak ve bu nedenle ve yerel bir koleksiyondan bir Android API üyesine geçirilirken kopyalama gerektirmez.

Herhangi bir arabirim uygulaması arabirimi kabul Android bir yönteme geçirin, örneğin geçirin bir `List<int>` için [ArrayAdapter&lt;int&gt;(bağlamı, int, IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)Oluşturucusu. *Ancak*, tüm uygulamaları için *dışında* Android.Runtime uygulamalar için bu içerir *kopyalama* Android çalışma zamanı VM içine Mono VM listeden. Liste sonraki, Android çalışma zamanı içinde değiştirildi (örneğin çağırma tarafından [ArrayAdapter&lt;T&gt;. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) yöntemi), bu değişiklikleri *almayacak* yönetilen kodda görünür. Varsa bir `JavaList<int>` olan kullanıldığında, bu değişiklikleri görünür olacaktır.

Rephrased, koleksiyonları arabirim olan uygulamaları *değil* listelenen yukarıdaki birinin **yardımcı sınıfı**es yalnızca sıralama [In]:

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

Java yöntemleri özelliklerini, uygun olduğunda dönüştürülür:

-  Java yöntemi çifti `T getFoo()` ve `void setFoo(T)` içine dönüştürülmüş `Foo` özelliği. Örnek: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/).

-  Java yöntemi `getFoo()` özelliği salt okunur Foo dönüştürülür. Örnek: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/).

-  Yalnızca küme özellikleri oluşturulmaz.

-  Özellikleri *değil* özellik türü bir dizi olacaksa oluşturulur.



### <a name="events-and-listeners"></a>Olaylar ve dinleyicileri

Android API üstünde Java oluşturulur ve olay dinleyicileri takma için Java düzeni bileşenlerini izleyin. Bu desen anonim bir sınıf oluşturun ve yöntemleri geçersiz kılmak için bildirmek kullanıcının gerektiren gibi uğraş olma eğilimindedir, örneğin, bu şeyleri Android Java ile nasıl uygulanır:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

Eşdeğer olayları kullanarak C# kodunda olacaktır:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Yukarıdaki mekanizmaları her ikisi de Xamarin.Android ile kullanılabilir olduğunu unutmayın. Dinleyici arabirimini uygulayan ve View.SetOnClickListener ile ekleme ya da normal C# paradigmalarına Click olayını hiçbirini ile oluşturulan bir temsilci ekleyebilirsiniz.

Dinleyici geri çağırma yöntemi void dönüş olduğunda, temel API öğeleri oluşturuyoruz bir [EventHandler&lt;TEventArgs&gt; ](https://developer.xamarin.com/api/type/System.EventHandler%601/) temsilci. Biz bu dinleyici türleri için yukarıdaki örnek gibi bir olayı oluşturur. Ancak, dinleyicisi geri çağırma void olmayan ve olmayan döndürürse- **boolean** değeri, olaylar ve EventHandlers kullanılmaz. Biz yerine geri çağırma imza için belirli bir temsilci oluşturmak ve olayları yerine özellikleri ekleyin. Temsilci çağırma sıra ile ilgilidir ve işleme dönmek için nedenidir. Bu yaklaşım, ne Xamarin.iOS API'si ile yapılır yansıtır.

C# olayları veya özellikleri yalnızca otomatik olarak oluşturulur, Android olay kayıt yöntemi:

1. Sahip bir `set` önek, örneğin [ *ayarlamak*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/).

1. Sahip bir `void` dönüş türü.

1. Yalnızca bir parametre kabul eden parametre türü bir arabirimdir, tek bir yöntem arabirimine sahiptir ve arabirim adı ile biten `Listener` , örneğin [View.OnClick *dinleyici*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/).


Dinleyici yöntemi arabirim, ayrıca, bir dönüş türü **boolean** yerine **void**, ardından oluşturulan *EventArgs* alt sınıfı, bir içerecek*İşlenmiş* özelliği. Değeri *işlenmiş* özelliği, dönüş değeri olarak kullanılır *dinleyici* yöntemi ve varsayılan olarak `true`.

Örneğin, Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) yöntemi kabul [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) arabirimi ve [View.OnKeyListener.onKey (Görünüm, int, KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) yöntemi, boolean bir dönüş türüne sahip. Xamarin.Android oluşturur karşılık gelen [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) olan olay bir [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/).
*KeyEventArgs* sınıfı sırayla sahip bir [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) dönüş değeri olarak kullanılan özellik *View.OnKeyListener.onKey()* yöntemi.

Biz diğer yöntemleri ve oluşturucuları temsilci tabanlı bağlantı kullanıma sunmak için aşırı eklemek istiyorsunuz. Ayrıca, birden çok geri aramalar sahip dinleyiciler bunlar tanımlandığı gibi bu dönüştürme şekilde tek tek geri aramalar uygulama makul, olup olmadığını belirlemek için bazı ek denetleme gerektirir. Karşılık gelen bir olay ise dinleyicileri C# ' ta kullanılması gerekir, ancak lütfen uygulamamızla temsilci kullanımını olabilir düşündüğünüz herhangi getirin. Bir temsilci alternatif yararlanabileceğini açıktı zaman biz de arabirimlerinin "Dinleyicisi" soneki olmayan bazı dönüşümleri yaptınız.

Tüm dinleyicileri arabirimlerin uygulamak [ `Android.Runtime.IJavaObject` ](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) dinleyicisi sınıflar bu arabirimi uygulamalıdır şekilde bağlamanın uygulama ayrıntıları nedeniyle arabirimi. Bu dinleyici arabirimi öğesinin bir alt uygulama tarafından yapılabilir [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) veya diğer bir Android etkinliği gibi Java nesne Sarmalanan.


### <a name="runnables"></a>Runnables

Java yararlanan [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) bir temsilci seçme düzeneğini sağlamak için arabirim. [Java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) bu arabirimin önemli bir tüketici bir sınıftır. Android API de arabiriminde işe.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) ve [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) önemli örnektir.

`Runnable` Arabirimi içeren tek bir void yöntemi [run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/). Bu nedenle kendisini C# bağlama için uygundur bir [System.Action](http://msdn.microsoft.com/en-us/library/system.action.aspx) temsilci. Bağlama içinde kabul aşırı sağladık bir `Action` parametresi tüketen tüm API üyeler için bir `Runnable` yerel API'sindeki örneğin [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) ve [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

Biz sol [IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) çeşitli türleri arabirimini uygulayan ve bu nedenle için bunları değiştirme yerine yerinde aşırı bayraklarıdır runnables doğrudan.


### <a name="inner-classes"></a>İç sınıflar

Java sahip iki farklı türde [iç içe sınıflar](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): statik sınıflar ve statik olmayan sınıfları iç içe geçmiş.

Java statik iç içe geçmiş sınıflar, C# iç içe geçmiş türler için aynıdır.

Statik olmayan iç içe sınıflar olarak da adlandırılan, *iç sınıfları*, önemli ölçüde farklıdır. Bunlar, kendilerini kapsayan türle örneğine dolaylı bir başvuru içeriyor ve statik üyeler (arasında diğer farklar bu genel bakışta kapsamı dışında) içeremez.

Bağlama ve C# kullanımı söz konusu olduğunda, statik iç içe geçmiş sınıflar normal iç içe geçmiş türler kabul edilir. İç sınıflar, bu sırada, iki önemli farklılıklar vardır:

1. Örtük kapsayan tür referansı açıkça Oluşturucusu parametre olarak sağlanması gerekir.

1. Bir iç sınıftan iç sınıfı devralırken *gerekir* iç içe içinde bir tür temel iç sınıfı içeren türünden devralan ve türetilmiş bir tür olarak C# aynı türde bir oluşturucu türünü içeren sağlamanız gerekir.


Örneğin, göz önünde bulundurun [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) iç sınıf. Bir iç sınıf olduğundan [WallpaperService.Engine() Oluşturucusu](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) bir başvuru alır bir [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) örneği (karşılaştırır ve buna karşılık, Java [WallpaperService.Engine () ) Oluşturucusu](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) hiçbir parametre alır).

Bir iç sınıf örneği türevi CubeWallpaper.CubeEngine şöyledir:

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

Not nasıl `CubeWallpaper.CubeEngine` içinde iç içe `CubeWallpaper`, `CubeWallpaper` içeren sınıfından devralan `WallpaperService.Engine`, ve `CubeWallpaper.CubeEngine` bildiri türü--alan bir kurucusunun `CubeWallpaper` bu durumda--tüm olarak yukarıda belirtilen.


### <a name="interfaces"></a>Arabirimler

Java arabirimleri ikisi C# ' dan sorunlara yol üyelerinin üç adet içerebilir:

1. Yöntemler

1. Türler

1. Alanlar


Java arabirimleri iki türlerine dönüştürülür:

1. Yöntem bildirimleri içeren (isteğe bağlı) bir arabirim. Bu arabirim, Java arabirimi olarak aynı ada sahip *dışında* de sahip bir ' *ı* ' öneki.

1. Herhangi bir alan içeren bir (isteğe bağlı) statik sınıf Java arabirimde bildirildi.

İç içe geçmiş türler "eşdüzey kapsayan arabirim adı öneki olarak ile iç içe geçmiş türler yerine kapsayan arabiriminin olacak şekilde yerleştirilir".

Örneğin, göz önünde bulundurun [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) arabirimi.
*Parcelable* arabirim yöntemleri, iç içe geçmiş türler ve sabitleri içerir. *Parcelable* arabirim yöntemleri içine yerleştirilir [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) arabirimi.
*Parcelable* arabirimi sabitleri içine yerleştirilir [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) türü. İç içe [android.os.Parcelable.ClassLoaderCreator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) ve [android.os.Parcelable.Creator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.Creator.html) türleridir şu anda değil genel türler desteği hizmetimizi; sınırlamaları nedeniyle bağlı desteklenen ise, bunlar olarak mevcut olabilir *Android.OS.IParcelableClassLoaderCreator* ve *Android.OS.IParcelableCreator* arabirimleri. Örneğin, iç içe geçmiş [android.os.IBinder.DeathRecpient](http://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) arabirimi bağlı olarak [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) arabirimi.


> [!NOTE]
> Xamarin.Android 1.9 ile başlayarak, Java arabirimi sabittir <em>yinelenen</em> Java taşıma basitleştirmek için kod. Bu dayanan taşıma Java kod geliştirmeye yardımcı [android sağlayıcısı](http://developer.android.com/reference/android/provider/package-summary.html) sabitleri arabirim.

Yukarıdaki türlerine ek olarak dört başka değişiklikler vardır:

1. Java arabirimi aynı ada sahip bir türü sabitleri içerecek şekilde oluşturulur.

1. Arabirim sabitleri de içeren türleri uygulanan Java arabirimlerinden gelen tüm sabitleri içerir.

1. Sabitler içeren bir Java arabirimini uygulayan tüm sınıflar tüm uygulanan arabirimlerinden sabitleri içeren yeni bir iç içe geçmiş InterfaceConsts tür alın.

1. *Consts* türü kullanılmıyor şimdi.


İçin *android.os.Parcelable* arabirimi, yani şimdi olacağı bir [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) türü sabitleri içerir. Örneğin, [Parcelable.CONTENTS_FILE_DESCRIPTOR](http://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) sabiti bağlı olarak [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) sabit yerine olarak *ParcelableConsts.ContentsFileDescriptor* sabit.

İçeren diğer arabirimleri uygulayan sabitleri henüz daha fazla sabitleri içeren arabirimler için tüm sabit birleşimi şimdi oluşturulur. Örneğin, [android.provider.MediaStore.Video.VideoColumns](http://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) arabirimini uygulayan [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) arabirimi. Ancak, 1.9 önce [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) türüne sahip üzerinde bildirilen sabitleri erişme hiçbir şekilde [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/).
Sonuç olarak, Java ifade *MediaStore.Video.VideoColumns.TITLE* C# ifade bağlanması gereken *MediaStore.Video.MediaColumnsConsts.Title* olmadan okuma Bul zor olduğu çok sayıda Java belgeleri. 1.9 içinde eşdeğer C# ifade olacaktır [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/).

Ayrıca, göz önünde bulundurun [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) Java uygulayan türü *Parcelable* arabirimi. Arabirimini uygulayan olduğundan, bu arabirimdeki tüm sabit örneğin paket türü "ile" erişilebilir *Bundle.CONTENTS_FILE_DESCRIPTOR* mükemmel geçerli bir Java ifadesidir.
Daha önce bu deyim için C# bağlantı noktası için hangi türünden görmek için uygulanan tüm arabirimler bakmak gerekir *CONTENTS_FILE_DESCRIPTOR* nereden geldiğini. Xamarin.Android 1.9 başlayarak, sabitleri içeren Java arabirimler uygulama sınıfları iç içe bir olacaktır *InterfaceConsts* tüm devralınan arabirimi sabitleri içerir türü. Bu çevirme sağlayacak *Bundle.CONTENTS_FILE_DESCRIPTOR* için [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/).

Son olarak, ile türleri bir *Consts* gibi soneki *Android.OS.ParcelableConsts* kullanılmaz, yeni sunulan InterfaceConsts dışında türlerin olan. Xamarin.Android 3. 0'kaldırılır.


## <a name="resources"></a>Kaynaklar

Görüntüleri, Düzen açıklamaları, ikili BLOB'ları ve dize sözlükleri dahil edilebilir olarak [kaynak dosyaları](http://developer.android.com/guide/topics/resources/providing-resources.html).
Çeşitli Android API için tasarlanmış [kaynak kimlikleri çalışması](http://developer.android.com/guide/topics/resources/accessing-resources.html) görüntüleri postalarla yerine, dize veya ikili BLOB'ların doğrudan.

Bir kullanıcı arabirimi düzeni içeren Örneğin, örnek bir Android uygulaması ( `main.axml`), bir Uluslararası tablo dizesi ( `strings.xml`) ve bazı simgeleri ( `drawable-*/icon.png`) kaynaklarıyla uygulamasının "Kaynaklar" dizininde engelleneceği:

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

Yerel Android API'ları doğrudan dosya adları ile çalıştırmayın, ancak bunun yerine kaynak kimlikleri çalışmayabilir. Kaynakları kullanan bir Android uygulamasını derlerken, derleme sistem kaynakları için dağıtım paketini ve adlı bir sınıf oluşturun `Resource` bulunan kaynakların her biri için belirteçleri içerir. Örneğin, yukarıdaki kaynakları düzeni için bu R sınıfı kullanıma.

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

Ardından kullanırsınız `Resource.Drawable.icon` başvuru `drawable/icon.png` dosyası veya `Resource.Layout.main` başvuru `layout/main.xml` dosya, veya `Resource.String.first_string` sözlük dosyasını ilk dizesinde başvurmak için `values/strings.xml`.


## <a name="constants-and-enumerations"></a>Sabitler ve Numaralandırmalar

Yerel Android API alın veya int anlamını belirlemek için sabit bir alan eşlenmelidir int'e döndüren birçok yöntem vardır. Bu yöntemleri kullanmak için kullanıcının hangi sabitleri uygun değerleri değerinden ideal olduğu görmek için belgelere için gereklidir.

Örneğin, göz önünde bulundurun [Activity.requestWindowFeature (int Featureıd)](http://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

Bu durumda, biz ilgili sabitleri .NET numaralandırması gruplamak ve numaralandırma yerine yapılacak yöntemi yeniden eşlemek çaba.
Bunu yaparak, biz IntelliSense seçimi olası değerlerin sunmak kullanabilirsiniz.

Yukarıdaki örnek olur: [Activity.RequestWindowFeature (WindowFeatures Featureıd)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)).

Bu hangi sabitleri birlikte ait çıkışı bulmak için çok elle yapılan bir işlemdir ve bu sabitleri hangi API'lerini geliştirmenize unutmayın. Lütfen daha iyi bir sabit olarak ifade edilen olacak API'sindeki hatalar sabitleri kullanım için dosya.
