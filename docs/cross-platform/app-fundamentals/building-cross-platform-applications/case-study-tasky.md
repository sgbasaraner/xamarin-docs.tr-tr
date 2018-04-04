---
title: 'Örnek olay incelemesi: Tasky'
description: Bu belgede, platformlar arası uygulamalar oluşturma ilkeleri Tasky taşınabilir örnek uygulama nasıl uygulanmış olan açıklanmaktadır. Ortak kod yeniden kullanım için yazma ve hedef iOS, Android ve Windows Phone platformlarının platforma özgü projeleri uygulama mobil uygulama tasarımı üzerinde dokunur.
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f8e663ab2e274bff1ae8b700586d4c6749f04545
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="case-study-tasky"></a>Örnek olay incelemesi: Tasky

_Bu belgede, platformlar arası uygulamalar oluşturma ilkeleri Tasky taşınabilir örnek uygulama nasıl uygulanmış olan açıklanmaktadır. Ortak kod yeniden kullanım için yazma ve hedef iOS, Android ve Windows Phone platformlarının platforma özgü projeleri uygulama mobil uygulama tasarımı üzerinde dokunur._

*Tasky* *taşınabilir* basit bir Yapılacaklar listesi uygulamasıdır. Bu belgenin nasıl tasarlandığı ve, yönergeleri yerleşik anlatılmaktadır [platformlar arası uygulamalar oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) belge. Tartışma aşağıdaki alanları kapsamaktadır:

<a name="Design_Process" />

## <a name="design-process"></a>Tasarım işlemi

Oluşturma önerilir bir kodlama başlamadan önce elde etmek istediğiniz için yol haritası. Birden çok yolla sunulan işlevselliği oluşturmakta olduğunuz nerede platformlar arası geliştirme için özellikle geçerlidir. Ne kaydeder zaman ve çaba daha sonra geliştirme döngüsü oluşturmakta bir Temizle fikir başlayarak.

 <a name="Requirements" />

### <a name="requirements"></a>Gereksinimler

Bir uygulama tasarlamanın ilk adımı, istenen özelliklerini belirlemektir. Bu üst düzey hedefleri olabilir veya ayrıntılı kullanım örnekleri. Tasky basit işlevsel gereksinimlere sahiptir:

 -  Görev listesini görüntüle
 -  Ekleme, düzenleme ve görevleri silme
 -  Bir görevin durumunu 'Tamamlandı' olarak ayarlayın

Platforma özgü özellikleri kullanımınız göz önünde bulundurmalısınız.  Tasky iOS bölge sınırlaması ya da Windows Phone Canlı döşeme yararlanabilir mi? İlk sürümünde platforma özgü özellikleri kullanmayan olsa bile, iş ve veri katmanlarını uyum sağlayabileceğinden emin olmak şimdi planlamanız gerekir.

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>Kullanıcı arabirimi tasarımı

Hedef platformlar genelinde uygulanabileceği bir üst düzey tasarımı başlayın. Not platform belirli UI kısıtlamalara ilgilenebilmek. Örneğin, bir `TabBarController` Windows Phone eşdeğer dört adede kadar görüntüleyebilir ancak iOS beşten fazla düğmeleri görüntüleyebilirsiniz.
Tercih ettiğiniz (kağıt çalışır) aracını kullanarak ekran akış çizin.

 [![](case-study-tasky-images/taskydesign.png "Seçim kağıt works aracını kullanarak ekran akış çizme")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>Veri modeli

Hangi verilerin depolanması gereken bilerek kullanmak için hangi Kalıcılık mekanizması belirlemenize yardımcı olacaktır. Bkz: [platformlar arası veri erişimi](~/cross-platform/app-fundamentals/index.md) aralarında karar vermeyle ilgili Yardım ve kullanılabilir depolama alanı mekanizmaları hakkında bilgi için. Bu proje için biz SQLite.NET kullanırsınız.

Her 'TaskItem' için üç özelliklerini depolamak Tasky gerekir:

 -  **Ad** – dize
 -  **Notlar** – dize
 -  **Bitti** – Boole

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>Çekirdek İşlevsellik

Kullanıcı arabirimi gereksinimlerini karşılamak üzere kullanmak için gereken API göz önünde bulundurun. Yapılacaklar listesi aşağıdaki işlevleri gerektirir:

 -   **Tüm Görevler listesinde** – tüm kullanılabilir görevler ana ekran listesini görüntülemek için
 -  **Bir görev alma** – görev satır işlemdeki
 -  **Bir görevi kaydetmek** – görev düzenlendiğinde
 -  **Bir görevi silmek** – bir görev silindiğinde
 -  **Boş görev oluşturma** – yeni bir görev oluşturulduğunda

Kodu yeniden elde etmek için bu API bir kez de uygulanmalıdır *taşınabilir sınıf kitaplığı*.

 <a name="Implementation" />

### <a name="implementation"></a>Uygulama

Uygulama tasarımı anlaşılan sonra platformlar arası uygulaması olarak nasıl uygulanabilir göz önünde bulundurun. Bu uygulamanın mimarisi olur. Yer alan yönergeleri izleyerek [platformlar arası uygulamalar oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) belge, uygulama kodu olmalıdır bozuk aşağıdaki parçalara aşağı:

 -   **Ortak kodun** – görev verilerini depolamak için yeniden kullanışlı kodunu içerir; bir Model sınıfı ve bir API kaydetme yönetmek için ortak bir proje ve verileri yükleme.
 -   **Platforma özgü kodu** – ortak kodun 'arka uç' olarak kullanan bir yerel kullanıcı Arabirimi her işletim sistemi için uygulama platforma özgü projeleri.

 [![](case-study-tasky-images/taskypro-architecture.png "Platforma özgü projeleri ortak kodun arka ucu olarak kullanan bir yerel kullanıcı Arabirimi her işletim sistemi için uygulama")](case-study-tasky-images/taskypro-architecture.png#lightbox)

Bu iki bölümden oluşur aşağıdaki bölümlerde açıklanmıştır.

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>Ortak (PCL) kod

Tasky taşınabilir ortak kodun paylaşmak için taşınabilir sınıf kitaplığı stratejisi kullanır. Bkz: [paylaşımı kodu seçenekleri](~/cross-platform/app-fundamentals/code-sharing.md) belge kod paylaşımını seçenekleri açıklaması.

Veri erişim katmanı ve veritabanı kod sözleşmeleri dahil olmak üzere tüm ortak kodun kitaplığı projesinde yerleştirilir.

Tam PCL projeyi aşağıda gösterilmiştir. Tüm taşınabilir Kitaplığı'nda kod hedeflenen her platform ile uyumlu. Her yerel uygulama dağıtıldığında, bu kitaplığın başvurur.

![](case-study-tasky-images/portable-project.png "Her yerel uygulama dağıtıldığında, bu kitaplığın başvurur.")

Aşağıdaki sınıf diyagramı katmanı tarafından gruplandırılmış sınıflarını gösterir. `SQLiteConnection` Demirbaş kod Sqlite NET paketten bir sınıftır. Tasky için özel kod sınıfları kalan var. `TaskItemManager` Ve `TaskItem` sınıfları platforma özgü uygulamalarına sunulan API temsil eder.

 [![](case-study-tasky-images/classdiagram-core.png "Platforma özgü uygulamaları sunulan API TaskItemManager ve TaskItem sınıfları temsil eder")](case-study-tasky-images/classdiagram-core.png#lightbox)

Ayrı katmanlara ad alanlarını kullanma her katman arasında başvurular yönetmenize yardımcı olur. Platforma özgü projeleri yalnızca eklemeniz bir `using` iş katmanı bildirimi. Veri katmanı ve veri erişim katmanı tarafından sunulan API yalıtılan `TaskItemManager` iş katmanı içinde.

 <a name="References" />

### <a name="references"></a>Referanslar

Taşınabilir sınıf kitaplıkları her platform ve çerçeve özellikleri için destek değişen düzeylerine sahip birden çok platform genelinde kullanılabilir olması gerekir. Bu nedenle üzerinde paketleri ve framework kitaplıkları kullanılabilir sınırlamalar vardır. Örneğin, Xamarin.iOS c# desteklemez `dynamic` anahtar sözcüğü, dolayısıyla bu tür kodu Android üzerinde çalışır halde taşınabilir sınıf kitaplığı dinamik koduna bağlıdır herhangi bir paket kullanamaz. Mac için Visual Studio uyumsuz paket ve başvuruları ekleme engeller, ancak sınırlamaları beklenmeyen durumları daha sonra önlemek için göz önünde bulundurmanız istersiniz.

Not: Projelerinizi kullanmadığınız framework kitaplıkları başvuru görürsünüz. Bu başvuruları Xamarin proje şablonları bir parçası olarak eklenir. Uygulamaları derlendiğinde bağlama işlemini başvurulmayan kodu, bu nedenle rağmen kaldıracak `System.Xml` bırakıldı tüm Xml işlevler kullanmıyorsanız çünkü başvurulan, onu son uygulamada dahil edilmez.

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>Veri katmanı (DT)

Veri katmanı için bir veritabanı, düz dosyalar veya başka bir mekanizma olup olmadığını verilerin fiziksel depolama alanı – yapar kodunu içerir. Tasky veri katmanı iki bölümden oluşur: SQLite NET kitaplığı ve yukarı kablo eklenen özel kod.

Nesne İlişkisel eşleme (ORM) veritabanı arabirim sağlayan SQLite NET kodu katıştırmak için (Frank Kreuger tarafından yayımlanan) Sqlite net nuget paketini Tasky kullanır. `TaskItemDatabase` Sınıfının devraldığı `SQLiteConnection` ve gerekli oluşturma, okuma, güncelleştirme, okumak ve SQLite için veri yazmak için Sil (CRUD) yöntem ekler. Bu bir başka projelerde yeniden kullanılan genel CRUD yöntemler basit Demirbaş uygulamasıdır.

`TaskItemDatabase` Olan tüm erişim karşı aynı örneği oluştuğunu sağlayarak bir singleton. Kilit birden çok iş parçacığından eşzamanlı erişimi engellemek için kullanılır.

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>Windows Phone üzerinde SQLite

Windows Phone, iOS ve her ikisi de sevk Android işletim sisteminin bir parçası olarak SQLite ile, uyumlu bir veritabanı altyapısı içermez. Tüm üç platformlarda kod paylaşmak için bir SQLite Windows phone yerel sürümü gereklidir. Bkz: [yerel bir veritabanı ile çalışan](~/xamarin-forms/app-fundamentals/databases.md) Sqlite için Windows Phone projenizi ayarlama hakkında daha fazla bilgi için.

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>Veri erişimi genelleştirmek için bir arabirimi kullanma

Veri katmanı bağımlılık alır `BL.Contracts.IBusinessIdentity` böylece birincil anahtar gerektiren soyut veri erişim yöntemleri uygulayabilirsiniz. Arabirimini uygulayan herhangi bir iş katmanı sınıf sonra veri katmanda kalıcı.

Arabirimi yalnızca birincil anahtarı olarak hareket edecek bir tamsayı özelliği belirtir:

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

Taban sınıf arabirimini uygulayan ve bir otomatik artırma birincil anahtarı olarak işaretlemek için SQLite NET öznitelikler ekler. Bu temel sınıfını uygulayan Business katmanındaki herhangi bir sınıf sonra veri katmanda kalıcı:

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

Veri katmanı arabirimini kullanan genel yöntemlere bu örneğidir `GetItem<T>` yöntemi:

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>Eşzamanlı erişimi engellemek için kilitleme

A [kilit](http://msdn.microsoft.com/en-us/library/c5kehkcz(v=vs.100).aspx) içinde uygulanan `TaskItemDatabase` veritabanına eşzamanlı erişimi engellemek için sınıf. Bu farklı iş parçacıklarından eş zamanlı erişim seri hale getirilmiş sağlamaktır (Aksi halde UI bileşeni veritabanı arka plan iş parçacığı, güncelleştiriyor aynı anda okuma girişimi). Kilit nasıl uygulandığını örneği aşağıda gösterilmiştir:

```csharp
static object locker = new object ();
public IEnumerable<T> GetItems<T> () where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return (from i in Table<T> () select i).ToList ();
    }
}
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

Veri katmanı kodu çoğunu başka projelerde yeniden kullanılabilir. Katmandaki yalnızca uygulamaya özgü kod `CreateTable<TaskItem>` Çağır `TaskItemDatabase` Oluşturucusu.

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>Veri erişim katmanı (DAL)

`TaskItemRepository` Sınıfı yalıtır veren kesin türü belirtilmiş bir API ile veri depolama mekanizmasını `TaskItem` oluşturulacak nesneleri silinmiş, alınan ve güncelleştirildi.

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>Koşullu derleme kullanma

Dosya konumunu ayarlamak için koşullu derleme sınıfını kullanır - bu Platform Geçitler uygulayan bir örneğidir. Her platformda farklı koduna yolunu döndürür özelliği derler. Kod ve platforma özgü derleyici yönergeleri burada gösterilir:

```csharp
public static string DatabaseFilePath {
    get {
        var sqliteFilename = "TaskDB.db3";
#if SILVERLIGHT
        // Windows Phone expects a local path, not absolute
        var path = sqliteFilename;
#else
#if __ANDROID__
        // Just use whatever directory SpecialFolder.Personal returns
        string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
        // we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder
#endif
        var path = Path.Combine (libraryPath, sqliteFilename);
                #endif
                return path;
    }
}
```

Platforma bağlı olarak, çıktı olur "<app
path>/Library/TaskDB.db3" iOS, "<app
path>/Documents/TaskDB.db3" Android veya yalnızca "TaskDB.db3" Windows Phone için.

### <a name="business-layer-bl"></a>İş Katmanı (bl'yi Denetlemeye)

İş katmanı modeli sınıfları ve bunları yönetmek için bir cephesi uygular.
İçinde Tasky modeldir `TaskItem` sınıfı ve `TaskItemManager` yönetmek için bir API sağlamak için cephesi deseni uygular `TaskItems`.

 <a name="Façade" />

#### <a name="faade"></a>Cephesi

 `TaskItemManager` sarmalar `DAL.TaskItemRepository` Get sağlamak için Kaydet ve Delete UI katmanları ve uygulama tarafından başvurulan yöntemleri.

İş kuralları ve mantığı, burada gerekirse – örneğin bir nesne kaydedilmeden önce karşılanması gereken tüm doğrulama kuralları konulabilir.

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>Platforma özgü kodu için API

Ortak kodun yazıldıktan sonra toplamak ve tarafından kullanıma sunulan verileri görüntülemek için kullanıcı arabirimi oluşturulmalıdır. `TaskItemManager` Sınıfı erişmek için uygulama kodu için basit bir API sağlamak için cephesi deseni uygular.

Her bir platforma özgü projesi ile yazılan kod genellikle sıkı bir şekilde cihazın yerel SDK eşleştirilmek ve yalnızca tarafından tanımlanan API'yi kullanarak ortak kodun erişimi `TaskItemManager`. Bu yöntem içerir ve iş sınıfları, çıkarır, gibi `TaskItem`.

Görüntüleri değil platformları arasında paylaşılan ancak bağımsız olarak her projeye eklendi. Farklı dosya adları, dizinler ve çözümleri kullanarak farklı şekilde, her platform görüntüleri işleme olduğundan bu önemlidir.

Kalan bölümler Tasky UI platforma özgü uygulama ayrıntıları ele almaktadır.

 <a name="iOS_App" />

## <a name="ios-app"></a>iOS App

Sınıfları Tasky uygulama depolama ve verileri almak için ortak PCL project kullanarak iOS uygulamak için gereken sayıda vardır. Tam iOS Xamarin.iOS projesi aşağıda gösterilmiştir:

 ![](case-study-tasky-images/taskyios-solution.png "iOS projesi burada gösterilen")

Sınıfları katmanlara gruplandırılmış Bu diyagramda gösterilmektedir.

 [![](case-study-tasky-images/classdiagram-android.png "Sınıfları katmanlara gruplandırılmış Bu diyagramda gösterilmektedir")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Referanslar

İOS uygulaması platforma özgü SDK kitaplıkları – ör başvurur. Xamarin.iOS ve MonoTouch.Dialog-1.

Ayrıca başvurmalıdır `TaskyPortableLibrary` PCL projesi.
Başvuruları listesi aşağıda gösterilmiştir:

 ![](case-study-tasky-images/taskyios-references.png "Başvuruları listesi burada gösterilen")

Uygulama katmanı ve kullanıcı arabirimi katmanı bu başvurular kullanarak bu projede uygulanır.

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Uygulama Katmanı (AL)

Uygulama Katmanı 'kullanıcı arabirimine PCL tarafından sunulan nesneleri bağlamak için ' gerekli platforma özgü sınıflar içerir. İOS özel uygulama görevleri görüntülemek yardımcı olmak üzere iki sınıf vardır:

 -   **EditingSource** – Bu sınıf görevler listesi kullanıcı arabirimine bağlamak için kullanılır. Çünkü `MonoTouch.Dialog` kullanılan manyetik Sil işlevselliğini etkinleştirmek için bu yardımcı uygulamak ihtiyacımız için görev listesi, `UITableView` . İOS özel Proje uyguladığı tek olacak şekilde geçirme silme iOS, ancak Android veya Windows Phone üzerinde yaygındır.
 -   **TaskDialog** – Bu sınıf, tek bir görev için kullanıcı Arabirimi bağlamak için kullanılır. Kullandığı `MonoTouch.Dialog` yansıma 'sarmalamak için ' API `TaskItem` doğru bir şekilde biçimlendirilmesi giriş ekranı izin vermek için doğru özniteliklerini içeren bir sınıf olan nesne.

`TaskDialog` Sınıfını kullanan `MonoTouch.Dialog` ekran oluşturmak için öznitelikleri temel sınıf özellikleri. Sınıf şöyle görünür:

```csharp
public class TaskDialog {
    public TaskDialog (TaskItem task)
    {
        Name = task.Name;
        Notes = task.Notes;
        Done = task.Done;
    }
    [Entry("task name")]
    public string Name { get; set; }
    [Entry("other task info")]
    public string Notes { get; set; }
    [Entry("Done")]
    public bool Done { get; set; }
    [Section ("")]
    [OnTap ("SaveTask")]    // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Save;
    [Section ("")]
    [OnTap ("DeleteTask")]  // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Delete;
}
```

Bildirim `OnTap` öznitelikleri gerektiren bir yöntem adı – bu yöntemleri sınıfında bulunmalıdır nerede `MonoTouch.Dialog.BindingContext` oluşturulur (Bu durumda, `HomeScreen` sonraki bölümde açıklanan sınıfı).

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>Kullanıcı arabirimi Katmanı (UI)

Kullanıcı arabirimi katmanı aşağıdaki sınıflarını oluşur:

1.   **AppDelegate** – yazı tipleri ve renkler uygulamada kullanılan stilini belirlemek için Görünüm API çağrıları içerir. Çalışan hiçbir başlatma görevlerini şekilde basit bir uygulama tasky `FinishedLaunching` .
2.   **Ekranlar** – alt sınıflarının `UIViewController` her ekran ve davranışını tanımlayın. Ekranlar tie birlikte uygulama katmanı sınıflarla kullanıcı Arabirimi ve ortak API ( `TaskItemManager` ). Bu örnekte, kodda ekranlar oluşturulur ancak bunlar Xcode'nın arabirimi oluşturucu veya film şeridi Tasarımcısı kullanılarak tasarlanmış.
3.   **Görüntüleri** – görsel öğeleri her uygulamanın önemli bir parçası olan. Tasky, iOS için normal ve Retina çözüm sağlanmalıdır giriş ekranı ve simge görüntülerine sahiptir.

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Giriş ekranı

Giriş ekranı bir `MonoTouch.Dialog` ekran SQLite veritabanı görevlerden listesini görüntüler. Öğesinden devralınan `DialogViewController` ve ayarlamak için kod uygulayan `Root` koleksiyonu içerecek şekilde `TaskItem` görüntülenecek nesneleri.

 [![](case-study-tasky-images/ios-taskylist.png "DialogViewController devralan ve görüntülenecek TaskItem nesneleri koleksiyonu içerecek şekilde kök ayarlamak için kod uygular")](case-study-tasky-images/ios-taskylist.png#lightbox)

Görüntüleme ve görev listesi ile etkileşim ilgili iki ana yöntemler şunlardır:

1.   **PopulateTable** – iş katmanın kullanan `TaskManager.GetTasks` koleksiyonu almak için yöntemini `TaskItem` görüntülenecek nesneleri.
2.   **Seçili** – bir satır işlemdeki zaman içinde yeni bir ekran görev görüntüler.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Görev Ayrıntıları ekranı

Görev Ayrıntıları silinmiş veya düzenlenebilmesi için görevleri izin veren bir giriş ekranı olur.

Tasky kullandığı `MonoTouch.Dialog`'s yansıma ekranı için API böylece yoktur hiçbir `UIViewController` uygulama. Bunun yerine, `HomeScreen` sınıfı oluşturur ve görüntüler bir `DialogViewController` kullanarak `TaskDialog` uygulama katmanı sınıfından.

Bu ekran görüntüsü gösterilmektedir boş bir ekran gösterir `Entry` filigran metni ayarı özniteliği **adı** ve **notları** alanlar:

 [![](case-study-tasky-images/ios-taskydetail.png "Bu ekran adı ve Notlar alanları filigran metni ayarlama girdi özniteliğini gösterir boş bir ekran gösterir")](case-study-tasky-images/ios-taskydetail.png#lightbox)

İşlevselliğini **görev ayrıntıları** (örneğin, kaydetme veya görevi silme) ekran uygulanan, içinde `HomeScreen` sınıfı bu nerede olduğundan `MonoTouch.Dialog.BindingContext` oluşturulur. Aşağıdaki `HomeScreen` yöntemi, görev ayrıntıları ekran desteklemektedir:

1.   **ShowTaskDetails** – oluşturur bir `MonoTouch.Dialog.BindingContext` bir ekran işlenecek. Özellik adlarını almak yansıma kullanarak giriş ekranı oluşturur ve gelen türleri `TaskDialog` sınıfı. Ek bilgi, filigran metni için giriş kutularının gibi özellikleri özniteliklerle uygulanır.
2.   **SaveTask** – bu yöntem başvuru `TaskDialog` aracılığıyla sınıfı bir `OnTap` özniteliği. Aldığında çağrılan **kaydetmek** basıldığında ve kullandığı bir `MonoTouch.Dialog.BindingContext` kullanarak değişiklikleri kaydetmeden önce kullanıcının girdiği verileri almak üzere `TaskItemManager` .
3.   **DeleteTask** – bu yöntem başvuru `TaskDialog` aracılığıyla sınıfı bir `OnTap` özniteliği. Kullandığı `TaskItemManager` birincil anahtarı (ID özelliği) kullanarak verileri silmek için.

 <a name="Android_App" />

## <a name="android-app"></a>Android App

Tam Xamarin.Android projesi, aşağıda gösterilen:

 ![](case-study-tasky-images/taskyandroid-solution.png "Android projesi burada gösterilen")

Katman tarafından gruplandırılmış sınıflarla sınıf diyagramı:

 [![](case-study-tasky-images/classdiagram-android.png "Katman tarafından gruplandırılmış sınıflarla sınıf diyagramı")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Referanslar

Android uygulama projesi, Android SDK erişim sınıfları platforma özgü Xamarin.Android derleme başvurusu olmalıdır.

Bu ayrıca PCL proje (ör. başvurmalıdır TaskyPortableLibrary) ortak veri ve iş katmanı kodu erişmek için kullanılır.

 ![](case-study-tasky-images/taskyandroid-references.png "Ortak veri ve iş katmanı kod erişmek için TaskyPortableLibrary")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Uygulama Katmanı (AL)

İnceledik iOS sürüm benzer daha önce uygulama katmanı Android Sürüm 'kullanıcı arabirimine çekirdek tarafından sunulan nesneleri bağlamak için ' gerekli platforma özgü sınıflar içerir.

 **TaskListAdapter** – listesini görüntülemek için<T> ihtiyacımız özel nesneleri görüntülemek için bir bağdaştırıcıyı uygulamak için nesnelerin bir `ListView`. Hangi düzenin listedeki her bir öğe için kullanılan bağdaştırıcı denetimleri – bu durumda kod bir Android yerleşik düzeni kullanan `SimpleListItemChecked`.

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Kullanıcı Arabirimi (UI)

Android uygulamanızın kullanıcı arabirimi katmanı, kod ve XML Biçimlendirme birleşimidir.

 -   **Kaynakları/düzeni** – ekranı düzeni ve satır hücre tasarım AXML dosyaları olarak uygulanır. AXML el ile yazılmış veya yerleştirilmiş genişletme görsel olarak Android için Xamarin UI Tasarımcısı'nı kullanarak olabilir.
 -   **Kaynakları/Drawable** – görüntüleri (simgeler) ve özel düğme.
 -   **Ekranlar** – her ekran ve davranışını tanımlamak etkinlik alt sınıflar. Uygulama katmanı sınıflarla kullanıcı Arabirimi ve ortak API birbirine bağlar (`TaskItemManager`).

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Giriş ekranı

Bir etkinlik alt giriş ekranı oluşur `HomeScreen` ve `HomeScreen.axml` (düğmesi ve görev listesi konumu) düzeni tanımlayan dosya. Ekran şöyle görünür:

 [![](case-study-tasky-images/android-taskylist.png "Ekran şöyle")](case-study-tasky-images/android-taskylist.png#lightbox)

Giriş ekranı kod düğmesini tıklatarak ve listedeki öğeleri tıklatarak yanı listede doldurmak için işleyiciler tanımlar `OnResume` yöntemi (olan görev ayrıntıları ekranında yapılan değişiklikleri yansıtır şekilde). Veri, iş katmanın kullanılarak yüklenir `TaskItemManager` ve `TaskListAdapter` uygulama katmanından.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Görev Ayrıntıları ekranı

Oluşan ayrıca görev ayrıntıları ekranına bir `Activity` bir alt kümesi ve bir AXML düzeni dosyası. Düzen giriş denetimlerini konumunu belirler ve C# sınıfı yüklemek ve kaydetmek için davranışını tanımlayan `TaskItem` nesneleri.

 [![](case-study-tasky-images/android-taskydetail.png "Yük ve TaskItem nesneleri kaydetmek için davranış sınıfı tanımlar")](case-study-tasky-images/android-taskydetail.png#lightbox)

Tüm PCL kitaplığı aracılığıyla başvurusudur `TaskItemManager` sınıfı.

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Windows Phone Uygulama
Tam Windows Phone projesini:

 ![](case-study-tasky-images/taskywp7-solution.png "Windows Phone Uygulama tam Windows Phone projesini")

Aşağıdaki diyagram katmanlara gruplandırılmış sınıflarını gösterir:

 [![](case-study-tasky-images/classdiagram-wp7.png "Bu diyagramda katmanlara gruplandırılmış sınıflarını gösterir")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>Referanslar

Platforma özgü proje gerekli platforma özgü kitaplıklar başvurmalıdır (gibi `Microsoft.Phone` ve `System.Windows`) geçerli bir Windows Phone uygulama oluşturmak için.

Bu ayrıca PCL proje (ör. başvurmalıdır `TaskyPortableLibrary`) kullanmaya `TaskItem` sınıfı ve veritabanı.

 ![](case-study-tasky-images/taskywp7-references.png "TaskItem sınıfı ve veritabanı kullanmak üzere TaskyPortableLibrary")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Uygulama Katmanı (AL)

Yeniden, iOS ve Android sürümleriyle gibi uygulama katmanı veri kullanıcı arabirimine bağlamak için Yardım görsel olmayan öğeler oluşur.

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

ViewModels sarmalamak PCL verilerden ( `TaskItemManager`) ve Silverlight/XAML veri bağlama tarafından kullanılabilecek şekilde gösterir. Bu, platforma özgü davranış (platformlar arası uygulamalar belgede açıklandığı gibi) örneğidir.

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Kullanıcı Arabirimi (UI)

XAML biçimlendirme içinde bildirilen ve nesneleri görüntülemek için gereken kod miktarını azaltmak benzersiz bir veri bağlama özelliğine sahiptir:

1.   **Sayfaları** – XAML dosyaları ve bunların arkasındaki koda kullanıcı arabirimi tanımlayın ve ViewModels ve PCL proje verilerini toplamak ve görüntülemek için başvuru.
2.   **Görüntüleri** – giriş ekranı, arka plan ve simge görüntüleri olan kullanıcı arabirimi önemli bir parçası.

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

MainPage sınıfı kullandığı `TaskListViewModel` XAML'ın veri bağlama özellikleri kullanarak verileri görüntülemek için. Sayfanın `DataContext` zaman uyumsuz olarak doldurulmuş görünüm model olarak ayarlanmış. `{Binding}` XAML sözdiziminde verinin nasıl görüntüleneceğini belirler.

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>TaskDetailsPage

Her görev bağlama tarafından görüntülenen `TaskViewModel` TaskDetailsPage.xaml tanımlanan XAML için. Görev veriler aracılığıyla alınır `TaskItemManager` iş katmanı içinde.

 <a name="Results" />

## <a name="results"></a>Sonuçları

Sonuçta elde edilen uygulamalar her platformda şöyle:

 <a name="iOS" />

#### <a name="ios"></a>iOS

'Add' düğmesi gibi iOS standart kullanıcı arabirimi tasarımı uygulamanın kullandığı gezinti çubuğunda konumlandırılmış ve yerleşik kullanarak **artı (+)** simgesi. Ayrıca varsayılan kullanır `UINavigationController` 'geri' düğmesini davranışı ve tabloda 'manyetik-Sil' destekler.

 [![](case-study-tasky-images/ios-taskylist.png "Ayrıca varsayılan UINavigationController geri düğmesini davranışı kullanır ve tabloda manyetik delete destekler") ](case-study-tasky-images/ios-taskylist.png#lightbox) [ ![ ] (case-study-tasky-images/ios-taskylist.png "UINavigationController varsayılan de kullanır Düğme davranışı yedekleyin ve tabloda manyetik delete destekler")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

#### <a name="android"></a>Android

Android uygulaması yerleşik düzeni 'görüntülenen bir onay işareti' gerektiren satırların dahil olmak üzere yerleşik denetimlerini kullanır. Donanım/sistem geri davranışı bir ekran geri düğmesini ek olarak desteklenir.

 [![](case-study-tasky-images/android-taskylist.png "Bir ekran geri düğmesini yanı sıra donanım/sistem geri davranışı desteklenen")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "donanım/sistem geri davranışını ek olarak desteklenen bir ekran Geri düğmesi")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

#### <a name="windows-phone"></a>Windows Phone

Windows Phone Uygulama gezinme çubuğu üstünde yerine ekranın altındaki uygulama çubuğunda doldurma standart bir düzen kullanır.

 [![](case-study-tasky-images/wp-taskylist.png "Gezinme çubuğu üstünde yerine ekranın altındaki uygulama çubuğunda doldurma standart düzeni, Windows Phone Uygulama kullanan") ](case-study-tasky-images/wp-taskylist.png#lightbox) [ ![ ] (case-study-tasky-images/wp-taskylist.png "Windows Phone uygulaması standart kullanır Gezinme çubuğu üstünde yerine ekranın altındaki uygulama çubuğunda doldurma düzeni")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>Özet

Bu belge kodu yeniden kullanma üç mobil platformlarda kolaylaştırmak için basit bir uygulama katmanlı uygulama tasarımı ilkeleri nasıl uygulanmış ayrıntılı bir açıklama sağladı: iOS, Android ve Windows Phone.

Uygulama katmanları tasarlamak için kullanılan işlemi açıklanmıştır ve hangi kod ele alınan &amp; işlevselliği, her katmanda uygulandıktan.

Kod yüklenebilir [github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable).

## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası uygulamalar oluşturma (ana belge)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky taşınabilir örnek uygulaması (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
