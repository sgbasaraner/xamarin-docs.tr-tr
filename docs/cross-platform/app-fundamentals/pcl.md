---
title: Taşınabilir sınıf kitaplıkları giriş
description: Bu makalede, taşınabilir sınıf kitaplığı (PCL) projeleri tanıtır ve oluşturma ve Mac ve Visual Studio için Visual Studio projelerinde PCL tüketen anlatılmaktadır.
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: d80a4125d7447b19f001c349aff006dc4744f4a6
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-portable-class-libraries"></a>Taşınabilir sınıf kitaplıkları giriş

_Bu makalede, taşınabilir sınıf kitaplığı (PCL) projeleri tanıtır ve oluşturma ve Mac ve Visual Studio için Visual Studio projelerinde PCL tüketen anlatılmaktadır._


Platformlar arası uygulamalar oluşturma kilit bir bileşeni çeşitli platforma özgü projeler arasında kod paylaşamaz. Ancak, bu farklı platformlar genellikle farklı bir alt kümesi .NET temel sınıf kitaplığı (BCL) kullanır ve bu nedenle gerçekte farklı bir .NET Core kitaplığı profili yerleşik olarak karmaşık. Başka bir deyişle, her platform yalnızca her platform için ayrı Sınıf Kitaplığı projelerinde gerektirecek şekilde görünecekleri şekilde aynı profile, hedeflenen sınıf kitaplıkları kullanabilirsiniz.

Bu sorunu gidermek için kod paylaşımını üç ana yaklaşım vardır: **.NET standart projeleri**, **taşınabilir sınıf kitaplığı (PCL) projeleri**, ve **paylaşılan varlık projeleri**.

- **.NET standart projeleri** [.NET standart](~/cross-platform/app-fundamentals/net-standard.md).
-  **PCL** projeleri hedefleyen bir bilinen BCL sınıfları/özellikler kümesi desteği özel profiller. Ancak, bunlar genellikle kendi kitaplıklara profil belirli kodu ayırmak için fazladan mimari çaba gerektiren PCL için aşağı yan olur. Bu iki yaklaşımı hakkında daha ayrıntılı bilgi için bkz: [paylaşımı kodu seçenekleri Kılavuzu](~/cross-platform/app-fundamentals/code-sharing.md) .
-  **Paylaşılan varlık projeleri** kod bir çözüm içinde ve genellikle paylaşmak üzere hızlı ve basit bir yol kullanır (daha fazla bilgi için kullanacağı çeşitli platformlar için kod yolları belirtmek için koşullu derleme yönergeleri tek bir dizi dosyaları ve teklifleri kullanın bilgi [paylaşılan projeleri makale](~/cross-platform/app-fundamentals/shared-projects.md) ve [Xamarin platformlar arası çözüm Kılavuzu ayarı](~/cross-platform/app-fundamentals/code-sharing.md) ).


Bu sayfayı nasıl oluşturulacağını açıklar bir **PCL** sonra birden çok platforma özgü projeler tarafından başvurulan belirli bir profil hedefleyen projede.


## <a name="what-is-a-portable-class-library"></a>Taşınabilir sınıf kitaplığı nedir?

Bir uygulama veya bir kitaplık projesi oluşturduğunuzda, sonuçta elde edilen DLL için oluşturulan belirli bir platformda çalışmaya sınırlıdır. Bu, bir Windows uygulaması için bir derleme yazma ve ardından onu Xamarin.iOS ve Xamarin.Android yeniden kullanılarak engeller.

Ancak, bir taşınabilir sınıf kitaplığı oluşturduğunuzda, kodunuzu çalıştırmak istediğiniz platformları bir birleşimini seçebilirsiniz. Taşınabilir sınıf kitaplığı oluşturma sırasında yaptığınız Uyumluluk seçeneği kitaplığı desteklediği platformları tanımlayan bir "Profili" tanımlayıcısına çevrilir.

Aşağıdaki tabloda .NET platforma göre değişir özelliklerden bazıları gösterilir. Belirli aygıtlar/platformlarda çalıştırması garantili olan bir PCL derleme yazmak için proje oluşturduğunuzda, hangi desteği gereklidir seçmeniz yeterlidir.

|Özellik|.NET Framework|UWP uygulamaları|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|Çekirdek|Y|Y|Y|Y|Y|
|LINQ|Y|Y|Y|Y|Y|
|IQueryable|Y|Y|Y|7.5 +|Y|
|Serileştirme|Y|Y|Y|Y|Y|
|Veri ek açıklamaları|4.0.3 +|Y|Y||Y|

Xamarin sütun Xamarin.iOS ve Xamarin.Android destekleyen Visual Studio ile gönderilen tüm profiller ve oluşturduğunuz kitaplıkları özelliklerin kullanılabilirliği desteklemek için seçtiğiniz diğer platformlar tarafından yalnızca sınırlı olgu yansıtır.

Bu birleşimleridir profilleri içerir:

-  .NET 4 veya .NET 4.5
-  Silverlight 5
-  Windows Phone 8
-  UWP uygulamaları

Daha fazla bilgiyi farklı profilleri yetenekleri hakkında [Microsoft'un Web sitesi](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx) ve başka bir topluluk üyenin bkz [PCL profil özeti](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) desteklenen framework bilgileri ve diğer notlar içerir.



Kod paylaşmak için bir PCL oluşturma Artıları ve eksileri dosya bağlama alternatif karşı sayısı vardır:


**Avantajları**

1. Merkezi kod paylaşımını – yazmak ve diğer kitaplıkları ve uygulamaları tarafından kullanılabilecek tek bir projede kod sınayın.
1. Yeniden düzenleme işlemleri tüm kod etkiler (taşınabilir sınıf kitaplığı ve platforma özgü projeleri) çözümde yüklendi.
1. PCL proje kolayca bir çözümde diğer projeler tarafından başvurulabilir veya başkalarının kendi çözümlerinde başvurmak çıkış derlemesi paylaşılabilir.


**Dezavantajlar**

1. Aynı taşınabilir sınıf kitaplığı birden çok uygulama arasında paylaşıldığından, platforma özgü kitaplıkları (ör. başvurulamaz Community.CsharpSqlite.WP7).
1. Taşınabilir sınıf kitaplığı alt Aksi durumda MonoTouch ve Mono (örneğin, DllImport veya System.IO.File) Android için kullanılabilir olacak sınıfları içermeyebilir.



Bazı ölçüde taşınabilir Sınıf Kitaplığı'nda tanımlanan bir arabirim veya temel sınıf karşı platform projelerinde gerçek uygulama kodu için sağlayıcı desenini veya bağımlılık ekleme kullanarak hem olumsuz atlatılabilir.



Bu diyagramda kod paylaşmak için bir taşınabilir sınıf kitaplığı kullanma, ancak aynı zamanda platforma bağımlı özellikleri geçirmek için bağımlılık ekleme kullanılarak platformlar arası uygulama mimarisi gösterilmektedir:



[![](pcl-images/image1.png "Kod paylaşmak için bir taşınabilir sınıf kitaplığı kullanma, ancak aynı zamanda platforma bağımlı özellikleri geçirmek için bağımlılık ekleme kullanılarak platformlar arası uygulama mimarisi Bu diyagramda gösterilmektedir")](pcl-images/image1.png#lightbox)



# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio Mac gözden geçirme


Bu bölümde oluşturmak ve Mac için Visual Studio kullanarak bir taşınabilir sınıf kitaplığı kullanma anlatılmaktadır Başvurmak için tam bir uygulama PCL örnek bölümüne.



### <a name="creating-a-pcl"></a>Bir PCL oluşturma


Çözümünüz için taşınabilir sınıf kitaplığı ekleme, normal bir kitaplık projesine ekleme ile çok benzer.


1. Yeni Proje iletişim kutusuna seçin `Multiplatform > Library > Portable Library` seçeneği


    ![](pcl-images/image2.png "Multiplatform > kitaplığı > Taşınabilir Kitaplığı")


1. Mac için Visual Studio'da bir PCL oluşturulduğunda Xamarin.iOS ve Xamarin.Android için çalışan bir profille otomatik olarak yapılandırılır. PCL projesi bu ekran görüntüsünde gösterildiği gibi görünür:

    ![](pcl-images/image3.png "Bu ekran görüntüsünde gösterildiği gibi PCL proje görünür")

PCL artık kodun eklenmesi için hazırdır. Ayrıca diğer projeler (uygulama projeleri, kitaplık projeleri ve hatta diğer PCL projeleri) tarafından başvurulabilir.



### <a name="editing-pcl-settings"></a>PCL ayarlarını düzenleme


Bu proje için PCL ayarlarını görüntülemek ve değiştirmek için projeye sağ tıklayın ve seçin **Seçenekleri > Yapı > Genel** burada gösterilen ekranı görmek için:



[![](pcl-images/image4.png "Bu proje için PCL ayarlarını görüntülemek ve değiştirmek için projeye sağ tıklayın ve seçenekleri yapı burada gösterilen ekranı görmek için genel seçin")](pcl-images/image4.png#lightbox)



Bu ekranda ayarları bu belirli PCL uyumlu hangi platformları denetler. Bu seçeneklerin hiçbirini değiştirme değiştirir ne özellikler kullanılabilir sırayla denetler bu PCL tarafından kullanılan profil taşınabilir kod içinde kullanılan.



Herhangi birini değiştirmek `Target Framework` seçenekleri otomatik olarak güncelleştirir `Current Profile`; uyumsuz seçenekler seçtiyseniz aynı zamanda bir uyarı ekranı görüntüler.



[![](pcl-images/image5.png "Hedef Framework seçeneklerinden herhangi birini değiştirmek otomatik olarak güncelleştirir geçerli profil uyumsuz seçenekler seçtiyseniz aynı zamanda bir uyarı ekranı görüntüler")](pcl-images/image5.png#lightbox)



Kod PCL eklendikten sonra profili değiştirdiyseniz, kod profil yeni seçili parçası olmayan özellikleri başvuruyorsa kitaplığı artık derlenir mümkündür.


## <a name="working-with-a-pcl"></a>Bir PCL ile çalışma


Kod PCL kitaplığa yazıldığında Mac Düzenleyicisi için Visual Studio seçilen profil sınırlamaları tanıması ve otomatik tamamlama seçenekleri uygun şekilde ayarlayın. Örneğin, Mac için Visual Studio'da kullanılan varsayılan profili (Profile136) kullanarak bu ekran System.IO otomatik tamamlama seçeneklerini gösterir – kullanılabilir sınıfların yaklaşık yarısını görüntülenir gösteren scrollbar dikkat edin (aslında vardır yalnızca 14 sınıflar kullanılabilir).



[![](pcl-images/image6.png "Visual Studio'da kullanılabilir sınıfların yaklaşık yarısını aslında var. görüntülenme şeklini gösteren scrollbar sınıflardır yalnızca 14 kullanılabilir Mac uyarısı için kullanılan Profile136 varsayılan profili kullanarak g/ç")](pcl-images/image6.png#lightbox)



Bir Xamarin.iOS veya Xamarin.Android projesinde – otomatik tamamlama System.IO ile olduğunu kullanılabilir dahil yaygın olarak kullanılan sınıflar gibi 40 sınıfları karşılaştırmak `File` ve `Directory` olmayan herhangi bir PCL profil.



[![](pcl-images/image7.png "Yaygın olarak dahil mevcut hiçbir PCL profili olmayan dosya ve dizin gibi sınıflara kullanılan 40 sınıfları vardır")](pcl-images/image7.png#lightbox)



Bu PCL kullanmanın temel dengelemeyi yansıtır – tüm olası platformlarda karşılaştırılabilir uygulamaları olmadığından belirli API'leri için kullanılabilir değil kod sorunsuz olarak birçok platformda paylaşmak yeteneği anlamına gelir.



### <a name="using-pcl"></a>PCL kullanma


PCL Proje oluşturulduktan sonra aynı şekilde normalde başvurular ekleyin herhangi uyumlu bir uygulama veya kitaplık projeye ait bir başvuru ekleyebilirsiniz. Visual Studio'da Mac için başvurular düğümünü sağ tıklatın ve seçin `Edit References…` gösterildiği gibi projeleri sekmesine geçmenizi sağlar:



[![](pcl-images/image8.png "Mac için Visual Studio, başvuruları düğümüne sağ tıklayın ve düzenlemek başvuruları seçin, ardından gösterildiği gibi projeleri sekmesine geçin")](pcl-images/image8.png#lightbox)



Aşağıdaki ekran görüntüsünde Xamarin.iOS projesi PCL kitaplığı alt ve o PCL kitaplığına bir başvuru gösteren TaskyPortable örnek uygulama için çözüm paneli gösterir.



[![](pcl-images/image9.png "TaskyPortable örnek uygulama için çözüm paneli")](pcl-images/image9.png#lightbox)



Bir PCL çıktısını (IE. sonuçta elde edilen DLL derleme) de çoğu projeler başvuru olarak eklenebilir. Bu PCL platformlar arası bileşenleri ve kitaplıkları sevk için ideal bir yöntem sağlar.




# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)




## <a name="visual-studio-walkthrough"></a>Visual Studio'da izlenecek yollar


Bu bölümde, Visual Studio kullanarak bir taşınabilir sınıf kitaplığı oluşturmak nasıl aracılığıyla anlatılmaktadır. Başvurmak için tam bir uygulama PCL örnek bölümüne.



### <a name="creating-a-pcl"></a>Bir PCL oluşturma


Visual Studio çözümünüzde bir PCL ekleme, normal bir proje eklemek için biraz farklıdır.



1. Yeni Proje Ekle ekranında şunları seçin `Portable Class Library` seçeneği


    ![](pcl-images/image10.png "Taşınabilir sınıf kitaplığı")


1. Profil yapılandırılabilir böylece visual Studio için aşağıdaki iletişim kutusunu hemen ister.
 Tamam'a basın ve desteklemek için gereken platformlar değer.


    ![](pcl-images/image11.png "Tamam'a basın ve desteklemek için gereken platformlar değer")


1. PCL proje Çözüm Gezgini'nde gösterildiği gibi görünür. Başvurular düğümünü kitaplığı (PCL profili tarafından tanımlanan) .NET Framework'ün bir alt kullandığını belirtir.

    ![](pcl-images/image12.png "NET Framework PCL profili tarafından tanımlanan")

PCL artık kodun eklenmesi için hazırdır. Ayrıca diğer projeler (uygulama projeleri, kitaplık projeleri ve hatta diğer PCL projeleri) tarafından başvurulabilir.



### <a name="editing-pcl-settings"></a>PCL ayarlarını düzenleme


PCL ayarları görüntülenebilir ve projeye sağ tıklayıp seçerek değiştirilen **Özellikler > Kitaplığı** bu ekran görüntüsünde gösterildiği gibi:



[![](pcl-images/image13.png "PCL ayarları görüntülenebilir ve bu ekran görüntüsünde gösterildiği gibi projeye sağ tıklayıp özellikler kitaplığı seçerek değiştirildi")](pcl-images/image13.png#lightbox)



Kod PCL eklendikten sonra profili değiştirdiyseniz, kod profil yeni seçili parçası olmayan özellikleri başvuruyorsa kitaplığı artık derlenir mümkündür.



### <a name="working-with-a-pcl"></a>Bir PCL ile çalışma


Kod PCL kitaplığa yazıldığında, Visual Studio seçilen profil sınırlamaları tanımak ve IntelliSense seçenekleri uygun şekilde ayarlayın. Örneğin, varsayılan profili (Profile136) kullanarak bu ekran System.IO otomatik tamamlama seçeneklerini gösterir – kullanılabilir sınıfların yaklaşık yarısını görüntülenir gösteren scrollbar dikkat edin (gerçekte yalnızca 14 sınıfları kullanılabilir yok).



[![](pcl-images/image14.png "Varsayılan profili Profile136 kullanarak g/ç")](pcl-images/image14.png#lightbox)



Normal bir proje ile – otomatik tamamlama System.IO ile olduğunu kullanılabilir dahil yaygın olarak kullanılan sınıflar gibi 40 sınıfları karşılaştırmak `File` ve `Directory` olmayan herhangi bir PCL profil.



[![](pcl-images/image15.png "Normal projesinde otomatik olarak tamamlama")](pcl-images/image15.png#lightbox)



Bu PCL kullanmanın temel dengelemeyi yansıtır – tüm olası platformlarda karşılaştırılabilir uygulamaları olmadığından belirli API'leri için kullanılabilir değil kod sorunsuz olarak birçok platformda paylaşmak yeteneği anlamına gelir.



### <a name="using-pcl"></a>PCL kullanma


PCL Proje oluşturulduktan sonra aynı şekilde normalde başvurular ekleyin herhangi uyumlu bir uygulama veya kitaplık projeye ait bir başvuru ekleyebilirsiniz. Visual Studio'da başvurular düğümünü sağ tıklatın ve seçin `Add Reference...` için geçiş **çözüm: projeleri** sekmesinde gösterildiği gibi:



[![](pcl-images/image16.png "Gösterildiği gibi Projeler sekmesi")](pcl-images/image16.png#lightbox)



Aşağıdaki ekran görüntüsünde Xamarin.iOS projesi PCL kitaplığı alt ve o PCL kitaplığına bir başvuru gösteren TaskyPortable örnek uygulama için çözüm bölmesinde gösterilir.



[![](pcl-images/image17.png "TaskyPortable örnek uygulama için çözüm bölmesi")](pcl-images/image17.png#lightbox)



Bir PCL çıktısını (IE. sonuçta elde edilen DLL derleme) de çoğu projeler başvuru olarak eklenebilir.
Bu PCL platformlar arası bileşenleri ve kitaplıkları sevk için ideal bir yöntem sağlar.




-----



## <a name="pcl-example"></a>PCL örneği


[TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) örnek uygulama Xamarin ile taşınabilir sınıf kitaplığı nasıl kullanılabileceğini gösterir.
Bazı ekran görüntüleri iOS, Android ve Windows Phone çalıştıran ortaya çıkan uygulamalar şunlardır:



[![](pcl-images/image18.png "İOS, Android ve Windows Phone çalıştıran ortaya çıkan uygulamaları bazı ekran görüntüleri İşte")](pcl-images/image18.png#lightbox)



Tamamen taşınabilir kodu verileri ve mantığı sınıfların sayısı paylaşır ve ayrıca bir SQLite veritabanı uygulamasını bağımlılık ekleme kullanılarak platforma özgü gereksinimler içerecek şekilde nasıl gösterir.




Çözüm yapısı aşağıda verilmiştir (Mac ve Visual Studio için Visual Studio'da sırasıyla):



[![](pcl-images/image19.png "Çözüm yapısı burada Visual Studio'da Mac ve Visual Studio için sırasıyla gösterilir")](pcl-images/image19.png#lightbox)



Tanıtım amacıyla yeniden bir Özet sınıf bir taşınabilir sınıf kitaplığı derlenebilir için SQLite NET kodu platforma özgü parça (SQLite uygulamaları her farklı işletim sistemi üzerinde çalışmak için) sahip olduğundan ve iOS ve Android projeleri sınıflarında olarak uygulanan gerçek kodu.



### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

Taşınabilir sınıf kitaplığı destekleyebileceği .NET özelliklerinde sınırlıdır. Birden fazla platformda çalışacak derlendiğinden yapamazsınız kullanımı `[DllImport]` SQLite-NET kullanılan işlev. Bunun yerine SQLite NET bir soyut sınıfı olarak uygulanan ve paylaşılan kod rest başvurulan. Bir ayıklama soyut API aşağıda gösterilmiştir:


```csharp
public abstract class SQLiteConnection : IDisposable {

    public string DatabasePath { get; private set; }
    public bool TimeExecution { get; set; }
    public bool Trace { get; set; }
    public SQLiteConnection(string databasePath) {
         DatabasePath = databasePath;
    }
    public abstract int CreateTable<T>();
    public abstract SQLiteCommand CreateCommand(string cmdText, params object[] ps);
    public abstract int Execute(string query, params object[] args);
    public abstract List<T> Query<T>(string query, params object[] args) where T : new();
    public abstract TableQuery<T> Table<T>() where T : new();
    public abstract T Get<T>(object pk) where T : new();
    public bool IsInTransaction { get; protected set; }
    public abstract void BeginTransaction();
    public abstract void Rollback();
    public abstract void Commit();
    public abstract void RunInTransaction(Action action);
    public abstract int Insert(object obj);
    public abstract int Update(object obj);
    public abstract int Delete<T>(T obj);

    public void Dispose()
    {
        Close();
    }
    public abstract void Close();

}
```


Paylaşılan kod kalanı soyut sınıf "depolama" ve "nesneleri veritabanından alma" için kullanır. Bu Özet sınıf kullanan herhangi bir uygulamada biz gerçek veritabanı işlevselliği sağlayan tam bir uygulama geçmesi gerekir.



### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid ve TaskyiOS


İOS ve Android uygulaması projeleri kullanıcı arabirimi ve kablo li PCL paylaşılan kod için kullanılan diğer platforma özgü kod içerir.



Bu projeleri soyut veritabanı bu platformda çalıştığından API uygulaması da içerir. Uygulama iOS ve Android Sqlite veritabanı altyapısı işletim sisteminde yerleşik olduğundan kullanabilirsiniz `[DllImport]` somut bir uygulama veritabanı bağlantısı sağlamak için gösterildiği gibi. Platforma özgü uygulama kodunun bir alıntı burada gösterilir:


```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```


Tam uygulamayı örnek kodda görülebilir.

### <a name="taskywinphone"></a>TaskyWinPhone


Windows Phone Uygulama XAML ile oluşturulmuş kullanıcı Arabiriminde sahip ve kullanıcı arabirimiyle paylaşılan nesneler bağlanmak için başka bir platforma özgü kod içerir.



İOS ve Android için kullanılan uygulama aksine, Windows Phone Uygulama oluşturmalı ve bir örneğini kullanması `Community.Sqlite.dll` API özetindeki bir parçası olarak veritabanı. Kullanarak yerine `DllImport`, açık gibi yöntemleri başvuru Community.Sqlite derleme karşı uygulanır `TaskWinPhone` projesi. İOS ve Android sürümü yukarıdaki ile karşılaştırma için bir alıntı burada gösterilen


```csharp
public static Result Open(string filename, out Sqlite3.sqlite3 db)
{
    db = new Sqlite3.sqlite3();
    return (Result)Sqlite3.sqlite3_open(filename, ref db);
}

public static Result Close(Sqlite3.sqlite3 db)
{
    return (Result)Sqlite3.sqlite3_close(db);
}
```


## <a name="summary"></a>Özet


Bu makalede kısaca avantajları ve taşınabilir sınıf kitaplıkları Tuzaklar ele alınan, nasıl oluşturulacağını ve gelen PCLs tüketen gösterilen Mac ve Visual Studio; için Visual Studio içinde Son olarak eylem bir PCL gösteren tam bir örnek uygulama – TaskyPortable – sunulan ve.


## <a name="related-links"></a>İlgili bağlantılar

- [TaskyPortable (örnek)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Platformlar Arası Uygulamalar Oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Taşınabilir Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [Paylaşılan Projeler](~/cross-platform/app-fundamentals/shared-projects.md)
- [Paylaşım kodu seçenekleri](~/cross-platform/app-fundamentals/code-sharing.md)
- [.NET Framework (Microsoft) ile platformlar arası geliştirme](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
