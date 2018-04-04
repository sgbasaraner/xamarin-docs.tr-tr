---
title: Etkinlik yaşam döngüsü
description: Android uygulamaları temel yapı bloğu etkinliklerin olduğundan ve farklı durumlara sayısında bulunabilir. Etkinlik yaşam döngüsü örneklemesi ile başlar ve yok etme ile sona erer ve birçok durumları arasında içerir. Bir etkinlik durumu değiştiğinde, uygun yaşam döngüsü olay yöntemi, Yaklaşan durum değişikliği etkinliği bildiren ve bu değişikliği uyarlamak için kod yürütmesine izin veren adı verilir. Bu makalede etkinlikleri yaşam döngüsü inceler ve Sorumluluk açıklar bir etkinlik her iyi çalışan, güvenilir bir uygulamanın parçası olarak bu durum değişiklikleri sırasında vardır.
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: f35f3e59d8b669795ade3d370894e45866cea1ff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="activity-lifecycle"></a>Etkinlik yaşam döngüsü

_Android uygulamaları temel yapı bloğu etkinliklerin olduğundan ve farklı durumlara sayısında bulunabilir. Etkinlik yaşam döngüsü örneklemesi ile başlar ve yok etme ile sona erer ve birçok durumları arasında içerir. Bir etkinlik durumu değiştiğinde, uygun yaşam döngüsü olay yöntemi, Yaklaşan durum değişikliği etkinliği bildiren ve bu değişikliği uyarlamak için kod yürütmesine izin veren adı verilir. Bu makalede etkinlikleri yaşam döngüsü inceler ve Sorumluluk açıklar bir etkinlik her iyi çalışan, güvenilir bir uygulamanın parçası olarak bu durum değişiklikleri sırasında vardır._

## <a name="activity-lifecycle-overview"></a>Etkinlik yaşam döngüsüne genel bakış

Android için belirli bir olağan dışı programlama kavram etkinliklerdir. Geleneksel uygulama geliştirme genellikle uygulamayı başlatmak için yürütülen statik main yöntemi yoktur. Android ile ancak farklı noktalardır; Android uygulamaları, uygulama içinde herhangi bir kayıtlı etkinliği aracılığıyla başlatılabilir. Uygulamada, çoğu uygulama yalnızca uygulama giriş noktası olarak belirtilen belirli bir etkinliğe sahip olur. Ancak, uygulama çökerse veya sonlandırılır işletim sistemi tarafından işletim Sisteminin en son açık etkinlik uygulama veya başka bir yerde önceki etkinlik yığın içinde yeniden başlatmayı deneyin.
Ayrıca, işletim sistemi bunlar etkin değilken duraklatma etkinliklerini ve bellek yetersiz olması durumunda bunları geri. Dikkat edin, bir etkinlik, özellikle de veri önceki etkinliklerden etkinlik bağımlı yeniden gerektiğinde, durumunu doğru şekilde geri yüklemek uygulama izin verecek şekilde yapılmalıdır.

Etkinlik yaşam döngüsü, bir etkinlik yaşam döngüsü boyunca yöntemleri OS çağrıları koleksiyonu olarak uygulanır. Bu yöntemler, geliştiricilerin uygulamalarını durumu ve kaynak yönetimi gereksinimlerini karşılamak için gerekli olan işlevselliği uygulamak olanak sağlar.

Etkinlik yaşam döngüsü tarafından kullanıma sunulan hangi yöntemlerin uygulanması gerektiğini belirlemek için her etkinlik gereksinimlerini çözümlemek uygulama geliştiricisi için son derece önemlidir. Bunun Sağlanamaması, uygulama kararsızlığına, kilitlenme, kaynak oluşan şişirmeyi ve muhtemelen temel işletim sistemi kararsızlığı neden olabilir.

Bu bölümde ayrıntılı, etkinlik yaşam döngüsü inceler dahil olmak üzere:

-  Etkinlik durumları
-  Yaşam döngüsü yöntemleri
-  Bir uygulamanın durumunu korur


Bu bölümde ayrıca içeren bir [izlenecek yol](~/android/app-fundamentals/activity-lifecycle/saving-state.md) durumu etkinlik yaşam döngüsü sırasında verimli bir şekilde kaydetme konusunda pratik örnekler sağlayan. Bu bölümün sonunda tarafından etkinlik yaşam döngüsü ve bir Android uygulamasını desteklemek nasıl bir anlamış olmanız gerekir.

## <a name="activity-lifecycle"></a>Etkinlik yaşam döngüsü

Android etkinlik yaşam döngüsü, bir kaynak yönetimi çerçevesiyle Geliştirici sağlamak etkinliği sınıfında kullanıma sunulan bir yöntem koleksiyonu oluşur. Bu çerçeve, geliştiricilerin uygulamanın içindeki her etkinliği benzersiz durum yönetimi gereksinimlerini karşılamak ve düzgün şekilde kaynak yönetimini işlemesine olanak sağlar.

### <a name="activity-states"></a>Etkinlik durumları

Android işletim sistemi kendi durumuna bağlı etkinlikler istemlerde. Bu, artık bellek ve kaynakları geri kazanmak işletim sistemi izin vererek kullanımda olan etkinliklerini belirlemek Android yardımcı olur. Aşağıdaki diyagram, bir etkinlik yaşam süresi boyunca Git durumlarını gösterir:

[![Etkinlik durumları diyagramı](images/image1-sml.png)](images/image1.png#lightbox)

Bu durumlar 4 ana gruplar halinde şu şekilde ayrılabilir:

1.  *Etkin ya da çalışan* &ndash; etkinlikleri kabul etkin veya ön planda olmaları durumunda çalıştıran etkinlik yığının en üst da bilinir. Bu Android en yüksek öncelik etkinliğinde olarak kabul edilir ve bu nedenle yalnızca olağanüstü durumlarda, işletim sistemi tarafından sonlandırılacak, daha fazla bellek kullanmak etkinlik çalışırsa gibi bu UI yanıt vermemesine neden olabileceğinden gibi cihaz üzerinde kullanılabilir.

1.  *Duraklatılmış* &ndash; Aygıt uyku moduna veya bir etkinlik hala görünür, ancak yeni, tam boyutlu olmayan veya saydam etkinliği tarafından kısmen gizli etkinlik olarak kabul edilir duraklatıldı. Duraklatılmış etkinlikleri hala etkin olan, bunlar tüm durumu ve üye bilgilerini korur ve Pencere Yöneticisi eklenen kalır. Bu ikinci olarak kabul edilir en yüksek öncelik etkinlik Android ve şekilde yalnızca sonlandırılacak işletim sistemi tarafından bu etkinliği sonlandırılması etkin/çalışan etkinlik kararlı ve esnek tutmanızı için gereken kaynak gereksinimlerini yerine getirecek durumunda.

1.  *Durdurulmuş Backgrounded* &ndash; başka bir etkinlik tarafından tamamen yapılabileceği etkinlikleri durdurulmuş veya arka planda değerlendirilir.
    Durdurulan etkinlikleri olası ancak durdurulmuş etkinlikler üç durumdan en düşük öncelik olduğu kabul edilir ve işletim sistemi etkinlikleri ilk kaynak karşılamak için bu durumda, bu nedenle KILL sürece, bunların durumunu ve üye bilgilerini korumak hala deneyin. daha yüksek öncelik etkinlikleri gereksinimleri.

1.  *Yeniden* &ndash; herhangi bir yerde bulunan bir etkinlik Android tarafından bellekten kaldırılacak yaşam döngüsü için durdurulmuş duraklatıldı için mümkündür. Kullanıcı geri giderse başlatılmalıdır, etkinlik, daha önce kaydedilen bir duruma geri ve kullanıcıya gösterilir.


### <a name="activity-re-creation-in-response-to-configuration-changes"></a>Yapılandırma değişiklikleri yanıtta etkinlik yeniden oluşturma

Daha karmaşık önemlidir yapmak için Android yapılandırma değişikliklerini adlı karışımında bir daha fazla İngiliz anahtarı oluşturur. Yapılandırma değişiklikleri olan bir etkinlik yapılandırma değiştiğinde aygıt olduğunda gibi oluşan hızlı etkinliği yok etme/yeniden-creation döngüleri [Döndürülmüş](~/android/app-fundamentals/handling-rotation.md) (ve etkinlik yatay veya dikey yeniden oluşturulması gerekiyor Mod) klavye görüntülenir (ve etkinlik kendisini yeniden boyutlandırmak için fırsat ile sunulan olduğunda), ya da, diğerlerinin yanı sıra bir yerleştirme cihaz yapıldığında.

Yapılandırma değişiklikleri hala bir etkinlik durdurup sırasında ortaya çıkabilecek aynı etkinlik durumu değişiklikleri neden. Ancak, bir uygulama yanıt hissettirir ve iyi yapılandırma değişiklikleri sırasında gerçekleştirir emin olmak için bunlar mümkün olan en kısa sürede yapılması önemlidir. Bu nedenle, Android durumu sırasında yapılandırma değişiklikleri kalıcı hale getirmek için kullanılan belirli bir API vardır.
Daha sonra bu konuda şu konulara değineceğiz [durumu ömrü boyunca yönetme](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle) bölümü.

### <a name="activity-lifecycle-methods"></a>Etkinlik yaşam döngüsü yöntemleri

Android SDK ve uzantıya göre Xamarin.Android framework sağlayan güçlü bir model bir uygulama içinde etkinliklerin durumunu yönetmek için. Bir etkinliğin durumunu değiştirirken, etkinlik bu etkinlik belirli yöntemlerini çağıran işletim sistemi tarafından bildirilir. Aşağıdaki diyagram ilişkide etkinlik yaşam döngüsü için bu yöntemleri gösterir:

[![Etkinlik yaşam döngüsü akış çizelgesi](images/image2-sml.png)](images/image2.png#lightbox)

Bir geliştirici olarak, bir etkinlik bu yöntemi geçersiz kılarak durum değişiklikleri işleyebilir. Ancak, tüm yaşam döngüsü yöntemleri kullanıcı Arabirimi iş parçacığı üzerinde olarak adlandırılır ve yeni etkinlik, vb. görüntüleme geçerli etkinlik gizleme gibi kullanıcı Arabirimi iş, sonraki parçası gerçekleştirmeyi OS engeller unutmayın önemlidir. Bu nedenle, bu yöntemleri kodda düzgün çalışıp eşitleyerek uygulama yapmak mümkün olduğunca kısa olmalıdır. Tüm uzun süre çalışan görevler arka plan iş parçacığında yürütülmelidir.

Her biri bu yaşam döngüsü yöntemleri ve bunların kullanılması inceleyelim:

#### <a name="oncreate"></a>OnCreate

[OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) bir etkinlik oluşturulduğunda çağrılacak ilk yöntemdir.
`OnCreate` her zaman bir etkinlik tarafından gibi gerekli olabilecek tüm başlangıç başlatmaları gerçekleştirmek için geçersiz kılınır:

-  Görünümler oluşturma
-  Değişkenleri başlatma
-  Statik verileri listelerine bağlama


`OnCreate` alan bir [paket](https://developer.xamarin.com/api/type/Android.OS.Bundle/) depolamak ve paket null değilse etkinlikler arasında durum bilgilerini ve nesneleri geçirme için bir sözlük olan parametre, bu gösterir etkinlik yeniden başlatmak ve onun durumundan geri yüklemeniz gerekir önceki örneği. Aşağıdaki kod paketindeki değerleri almak nasıl gösterir:

```csharp
protected override void OnCreate(Bundle bundle)
{
   base.OnCreate(bundle);

   string intentString;
   bool intentBool;

   if (bundle != null)
   {
      intentString = bundle.GetString("myString");
      intentBool = bundle.GetBoolean("myBool");
   }

   // Set our view from the "main" layout resource
   SetContentView(Resource.Layout.Main);
}
```

Bir kez `OnCreate` sahip tamamlanmış, Android çağıracak `OnStart`.

#### <a name="onstart"></a>OnStart

[OnStart](https://developer.xamarin.com/api/member/Android.App.Activity.OnStart/) her zaman sistem tarafından çağrılan `OnCreate` tamamlandı. Bir etkinlik etkinlik görünümlerinin geçerli değerlerini yenileme gibi görünür duruma gelmesi herhangi belirli görevleri sağ yapmanız gerekiyorsa etkinlikler bu yöntemi geçersiz kılabilir. Android çağıracaktır `OnResume` hemen sonra bu yöntem.

#### <a name="onresume"></a>OnResume

Sistem çağrıları [OnResume](https://developer.xamarin.com/api/member/Android.App.Activity.OnResume/) etkinlik olduğunda kullanıcıyla etkileşim başlatmaya hazır.
Etkinlikler gibi görevleri gerçekleştirmek için bu yöntemin üzerine yazması gerekir:

-  Kare hızları (oyun yapı ortak bir görevde) ramping
-  Animasyon başlangıç
-  GPS güncelleştirmeleri dinleme
-  Herhangi bir ilgili uyarıları veya iletişim kutularını görüntüleme
-  Dış olay işleyicilerini wire


Örnek olarak, aşağıdaki kod parçacığını kamera başlatma gösterilmektedir:

```csharp
public void OnResume()
{
    base.OnResume(); // Always call the superclass first.

    if (_camera==null)
    {
        // Do camera initializations here
    }
}
```

`OnResume` herhangi bir işlem olduğu için önemlidir yapılmış `OnPause` de beklemediğiniz Bitti'yi olmalıdır `OnResume`, sonra yürütülecek garanti yalnızca yaşam döngüsü yöntemi olduğundan `OnPause` etkinlik geri yaşama getirilirken.

#### <a name="onpause"></a>OnPause

[OnPause](https://developer.xamarin.com/api/member/Android.App.Activity.OnPause/) sistem hakkında arka plan veya etkinlik kısmen getirilmemeli olur, etkinlik put üzereyken çağrılır. İçin gerekirse etkinlikler bu yöntemin üzerine yazması gerekir:

-   Kalıcı veri kaydedilmemiş değişiklikleri

-   Destroy veya kaynakları tüketen diğer nesneleri Temizle

-   Çerçeve hızları ve duraklatma animasyonları aşağı artırma

-   Dış olay işleyicileri veya bildirim işleyicilere (yani olanlar bir hizmetine bağlı) kaydını silin. Bu etkinlik bellek sızıntıları önlemek için yapılmalıdır.

-   Etkinlik herhangi bir iletişim kutuları veya uyarıları görüntülenen değilse, benzer şekilde, bunlar ile temizlenmesi gerekir `.Dismiss()` yöntemi.

Etkinlik yapamazsınız gibi örnek olarak, kamera, aşağıdaki kod parçacığını yayın bunu duraklatıldı sırada kullanın:

```csharp
public void OnPause()
{
    base.OnPause(); // Always call the superclass first

    // Release the camera as other activities might need it
    if (_camera != null)
    {
        _camera.Release();
        _camera = null;
    }
}
```

Sonra adlı iki olası yaşam döngüsü yöntem `OnPause`:

1.  `OnResume` Etkinlik için ön döndürülecek ise çağrılır.
1.  `OnStop` Etkinlik arka planda yerleştirilmişse çağrılır.


#### <a name="onstop"></a>OnStop

[OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) etkinliği artık kullanıcıya görünür olduğunda çağrılır. Aşağıdakilerden biri oluştuğunda bu gerçekleşir:

-  Yeni bir etkinlik başlatıldığına ve bu faaliyeti kapsayan.
-  Varolan bir etkinlik için ön duruma getirilir.
-  Etkinlik yok edilir.


`OnStop` her zaman zaman Android kaynaklar için gerek duyuldu ve etkinlik arka plan düzgün olamaz gibi düşük bellek durumlarda çağrılamaz. Bu nedenle, bağlı olmayan en iyisidir `OnStop` bir etkinlik yok etme için hazırlarken adlı. Bu olur sonra çağrılabilir sonraki yaşam döngüsü yöntemleri `OnDestroy` etkinlik yerine koyma, edecekse veya `OnRestart` etkinlik geri kullanıcıyla etkileşim geliyorsa.

#### <a name="ondestroy"></a>OnDestroy

[OnDestroy](https://developer.xamarin.com/api/member/Android.App.Activity.OnDestroy/) yok ve bellekten tamamen kaldırılabileceği önce bir etkinlik örneğinde adlı son yöntemidir. Olağanüstü durumlar Android sonuçlanır etkinlik barındırma uygulama işlemi sonlandırılamadı `OnDestroy` değil çağrılan. Etkinliklerin çoğu bu yöntem çünkü çoğu temizlemek ve kapatma tamamlandı gerçekleştireceksiniz değil `OnPause` ve `OnStop` yöntemleri. `OnDestroy` Yöntemi geçersiz genellikle uzun temizlemek için kaynakları çalıştıran kaynakları sızıntısı. Buna örnek olarak başlatılan içinde arka plan iş parçacıkları olabilir `OnCreate`.

Etkinlik yok sonra çağrılan hiçbir yaşam döngüsü yöntemi olacaktır.

#### <a name="onrestart"></a>OnRestart

[OnRestart](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestart/) etkinliklerinizi, önce yeniden başlatıldığına durdurulduktan sonra çağrılır. Bu iyi bir örneği, kullanıcı uygulamada bir etkinlik sırasında ev düğmeyi bastığında olacaktır. Bu durumda `OnPause` ve ardından `OnStop` yöntemleri çağrılmadan ve etkinlik arka plana taşınır, ancak değil yok. Görev Yöneticisi'ni veya benzer bir uygulama kullanarak uygulamayı geri yüklemek için Android çağıracak sonra kullanıcı olsaydı `OnRestart` etkinliğin yöntemi.

Ne tür bir mantık içinde uygulanması için hiçbir genel yönergeleri vardır `OnRestart`. Bunun nedeni, `OnStart` olup etkinlik oluşturuluyor bağımsız olarak her zaman çağrılır veya etkinlik tarafından gerekli tüm kaynakları olarak başlatılması için yeniden başlatılıyor `OnStart`, yerine `OnRestart`.

Sonra çağrılan sonraki yaşam döngüsü yöntemi `OnRestart` olacaktır `OnStart`.

### <a name="back-vs-home"></a>VS yedekleyin. Ana Sayfası

İki ayrı düğmeleri birçok Android cihazları vardır: bir "Geri" düğmesi ve bir "Home" düğmesi. Bunun bir örneğini Android 4.0.3 aşağıdaki ekran görüntüsünde görebilirsiniz:

[![Geri ve giriş düğmeleri](images/image4-sml.png)](images/image4.png#lightbox)

Bir uygulama arka planda yerleştirme aynı etkiye sahip görünse de iki düğmeler arasındaki zarif bir fark yoktur. Bir kullanıcı geri düğmesine tıkladığında, bunların etkinliği ile yapılır Android söylemiş olursunuz. Android etkinlik yok. Kullanıcı Giriş düğmesini tıklattığında buna karşılık, etkinliği yalnızca arka plan içine yerleştirildiğinde &ndash; Android değil etkinlik sonlandırın.

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>Yaşam döngüsü boyunca durumunu yönetme

Bir etkinlik durdurulmuş ya da yok sistem sonraki rehydration etkinlik durumunu kaydetmek için fırsatı sunar.
Bu durumu kaydedildi, örnek durumu adlandırılır. Android örnek durum etkinlik yaşam döngüsü sırasında depolamak için üç seçenek sunar:

1. İlkel değerleri depolama bir `Dictionary` olarak bilinen bir [paket](https://developer.xamarin.com/api/type/Android.OS.Bundle/) Android durumunu kaydetmek için kullanır.

1. Özel bir sınıf oluşturma, bit eşlemler gibi karmaşık değerler tutar. Android durumunu kaydetmek için bu özel sınıfını kullanır.

1. Yapılandırma değişikliği yaşam döngüsü atlamak ve etkinlik durumda korumak için tam sorumluluk varsayılarak.


Bu kılavuz, ilk iki seçeneği kapsar.



### <a name="bundle-state"></a>Paket durumu

Örneğin durumunu kaydetmek için birincil seçeneği olarak bilinen bir anahtar/değer sözlük nesnesi kullanmaktır bir [paket](https://developer.xamarin.com/api/type/Android.OS.Bundle/).
Bir etkinlik oluşturulduğunda, geri çağırma `OnCreate` yöntemi, bir paket parametre olarak geçirilir, bu paket, örneğin durumu geri yüklemek için kullanılabilir. Hızlı bir şekilde çalışmaz veya kolayca anahtar/değer çiftleri (örneğin, bit eşlemler); için seri hale karmaşıksa veriler için bir paket kullanmak için önerilmez Bunun yerine, dize gibi basit değerleri için kullanılmalıdır.

Bir etkinlik kaydetme ve paketteki örnek durumu alınırken yardımcı olmak için yöntemleri sağlar:

-   [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) &ndash; etkinlik bozulduğunda bu Android tarafından çağrılır. Herhangi bir anahtar/değer durumu öğesini kalıcı hale getirmek gerekiyorsa etkinlikler bu yöntem uygulayabilirsiniz.

-   [OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) &ndash; bu sonra adlandırılır `OnCreate` yöntemi tamamlandıktan ve başlatma işlemi tamamlandıktan sonra durumuna geri yüklemek bir etkinlik için başka bir fırsatı sağlar.

Aşağıdaki diyagramda, bu yöntemleri nasıl kullanıldığı gösterilmektedir:

[![Paket durumları akış çizelgesi](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) etkinlik durduruldu olarak adlandırılır. Etkinlik durumundayken depolayabilir bir paket parametre alırsınız. Bir aygıt bir yapılandırma değişikliği yaşandığında, bir etkinlik kullanabilirsiniz `Bundle` kılarak etkinlik durumu korumak için geçirilen nesne `OnSaveInstanceState`. Örneğin, aşağıdaki kodu göz önünde bulundurun:

```csharp
int c;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  this.SetContentView (Resource.Layout.SimpleStateView);

  var output = this.FindViewById<TextView> (Resource.Id.outputText);

  if (bundle != null) {
    c = bundle.GetInt ("counter", -1);
  } else {
    c = -1;
  }

  output.Text = c.ToString ();

  var incrementCounter = this.FindViewById<Button> (Resource.Id.incrementCounter);

  incrementCounter.Click += (s,e) => {
    output.Text = (++c).ToString();
  };
}
```

Yukarıdaki kod adlı bir tamsayı artırır `c` adlı bir düğmeye zaman `incrementCounter` tıklandığında, sonuçta görüntüleyen bir `TextView` adlı `output`. Bir yapılandırma değişikliği - Örneğin, gerçekleştiğinde ne zaman cihaz Döndürülmüş - yukarıdaki kodu değerini kaybeder `c` çünkü `bundle` olacaktır `null`aşağıdaki şekilde gösterildiği gibi:

[![Görüntü önceki değeri göstermiyor](images/07-sml.png)](images/07.png#lightbox)

Değerini korumak için `c` Bu örnekte, etkinlik kılabilirsiniz `OnSaveInstanceState`, aşağıda gösterildiği gibi pakete değer kaydetme:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

Şimdi aygıt için yeni bir yönlendirme döndürüldüğünde, tamsayı pakete kaydedilir ve satırıyla alınır:

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> Bu her zaman önemli çağrısı temel uygulamasıdır `OnSaveInstanceState` böylece hiyerarşisini görüntüleme durumunu da kaydedilebilir.



##### <a name="view-state"></a>Görünüm durumu

Geçersiz kılma `OnSaveInstanceState` yukarıdaki örnekte sayacı gibi yönlendirmesini değişiklikler arasında bir etkinlikte geçici verileri kaydetme için uygun bir mekanizmadır. Ancak, varsayılan uygulaması `OnSaveInstanceState` her görünüm için kullanıcı arabiriminde geçici verileri kaydetme her görünüm atanan bir kimliği var olduğu sürece ilgilenebilmek. Örneğin, bir uygulama olan deyin bir `EditText` XML dosyasında şu şekilde tanımlanan öğe:

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

Bu yana `EditText` denetimi sahip bir `id` atanan kullanıcı bazı veriler girdiği ve cihaz döndürür, verilerin yine, aşağıda gösterildiği gibi görüntülenir:

[![Veri yatay modunda korunur](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) sonra çağrılacak `OnStart`. Bir etkinlik daha önce bir pakete ait önceki sırasında kaydedilmiş durumunu geri yüklemek için fırsat sunar `OnSaveInstanceState`. İçin sağlanan aynı paket budur `OnCreate`, ancak.

Aşağıdaki kod, nasıl durumu içinde geri yüklenebilir gösterir `OnRestoreInstanceState`:

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

Durumu geri zaman geçici bazı esneklik sağlamak için bu yöntem bulunmaktadır. Bazen örnek durum geri yüklemeden önce tüm başlatmaları bitti kadar bekleyin daha uygundur. Ayrıca, varolan bir etkinlik öğesinin bir alt yalnızca belirli değerleri örneği durumundan geri yüklemek isteyebilirsiniz. Çoğu durumda, geçersiz kılmak ise gerekli değildir `OnRestoreInstanceState`, çoğu etkinlikler için sağlanan paket kullanarak duruma geri beri `OnCreate`.

Durum kullanan kaydetme örneği için bir `Bundle`, başvurmak [izlenecek - kaydetme etkinlik durumu](saving-state.md).


#### <a name="bundle-limitations"></a>Paket sınırlamaları

Ancak `OnSaveInstanceState` kolaylaştırır kolayca geçici verileri kaydetmek için bazı sınırlamalar vardır:

-   Tüm durumlarda çağrılmaz. Örneğin, tuşuna basarak **giriş** veya **geri** bir etkinlik çıkmak için sonuçlanır değil `OnSaveInstanceState` çağrılıyor.

-   Paket içine geçirilen `OnSaveInstanceState` görüntüleri gibi büyük nesneler için tasarlanmamıştır. Büyük nesneler, nesneyi kaydetme durumunda [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) aşağıda açıklandığı gibi daha iyidir.

-   Paket kullanılarak kaydedilen verileri seri, gecikmeleri yol açabilir.

Ancak paket durumu kadar bellek kullanmayan basit veriler için kullanışlı *yapılandırma olmayan örnek veri* olan daha karmaşık veri ya da almak pahalıdır veriler için kullanışlıdır, örneğin bir web hizmeti çağrısı veya bir karmaşık veritabanı sorgusu. Olmayan yapılandırma örneği verileri gerektiğinde bir nesneyi kaydedildi. Sonraki bölümde tanıtır `OnRetainNonConfigurationInstance` daha karmaşık veri türleri yapılandırma değişikliklerini aracılığıyla koruma bir yolu olarak.


### <a name="persisting-complex-data"></a>Karmaşık veri kalıcı yapma

Paketteki kalıcı verilere ek olarak, Android ayrıca veri kılarak destekleyen [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) ve örneğini döndüren bir `Java.Lang.Object` devam ettirmek için verileri içerir. İki birincil yararları vardır `OnRetainNonConfigurationInstance` durumunu kaydetmek için:

-   Döndürülen nesne `OnRetainNonConfigurationInstance` bellek bu nesne koruyacağından de daha büyük ve daha karmaşık veri türleriyle gerçekleştirir.

-   `OnRetainNonConfigurationInstance` Yöntemi adlı isteğe bağlı ve yalnızca gerekli olduğunda. El ile bir önbellek kullanmaktan daha ekonomik budur.

Kullanarak `OnRetainNonConfigurationInstance` birden çok kez verileri almak üzere pahalı olduğu senaryolar için uygun, örn. web hizmeti çağrıları şeklindedir. Örneğin, Twitter arar aşağıdaki kodu göz önünde bulundurun:

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);
    SearchTwitter ("xamarin");
  }

  public void SearchTwitter (string text)
  {
    string searchUrl = String.Format("http://search.twitter.com/search.json?" + "q={0}&rpp=10&include_entities=false&" + "result_type=mixed", text);

    var httpReq = (HttpWebRequest)HttpWebRequest.Create (new Uri (searchUrl));
    httpReq.BeginGetResponse (new AsyncCallback (ResponseCallback), httpReq);
  }

  void ResponseCallback (IAsyncResult ar)
  {
    var httpReq = (HttpWebRequest)ar.AsyncState;

    using (var httpRes = (HttpWebResponse)httpReq.EndGetResponse (ar)) {
      ParseResults (httpRes);
    }
  }

  void ParseResults (HttpWebResponse httpRes)
  {
    var s = httpRes.GetResponseStream ();
    var j = (JsonObject)JsonObject.Load (s);

    var results = (from result in (JsonArray)j ["results"] let jResult = result as JsonObject select jResult ["text"].ToString ()).ToArray ();

    RunOnUiThread (() => {
      PopulateTweetList (results);
    });
  }

  void PopulateTweetList (string[] results)
  {
    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
  }
}
```

Bu kod sonuçları JSON olarak biçimlendirilmiş web alır, bunları ayrıştırır ve ardından sonuçları aşağıdaki ekran görüntüsünde gösterildiği gibi bir liste sunar:

[![Ekranda gösterilen sonuçları](images/06-sml.png)](images/06.png#lightbox)

Bir aygıt döndürüldüğünde - bir yapılandırma değişikliği - Örneğin, oluştuğunda kodu işlem yinelenir. Başlangıçta alınan sonuçları yeniden kullanmak ve gereksiz, yedekli ağ çağrıları neden değil için biz kullanabilirsiniz `OnRetainNonconfigurationInstance` aşağıda gösterildiği gibi sonuçları kaydetmek için:

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  TweetListWrapper _savedInstance;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    var tweetsWrapper = LastNonConfigurationInstance as TweetListWrapper;

    if (tweetsWrapper != null) {
      PopulateTweetList (tweetsWrapper.Tweets);
    } else {
      SearchTwitter ("xamarin");
    }

    public override Java.Lang.Object OnRetainNonConfigurationInstance ()
    {
      base.OnRetainNonConfigurationInstance ();
      return _savedInstance;
    }

    ...

    void PopulateTweetList (string[] results)
    {
      ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
      _savedInstance = new TweetListWrapper{Tweets=results};
    }
}
```

Cihaz döndürüldüğünde, özgün sonuçları alınır artık `LastNonConfiguartionInstance` özelliği. Bu örnekte, sonuçları oluşur bir `string[]` tweet'leri içeren. Bu yana `OnRetainNonConfigurationInstance` gerektiren bir `Java.Lang.Object` döndürülüp döndürülmediğini, `string[]` bir sınıfta o alt sınıfların paketlenir `Java.Lang.Object`, aşağıda gösterildiği gibi:

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

Örneğin, kullanılmaya çalışılıyor bir `TextView` yönteminden döndürülen nesne olarak `OnRetainNonConfigurationInstance` etkinlik kod tarafından gösterildiği gibi sızıntısı:

```csharp
TextView _textView;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  var tv = LastNonConfigurationInstance as TextViewWrapper;

  if(tv != null) {
    _textView = tv;
    var parent = _textView.Parent as FrameLayout;
    parent.RemoveView(_textView);
  } else {
    _textView = new TextView (this);
    _textView.Text = "This will leak.";
  }

  SetContentView (_textView);
}

public override Java.Lang.Object OnRetainNonConfigurationInstance ()
{
  base.OnRetainNonConfigurationInstance ();
  return _textView;
}
```

Bu bölümde, biz basit durum verilerle korumak öğrendiniz `Bundle`, ve daha karmaşık veri türleriyle kalır `OnRetainNonConfigurationInstance`.

## <a name="summary"></a>Özet

Android etkinlik yaşam döngüsü, bir uygulamadaki etkinlik durumu yönetimi için güçlü bir çerçeve sunar ancak anlamanıza ve uygulamanıza zor olabilir. Bu bölümde bir etkinlik bu durumlar ile ilişkili yaşam döngüsü yöntemleri yanı sıra, yaşam süresi sırasında gidebilir farklı durumları getirmiştir. Ardından, kılavuzu ne tür bir mantık bu yöntemlerin her biri gerçekleştirilmelidir konusunda sağlandı.


## <a name="related-links"></a>İlgili bağlantılar

- [Döndürmeyi İşleme](~/android/app-fundamentals/handling-rotation.md)
- [Android Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)
