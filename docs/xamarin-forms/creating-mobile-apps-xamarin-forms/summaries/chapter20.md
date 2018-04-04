---
title: Bölüm 20 özeti. Zaman uyumsuz ve dosya g/ç
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 521a8b18e74e078b9caafc6c79c7e418e1f5e08f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Bölüm 20 özeti. Zaman uyumsuz ve dosya g/ç

 Bir grafik kullanıcı arabirimi kullanıcı girişini olaylarına sırayla yanıtlaması gerekir. Bu kullanıcı girişi olayların tüm işleme genellikle olarak adlandırılan bir tek iş parçacığı içinde gerçekleşmelidir gelir *ana iş parçacığı* veya *kullanıcı Arabirimi iş parçacığı*.

Kullanıcıların grafik kullanıcı arabirimleri esnek olmasını bekler. Başka bir deyişle, bir program kullanıcı girişi olayları hızlı bir şekilde işlemelidir. Bu mümkün değilse, ardından işleme yürütme İkincil iş parçacıklarının sahip olmalıdır.

Bu kitap birkaç örnek programlarda kullanmış [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) sınıfı. Bu sınıftaki [ `BeginGetReponse` ](https://developer.xamarin.com/api/member/System.Net.WebRequest.BeginGetResponse/p/System.AsyncCallback/System.Object/) yöntemi bu tamamlandığında, bir geri çağırma işlevi çağıran bir çalışan iş parçacığı başlatır. Program çağırmanız gerekir ancak, bu geri çağırma işlevi çalışan iş parçacığı çalışır, böylece [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) kullanıcı arabirimini erişim yöntemi.

Zaman uyumsuz işleme için daha yeni bir yaklaşım, .NET ve C# içinde kullanılabilir. Bu içerir [ `Task` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task/) ve [ `Task<TResult>` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task%3CTResult%3E/) sınıfları ve diğer türleri [ `System.Threading` ](https://developer.xamarin.com/api/namespace/System.Threading/) ve [ `System.Threading.Tasks` ](https://developer.xamarin.com/api/namespace/System.Threading.Tasks/) ad alanları, C# 5.0 yanı sıra `async` ve `await` anahtar sözcükler. Bu bölümde odaklanır olmasıdır.

## <a name="from-callbacks-to-await"></a>Await geri çağrıları

`Page` Sınıfının kendisi uyarı kutuları görüntülemek için üç zaman uyumsuz yöntemler içerir:

- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) döndüren bir `Task` nesnesi
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) döndüren bir `Task<bool>` nesnesi
- [`DisplayActionSheet`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) döndüren bir `Task<string>` nesnesi

`Task` Nesneleri belirtmek bu yöntemleri görev tabanlı zaman uyumsuz DOKUNUN bilinen desen uygulamak. Bunlar `Task` nesneleri hızla yönteminden döndürülen. `Task<T>` Değerleri, bir "promise" oluşturan dönmek türde bir değer `TResult` görev tamamlandığında kullanılabilir olur. `Task` Dönüş değeri tamamlandı ancak hiçbir değer ile döndürülen bu işlem zaman uyumsuz bir eylemi gösterir.

Bu durumlarda `Task` kullanıcı uyarı kutusu kapatılır tam durumdur.  

### <a name="an-alert-with-callbacks"></a>Geri aramalar olan bir uyarı

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) örnek nasıl yapılacağını gösteren `Task<bool>` dönüş nesneleri ve `Device.BeginInvokeOnMainThread` geri arama yöntemleri kullanarak aramaları.

### <a name="an-alert-with-lambdas"></a>Lambda'lar olan bir uyarı

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) örnek anonim lambda işlevlerinin işleme için nasıl kullanılacağı gösterilmektedir `Task` ve `Device.BeginInvokeOnMainThread` çağrıları.  

### <a name="an-alert-with-await"></a>Bekleme olan bir uyarı

Daha basit bir yaklaşım içerir `async` ve `await` C# 5'te tanıtılan anahtar sözcükler. [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) örnek kullanımlarını gösterir.

### <a name="an-alert-with-nothing"></a>Hiçbir şey olan bir uyarı

Zaman uyumsuz yöntem döndürürse `Task` yerine `Task<TResult>`, sonra da program zaman uyumsuz görev tamamlandığında öğrenmek gerekli olmayan aşağıdaki tekniklerden birini kullanırsanız gerekmez. [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) örnek bu gösterir.

### <a name="saving-program-settings-asynchronously"></a>Program ayarları kaydediliyor zaman uyumsuz olarak

[ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) kullanımını gösteren örnek [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) yöntemi `Application` olmadan değiştikçe program ayarlarını kaydetmek için geçersiz kılma `OnSleep` yöntemi.

### <a name="a-platform-independent-timer"></a>Platformdan bağımsız Zamanlayıcı

Kullanmak da mümkündür [ `Task.Delay` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Delay/p/System.Int32/) platformdan bağımsız süreölçer oluşturmak için. [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) örnek bu gösterir.

## <a name="file-inputoutput"></a>Giriş/Çıkış dosyası

Geleneksel olarak, .NET [ `System.IO` ](https://developer.xamarin.com/api/namespace/System.IO/) ad alanı, dosya g/ç desteği kaynak açıldı. Bu ad alanındaki bazı yöntemleri zaman uyumsuz işlemleri desteklemesine rağmen çoğu yoktur. Ad alanı, Gelişmiş dosya g/ç işlevleri gerçekleştirmek birkaç basit yöntem çağrılarını da destekler.

### <a name="good-news-and-bad-news"></a>İyi haber ve hatalı Haberleri

Xamarin.Forms uygulaması yerel depolamayı destekle tarafından desteklenen tüm platformlar &mdash; uygulamaya özel depolama.

Xamarin.iOS ve Xamarin.Android kitaplıkları Xamarin açıkça bu iki platform için uyarlanabilir .NET sürümünü içerir. Sınıflardan bunlar `System.IO` bu iki platform dosya g/ç uygulama yerel depolama ile gerçekleştirmek için kullanabilirsiniz.

Ancak, bunlar için arama yaparsanız `System.IO` Xamarin.Forms PCL sınıflarında, olmaz bulduğunuz bunları. Windows çalışma zamanı API, tamamen revamped Microsoft dosya g/ç sorunudur. Windows 8.1, Windows Phone 8.1 ve evrensel Windows platformu hedefleyen programlar kullanmayın `System.IO` dosya g/ç için.

Bu kullanmanız gerekecektir anlamına gelir [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) (ilk ele [ **Bölüm 9. Platforma özgü API çağrıları** ](chapter09.md) dosya g/ç uygulamak için.

### <a name="a-first-shot-at-cross-platform-file-io"></a>Platformlar arası dosya g/ç sırasında ilk görüntüsü

[ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) örnek tanımlayan bir [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) dosya g/ç ve tüm platformlarda bu arabirim uygulamaları için arabirim. Ancak, Windows çalışma zamanı dosya g/ç yöntemleri zaman uyumsuz olduğundan bu arabirim yöntemleri ile Windows çalışma zamanı uygulamaları çalışmıyor.

### <a name="accommodating-windows-runtime-file-io"></a>Windows çalışma zamanı dosya g/ç destekleme

Windows çalışma zamanı altında çalışan programları kullanın sınıflarda [ `Windows.Storage` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.aspx) ve [ `Windows.Storage.Streams` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.aspx) ad alanları için dosya g/ç, uygulama yerel depolama da dahil olmak üzere. Microsoft, 50'den fazla milisaniye kullanıcı Arabirimi iş parçacığı engelleme önlemek için zaman uyumsuz gerektiren herhangi bir işlem belirlediğinden bu dosya g/ç yöntemler çoğunlukla zaman uyumsuz.

Diğer uygulamalar tarafından kullanılır böylece bu yeni bir yaklaşım gösteren kod Kitaplığı'nda olacaktır.

## <a name="platform-specific-libraries"></a>Platforma özgü kitaplıkları

Yeniden kullanılabilir kod kitaplıklarında depolamak için avantajlıdır. Yeniden kullanılabilir kod farklı parçalarını tamamen farklı işletim sistemleri için olduğunda tabii daha zordur.

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) çözüm bir yaklaşım gösterir. Bu çözüm yedi farklı projelere içerir:

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), normal bir Xamarin.Forms PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), an iOS class library
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), an Android class library
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), a Universal Windows class library
- [**Xamarin.FormsBook.Platform.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Windows), a PCL for Windows 8.1.
- [**Xamarin.FormsBook.Platform.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinPhone), a PCL for Windows Phone 8.1
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), a shared project for code that is common to all the Windows platforms

Tüm Bireysel platform projeleri (dışında **Xamarin.FormsBook.Platform.WinRT**) başvuran **Xamarin.FormsBook.Platform**. Bir başvuru üç Windows projeleri sahip **Xamarin.FormsBook.Platform.WinRT**.

Tüm projeleri içeren bir statik `Toolkit.Init` Xamarin.Forms uygulaması çözümdeki bir proje ile tarafından doğrudan başvurulduğunda kitaplığı yüklendiğinden emin olmak için yöntem.

**Xamarin.FormsBook.Platform** projeyi içeren yeni [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) arabirimi. Tüm yöntemleri şimdi adlarıyla sahip `Async` sonekleri ve return `Task` nesneleri.

**Xamarin.FormsBook.Platform.WinRT** projeyi içeren [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) Windows çalışma zamanı için sınıf.

**Xamarin.FormsBook.Platform.iOS** projeyi içeren [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) iOS için sınıf. Bu yöntemleri şimdi zaman uyumsuz olması gerekir. Yöntemlerin bazıları tanımlanan yöntemleri zaman uyumsuz sürümlerini kullanan `StreamWriter` ve `StreamReader`: [ `WriteAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamWriter.WriteAsync/p/System.String/) ve [ `ReadToEndAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamReader.ReadToEndAsync()/). Diğer bir sonuç dönüştürme bir `Task` kullanarak nesne [ `FromResult` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.FromResult%7BTResult%7D/p/TResult/) yöntemi.

**Xamarin.FormsBook.Platform.Android** projeyi içeren bir benzer [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) Android için sınıf.

**Xamarin.FormsBook.Platform** projesini de içeren bir [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) kullanımını kolaylaştırır sınıfı `DependencyService` nesnesi.

Bu kitaplıklar kullanmak için uygulama çözümünü tüm projelerde içermelidir **Xamarin.FormsBook.Platform** çözümü ve her uygulama projeleri içinde karşılık gelen kitaplığına bir başvuru olmalıdır  **Xamarin.FormsBook.Platform**.

[ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) çözümü nasıl kullanılacağını gösteren **Xamarin.FormsBook.Platform** kitaplıkları. Projelerin her biri için bir çağrı sahip `Toolkit.Init`. Uygulama zaman uyumsuz dosya g/ç kullanır işlevleri.

### <a name="keeping-it-in-the-background"></a>Arka planda tutma

Birden çok zaman uyumsuz yöntemleri çağrı yapmak kitaplıkları yöntemleri &mdash; gibi `WriteFileAsync` ve `ReadFileASync` Windows çalışma zamanı yöntemleri [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) sınıfı &mdash; biraz yapılabilir kullanarak daha verimli [ `ConfigureAwait` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task%3CTResult%3E.ConfigureAwait/p/System.Boolean/) geri kullanıcı arabirimi iş parçacığı geçişi önlemek için yöntem.

### <a name="dont-block-the-ui-thread"></a>Kullanıcı Arabirimi iş parçacığı engelleme!

Bazen kullanmaktan kaçının tempting `ContinueWith` veya `await` kullanarak [ `Result` ](https://developer.xamarin.com/api/property/System.Threading.Tasks.Task%3CTResult%3E.Result/) yöntemleri özelliği. Kullanıcı Arabirimi iş parçacığı engellemek veya bile uygulamayı kapatmak için bu kaçınılmalıdır.

## <a name="your-own-awaitable-methods"></a>Bildirdiğimize yöntemlerinizi

Aşağıdakilerden birini geçirerek biraz kod zaman uyumsuz olarak çalıştırabilirsiniz [ `Task.Run` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Run/p/System.Action/) yöntemleri. Çağırabilirsiniz `Task.Run` bazı ek yükü işleme bir zaman uyumsuz yöntem içinde.

Çeşitli `Task.Run` desenleri aşağıda ele alınmıştır.

### <a name="the-basic-mandelbrot-set"></a>Temel Mandelbrot kümesi

Gerçek zamanlı olarak ayarlamak Mandelbrot çizmek için [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplık sahip bir [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) yapısı gösterilene benzer `System.Numerics` ad alanı.

[ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) örnek sahip bir `CalculateMandeblotAsync` yöntemi temel beyaz Mandelbrot kümesi hesaplar ve kullandığı kendi arka plan kod dosyasına [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)bir bit eşlem'i yerleştirilecek.

### <a name="marking-progress"></a>İlerleme işaretleme

Zaman uyumsuz bir yöntem rapor ilerlemesini için örneği bir [ `Progress<T>` ](https://developer.xamarin.com/api/type/System.Progress%3CT%3E/) sınıfı ve türünde bir bağımsız değişken için zaman uyumsuz yöntem tanımlamak [ `IProgress<T>` ](https://developer.xamarin.com/api/type/System.IProgress%3CT%3E/). Bu, gösterilmiştir [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) örnek.

### <a name="cancelling-the-job"></a>İşi iptal ediliyor

İşlem iptal edilemez olması için zaman uyumsuz bir yöntem de yazabilirsiniz. Adlı bir sınıf ile başlayan [ `CancellationTokenSource` ](https://developer.xamarin.com/api/type/System.Threading.CancellationTokenSource/). [ `Token` ](https://developer.xamarin.com/api/property/System.Threading.CancellationTokenSource.Token/) Özelliktir türünde bir değer [ `CancellationToken` ](https://developer.xamarin.com/api/type/System.Threading.CancellationToken/). Bu zaman uyumsuz işlev geçirilir. Bir programı çağırır [ `Cancel` ](https://developer.xamarin.com/api/member/System.Threading.CancellationTokenSource.Cancel()/) yöntemi `CancellationTokenSource` (genellikle, bir eylem kullanıcı tarafından yanıta) zaman uyumsuz işlev iptal etmek için.

Zaman uyumsuz yöntem düzenli olarak kontrol edebilirsiniz [ `IsCancellationRequested` ](https://developer.xamarin.com/api/property/System.Threading.CancellationToken.IsCancellationRequested/) özelliği `CancellationToken` ve özellik ise çıkış `true`, veya yalnızca arama [ `ThrowIfCancellationRequested` ](https://developer.xamarin.com/api/member/System.Threading.CancellationToken.ThrowIfCancellationRequested()/) yöntemi, yöntem durumda ile biten bir [ `OperationCancelledException` ](https://developer.xamarin.com/api/type/System.OperationCanceledException/).

[ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) örnek bir işlem iptal edilemez işlevinin kullanımı gösterilmiştir.

### <a name="an-mvvm-mandelbrot"></a>MVVM Mandelbrot

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) örnek daha geniş bir kullanıcı arabirimine sahiptir ve çoğunlukla bağlı olduğu bir [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) ve [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)sınıfları:

[![Üçlü ekran görüntüsü Mandelbrot X F](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>Geri Web'e

[ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) Bazı örnekleri kullanılan sınıfı zaman uyumsuz programlama modeli veya APM adlı eski moda bir zaman uyumsuz protokolünü kullanır. Böyle bir sınıfın birini kullanarak modern DOKUNUN Protokolü dönüştürebilirsiniz `FromAsync` yöntemleri [ `TaskFactory` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskFactory%3CTResult%3E/) sınıfı. [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) örnek bu gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 20 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Bölüm 20 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Dosyalarla Çalışma](~/xamarin-forms/app-fundamentals/files.md)
