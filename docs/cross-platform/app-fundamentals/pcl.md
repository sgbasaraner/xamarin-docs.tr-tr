---
title: Taşınabilir sınıf kitaplığı (PCL) giriş
description: Bu makalede, taşınabilir sınıf kitaplığı (PCL) projeleri tanıtır ve oluşturma ve PCL projeleri Visual Studio'da, Mac ve Visual Studio için kullanma gösterilmektedir.
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 83b1da5cd10a46b8480b0755eeb16bf7434a5906
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270095"
---
# <a name="portable-class-libraries-pcl"></a>Taşınabilir sınıf kitaplığı (PCL)

> [!WARNING]
> Taşınabilir sınıf kitaplıkları (PCLs), Visual Studio'nun en son sürümlerde kullanım dışı olarak kabul edilir.
> Hala açabilir, düzenleme ve PCLs derlemek, yeni projeler için bunu kullanmak için önerilir [.NET standart kitaplıkları](~/cross-platform/app-fundamentals/net-standard.md).

Platformlar arası uygulamalar oluşturmanın önemli bir bileşeni, çeşitli platforma özgü projeler arasında kod paylaşma çağrılabilmesidir. Ancak, bu farklı platformlar çoğunlukla farklı bir alt kümesi .NET temel sınıf kitaplığı (BCL) kullanır ve bu nedenle aslında farklı bir .NET Core kitaplığı profili oluşturulan olarak karmaşık. Başka bir deyişle, her platform yalnızca her platform için ayrı bir sınıf kitaplığı projeleri gerektirecek şekilde görünecekleri şekilde, aynı profile hedeflenen sınıf kitaplıkları kullanabilirsiniz.

Bu sorunu gidermek için kod paylaşımı üç ana yaklaşım vardır: **.NET Standard projelerine**, **paylaşılan varlık projeleri**, ve **taşınabilir sınıf kitaplığı (PCL) projeleri**.

- **.NET standard projelerine** .NET kod daha fazla bilgi edinin, paylaşımını için tercih edilen bir yaklaşım olan [.NET Standard projelerine ve Xamarin](~/cross-platform/app-fundamentals/net-standard.md).
- **Varlık projeleri paylaşılan** , kodu ve genellikle bir çözüm içinde paylaşmak hızlı ve basit bir yol (daha fazla bilgi için kullanacak çeşitli platformlara yönelik kod yollarını belirtmek için koşullu derleme yönergeleri kullanan tek bir kümesi sunar ve dosyaların kullanın bilgi [paylaşılan projeler makale](~/cross-platform/app-fundamentals/shared-projects.md)).
- **PCL** proje bilinen birtakım BCL sınıfları/özellikleri destekleyen özel profilleri hedefliyor. Ancak, bunlar genellikle belirli kod profili kendi kitaplığına ayırmak için Mimari fazladan çaba gerektirir PCL aşağı tarafı olur.

Bu sayfada nasıl oluşturulacağını açıklar bir **PCL** birden çok platforma özgü projeleri tarafından başvurulabilir belirli bir profil hedefleyen bir proje.

## <a name="what-is-a-portable-class-library"></a>Taşınabilir sınıf kitaplığı nedir?

Bir uygulama veya kitaplık projesi oluşturduğunuzda, ortaya çıkan DLL'yi için oluşturulan belirli bir platform üzerinde çalışmaya sınırlıdır. Bu, bir Windows uygulaması için bir bütünleştirilmiş kod yazma ve ardından, Xamarin.iOS ve Xamarin.Android yeniden kullanarak engeller.

Ancak, taşınabilir sınıf kitaplığı oluşturduğunuzda, kodunuzu çalıştırmak için istediğiniz platformları bir birleşimini seçebilirsiniz. Taşınabilir sınıf kitaplığı oluşturma sırasında yaptığınız Uyumluluk seçeneği, kitaplığı destekler hangi platformları tanımlayan bir "Profil" tanımlayıcısı çevrilir.

Aşağıdaki tabloda bazı özelliklerini .NET platforma göre farklılık gösterir. Belirli aygıtlar/platformlarda çalıştırması garantili olan bir PCL bütünleştirilmiş kod yazmak için projeyi oluşturduğunuzda hangi desteği gereklidir seçmeniz yeterlidir.

|Özellik|.NET Framework|UWP uygulamaları|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|Çekirdek|Y|Y|Y|Y|Y|
|LINQ|Y|Y|Y|Y|Y|
|IQueryable|Y|Y|Y|7.5 +|Y|
|Serileştirme|Y|Y|Y|Y|Y|
|Veri ek açıklamaları|4.0.3 +|Y|Y||Y|

Xamarin sütun Xamarin.iOS ve Xamarin.Android Visual Studio ile birlikte gelen tüm profilleri destekler ve oluşturduğunuz tüm kitaplıkları özelliklerinin kullanılabilirliği yalnızca desteklemeyi seçtiğiniz diğer platformlar tarafından sınırlandırılacak olgu yansıtır.

Bu, birleşimleridir profilleri içerir:

- .NET 4 veya .NET 4.5
- Silverlight 5
- Windows Phone 8
- UWP uygulamaları

Daha fazla bilgi edinebilirsiniz farklı profiller özellikleri hakkında [Microsoft'un Web sitesini](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx) ve başka bir topluluk üyesinin [PCL profilinin Özet](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) desteklenen framework bilgileri ve diğer notlar içerir.

**Avantajları**

1. Merkezi kod paylaşımı – yazmak ve diğer kitaplıkları ve uygulamaları tarafından kullanılabilecek tek bir projede kod test edin.
2. Yeniden düzenleme işlemleri tüm kod etkiler (taşınabilir sınıf kitaplığı ve platforma özgü projeleri) çözüm yüklendi.
3. PCL projesine kolaylıkla bir çözüm içindeki diğer projeleri tarafından başvurulabilir veya çıkış derlemesi diğerleri kendi çözümlerinde başvurmak için paylaşılabilir.

**Dezavantajlar**

1. Aynı taşınabilir sınıf kitaplığı birden çok uygulama arasında paylaşıldığından, platforma özgü kitaplıklar (örn. başvurulamaz Community.CsharpSqlite.WP7).
2. Taşınabilir sınıf kitaplığı alt, aksi takdirde MonoTouch hem de Android (örneğin, DllImport veya System.IO.file) için Mono kullanılabilirdi sınıfları içermeyebilir.

> [!NOTE]
> Taşınabilir sınıf kitaplıkları Visual Studio'nun en son sürümü kullanımdan kaldırıldı ve [.NET standart kitaplıkları](net-standard.md) yerine önerilir.

Bir dereceye kadar her iki dezavantajları kullanımınız taşınabilir Sınıf Kitaplığı'nda tanımlanan bir arabirim veya temel sınıf karşı platformu projelerinde kod sağlayıcısı desen veya bağımlılık ekleme kullanılarak atlatılabilir.

Kodu paylaşmak için taşınabilir sınıf kitaplığı kullanarak, ancak aynı zamanda platforma bağımlı özellikleri geçirmek için bağımlılık ekleme kullanılarak bir platformlar arası uygulama mimarisi Bu diyagramda gösterilmektedir:

[![](pcl-images/image1.png "Kodu paylaşmak için taşınabilir sınıf kitaplığı kullanarak, ancak aynı zamanda platforma bağımlı özellikleri geçirmek için bağımlılık ekleme kullanılarak bir platformlar arası uygulama mimarisi Bu diyagramda gösterilmektedir")](pcl-images/image1.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio Mac gözden geçirme

Bu bölümde oluşturma ve Mac için Visual Studio kullanarak bir taşınabilir sınıf kitaplığı kullanma konusunda yol göstermektedir Başvurmak için tam bir uygulamaya PCL örnek bölümüne.

### <a name="creating-a-pcl"></a>Bir PCL oluşturma

Normal bir kitaplık projesi eklemek için çözümünüze bir taşınabilir sınıf kitaplığı ekleme çok benzer.

1. İçinde **yeni proje** iletişim kutusunda **çok platformlu > Kitaplık > taşınabilir kitaplık** seçeneği:

    ![Yeni PCL projesi oluşturma](pcl-images/image2.png)

1. Mac için Visual Studio'da bir PCL oluşturulduğunda, Xamarin.iOS ve Xamarin.Android için çalışan bir profil ile otomatik olarak yapılandırılır. PCL projesine, bu ekran görüntüsünde gösterildiği gibi görünür:

    ![Çözüm panelinde PCL projesine](pcl-images/image3.png)

PCL artık eklenecek kodu için hazırdır. De (uygulama projeleri, kitaplık projeleri ve hatta diğer PCL projeleri) diğer projeleri tarafından başvurulabilir.

### <a name="editing-pcl-settings"></a>PCL ayarlarını düzenleme

Bu proje için PCL ayarlarını görüntülemek ve değiştirmek için projeye sağ tıklayıp seçin **Seçenekleri > derleme > Genel** burada gösterilen ekran görmek için:

[![Profili ayarlamak için PCL proje seçenekleri](pcl-images/image4-sml.png)](pcl-images/image4.png#lightbox)

Tıklayın **Değiştir...**  bu taşınabilir sınıf kitaplığı için hedef profil değiştirmek için.

Kod için PCL eklendikten sonra profili değiştirilirse, yeni seçilen profilinin bir parçası olmayan özellikleri kod başvuruyorsa kitaplığı artık derlemez mümkündür.

## <a name="working-with-a-pcl"></a>Bir PCL ile çalışma

PCL kitaplığa kodu yazıldığında Düzenleyicisi Mac için Visual Studio seçili profil sınırlamaları tanımak ve otomatik tamamlama seçenekleri uygun şekilde ayarlayın. Örneğin, Mac için Visual Studio'da kullanılan varsayılan profil (Profile136) kullanarak bu ekran görüntüsü System.IO için otomatik tamamlama seçenekleri gösterir: kullanılabilir sınıfların yaklaşık yarısını görüntülenir gösteren kaydırma çubuğunu dikkat edin (aslında vardır yalnızca 14 sınıflar kullanılabilir).

[![IntelliSense bir PCL System.IO sınıfına 14 sınıflar listesi](pcl-images/image6.png)](pcl-images/image6.png#lightbox)

Otomatik Tamamlama Xamarin.iOS veya Xamarin.Android projesinde – System.IO ile olduğunu kullanılabilir dahil yaygın olarak kullanılan sınıflar gibi 40 sınıflar karşılaştırma `File` ve `Directory` olmayan herhangi bir PCL profil.

[![.NET Framework System.IO ad alanındaki 40 sınıflar IntelliSense listesi](pcl-images/image7.png)](pcl-images/image7.png#lightbox)

Bu PCL kullanarak, temel alınan dengelemeyi yansıtır: kod birçok platformlar arasında sorunsuz bir şekilde paylaşma olanağı olası tüm platformlarda karşılaştırılabilir uygulamaları olmadığından bazı API'leri kullanabileceğiniz olmadığı anlamına gelir.

### <a name="using-pcl"></a>PCL kullanarak

PCL projesi oluşturulduktan sonra ona bir başvuru uyumlu hiçbir uygulama veya kitaplık projesi başvuruları normalde eklediğiniz yolla ekleyebilirsiniz. Mac için Visual Studio, başvurular düğümüne sağ tıklayın ve seçin **başvuruları Düzenle...**  dönersiniz **projeleri** gösterildiği sekmesinde:

[![Bir PCL başvuruları Düzenle seçeneği aracılığıyla bir başvuru ekleyin](pcl-images/image8.png)](pcl-images/image8.png#lightbox)

Aşağıdaki ekran görüntüsünde, Xamarin.iOS projesi PCL kitaplığı altındaki ve bu PCL kitaplığına bir başvuruyu gösteren TaskyPortable örnek uygulama için çözüm bölmesi gösterir.

[![TaskyPortable örnek çözüm gösteren PCL projesine](pcl-images/image9.png)](pcl-images/image9.png#lightbox)

Bir PCL çıktısı (IE. elde edilen derlemeyi DLL) çoğu proje başvuru olarak da eklenebilir. Bu, PCL platformlar arası bileşenler ve kitaplıkları oluşturmak için ideal bir yol sağlar.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Visual Studio gözden geçirme

Bu bölümde, oluşturma ve Visual Studio kullanarak bir taşınabilir sınıf kitaplığı kullanma hakkında bilgi vermektedir. Başvurmak için tam bir uygulamaya PCL örnek bölümüne.

### <a name="creating-a-pcl"></a>Bir PCL oluşturma

Visual Studio çözümünüzde bir PCL ekleme, normal bir proje eklemek için biraz farklıdır:

1. İçinde **Yeni Proje Ekle** ekranındayken **sınıf kitaplığı (eski taşınabilir)** seçeneği. Bu proje türü kullanım dışıdır sağdaki açıklama öneren unutmayın.

    [![Taşınabilir sınıf kitaplığı oluşturmak için yeni proje penceresinin](pcl-images/image10-sml.png "taşınabilir sınıf kitaplığı")](pcl-images/image10.png#lightbox)

2. Profil yapılandırılabilir, böylece visual Studio hemen aşağıdaki iletişim kutusuyla sor.
 Tamam'a basın ve desteklemek için gereken platformlar kalın.

    [![Hedef platformları kitaplığı seçin](pcl-images/image11-sml.png "işaretleyerek Tamam'a basın ve desteklemek için gereken platformlar")](pcl-images/image11.png#lightbox)

3. PCL projesine Çözüm Gezgini'nde gösterildiği görünür &ndash; metin **(taşınabilir)** bir PCL belirtmek için proje adı görünür:

    ![NET Framework PCL profili tarafından tanımlanan](pcl-images/image12.png "NET Framework PCL profili tarafından tanımlanan")

PCL artık eklenecek kodu için hazırdır. De (uygulama projeleri, kitaplık projeleri ve hatta diğer PCL projeleri) diğer projeleri tarafından başvurulabilir.

### <a name="editing-pcl-settings"></a>PCL ayarlarını düzenleme

PCL ayarları görüntülenebilir ve projeye sağ tıklayıp seçerek değiştirilen **özellikleri > Kitaplık** bu ekran görüntüsünde gösterildiği gibi:

[![Platform hedefleri Düzenle](pcl-images/image13-sml.png)](pcl-images/image13.png#lightbox)

Kod için PCL eklendikten sonra profili değiştirilirse, yeni seçilen profilinin bir parçası olmayan özellikleri kod başvuruyorsa kitaplığı artık derlemez mümkündür.

> [!TIP]
> Ayrıca, bildiren bir ileti olup **. NETStandard kod paylaşmak için önerilen yöntem olduğu**. Bu, hala desteklenmektedir PCLs sırasında göstergesidir, .NET standart sürümüne yükseltme için önerilir.

### <a name="working-with-a-pcl"></a>Bir PCL ile çalışma

PCL kitaplığa kodu yazıldığında, Visual Studio seçili profil sınırlamaları tanımak ve IntelliSense seçeneklerini buna göre ayarlayabilirsiniz. Örneğin, varsayılan profil (Profile136) kullanarak bu ekran görüntüsü System.IO için otomatik tamamlama seçenekleri gösterir: kullanılabilir sınıfların yaklaşık yarısını görüntülenir gösteren kaydırma çubuğunu dikkat edin (aslında yalnızca 14 sınıfı vardır kullanılabilir).

[![Sınırlı sayıda bir PCL'de kullanılabilen g/ç sınıfları](pcl-images/image14.png)](pcl-images/image14.png#lightbox)

Otomatik Tamamlama normal bir projede – System.IO ile olduğunu kullanılabilir dahil yaygın olarak kullanılan sınıflar gibi 40 sınıflar karşılaştırma `File` ve `Directory` olmayan herhangi bir PCL profil.

[![Pek çok daha fazla g/ç sınıfları .NET Framework içinde kullanılabilir](pcl-images/image15.png)](pcl-images/image15.png#lightbox)

Bu PCL kullanarak, temel alınan dengelemeyi yansıtır: kod birçok platformlar arasında sorunsuz bir şekilde paylaşma olanağı olası tüm platformlarda karşılaştırılabilir uygulamaları olmadığından bazı API'leri kullanabileceğiniz olmadığı anlamına gelir.

> [!TIP]
> .NET standard 2.0 bir çok büyük API yüzey alanı PCLs, System.IO ad alanı dahil olmak üzere daha temsil eder. Yeni projeler için .NET Standard PCL önerilir.

### <a name="using-pcl"></a>PCL kullanarak

PCL projesi oluşturulduktan sonra ona bir başvuru uyumlu hiçbir uygulama veya kitaplık projesi başvuruları normalde eklediğiniz yolla ekleyebilirsiniz. Visual Studio'da başvurular düğümüne sağ tıklayın ve seçin `Add Reference...` dönersiniz **çözüm > Projeler** gösterildiği sekmesinde:

[![Başvuru projelerini Ekle sekmesi aracılığıyla bir PCL bir başvuru ekleyin](pcl-images/image16.png)](pcl-images/image16.png#lightbox)

Aşağıdaki ekran görüntüsünde, Xamarin.iOS projesi PCL kitaplığı altındaki ve bu PCL kitaplığına bir başvuruyu gösteren TaskyPortable örnek uygulama için çözüm bölmesinde gösterir.

[![PCL Kitaplığı gösteren TaskyPortable örnek çözüm](pcl-images/image17.png)](pcl-images/image17.png#lightbox)

Bir PCL çıktısı (IE. elde edilen derlemeyi DLL) çoğu proje başvuru olarak da eklenebilir.
Bu, PCL platformlar arası bileşenler ve kitaplıkları oluşturmak için ideal bir yol sağlar.

-----

## <a name="pcl-example"></a>PCL örnek

[TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) örnek uygulaması, Xamarin ile taşınabilir sınıf kitaplığı nasıl kullanılabileceğini gösterir.
Bazı ekran görüntülerini iOS ve Android çalıştıran elde edilen uygulamalar şunlardır:

[![](pcl-images/image18.png "İşte bazı iOS, Android ve Windows Phone üzerinde çalışan elde edilen uygulama ekran görüntüleri")](pcl-images/image18.png#lightbox)

Bir dizi tamamen taşınabilir kod olan verileri ve mantıksal sınıflar paylaşır ve ayrıca nasıl platforma özgü gereksinimleri için SQLite veritabanı uygulama bağımlılık ekleme kullanılarak ekleyeceğinizi gösterir.

Çözüm yapı aşağıda verilmiştir (Mac ve Visual Studio için Visual Studio'da sırasıyla):

[![](pcl-images/image19.png "Çözüm yapı burada Visual Studio'da Mac ve Visual Studio için sırasıyla gösterilir")](pcl-images/image19.png#lightbox)

Tanıtım amacıyla, yeniden düzenlenen bir soyut sınıf taşınabilir sınıf kitaplığı derlenebilir için SQLite NET kod (her farklı bir işletim sisteminde SQLite uygulamaları ile çalışmak için), platforma özgü parçaları sahip olduğundan ve iOS ve Android projeleri sınıflarında olarak uygulanan gerçek kod.

### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

Taşınabilir sınıf kitaplığı, bunu destekleyen .NET özellikleri sınırlıdır. Birden çok platformda çalışacak derlendiğinden yapamaz kullanım `[DllImport]` SQLite-NET kullanılan işlevselliği. Bunun yerine SQLite NET bir soyut sınıfı uygulanır ve ardından paylaşılan kodun geri kalanını başvuru. Soyut API'sinin ayıklama aşağıda gösterilmiştir:

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

Paylaşılan kodun geri kalanını "Mağaza" ve "nesneleri veritabanından alma" soyut sınıfını kullanır. Bu Özet sınıf kullanan tüm uygulamaları biz asıl veritabanını işlevini sağlayan tam bir uygulama geçmesi gerekir.

### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid ve TaskyiOS

İOS ve Android uygulaması projeleri kullanıcı arabirimi ve kablo artırmayı PCL paylaşılan kod kullanılan diğer platforma özgü kod içerir.

Bu projeler, ayrıca soyut veritabanı bu platform üzerinde çalışan bir API uygulaması içerir. Uygulama iOS ve Android'de Sqlite veritabanı altyapısı işletim sisteminde yerleşik olduğundan kullanabilirsiniz `[DllImport]` somut bir uygulama bir veritabanı bağlantı sağlamak için gösterildiği gibi. Platforma özel uygulama kodunu bir alıntı aşağıda gösterilmiştir:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```

Örnek kodu tam uygulamayı görülebilir.

## <a name="summary"></a>Özet

Bu makalede kısaca avantajları ve zorlukları belirlemenizin taşınabilir sınıf kitaplıkları ele alınan, gelen PCLs oluşturup nasıl gösterilen Mac ve Visual Studio; Visual Studio içinde ve son olarak eylem bir PCL gösteren tam bir örnek uygulama – TaskyPortable – kullanıma sunulmuştur.

## <a name="related-links"></a>İlgili bağlantılar

- [TaskyPortable (örnek)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Platformlar Arası Uygulamalar Oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Visual Basic taşınabilir](~/cross-platform/platform/visual-basic/index.md)
- [Paylaşılan Projeler](~/cross-platform/app-fundamentals/shared-projects.md)
- [Kod paylaşma seçenekleri](~/cross-platform/app-fundamentals/code-sharing.md)
- [.NET Framework (Microsoft) ile platformlar arası geliştirme](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)
