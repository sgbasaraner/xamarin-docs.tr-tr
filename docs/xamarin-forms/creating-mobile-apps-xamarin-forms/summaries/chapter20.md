---
title: Bölüm 20 özeti. Zaman uyumsuz ve dosya g/ç
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 20 özeti. Zaman uyumsuz ve dosya g/ç'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 2ff54b65b1dca9798c91f147da7e8482649e40d2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996288"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Bölüm 20 özeti. Zaman uyumsuz ve dosya g/ç

 Bir grafik kullanıcı arabirimi, kullanıcı girişini olayları sıralı olarak yanıt vermelidir. Bu kullanıcı girişi olaylarını tüm işlemler genellikle olarak adlandırılan bir tek iş parçacığı içinde gerçekleşmelidir gelir *ana iş parçacığı* veya *UI iş parçacığı*.

Kullanıcılar, hızlı yanıt için grafik kullanıcı arabirimleri bekler. Başka bir deyişle, bir program kullanıcı girişi olayları hızlı bir şekilde işlemesi gerekir. Bu mümkün değilse, ardından işlem yürütme İkincil iş parçacıkları sahip olmalıdır.

Bu kitap birkaç örnek programlarında kullanmış [ `WebRequest` ](xref:System.Net.WebRequest) sınıfı. Bu sınıftaki [ `BeginGetReponse` ](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object)) yöntemi tamamladıktan sonra bir geri çağırma işlevini çağırır bir çalışan iş parçacığı başlatılır. Program çağırmanız gerekir ancak bu geri çağırma işlevi çalışan iş parçacığında çalışır, böylece [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) kullanıcı arabirimine yöntemi.

Zaman uyumsuz işleme için daha modern bir yaklaşımı, .NET ve C# içinde kullanılabilir. Bu içerir [ `Task` ](xref:System.Threading.Tasks.Task) ve [ `Task<TResult>` ](xref:System.Threading.Tasks.Task`1) sınıfları ve diğer türleri [ `System.Threading` ](xref:System.Threading) ve [ `System.Threading.Tasks` ](xref:System.Threading.Tasks) ad alanlarında, C# 5.0 yanı sıra `async` ve `await` anahtar sözcükleri. Bu bölümde odaklanır olmasıdır.

## <a name="from-callbacks-to-await"></a>Await geri çağrılar

`Page` Sınıfın kendisi uyarı kutularını görüntülemek için üç zaman uyumsuz yöntemleri içerir:

- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) döndürür bir `Task` nesnesi
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) döndürür bir `Task<bool>` nesnesi
- [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) döndürür bir `Task<string>` nesnesi

`Task` Nesnelerini bu yöntemler görev tabanlı zaman uyumsuz DOKUNUN bilinen deseni kullandığını gösterir. Bunlar `Task` nesneleri hızla yönteminden döndürülen. `Task<T>` Değerleri, "promise" oluşturur dönüş türünde bir değer `TResult` görev tamamlandığında kullanılabilir olacaktır. `Task` Dönüş değeri, zaman uyumsuz bir eylemi tamamlandı ancak hiçbir değer ile döndürülen olacak gösterir.

Tüm bu gibi durumlarda, `Task` kullanıcı uyarı kutusu kapatılana olduğunda tamamlandıktan.  

### <a name="an-alert-with-callbacks"></a>Geri çağırmaları olan bir uyarı

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) örnek nasıl işleneceğini gösterir `Task<bool>` dönüş nesneleri ve `Device.BeginInvokeOnMainThread` kullanarak geri çağırma yöntemleri çağırır.

### <a name="an-alert-with-lambdas"></a>Bir uyarı ile lambda ifadeleri

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) örnek anonim lambda işlevleri işleme için kullanma gösterilmektedir `Task` ve `Device.BeginInvokeOnMainThread` çağırır.  

### <a name="an-alert-with-await"></a>Bir uyarı await ile

Daha basit bir yaklaşım gerektirir `async` ve `await` C# 5'te sunulan anahtar sözcükleri. [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) örnek kullanımlarını gösterir.

### <a name="an-alert-with-nothing"></a>Bir uyarı hiçbir şey

Zaman uyumsuz yöntemin döndürürse `Task` yerine `Task<TResult>`, sonra da programı, zaman uyumsuz görev tamamlandığında bilmeniz gerekmez, aşağıdaki tekniklerden birini kullanmanız gerekmez. [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) örnek bunu gösterir.

### <a name="saving-program-settings-asynchronously"></a>Program ayarlarını kaydetme zaman uyumsuz olarak

[ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) örnek, kullanımını gösterir [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) yöntemi `Application` olmadan değiştikçe programı ayarlarını kaydetmek için geçersiz kılma `OnSleep` yöntemi.

### <a name="a-platform-independent-timer"></a>Platformdan bağımsız Zamanlayıcı

Kullanmak mümkün mü [ `Task.Delay` ](xref:System.Threading.Tasks.Task.Delay(System.Int32)) platformdan bağımsız Zamanlayıcı oluşturmak için. [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) örnek bunu gösterir.

## <a name="file-inputoutput"></a>Giriş/Çıkış dosyası

Geleneksel olarak, .NET [ `System.IO` ](xref:System.IO) ad alanı, kaynak dosya g/ç desteği geldi. Bu ad alanındaki bazı yöntemler, zaman uyumsuz işlemler desteklese de çoğu yoktur. Ad alanı ayrıca Gelişmiş dosya g/ç işlevleri gerçekleştiren birkaç basit bir yöntem çağrılarını destekler.

### <a name="good-news-and-bad-news"></a>Güzel bir haberimiz var ve bozuk Haberleri

Xamarin.Forms uygulaması yerel depolamayı destekle tarafından desteklenen tüm platformlar &mdash; uygulamaya özel depolama.

Xamarin.iOS ve Xamarin.Android kitaplıkları, .NET, Xamarin, açıkça bu iki platform için uyarlanabilir bir sürümünü içerir. Sınıflardan bunlar `System.IO` iki bu platformlarda uygulama yerel depolama ile dosya g/ç gerçekleştirmek için kullanabilirsiniz.

Ancak, bunlar için arama yaparsanız `System.IO` Xamarin.Forms PCL sınıflarda, bulamaz bunları. Bu tamamen modernize Microsoft dosya g/ç için Windows Runtime API sorunudur. Windows 8.1, Windows Phone 8.1 ve evrensel Windows platformu hedefleyen programlar kullanmayın `System.IO` dosya g/ç için.

Kullanmanız gerekir yani [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) (ilk ele [ **Bölüm 9. Platforma özgü API çağrıları** ](chapter09.md) dosya g/ç uygulamak için.

### <a name="a-first-shot-at-cross-platform-file-io"></a>Platformlar arası dosya g/ç sırasında ilk bir görüntüsü

[ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) örnek tanımlayan bir [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) dosya g/ç ve tüm platformlarda bu arabirimin uygulamaları için arabirim. Ancak, Windows çalışma zamanı dosya g/ç yöntemleri zaman uyumsuz olduğundan bu arabirim yöntemleri ile Windows çalışma zamanı uygulamaları çalışmaz.

### <a name="accommodating-windows-runtime-file-io"></a>Windows çalışma zamanı dosya g/ç destekleme

Windows çalışma zamanı altında çalışan programları kullanın sınıflarda [ `Windows.Storage` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.aspx) ve [ `Windows.Storage.Streams` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.aspx) ad alanları için dosya g/ç uygulama yerel depolama da dahil olmak üzere. Microsoft, karar 50'den fazla milisaniye UI iş parçacığı engellenmesini önlemek için zaman uyumsuz gerektiren herhangi bir işlem olduğundan, bu dosya g/ç yöntemler çoğunlukla zaman uyumsuz olması.

Böylece diğer uygulamalar tarafından kullanılabilir, bu yeni bir yaklaşım gösteren kod Kitaplığı'nda olacaktır.

## <a name="platform-specific-libraries"></a>Platforma özgü kitaplıklar

Yeniden kullanılabilir kod kitaplıkları depolamak için avantajlıdır. Yeniden kullanılabilir kod farklı parçaları için tamamen farklı işletim sistemleri olduğunda daha da zordur.

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) çözüm, bir yaklaşımı gösterir. Bu çözüm, yedi farklı projeler içeriyor:

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), normal bir Xamarin.Forms PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), bir iOS sınıf kitaplığı
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), an Android class library
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), a Universal Windows class library
- [**Xamarin.FormsBook.Platform.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Windows), a PCL for Windows 8.1.
- [**Xamarin.FormsBook.Platform.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinPhone), a PCL for Windows Phone 8.1
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), a shared project for code that is common to all the Windows platforms

Tüm Bireysel platformu projelerinde (dışında **Xamarin.FormsBook.Platform.WinRT**) başvuru **Xamarin.FormsBook.Platform**. Üç Windows proje başvuru sahip **Xamarin.FormsBook.Platform.WinRT**.

Tüm projeleri içeren bir statik `Toolkit.Init` doğrudan bir Xamarin.Forms uygulaması çözümde bir proje tarafından başvuruluyor, kitaplık yüklendiğinden emin olmak için yöntemi.

**Xamarin.FormsBook.Platform** projesini içeren yeni [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) arabirimi. Tüm yöntemleri artık ile adlara sahip `Async` sonekleri ve dönüş `Task` nesneleri.

**Xamarin.FormsBook.Platform.WinRT** projesini içeren [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) Windows çalışma zamanı sınıfı.

**Xamarin.FormsBook.Platform.iOS** projesini içeren [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) iOS için sınıf. Bu yöntemler artık zaman uyumsuz olması gerekir. Bazı yöntemlere içinde tanımlanan yöntemleri zaman uyumsuz sürümlerini kullanan `StreamWriter` ve `StreamReader`: [ `WriteAsync` ](xref:System.IO.StreamWriter.WriteAsync(System.String)) ve [ `ReadToEndAsync` ](xref:System.IO.StreamReader.ReadToEndAsync). Başkaları için bir sonuç dönüştürme bir `Task` kullanarak nesne [ `FromResult` ](xref:System.Threading.Tasks.Task.FromResult*) yöntemi.

**Xamarin.FormsBook.Platform.Android** projesini içeren bir benzer [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) Android için sınıf.

**Xamarin.FormsBook.Platform** proje de içeren bir [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) kolaylaştırır sınıfı `DependencyService` nesne.

Bu kitaplıklar kullanmak için uygulama çözümü, tüm projelerde içermelidir **Xamarin.FormsBook.Platform** çözüm ve her uygulama projelerinden birine karşılık gelen kitaplığına bir başvuru olmalıdır  **Xamarin.FormsBook.Platform**.

[ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) çözüm nasıl kullanılacağını gösterir **Xamarin.FormsBook.Platform** kitaplıkları. Projelerin her biri için bir çağrı sahip `Toolkit.Init`. Uygulama zaman uyumsuz dosya g/ç yararlanır işlevleri.

### <a name="keeping-it-in-the-background"></a>Arka planda tutma

Birden çok zaman uyumsuz yöntem çağrı yapmak kitaplıkları yöntemleri &mdash; gibi `WriteFileAsync` ve `ReadFileASync` Windows çalışma zamanı yöntemleri [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) sınıfı &mdash; biraz yapılabilir kullanarak daha verimli [ `ConfigureAwait` ](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean)) geri kullanıcı arabirimi iş parçacığına geçiş önlemek için yöntemi.

### <a name="dont-block-the-ui-thread"></a>UI iş parçacığı engelleme!

Bazen kullanmaktan kaçının daha cazip `ContinueWith` veya `await` kullanarak [ `Result` ](xref:System.Threading.Tasks.Task`1.Result) yöntemleri özelliği. UI iş parçacığını engelleyen veya hatta uygulamayı kapatmak için bu kaçınılmalıdır.

## <a name="your-own-awaitable-methods"></a>Beklenebilir yöntemlerinizi

Biri olarak geçirerek bazı kod zaman uyumsuz olarak çalıştırabilirsiniz [ `Task.Run` ](xref:System.Threading.Tasks.Task.Run(System.Action)) yöntemleri. Çağırabilirsiniz `Task.Run` bazı ek yükü işler zaman uyumsuz bir yöntem içinde.

Çeşitli `Task.Run` desenleri aşağıda ele alınmıştır.

### <a name="the-basic-mandelbrot-set"></a>Temel Mandelbrot kümesi

Gerçek zamanlı olarak ayarlanmış Mandelbrot çizmek için [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığın bir [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) yapısı gösterilene benzer `System.Numerics` ad alanı.

[ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) örnek sahip bir `CalculateMandeblotAsync` yöntemi temel beyaz Mandelbrot kümesi hesaplar ve kullandığı arka plan kod dosyasındaki [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)üzerinde bir bit eşlem yerleştirmek için.

### <a name="marking-progress"></a>İlerleme işaretleme

Zaman uyumsuz bir yöntemden alınan ilerleme durumunu raporlamak örneği oluşturabilir bir [ `Progress<T>` ](xref:System.Progress`1) türünde bir bağımsız değişken için zaman uyumsuz bir yöntemi tanımlamak ve sınıf [ `IProgress<T>` ](xref:System.IProgress`1). Bu gösterilmiştir [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) örnek.

### <a name="cancelling-the-job"></a>İşi iptal ediliyor

Zaman uyumsuz bir yöntem edilebilen olmasını da yazabilirsiniz. Adlı bir sınıf ile başlayan [ `CancellationTokenSource` ](xref:System.Threading.CancellationTokenSource). [ `Token` ](xref:System.Threading.CancellationTokenSource.Token) Özelliktir türünde bir değer [ `CancellationToken` ](xref:System.Threading.CancellationToken). Bu zaman uyumsuz bir işleve geçirilir. Bir program [ `Cancel` ](xref:System.Threading.CancellationTokenSource.Cancel) yöntemi `CancellationTokenSource` (genellikle, kullanıcı tarafından bir eyleme yanıt) zaman uyumsuz işlev iptal etmek için.

Zaman uyumsuz yöntemin düzenli aralıklarla kontrol edebilirsiniz [ `IsCancellationRequested` ](xref:System.Threading.CancellationToken.IsCancellationRequested) özelliği `CancellationToken` ve özellik ise çıkış `true`, ya da yalnızca çağrı [ `ThrowIfCancellationRequested` ](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested) yöntemi, ile biten yöntem durumda bir [ `OperationCancelledException` ](xref:System.OperationCanceledException).

[ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) örnek edilebilen bir işlev kullanımını gösterir.

### <a name="an-mvvm-mandelbrot"></a>MVVM Mandelbrot

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) örnek daha geniş bir kullanıcı arabirimine sahiptir ve çoğunlukla dayanır bir [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) ve [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)sınıflar:

[![Üç ekran görüntüsü Mandelbrot X F](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>Web geri

[ `WebRequest` ](xref:System.Net.WebRequest) Bazı örneklerde kullanılan sınıfı zaman uyumsuz programlama modeli veya APM adlı eski moda bir zaman uyumsuz protokolünü kullanır. Sınıflardan birini kullanarak modern DOKUNUN Protokolü dönüştürebilirsiniz `FromAsync` yöntemleri [ `TaskFactory` ](xref:System.Threading.Tasks.TaskFactory`1) sınıfı. [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) örnek bunu gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 20 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Bölüm 20 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Dosyalarla Çalışma](~/xamarin-forms/app-fundamentals/files.md)
