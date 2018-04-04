---
title: Dosya sistemi ile çalışma
description: Xamarin.iOS aynı System.IO sınıfları dosyaları ve dizinleri herhangi bir .NET uygulamasında kullanacağınız iOS içinde çalışmak için kullanabilirsiniz. Ancak, bilinen sınıflar ve yöntemler rağmen iOS oluşturulan veya erişilen dosyalar üzerinde bazı kısıtlamalar uygular ve ayrıca özel özellikleri dizinleri belirli sağlar. Bu makalede, bu sınırlamaları ve özellikler açıklanmaktadır ve dosya erişimi bir Xamarin.iOS uygulaması nasıl çalıştığını gösterir.
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 5c6a5233c9cdc043986f106712895439fa008b41
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-file-system"></a>Dosya sistemi ile çalışma

_Xamarin.iOS aynı System.IO sınıfları dosyaları ve dizinleri herhangi bir .NET uygulamasında kullanacağınız iOS içinde çalışmak için kullanabilirsiniz. Ancak, bilinen sınıflar ve yöntemler rağmen iOS oluşturulan veya erişilen dosyalar üzerinde bazı kısıtlamalar uygular ve ayrıca özel özellikleri dizinleri belirli sağlar. Bu makalede, bu sınırlamaları ve özellikler açıklanmaktadır ve dosya erişimi bir Xamarin.iOS uygulaması nasıl çalıştığını gösterir._

Xamarin.iOS kullanabilirsiniz ve `System.IO` sınıfları *.NET temel sınıf kitaplığı (BCL)* iOS dosya sistemi erişmek için. `File` Sınıfı, oluşturma, silme ve dosyaları okumasına olanak sağlar ve `Directory` sınıfı oluşturamaz, silemez veya dizinleri içeriğini listeleme olanak tanır. Aynı zamanda `Stream` alt sınıfların bir büyük ölçüde dosya işlemleri (örneğin, bir dosya içinde sıkıştırma veya konumu arama) üzerinde denetim sağlar.

iOS uygulama bir uygulamanın verilerin güvenliğini korumak için ve kullanıcıları malignant uygulamalardan korumak için dosya sistemiyle yapabileceklerinizi bazı kısıtlamalar getirir. Bu kısıtlamalar parçası olan *uygulama Sandbox* – dosyaları, tercihleri, ağ kaynaklarına, donanım, vb. bir uygulamanın erişimi sınırlayan kurallar kümesi. Uygulamanın kendi ana dizini (yüklü konum); içindeki dosyaları okuma ve yazma sınırlıdır başka bir uygulamanın dosyalarını erişemiyor.

iOS ayrıca bazı dosya sistemine özgü özelliklere sahiptir: belirli dizinler yedeklemeler ve yükseltmeleri göre özel işleme gerektiren ve uygulamalar da iTunes aracılığıyla dosyaları paylaşma.

Bu makalede özellik ve iOS kısıtlamalarını ayrıntılı olarak dosya sistemi ve Xamarin.iOS bazı basit dosya sistemi işlemleri yürütmek için nasıl kullanılacağını gösteren örnek bir uygulama içerir:

 [![](file-system-images/05-sampleapp.png "Bazı basit dosya sistemi işlemleri yürütülürken iOS örneği")](file-system-images/05-sampleapp.png#lightbox)

 <a name="General_File_Access" />


## <a name="general-file-access"></a>Genel dosya erişimi

Xamarin.iOS .NET kullanmanıza olanak verir `System.IO` iOS dosya sistemi işlemleri için sınıflar.

Aşağıdaki kod parçacıkları bazı ortak işlemleri göstermektedir. Bulabilirsiniz tüm aşağıda bunları `SampleCode.cs` dosyasında bu makalede örnek uygulama.

 <a name="Working_with_directories" />


### <a name="working-with-directories"></a>Dizinleri ile çalışma

Bu kod alt dizinleri geçerli dizinde numaralandırır (tarafından belirtilen ". /" parametresi), uygulama yürütülebilir dosyasının konumuna olduğu.
Tüm dosya ve uygulamanız (hata ayıklarken konsol penceresinde görüntülenir) ile dağıtılan klasörlerin listesini, çıktı olacaktır.

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

 <a name="Reading_files" />


### <a name="reading-files"></a>Dosyaları okuma

Metin dosyası okuma için yalnızca tek satırlık bir kod gerekir. Bu örnek uygulama çıktı penceresinde bir metin dosyasının içeriğini görüntüler.

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

 <a name="XML_Serialization" />


### <a name="xml-serialization"></a>XML seri hale getirme

Tam ile çalışma rağmen `System.Xml` ad alanı bu makalenin kapsamı dışındadır, böyle bir StreamReader kullanarak kolayca bir XML belgesi dosya sisteminden seri durumdan çıkarabiliyorsa:

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

İçin MSDN belgelerine başvurun [System.Xml](http://msdn.microsoft.com/en-us/library/system.xml.aspx) hakkında daha fazla bilgi için ad alanı [seri hale getirme](http://msdn.microsoft.com/en-us/library/system.xml.serialization.aspx). Da gözden geçirmelisiniz [Xamarin.iOS belgelerine](~/ios/deploy-test/linker.md) bağlayıcı üzerinde – genellikle, eklemeniz gerekir `[Preserve]` özniteliği düşündüğünüz serileştirmek için sınıflar.

 <a name="Creating_Files_and_Directories" />


### <a name="creating-files-and-directories"></a>Dosyalar ve dizinler oluşturma

Bu örnek nasıl kullanılacağını göstermektedir `Environment` burada biz oluşturabilirsiniz dosyaları ve dizinleri belgeleri klasörüne erişmek için sınıf.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

Bir dizin oluşturma çok benzer bir işlemdir:

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

System.IO ad alanı hakkında daha fazla bilgi için bkz: [MSDN belgelerine](http://msdn.microsoft.com/en-us/library/system.io.aspx).


### <a name="serializing-json"></a>JSON seri hale getirme

JSON ile çalışan bir Xamarin.iOS uygulaması verilerde çok kolaydır kullanarak [Json.NET](http://www.newtonsoft.com/json) .NET NuGet paketi için yüksek performanslı JSON çerçevesi. Uygulamanızın projeye NuGet paketini eklemeniz yeterlidir: 

[![](file-system-images/json01.png "Uygulamaları projesi için NuGet paketi ekleme")](file-system-images/json01.png#lightbox)

Ardından, serileştirme/seri durumundan çıkarma için veri modeli olarak görev yapması için bir sınıf ekleyin (Bu durumda `Account.cs`):

```csharp
using System;
using System.Collections.Generic;
using Foundation; // for Preserve attribute, which helps serialization with Linking enabled

namespace FileSystem
{
    [Preserve]
    public class Account
    {
        #region Computed Properties
        public string Email { get; set; }
        public bool Active { get; set; }
        public DateTime CreatedDate { get; set; }
        public List<string> Roles { get; set; }
        #endregion

        #region Constructors
        public Account() {

        }
        #endregion
    }
}
```

Son olarak, bir örneğini oluşturmak `Account` sınıfı, json verilerini seri hale getirmek ve bir dosyaya yazma:

```csharp
// Create a new record
var account = new Account(){
    Email = "monkey@xamarin.com",
    Active = true,
    CreatedDate = new DateTime(2015, 5, 27, 0, 0, 0, DateTimeKind.Utc),
    Roles = new List<string> {"User", "Admin"}
};

// Serialize object
var json = JsonConvert.SerializeObject(account, Newtonsoft.Json.Formatting.Indented);

// Save to file
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "account.json");
File.WriteAllText(filename, json);
```
JSON için .NET 's başvuran [belgelerine](http://www.newtonsoft.com/json/help) bir .NET uygulamasında json verilerle çalışma hakkında daha fazla bilgi.

<a name="Special_Considerations" />

## <a name="special-considerations"></a>Özel durumlar

Xamarin.iOS ve .NET dosya işlemleri, iOS ve Xamarin.iOS arasındaki benzerlikler rağmen .NET bazı önemli farklar vardır.

 <a name="runtimeaccessible" />


### <a name="making-project-files-accessible-at-runtime"></a>Proje dosyalarını çalışma zamanında erişilebilir hale getirme

Varsayılan olarak, projeniz için bir dosya eklerseniz, son derlemesinde bulunan olmaz ve bu nedenle uygulamanız için kullanılamaz. Bütünleştirilmiş kodunda bir dosya eklemek için içerik denen bir özel yapı eylemiyle işaretlemeniz gerekir.

Bir dosya eklemek için işaretlenecek dosyaları üzerinde sağ tıklayın ve seçin **yapı eylemi &gt; içerik** Mac için Visual Studio'da Ayrıca değiştirebilirsiniz **yapı eylemi** dosyanın içinde **özellikleri** sayfası.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Büyük/küçük harfe duyarlılık

İOS dosya sistemi olduğunu anlamak önemlidir *büyük küçük harfe duyarlı*. Yani, dosya ve dizin adları tam olarak eşleşmelidir – README.txt ve readme.txt farklı dosya adları olarak kabul edilir.

Bu .NET geliştiricilerinin aşina olduğu Windows dosya sistemi ile daha karmaşık olabilir *büyük küçük harfe duyarlı*– "Dosyalar", "Dosyalar" ve "dosyalar" tüm başvurur aynı dizine.

Bu nedenle, iOS cihazları büyük/küçük harfe duyarlıdır ve kodunuzu ile göz önünde yazılması gerekir, ancak Simulator olan iOS duyarlı varsayılan durumda değil. Bu, dosya adı büyük/küçük harf dosyasını ve kod başvuruları arasında farklıysa, bu durum, gerçek bir aygıtta başarısız olmasına neden olabilir ancak bu kodunuzu hala benzeticisinde çalışabilir anlamına gelir. Bu neden erken ve genellikle iOS geliştirme sırasında gerçek bir aygıta dağıtmak önemli olduğunu nedenlerinden biridir.

 <a name="Path_Separator" />


### <a name="path-separator"></a>Yol ayırıcısı

iOS kullanan eğik çizgi '/' yol ayırıcısı olarak (ters eğik çizgi kullanan Windows'dan farklı ' \').

Kafa karıştırıcı bu farklılık nedeniyle kullanmak için iyi bir uygulamadır `System.IO.Path.Combine` geçerli platform, yerine stillerinizin belirli yol ayırıcısı ayarlar yöntemi. Bu kodunuzu diğer platformlar için daha kolay taşınabilir sağlayan basit bir adımdır.

 <a name="Application_Sandbox" />


## <a name="application-sandbox"></a>Uygulama korumalı alan

Dosya sistemi (ve diğer kaynakları ağ ve donanım özelliklerini gibi), uygulamanızın erişim güvenlik nedenleriyle sınırlıdır. Bu kısıtlama olarak bilinen *uygulama Sandbox*. Dosya sistemi bakımından uygulamanızı dosyaları ve dizinleri kendi ana dizini oluşturma ve silme sınırlıdır.

Giriş dizini benzersiz bir konuma uygulamanız ve tüm verilerin depolandığı dosya sisteminde ' dir. Seçin (değiştirdiğinizde, uygulamanız için giriş dizini konumunu veya olamaz); ancak iOS ve Xamarin.iOS özellikleri ve dosyalar ve dizinler içinde yönetmek için yöntemler sağlar.

 <a name="The_Application_Bundle" />


## <a name="the-application-bundle"></a>Uygulama paketi

*Uygulama paket* uygulamanızı içeren klasör.
Dizin adı eklenen .app soneki sağlayarak, diğer klasörlerinden ayrılır. Uygulama paketi yürütülebilir dosyanızı ve tüm içeriği (dosyaları, görüntüler, vb.) projeniz için gerekli içerir.

Mac OS, uygulama paketine göz atın, farklı bir simge ile diğer dizinlerde bakın (ve .app soneki gizli daha); görünür Ancak, yalnızca işletim sisteminin farklı görüntüleyen bir normal dizin olur.

Örnek kod için uygulama paketi görüntülemek için Visual Studio'da projeye Mac seçin için sağ tıklayın ve **içeren klasör Aç**. Ardından gidin **bin/Debug/** bulabileceğiniz bir uygulama simgesi (aşağıdaki ekran görüntüsüne benzer).

 [![](file-system-images/40-bundle.png "Uygulama simgesi bu ekran görüntüsüne benzer bulmak için bin/Debug gidin")](file-system-images/40-bundle.png#lightbox)

Bu simgeyi sağ tıklatın ve seçin **paket içeriklerini görüntüleme** uygulama paket dizinin içeriğini gidin. Aşağıda gösterildiği gibi içeriği yalnızca normal bir dizinin içeriğini gibi görünür:

 [![](file-system-images/45-bundle.png "Uygulama paketi içeriği")](file-system-images/45-bundle.png#lightbox)

Uygulama paketi simulator veya Cihazınızı test sırasında yüklenenler ve sonuçta için Apple App Store eklenmek üzere gönderildiğinde şeklindedir.

 <a name="Application_Directories" />


## <a name="application-directories"></a>Uygulama dizinleri

Uygulamanızı bir aygıta yüklendiğinde, işletim sistemi kendi ana dizini oluşturur ve, uygulama paket içine yerleştirir. Kodunuzu verileri okumak için uygulama paketi erişebilirsiniz, ancak hiçbir şey bu kök dizinine imzalanmış olduğu ve herhangi bir değişiklik uygulamanız geçersiz kılmak ve başlamasını engellemek yazılması gerekir.

Bu nedenle, hiçbir şey kök dizinine yazılması gereken rağmen <b>iOS 7 ve önceki içinde</b> çeşitli uygulama kök dizini içinde kullanılabilir dizinler oluşturur. <b>İOS 8 kullanıcı erişilebilir dizinleri olan <a href="https://developer.apple.com/library/ios/technotes/tn2406/_index.html" target="_blank">değil bulunan</a> uygulama kökü içinde</b>.

Bu dizinleri ve bunların amaçları aşağıda listelenmiştir:

&nbsp;

|Dizin|Açıklama|
|---|---|
|[ApplicationName] .app /|**İOS 7 ve önceki içinde** bu `ApplicationBundle` uygulama yürütülebilir dosyasının depolandığı dizin. Bu dizin (örneğin, görüntüler ve Mac projesi için Visual Studio'da kaynaklar olarak işaretledikten diğer dosya türleri) uygulamanızı oluşturduğunuz dizin yapısını bulunmaktadır.<br /><br />İçinde uygulama paket içerik dosyalarını erişmeniz gerekiyorsa, bu dizinin yolunu aracılığıyla kullanılabilir `NSBundle.MainBundle.BundlePath` özelliği.|
|Belge /|Kullanıcı belgeleri ve uygulama veri dosyalarını depolamak için bu dizin kullanın.<br /><br />Bu dizinin içindekileri kullanıcıya (Bu varsayılan olarak devre dışı bırakılmış olsa) paylaşımı iTunes dosya yoluyla kullanılabilir hale getirilebilir. Ekleme bir `UIFileSharingEnabled` Info.plist dosyasını kullanıcıların bu dosyalara erişmesine izin vermek için mantıksal anahtar.<br /><br />Bir uygulama, hemen dosya paylaşımı sağlamaz olsa bile, kullanıcıların bu dizinde gizleneceğini dosyaları yerleştirmekten kaçınmalısınız (veritabanı dosyaları gibi bunları paylaşmak istediğiniz sürece). Hassas dosyalar gizli kaldığı sürece bu dosyaları değil kullanıma sunulan (ve olması olası, değiştirilen veya silinen iTunes tarafından taşınan) sonraki bir sürümde dosya paylaşımı etkinse.<br /><br /> Kullanabileceğiniz `Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments)` uygulamanız için belgeleri dizinin yolunu almak için yöntemi.<br /><br />Bu dizinin içindekileri iTunes tarafından yedeklenir.|
|Kitaplığı /|Kitaplık dizinine doğrudan veritabanları veya diğer uygulama tarafından üretilen dosyalar gibi bir kullanıcı tarafından oluşturulmaz dosyalarını depolamak için uygun bir yerdir. Bu dizinin içindekileri hiçbir zaman kullanıcının iTunes aracılığıyla sunulur.<br /><br />Kitaplığı'nda, kendi alt dizinler oluşturabilirsiniz; Ancak, zaten var. tercihlerini ve önbellekleri dahil olmak üzere, farkında olmalıdır bazı sistem tarafından oluşturulan dizinler burada.<br /><br />(Dışında önbellekleri alt) bu dizinin içindekileri iTunes tarafından yedeklenir. Kitaplıkta oluşturduğunuz özel dizinler yedeklenecek.|
|Kitaplık/Tercihler /|Uygulamaya özgü tercih dosyaları bu dizinde depolanır. Bu dosyaları doğrudan oluşturmayın. Bunun yerine, kullanın `NSUserDefaults` sınıfı.<br /><br />Bu dizinin içindekileri iTunes tarafından yedeklenir.|
|Kitaplığı/önbellekleri /|Önbellekleri uygulamanızı yardımcı olabilecek veri dosyalarını depolamak için uygun bir yerdir çalıştırın, ancak, gerekirse kolaylıkla yeniden oluşturulabilir dizindir. Uygulama oluşturup gerektiğinde bu dosyaları silmek ve gerekirse, bu dosyalar yeniden oluşturabilirsiniz. Uygulama çalışırken, bunu yapmaz ancak iOS 5 (altında son derece düşük depolama durumlar), bu dosyalar da silebilir.<br /><br />Bu dizinin içindekileri bunlar kullanıcı cihazı yükler, mevcut olmaz anlamına gelir, iTunes tarafından yedeklenmez ve bunlar uygulamanızı güncelleştirilmiş bir sürümünü yükledikten sonra bulunmuyor olabilir.<br /><br />Örneği için uygulamanızın ağa bağlanamıyor durumunda, verileri veya iyi bir çevrimdışı deneyimi sağlamak için dosyaları saklamak için önbellekleri dizin kullanabilirsiniz. Uygulamayı kaydedin ve bu verileri hızlı bir şekilde ağ yanıtlar için beklenirken alabilir, ancak bunu yedeklenmesi gerekmez ve kolayca kurtarılan veya geri yükleme veya sürüm güncelleştirdikten sonra yeniden oluşturulacak.|
|tmp/|Uygulamalar, yalnızca bu dizindeki kısa bir süre için gereken geçici dosyaları depolayabilir. Artık gerekli olduklarında alanından tasarruf etmek için dosyaların silinmesi gerekir. Bir uygulama çalışmadığı zaman işletim sistemi dosyaları bu dizinden de silebilir.<br /><br />Bu dizinin içindekileri iTunes tarafından yedeklenmez.<br /><br />Örneğin, tmp dizininde (örneğin, Twitter Avatar veya e-posta eklerini) kullanıcıya görüntülenmesi için indirilen, ancak bunlar görüntülenebilir (ve gelecekte gerekliyse yeniden karşıdan sonra hangi silinemedi geçici dosyaları depolamak için kullanılabilir ).|

Bu ekran görüntüsü bir Bulucu penceresinde dizin yapısını gösterir:

 [![](file-system-images/08-library-directory.png "Bu ekran görüntüsü, bir Bulucu penceresinde dizin yapısını gösterir")](file-system-images/08-library-directory.png#lightbox)

 <a name="Accessing_Other_Directories_Programmatically" />

### <a name="accessing-other-directories-programmatically"></a>Diğer dizinlerde program aracılığıyla erişme

Erişilen önceki dizin ve dosya örnekler `Documents` dizin. Başka bir dizine gerekir oluşturmak yolu kullanarak yazmak için "..." aşağıda gösterildiği gibi sözdizimi:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

Bir dizin oluşturma çok benzerdir:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var directoryname = Path.Combine (library, "NewLibraryDirectory");
Directory.CreateDirectory(directoryname);
```

Yollara `Caches` ve `tmp` dizinler oluşturulmasını şöyle:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var cache = Path.Combine (documents, "..", "Library", "Caches");
var tmp = Path.Combine (documents, "..", "tmp");
```

 <a name="Sharing_Files_with_the_User_through_iTunes" />


## <a name="sharing-files-with-the-user-through-itunes"></a>İTunes aracılığıyla kullanıcıyla dosyaları paylaşma

Kullanıcıların, uygulamanızın belgeleri dizindeki dosyaların düzenleyerek erişebileceği `Info.plist` ve oluşturma bir **uygulamasının desteklediği iTunes paylaşımı** (`UIFileSharingEnabled`) girişi **kaynak** görünümü olarak Burada gösterilen:

 [![](file-system-images/09-uifilesharingenabled-plist.png "Uygulama ekleme özelliği paylaşımı iTunes destekler")](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

Bu dosyalar iTunes aygıtın bağlı olduğunu ve kullanıcının seçtiği erişilebilir `Apps` sekmesi. Örneğin, aşağıdaki ekran seçilen uygulama iTunes paylaşılan dosyaları gösterir:

 [![](file-system-images/10-itunes-file-sharing.png "Bu ekran, seçili uygulama iTunes paylaşılan dosyaları gösterir.")](file-system-images/10-itunes-file-sharing.png#lightbox)

Kullanıcılar yalnızca en üst düzey öğeler bu dizinde iTunes aracılığıyla erişebilir. (Bunlar bunları kendi bilgisayarlarına kopyalayabilir veya bunları silmek rağmen), herhangi bir alt dizine içeriğini göremezsiniz. Kullanıcıların iOS cihazlarını okuyabilmeniz Örneğin, GoodReader ile uygulama ile PDF ve EPUB dosyaları paylaşılabilir.

Dikkatli değilseniz, bunların belgeleri klasörünün içeriğini değiştirme kullanıcılar sorunlara neden olabilir. Uygulamanız bu dikkate alın ve Belgeler klasörünü bozucu güncelleştirmeleri dayanıklı olmasını gerekir.

Bu makale için örnek kod Belgeler klasöründe bir dosya ve klasör oluşturur (içinde **SampleCode.cs**) ve dosya paylaşımı sağlar **Info.plist** dosya. Bu ekran, bunlar iTunes nasıl göründüğünü gösterir:

 [![](file-system-images/15-itunes-file-sharing-example.png "Bu ekran görüntüsü, dosyaları iTunes nasıl göründüğünü gösterir")](file-system-images/15-itunes-file-sharing-example.png#lightbox)

Başvurmak [görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) oluşturduğunuz tüm özel belge türlerinin ve uygulama için simgeler ayarlama hakkında bilgi için makalenin.

Varsa `UIFileSharingEnabled` anahtar yanlış ya da mevcut değil, sonra olan dosya paylaşımı, devre dışı varsayılan olarak, ve kullanıcılar, Documentsdirectory ile etkileşim mümkün olmayacak.

 <a name="Backup_and_Restore" />

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Bir aygıt tarafından iTunes yedeklendiğinde, uygulamanızın giriş dizininde oluşturulan tüm dizinleri aşağıdakiler dışında kaydedilecek:

-   **[ApplicationName] .app** – uygulama paket *mu* , ancak paket (bir uygulama güncelleştirme, örneğin yüklendiğinde) değiştiğinde yedeklenir. Bu dizin yine de, imzalanmış olduğundan ve yüklemeden sonra değişmeden kalmalıdır değiştirmeniz gerekir. 
-   **Kitaplığı/önbellekleri** – önbellek dizini yedeklenmesi gerekmez çalışma dosyaları için tasarlanmıştır. 
-   **tmp** – bu dizin oluşturuldu ve artık gerekmediğinde silindi geçici dosyalar için kullanılan veya alanı gerektiğinde, iOS için dosyaları siler. 


Büyük miktarda veri yedekleme uzun sürebilir. Herhangi belirli bir belge veya verileri yedeklemek için ihtiyacınız karar verirseniz, uygulamanızın yalnızca belgeleri ve kitaplık kullanması gereken bu klasörler. Geçici veri veya ağ üzerinden kolayca alınabilir dosyaları için önbellekleri veya tmp dizininde kullanın.

'Bir aygıtı önemli ölçüde disk alanı azaldığında iOS dosya sistemi temizleyecek olduğunu' unutmayın. Bu işlem tüm dosyaları kitaplığı/önbellekleri tmp yönetimden kaldırır ve şu anda çalışmayan uygulamaların klasör.

 <a name="Complying_with_iOS5_iCloud_Backup_Restrictions" />

## <a name="complying-with-ios5-icloud-backup-restrictions"></a>İOS5 İcloud'a yedekleme kısıtlamaları uymak

Sunulan Apple *İcloud'a yedekleme* işlevselliği iOS 5 ile. İcloud'a yedekleme etkin olduğunda, uygulamanızın giriş dizinindeki tüm dosyalar (örneğin, uygulama paketi, genellikle yedeklenen değil dizinleri hariç `Caches` ve `tmp`) iCloud sunucuya yedeklenmiş. Cihazlarını kaybolması, çalınması zarar görmüş veya durumda bu özellik bir tam yedekleme kullanıcıyla sağlar.

İCloud yalnızca 5 Gb 'ücretsiz' alanı her kullanıcı ve gereksiz yere bant genişliği kullanmaktan kaçının olanak sağladığından, Apple yalnızca yedekleme temel kullanıcı tarafından oluşturulan veri uygulamalarını bekler. İOS veri depolama yönergelerine uymak için aşağıdaki öğeleri uygun olarak yüklemeyi yedeklenecek veri miktarını sınırlamanız gerekir:

-  Yalnızca kullanıcı tarafından oluşturulan veri ya da aksi takdirde, belgeleri dizininde (yedeklenmiş olan) yeniden oluşturulamaz veri depolayın. 
-  Kolayca yeniden oluşturulan ya da yeniden karşıdan başka verileri saklamak `Library/Caches` veya `tmp` (yedeklenen değil ve 'temizlendi '). 
-  İçin uygun olabilir dosyalarınız varsa `Library/Caches` veya `tmp` klasörü ancak istemiyorsanız 'temizlenmesi ', başka bir yerde saklayın (gibi `Library/YourData` ) ve Uygula ' değil geri Yukarı ' dosyaları İcloud'a yedekleme ba kullanmasını önlemek için özniteliği ndwidth ve depolama alanı. Dikkatle yönetebilmesi ve mümkün olduğunda silmek için bu veri alanı aygıta devam eder. 

' Değil geri Yukarı ' özniteliği kullanılarak ayarlanır `NSFileManager` sınıfı. Sınıfınıza olduğundan emin olun `using Foundation` ve arama `SetSkipBackupAttribute` şöyle:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

Zaman `SetSkipBackupAttribute` olan `true` dosya konumunda depolanır dizin bağımsız olarak yedeklenen, olmayacak (hatta `Documents` directory). Özniteliğini kullanarak sorgulama yapabilirsiniz `GetSkipBackupAttribute` yöntemi ve sıfırlama onu çağırarak `SetSkipBackupAttribute` yöntemiyle `false`, şöyle:

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>Veri arasında iOS uygulamaları ve uygulama uzantıları paylaşımı

Bir ana bilgisayar uygulaması (aksine, içeren uygulama) parçası olarak uygulama uzantıları çalıştırmak olduğundan, verilerin paylaşımını ek iş nedenle dahil otomatik değildir. Uygulama mekanizması iOS farklı uygulamaların veri paylaşımına izin ver kullanmasını gruplarıdır. Uygulamaların düzgün şekilde yapılandırıldığını doğru yetkilendirme ve sağlama dağıtıldıysa, kullanıcılar kendi normal iOS sandbox dışında paylaşılan dizine erişebilir.

### <a name="configure-an-app-group"></a>Bir uygulama grubu yapılandırma

Paylaşılan konumda kullanılarak yapılandırılmış bir [uygulama grubu](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), içinde yapılandırılmış **tanımlayıcıları & profilleri, sertifikaları** bölümünde [iOS Geliştirme Merkezi](https://developer.apple.com/devcenter/ios/). Bu değer, her projesinin başvurulmalıdır **Entitlements.plist**.

Oluşturma ve bir uygulama grubu yapılandırma hakkında daha fazla bilgi için bkz [uygulama grubu özellikleri](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Kılavuzu.

### <a name="files"></a>Dosyalar

İOS uygulaması ve uzantı ortak bir dosya yolu kullanarak dosyaları paylaşabilir (verilen düzgün yapılandırılmış doğru yetkilendirme ve sağlama):

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```
> [!IMPORTANT]
> Grup yolu döndürülür, `null`, yetkilendirmeler ve sağlama profilinin yapılandırmasını denetleyin ve doğru olduğundan emin olun.


<a name="Application_Version_Updates" />

## <a name="application-version-updates"></a>Uygulama sürümü güncelleştirmeleri

Uygulamanızı yeni bir sürümü yüklendiğinde, iOS yeni bir giriş dizini oluşturur ve yeni uygulama paket depolar. iOS sonra aşağıdaki klasörler, uygulama paket önceki sürümünden yeni giriş dizininize taşır:

-  **Belgeler**
-  **Kitaplığı**


Diğer dizinlerde de arasında kopyalanabilir ve yeni giriş dizininize altında put, ancak uygulamanız bu sistem davranışı kalmamanız gerekir böylece bunlar kopyalanacak garantili.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, dosya sistemi işlemleri herhangi bir uygulamayla diğer .NET gibi Xamarin.iOS ile basit gösterdi. Ayrıca, uygulama Sandbox sunulan ve neden güvenlik uygulamalarını inceledi. Ardından, bir uygulama paketini kavramı incelediniz. Son olarak, uygulamanızı kullanılabilir özelleştirilmiş dizinleri numaralandırılan ve uygulama yükseltmeleri ve yedekleme sırasında rollerine açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [FileSystemSampleCode](https://developer.xamarin.com/samples/FileSystemSampleCode/)
- [Dosya sistemi Programlama Kılavuzu](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [Dosyayı kaydetme uygulama destekler, türleri](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
