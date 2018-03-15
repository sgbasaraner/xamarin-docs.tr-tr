---
title: "5 - stratejileri paylaşımı pratik kod parçası"
ms.topic: article
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 4c102181a1d2c345e0376f53f1f343cbc7be5551
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2018
---
# <a name="part-5---practical-code-sharing-strategies"></a>5 - stratejileri paylaşımı pratik kod parçası

Bu bölümde, uygulama senaryoları için kod paylaşmak nasıl örnekler verilmektedir.



## <a name="data-layer"></a>Veri katmanı

Veri katmanı, bir depolama alt yapısı ve okuma ve yazma bilgi yöntemleri oluşur. Performans, esneklik ve platformlar arası uyumluluk SQLite için veritabanı altyapısı Xamarin platformlar arası uygulamalar için önerilir.
Çok çeşitli Windows, Android, iOS ve Mac dahil platformlar üzerinde çalışır


### <a name="sqlite"></a>SQLite

SQLite bir açık kaynak veritabanı uygulamasıdır. Kaynak ve belgeler bulunabilir [SQLite.org](http://www.sqlite.org/). SQLite desteği her mobil platformda kullanılabilir:

-  **iOS** – işletim sistemi için yerleşik olarak bulunur.
- **Android** – işletim sistemine Android 2.2 (API düzey 10) bu yana yerleşik olarak bulunur.
- **Windows** – bkz [SQLite Evrensel Windows platformu uzantısı](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).


Hatta veritabanı altyapısı ile tüm platformlarda kullanılabilir, veritabanına erişmek için yerel yöntemleri farklıdır. Hem iOS ve Android kullanılabilecek SQLite Xamarin.iOS ya da Xamarin.Android erişmek için yerleşik API'ler sunar, ancak yerel SDK yöntemleri kullanarak kod (dışında dize olarak depolandıkları varsayılarak belki SQL sorguları kendileri) paylaşma olanağı sunar . Yerel veritabanı işlevselliği arama hakkındaki ayrıntılar için `CoreData` iOS veya Android'ın `SQLiteOpenHelper` sınıf; bu seçenekler platformlar arası değildir çünkü bunlar bu belgenin kapsamı dışındadır.



### <a name="adonet"></a>ADO.NET

Xamarin.iOS ve Xamarin.Android desteği `System.Data` ve `Mono.Data.Sqlite` (Xamarin.iOS bkz [belgelerine](~/ios/data-cloud/system.data.md) daha fazla bilgi için).
Bu ad alanlarını kullanma, her iki platformlarında çalışır ADO.NET kod yazmanıza olanak sağlar. İçerecek şekilde projenin başvurular Düzenle `System.Data.dll` ve `Mono.Data.Sqlite.dll` ve bunlar kodunuzu deyimlerini kullanarak ekleyin:

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

Aşağıdaki örnek kod sonra çalışır:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

ADO.NET gerçek uygulamaları farklı yöntemler ve sınıfları (Bu örnekte yalnızca tanıtım amacıyla,) üzerinden açıkça ayrılır.



### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET – platformlar arası ORM

Depolama alanı sınıfları modellenir basitleştirmek bir ORM (veya nesne ilişkisel Eşleyici) çalışır. Yerine el ile SQL sorguları bu tablolar oluşturun veya seçin, el ile gelen ayıklanan INSERT ve DELETE veri alanları sınıf yazıp özellikleri, bir ORM sizin için yapar kod bir katmanı ekler. Yansıma sınıflarınızı yapısını incelemek için kullanarak, tabloları ve eşleşen bir sınıf ve veri okumak veya yazmak için sorgular oluşturmak sütunları otomatik olarak bir ORM oluşturabilirsiniz. Bu, yalnızca başlık altında tüm SQL işlemleri ilgilenir ORM nesne örneklerine almak ve göndermek uygulama kodu sağlar.

SQLite NET kaydedin ve SQLite, sınıfları almak için izin veren basit bir ORM olarak görev yapar. Platform SQLite erişim ile birlikte derleyici yönergeleri ve diğer püf noktaları karmaşıklığını gizler.

SQLite NET özellikleri:

-  Tablolar, Model sınıflarına öznitelikleri eklenerek tanımlanır.
-  Bir veritabanı örneği öğesinin bir alt kümesi tarafından temsil edilen `SQLiteConnection` , SQLite Net Kitaplığı'nda ana sınıfı.
-  Sorgulanan ve silinen nesneleri kullanarak veri eklenebilir. SQL deyimleri (SQL deyimlerini gerekirse yazabileceğiniz rağmen) gereklidir.
-  Temel LINQ sorgularını SQLite-NET tarafından döndürülen koleksiyonlar üzerinde gerçekleştirilebilir.


Kaynak kodu ve belgeler SQLite NET için şu adreste [SQLite-Net github'da](https://github.com/praeclarum/sqlite-net) ve her iki olay incelemeleri uygulanmıştır. SQLite NET kod basit bir örnek (gelen *Tasky Pro* örnek olay incelemesi) aşağıda gösterilmiştir.

İlk olarak, `TodoItem` sınıfı, bir veritabanı birincil anahtarı olması için bir alan tanımlamak için öznitelikler kullanır:

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

Böylece bir `TodoItem` kodunu (ve hiçbir SQL deyimlerini) aşağıdaki satırla üzerinde oluşturulacak tabloya bir `SQLiteConnection` örneği:

```csharp
CreateTable<TodoItem> ();
```

Tablodaki verileri de yönetilebilir diğer yöntemleriyle üzerinde `SQLiteConnection` (yeniden gerektirmeden SQL deyimlerini):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

Tüm örnekleri için örnek olay incelemesi kaynak koduna bakın.



## <a name="file-access"></a>Dosya erişimi

Dosya erişimi herhangi bir uygulama önemli bir parçası olarak belirli. Bir uygulama Ekle parçası olabilecek dosyaları ortak örnekleri:

-  SQLite veritabanı dosyaları.
-  Kullanıcı tarafından oluşturulan veriler (metin, görüntüler, ses, video).
-  (Resimler, html veya PDF dosyaları) önbelleğe alma için karşıdan yüklenen veri.




### <a name="systemio-direct-access"></a>System.IO doğrudan erişim

Xamarin.iOS ve Xamarin.Android izin sınıflarda kullanarak dosya sistemi erişimi `System.IO` ad alanı.

Her platform dikkate alınması gereken farklı erişim sınırlamaları vardır:

-  iOS uygulamaları çok sınırlı dosya sistemi erişimi olan bir korumalı alan çalışır. Daha fazla Apple nasıl yedeklenen belirli konumlara (ve diğerleri değil) belirterek dosya sistemini kullanması gereken belirler. Başvurmak [Xamarin.iOS dosya sisteminde çalışan](~/ios/app-fundamentals/file-system.md) daha fazla ayrıntı için Kılavuzu.
-  Android de uygulamayla ilgili belirli dizinlere erişimi kısıtlayan, ancak Ayrıca Dış medyaya (ör.) destekler SD kart) ve Paylaşılan verilere erişme.
-  Windows Phone 8 (Silverlight) doğrudan erişim izin verme – dosyaları yalnızca yönetilebilir kullanarak `IsolatedStorage`.
-  Windows 8.1 WinRT ve Windows 10 UWP projeleri yalnızca zaman uyumsuz işlemleri aracılığıyla önerir `Windows.Storage` API'ları, ama diğer platformlarından farklıdır.

#### <a name="example-for-ios-and-android"></a>Örneğin, iOS ve Android

Yazar ve bir metin dosyasını okur önemsiz bir örnek aşağıda verilmiştir.
Kullanarak `Environment.GetFolderPath` aynı kod iOS ve Android döndürmesi her kendi dosya sistemi kurallarına göre geçerli bir dizin çalıştırılmasına izin verir.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.ReadAllText (filePath));
```

Xamarin.iOS başvuran [dosya sistemi ile çalışma](~/ios/app-fundamentals/file-system.md) iOS özel dosya sistemi işlevler hakkında daha fazla bilgi için belge. Platformlar arası dosya erişim kodu yazarken, bazı dosya sistemleri büyük küçük harfe duyarlıdır ve farklı dizin ayırıcı sahip unutmayın. Dosya adları için her zaman aynı büyük küçük harf kullanmak iyi bir uygulamadır ve `Path.Combine()` dosya veya dizin yolu oluşturulurken yöntemi.



### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows 8 ve Windows 10 için Windows.Storage

*Xamarin.Forms ile Mobile Apps oluşturma* [defteri](https://developer.xamarin.com/r/xamarin-forms/book/)
[Bölüm 20. Zaman uyumsuz ve dosya g/ç](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) içeren [Windows 8.1 ve Windows 10 için örnek](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

Kullanarak bir [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) okuyup dosyaları desteklenen API'leri kullanılarak bu platformlardaki mümkündür:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Başvurmak [defteri bölüm](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) daha fazla ayrıntı için.


<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Windows Phone 7 ve 8 (Silverlight) üzerinde yalıtılmış depolama

Yalıtılmış depolama kaydetme ve tüm iOS, Android ve daha eski Windows Phone platformlarını dosyaları yükleme ortak bir API içindir.

Windows Phone Xamarin.iOS ve Xamarin.Android ortak dosya erişim kodu izin vermek için yazılacak uygulanan (Silverlight) içinde dosya erişimi için varsayılan mekanizmadır. `System.IO.IsolatedStorage` Sınıfı, tüm üç platformlarda üzerinden başvurulabilir bir [paylaşılan proje](~/cross-platform/app-fundamentals/shared-projects.md).

Başvurmak [yalıtılmış depolama genel bakış için Windows Phone](http://msdn.microsoft.com/en-us/library/windowsphone/develop/ff402541(v=vs.105).aspx) daha fazla bilgi için.

Yalıtılmış Depolama API'leri kullanılamayan [taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md). PCL için bir alternatif [PCLStorage NuGet](https://pclstorage.codeplex.com/)



### <a name="cross-platform-file-access-in-pcls"></a>PCLs, platformlar arası dosya erişimi

Ayrıca bir PCL uyumlu Nuget – olan [PCLStorage](https://www.nuget.org/packages/PCLStorage/) – Xamarin desteklenen platformlar ve en son Windows API'ları bu tesisler platformlar arası dosya erişim.


## <a name="network-operations"></a>Ağ işlemleri

Çoğu mobil uygulamalar, ağ bileşeni, örneğin olacaktır:

-  Görüntü, video ve ses (ör. indirme küçük resimleri, fotoğraflar, müzik).
-  Belgeleri (ör. indirme HTML, PDF).
-  Kullanıcı verileri (örneğin, fotoğraf veya metin) yükleniyor.
-  Web Hizmetleri veya 3. taraf API'leri (dahil olmak üzere SOAP, XML veya JSON) erişme.


Ağ kaynaklarına erişmek için .NET Framework birkaç farklı sınıflar sağlar: `HttpClient`, `WebClient`, ve `HttpWebRequest`.

### <a name="httpclient"></a>HttpClient

`HttpClient` Sınıfını `System.Net.Http` ad alanı Xamarin.iOS, Xamarin.Android ve çoğu Windows platformları'de kullanılabilir. Var olan bir [Microsoft HTTP istemci kitaplığı Nuget](https://www.nuget.org/packages/Microsoft.Net.Http/) bu API taşınabilir sınıf kitaplıkları (ve Windows Phone 8 Silverlight) getirmek için kullanılabilir.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

`WebClient` Sınıfı, uzak sunuculardan uzak verileri almak üzere basit bir API sağlar.

Evrensel Windows Platofrm işlemleri *gerekir* Xamarin.iOS ve Xamarin.Android (hangi üzerinde arka plan iş parçacığı yapılabilir) zaman uyumlu işlemler desteği olsa bile, zaman uyumsuz olabilir.

Basit asychronous kodunu `WebClient` bir işlemdir:

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` Ayrıca `DownloadFileCompleted` ve `DownloadFileAsync` ikili veri almak için.

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` ' den daha fazla özelleştirme sunar `WebClient` ve sonuç olarak kullanmak için daha fazla kod gerektirir.

Zaman uyumlu basit bir kod `HttpWebRequest` bir işlemdir:

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

Bir örnekte yoktur bizim [Web Hizmetleri belgelerinde](~/cross-platform/data-cloud/web-services/index.md).

 <a name="Reachability" />


### <a name="reachability"></a>Ulaşılabilirlik

Mobil aygıtlar hızlı Wi-Fi ağ koşulları çeşitli veya 4 G bağlantıları zayıf alanları ve yavaş kenar veri bağlantıları altında çalışır. Bu nedenle, ağ kullanılabilir olup olmadığını ve bu nedenle, ne tür bir ağ uzak sunuculara bağlanmayı denemeden önce olup olmadığını algılamak için iyi bir uygulamadır.

Bu durumlarda mobil uygulama sürebilir eylemler şunlardır:

-  Ağ kullanılamıyorsa, kullanıcı öneriyoruz. Bunlar el ile (ör. devre dışı bırakmış varsa Uçak modu veya Wi-Fi kapatma) sorunu çözüp.
-  Bağlantı 3 G ise, uygulamaları farklı davranabilir (örneğin, Apple uygulamalar üzerinde 3 G indirilmesi 20 MB'tan büyük izin vermiyor). Uygulamalar, aşırı yükleme hakkında kullanıcıyı uyarmak için bu bilgileri kullanabilir büyük dosyalar alınırken zaman.
-  Ağ kullanılabilir olsa bile, hedef sunucunun diğer istekleri başlatmadan önce bağlantılarını doğrulamak iyi bir uygulamadır. Bu uygulamanın ağ işlemlerini zaman aşımına uğramadan art arda önlemek ve kullanıcıya görüntülenecek daha bilgilendirici bir hata iletisi de olanak sağlar.


Var olan bir [Xamarin.iOS örnek](https://github.com/xamarin/monotouch-samples/tree/master/ReachabilitySample) kullanılabilir (Apple üzerinde temel [ulaşılabilirlik örnek kod](http://developer.apple.com/library/ios/#samplecode/Reachability/Introduction/Intro.html) ) ağ kullanılabilirliğini saptamaya yardımcı olmak üzere.


## <a name="webservices"></a>WebServices

Belgelerimize bakın [Web Hizmetleri ile çalışma](~/cross-platform/data-cloud/web-services/index.md), hangi kapsayan REST, erişme Xamarin.iOS kullanarak SOAP ve WCF uç noktaları. Elle craft web istekleri mümkündür ve ancak kitaplık vardır bu Azure, RestSharp ve ServiceStack dahil olmak üzere daha kolay yapmak kullanılabilir yanıtları ayrıştırılamıyor. Xamarin uygulamaları bile temel WCF işlemleri erişilebilir.

### <a name="azure"></a>Azure

Microsoft Azure, çok çeşitli mobil uygulamalar, veri depolama ve eşitleme ve anında iletme bildirimleri gibi hizmetleri sağlayan bir bulut platformudur.

Ziyaret [azure.microsoft.com](https://azure.microsoft.com/) ücretsiz deneyin.

### <a name="restsharp"></a>RestSharp

RestSharp web hizmetlerine erişimi basitleştirir REST istemcisi sağlamak için mobil uygulamalarda bulunan bir .NET kitaplıktır. Veri isteği ve KALAN yanıt ayrıştırmak için basit bir API sağlayarak yardımcı olur. RestSharp yararlı olabilir

[RestSharp Web sitesi](http://restsharp.org/) içeren [belgelerine](https://github.com/restsharp/RestSharp/wiki) RestSharp kullanarak REST istemcisini uygulama konusunda.
RestSharp üzerinde Xamarin.iOS ve Xamarin.Android örnekler sağlar [github](https://github.com/restsharp/RestSharp/).

Ayrıca bir Xamarin.iOS kod parçacığında olduğu bizim [Web Hizmetleri belgelerinde](~/cross-platform/data-cloud/web-services/index.md).

 <a name="ServiceStack" />


### <a name="servicestack"></a>ServiceStack

RestSharp ServiceStack hizmetlere erişmek için mobil uygulamalarda uygulanabileceği bir istemci kitaplığı yanı sıra bir web hizmetini barındırmak için hem bir sunucu tarafı çözümüdür.

[ServiceStack Web sitesi](http://servicestack.net/) belge ve kod örnekleri için proje ve bağlantıları amacını açıklar. Örnek, bir web hizmeti ve bunun yanı sıra erişebilmesi için çeşitli istemci tarafı uygulamalar tam bir sunucu tarafı uygulama verilebilir.

Var olan bir [Xamarin.iOS örnek](http://www.servicestack.net/monotouch/remote-info/) ServiceStack Web sitesi ve bir kod parçacığında bizim [Web Hizmetleri belgelerinde](~/cross-platform/data-cloud/web-services/index.md).


### <a name="wcf"></a>WCF

Xamarin araçları, bazı Windows Communication Foundation (WCF) hizmetlerini kullanma yardımcı olabilir. Genel olarak, Xamarin Silverlight çalışma zamanı ile birlikte gelen WCF aynı istemci-tarafı alt destekler. Bu en yaygın kodlama ve protokolü uygulamaları WCF içerir: metin olarak kodlanmış SOAP iletileri HTTP üzerinden aktarım protokolünü kullanarak `BasicHttpBinding`.

Boyutuna ve WCF framework karmaşıklığı nedeniyle, Xamarin'ın istemci alt etki alanı tarafından desteklenen kapsamı dışında döner geçerli ve gelecekteki hizmet uygulamaları olabilir. Ayrıca, WCF Destek Araçları proxy oluşturmak için bir Windows ortamında yalnızca kullanılabilir kullanılmasını gerektirir.

 <a name="Threading" />


## <a name="threading"></a>İş Parçacığı Oluşturma

Mobil uygulamalar için uygulama yanıt hızını önemlidir – kullanıcılar uygulamaları yüklemek ve hızlı bir şekilde gerçekleştirmek için bekler. Kullanıcı girişini kabul durakları uygulama kilitlendi, ağ istekleri gibi uzun süre çalışan engelleme çağrıları veya yavaş yerel işlemleri (örneğin, bir dosya unzipping) ile kullanıcı Arabirimi iş parçacığı oluşturan tie değil önemlidir göstermek için görünür 'dondurulmuş' ekranı. Özellikle başlatma işlemi uzun süre çalışan görevler içermemelidir – tüm mobil platformlar yüklemek için çok uzun sürdüğü bir uygulama KILL.

Başka bir deyişle, 'İlerleme göstergesi' veya görüntülemek hızlı 'kullanışlı' Aksi durumda kullanıcı Arabirimi ve zaman uyumsuz görevleri arka plan işlemleri gerçekleştirmek için kullanıcı arabirimi uygulamalıdır. Arka plan görevleri yürütme iş parçacığı, kullanımını ilerlemeyi göstermek için geri ana iş parçacığına iletişim kurmak için bir yol arka plan görevleri gereksinimlerini anlamına gelir veya ne zaman tamamladınız gerektirir.

 <a name="Parallel_Task_Library" />


### <a name="parallel-task-library"></a>Görev paralel kitaplığı

Paralel görev kitaplığı ile oluşturulan görevler, zaman uyumsuz olarak çalıştırın ve uzun süre çalışan işlemleri kullanıcı arabirimi engellenmeden tetiklemek çok kullanışlı hale getirme kendi çağıran iş parçacığı üzerinde döndürür.

Basit paralel görev işlemi şuna benzeyebilir:

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

Anahtar `TaskScheduler.FromCurrentSynchronizationContext()` hangi yöntemini çağırarak iş parçacığı SynchronizationContext.Current yeniden kullanacaktır (burada çalıştıran ana iş parçacığı `MainThreadMethod`) o iş parçacığı geri çağrıları sıralamakta bir yolu olarak. Bu yöntemi kullanıcı Arabirimi iş parçacığı üzerinde çağrılırsa, onu çalışacağı anlamına gelir `ContinueWith` işlemi kullanıcı Arabirimi iş parçacığı yeniden açın.

Kod görevler diğer iş parçacıklarından başlıyorsanız, kullanıcı Arabirimi iş parçacığı bir başvuru oluşturmak için şu biçimi kullanın ve görev hala geri için arama:

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />


### <a name="invoking-on-the-ui-thread"></a>Kullanıcı Arabirimi iş parçacığı üzerinde çağırma

Paralel görev kitaplığı kullanan değil için kodu, her platform kullanıcı Arabirimi iş parçacığı dön hazırlama işlemleri için kendi sözdizimi vardır:

-  **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
-  **Android** – `owner.RunOnUiThread(action)`
-  **Xamarin.Forms** – `Device.BeginInvokeOnMainThread(action)`
-  **Windows** – `Deployment.Current.Dispatcher.BeginInvoke(action)`



İOS ve Android sözdizimi, kodu kullanıcı Arabirimi iş parçacığı üzerinde geri arama gerektiren tüm yöntemlerde bu nesneyi geçirmek gereken başka bir deyişle, kullanılabilir olması için bir 'context' sınıf gerektirir.

Paylaşılan kod: kullanıcı Arabirimi iş parçacığı arama yapmak için izleyin [IDispatchOnUIThread örnek](http://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (courtesy, [ @follesoe ](http://jonas.follesoe.no/)). Bildirme ve program bir `IDispatchOnUIThread` arabirim paylaşılan kodda ve ardından platforma özgü sınıflar aşağıda gösterildiği gibi uygulayın:

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Xamarin.Forms geliştiriciler kullanması gereken [ `Device.BeginInvokeOnMainThread` ](~/xamarin-forms/platform/device.md#Device_BeginInvokeOnMainThread) ortak kod (paylaşılan projeleri veya PCL).

 <a name="Platform_and_Device_Capabilities_and_Degradation" />


## <a name="platform-and-device-capabilities-and-degradation"></a>Platform, cihaz özellikleri ve düşüşü

Daha fazla farklı özellikler postalarla belirli örnekleri Platform özelliklerini belgelerde verilmiştir. Bu, farklı özellikler ve uygulama için tam olası bile çalışamaz iyi kullanıcı deneyimini sunmak için bir uygulama düzgün biçimde düşmeye nasıl algılama ile ilgilidir.
