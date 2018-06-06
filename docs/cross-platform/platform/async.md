---
title: Zaman uyumsuz desteğine genel bakış
description: Bu belge ile zaman uyumsuz programlama açıklar ve bekleme, C# zaman uyumsuz kod yazmayı kolaylaştırmak için 5'te sunulan kavramlar.
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 22878695d93ae79bbbfe1b99961587ff0bf957be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782014"
---
# <a name="async-support-overview"></a>Zaman uyumsuz desteğine genel bakış

_C# 5 sunulan zaman uyumsuz programing basitleştirmek için iki anahtar sözcükler: async ve await. Bu anahtar sözcükler, başka bir iş parçacığında (örneğin, ağ erişimi) uzun süre çalışan işlemlerini yürütmek için görev paralel kitaplığı kullanan basit kod yazmak ve kolayca tamamlama sonuçlarına erişme olanak tanır. Xamarin.iOS ve Xamarin.Android en son sürümlerini zaman uyumsuz desteği ve await - bu belge, açıklamalar ve Xamarin ile yeni sözdizimini kullanarak bir örnek sağlar._

Xamarin'in zaman uyumsuz destek Mono 3.0 temeli üzerinde oluşturulmuştur ve .NET 4.5 mobil dostu sürümünde Silverlight mobil dostu bir sürümü olmaktan API profili yükseltir.

## <a name="overview"></a>Genel Bakış

Bu belge yeni zaman uyumsuz tanıtır ve anahtar sözcükler sonra zaman uyumsuz yöntemleri Xamarin.iOS ve Xamarin.Android uygulama bazı basit örnekler üzerinden yetenekte bekler.

C# 5 (pek çok örnekleri ve farklı kullanım senaryoları dahil) zaman uyumsuz bir yeni özelliklerin daha ayrıntılı bir tartışma için MSDN belgelerine başvurun [uyumsuz ve bekleme ile zaman uyumsuz programlama](http://msdn.microsoft.com/library/vstudio/hh191443.aspx).

Örnek uygulama basit zaman uyumsuz web (ana iş parçacığı engellenmeden) istekte sonra indirilen html ve karakter sayısı ile kullanıcı arabirimini güncelleştirir.

 [![](async-images/AsyncAwait_427x368.png "Örnek uygulama basit zaman uyumsuz web ana iş parçacığı engellenmeden istekte sonra indirilen html ve karakter sayısı ile kullanıcı arabirimini güncelleştirir")](async-images/AsyncAwait.png#lightbox)

Xamarin'in zaman uyumsuz destek Mono 3.0 temeli üzerinde oluşturulmuştur ve .NET 4.5 mobil dostu sürümünde Silverlight mobil dostu bir sürümü olmaktan API profili yükseltir.

## <a name="requirements"></a>Gereksinimler

C# 5 özellikleri Xamarin.iOS 6.4 ve Xamarin.Android 4.8 dahil Mono 3.0 gerektirir. Mono, Xamarin.iOS, Xamarin.Android ve Xamarin.Mac bunu yararlanmak için yükseltmeniz istenir.

## <a name="using-async-amp-await"></a>Async kullanma &amp; bekleme

 `async` ve `await` ana iş parçacığı, uygulamanızın engellenmeden uzun süre çalışan görevleri gerçekleştirmek için iş parçacıklı kod yazmayı kolaylaştırmak için görev paralel kitaplığı ile birlikte çalışan yeni C# dili özellikleridir.

## <a name="async"></a>async

### <a name="declaration"></a>Bildirim

`async` Anahtar sözcüğü bir yöntem bildirimi (veya bir lambda veya anonim yöntemi) yerleştirilir zaman uyumsuz olarak çalıştırabilirsiniz kodu içeren belirtmek için IE. arayanın iş parçacığı engelleme değil.

Bir yöntem olarak `async` en az birini içermelidir ifade veya deyimin bekler. Öyle değilse `await`eşzamanlı olarak çalışacak sonra s yöntemi mevcut (aynı olmamış gibi hiçbir `async` değiştiricisi). Bu ayrıca Derleyici Uyarısı (ancak hata değil) sonuçlanır.

### <a name="return-types"></a>Dönüş Türleri

Zaman uyumsuz yöntem döndürmelidir bir `Task`, `Task<TResult>` veya `void`.

Belirtin `Task` yöntemi başka bir değer döndürmezse, dönüş türü.

Belirtin `Task<TResult>` yöntemi bir değer döndürmesi gerekiyorsa nerede `TResult` döndürülen türü (gibi bir `int`, örneğin).

`void` Dönüş türü, çoğunlukla gerektiren olay işleyicileri için kullanılır. Void döndüren zaman uyumsuz yöntemleri çağıran kodu olamaz `await` sonucu üzerinde.

### <a name="parameters"></a>Parametreler

Zaman uyumsuz yöntemleri bildiremez `ref` veya `out` parametreleri.

## <a name="await"></a>bekleme

Bekleme işleci, zaman uyumsuz olarak işaretli bir yöntem içinde bir görev için uygulanabilir. Bu noktada yürütme durdurun ve görevi tamamlanana dek bekleyin yöntemi sağlar.

Kullanarak await arayanın iş parçacığı – engellemez çağırana yerine döndürülür. Örneğin, kullanıcı arabirimi iş parçacığı bir görevin bekleyen zaman engelleneceğini değil şekilde bu çağıran iş parçacığı engellenmemesini, anlamına gelir.

Görev tamamlandığında yöntemi kodu aynı zamanda yürütme devam ettirir. Bu, (varsa) bir try-catch-finally bloğu deneyin kapsamına döndürme içerir. await finally bloğunda veya bir catch kullanılamaz.

Daha fazla bilgi edinin [MSDN'de await](http://msdn.microsoft.com/library/vstudio/hh156528.aspx).

## <a name="exception-handling"></a>Özel Durum İşleme

Async yöntemi içinde oluşan özel durumlarını görevde depolanır ve görev olduğunda oluşturulur `await`ed. Bu özel durumlar yakalanan ve try-catch bloğu içinde işlenir.

## <a name="cancellation"></a>İptal Etme

Tamamlanması uzun zaman zaman uyumsuz yöntem iptal desteklemelidir. Genellikle, iptal gibi çağrılır:

- A `CancellationTokenSource` nesnesi oluşturulur.
- `CancellationTokenSource.Token` Örneği için bir işlem iptal edilemez zaman uyumsuz yöntem geçirilir.
- İptal istendi çağırarak `CancellationTokenSource.Cancel` yöntemi.

Görev kendisini iptal eder ve iptal bildirir.

İptal etme hakkında daha fazla bilgi için bkz: [zaman uyumsuz bir görevi iptal etme](http://msdn.microsoft.com/library/vstudio/jj155761.aspx) konusuna bakın.

## <a name="example"></a>Örnek

Karşıdan [örnek Xamarin çözüm](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (için iOS ve Android için), çalışan bir örnek görmek için `async` ve `await` mobil uygulamalarda. Kod örneği, bu bölümde daha ayrıntılı ele alınmıştır.

### <a name="writing-an-async-method"></a>Async yöntemi yazma

Aşağıdaki yöntem gösterilmektedir kodu nasıl bir `async` yöntemi ile bir `await`ed görevi:

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("http://xamarin.com"); // async method!

    // await! control returns to the caller and the task continues to run on another thread
    string contents = await contentsTask;

    ResultEditText.Text += "DownloadHomepage method continues after async call. . . . .\n";

    // After contentTask completes, you can calculate the length of the string.
    int exampleInt = contents.Length;

    ResultEditText.Text += "Downloaded the html and found out the length.\n\n\n";

    ResultEditText.Text += contents; // just dump the entire HTML

    return exampleInt; // Task<TResult> returns an object of type TResult, in this case int
}
```

Bu noktalar göz önünde bulundurun:

-  Yöntem bildirimi içerir `async` anahtar sözcüğü.
-  Dönüş türü `Task<int>` kodu çağırma erişebilmesi için `int` bu yöntemi, hesaplanan değer.
-  Return deyimi `return exampleInt;` bir tamsayı nesnesi – yöntemi döndürür olgu olduğu `Task<int>` dil geliştirmeleri bir parçasıdır.


### <a name="calling-an-async-method-1"></a>Zaman uyumsuz yöntem 1 çağırma

Bu düğmeye tıkladığınızda olay işleyicisi, yukarıda açıklanan yöntemi çağırmak için Android örnek uygulamasında bulunabilir:

```csharp
GetButton.Click += async (sender, e) => {

    Task<int> sizeTask = DownloadHomepage();

    ResultTextView.Text = "loading...";
    ResultEditText.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await sizeTask;

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultTextView.Text = "Length: " + intResult ;
    // "returns" void, since it's an event handler
};
```

Notlar:

-  Anonim temsilci async anahtar sözcüğü öneke sahip.
-  Zaman uyumsuz yöntem DownloadHomepage bir görev döndürür <int> sizeTask değişkende depolanır.
-  Kod sizeTask değişkeni bekler.  *Bu* yöntemi askıya alınır ve denetim, çağrıyı yapan kod döndürülür, zaman uyumsuz görev kendi iş parçacığında tamamlanana kadar yerdir.
-  Yürütme mu *değil* görev var. oluşturulan görev rağmen yönteminin ilk satırında oluşturulduğunda duraklatma. Bekleme anahtar yürütme burada duraklatıldı konumu belirtir.
-  Zaman uyumsuz görev tamamlandığında intResult ayarlanır ve bekleme satırından özgün iş parçacığı üzerinde yürütme devam eder.


### <a name="calling-an-async-method-2"></a>Zaman uyumsuz yöntem 2 çağırma

İOS örnek uygulama örnek alternatif bir yaklaşım göstermek için biraz daha farklı yazılır. Bunun yerine bu örnek anonim bir temsilci kullanmak daha bildirir bir `async` gibi normal olay işleyicisi atanan olay işleyicisi:

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

Olay işleyicisi yöntemi ardından aşağıda gösterildiği gibi tanımlanır:

```csharp
async void HandleTouchUpInside (object sender, EventArgs e)
{
    ResultLabel.Text = "loading...";
    ResultTextView.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await DownloadHomepage();

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultLabel.Text = "Length: " + intResult ;
}
```

Bazı önemli noktalar:

-  Yöntem olarak işaretlenmiş `async` ancak döndürür `void` . Bu genellikle yalnızca olay işleyicileri için yapılır (Aksi halde döndürecekti bir `Task` veya `Task<TResult>` ).
-  Kod `await` s `DownloadHomepage` doğrudan bir değişken atama yöntemi ( `intResult` ) farklı bir ara kullanıldığı biz önceki örnek olarak `Task<int>` görev başvurmak için değişkeni.  *Bu* burada denetim döndürülür çağırana zaman uyumsuz yöntem tamamlanana kadar başka bir iş parçacığında konumdur.
-  Zaman uyumsuz yöntem tamamlandıktan iade ettiğinde, yürütme sırasında sürdürür `await` tamsayı sonuç başka bir deyişle, döndürülen ve bir kullanıcı Arabirimi pencere öğesinde çizilir.


## <a name="summary"></a>Özet

Async kullanma ve await ana iş parçacığı engellenmeden uzun süre çalışan işlemleri arka plan iş parçacıkları oluşturma için gereken kod büyük ölçüde basitleştirir. Bunlar ayrıca görev tamamlandığında sonuçları erişmek kolaylaştırır.

Bu belge, Xamarin.iOS ve Xamarin.Android için yeni dil anahtar sözcükleri ve örnekler genel bakış verdiği.



## <a name="related-links"></a>İlgili bağlantılar

- [AsyncAwait (örnek)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [Geri aramalar bizim nesli olarak ifadesine gidin](http://tirania.org/blog/archive/2013/Aug-15.html)
- [Veri (iOS) (örnek)](https://developer.xamarin.com/samples/monotouch/Data/)
- [HttpClient (iOS) (örnek)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (örnek)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [Web Semineri: C# zaman uyumsuz iOS ve Android (video)](http://xamarin.wistia.com/medias/k27mc627xz)
- [Zaman uyumsuz programlama ile Async ve Await (MSDN)](http://msdn.microsoft.com/library/vstudio/hh191443.aspx)
- [Async uygulamanızda (MSDN) ayarlama ince](http://msdn.microsoft.com/library/vstudio/jj155761.aspx)
- [Await ve kullanıcı Arabirimi ve kilitlenmeleri! ! My! (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2011/01/13/10115163.aspx)
- [Görevler (MSDN) tamamlandıkça işleme](http://blogs.msdn.com/b/pfxteam/archive/2012/08/02/processing-tasks-as-they-complete.aspx)
- [Görev Tabanlı Zaman Uyumsuz Desen (TAP)](http://msdn.microsoft.com/library/hh873175.aspx)
- [C# 5 (Eric Lippert'ın blogu) – asynchrony anahtar sözcükleri giriş hakkında](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
