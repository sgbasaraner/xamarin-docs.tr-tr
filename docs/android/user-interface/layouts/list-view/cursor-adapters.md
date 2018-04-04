---
title: CursorAdapters kullanma
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: 20311cc50c87638391d8b078c405bc61baceb373
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="using-cursoradapters"></a>CursorAdapters kullanma


## <a name="overview"></a>Genel Bakış

Android, özellikle bir SQLite veritabanı sorgusu verileri görüntülemek için bağdaştırıcı sınıflar sağlar:

 **SimpleCursorAdapter** – benzer şekilde bir `ArrayAdapter` çünkü sınıflara olmadan kullanılabilir. Gerekli Parametreler (örneğin, imleç ve düzeni bilgileri) oluşturucuda belirtmeniz yeterlidir ve ardından atamak bir `ListView`.

 **CursorAdapter** – ne zaman, daha fazla veri bağlama üzerinden denetime ihtiyacınız öğesinden devralan bir temel sınıf değerleri düzen denetimleri (örneğin, denetimleri gizleme/gösterme veya özelliklerini değiştirme).

İmleç bağdaştırıcıları uzun listeleri SQLite içinde depolanan veriler ile kaydırmak için yüksek performanslı bir yol sağlar. Bir SQL sorgusu içinde kullanıcı kodu tanımlamanız gerekir bir `Cursor` nesne ve nasıl oluşturulacağı ve her satır için görünümleri doldurmak açıklanmaktadır.


## <a name="creating-an-sqlite-database"></a>Bir SQLite veritabanı oluşturma

İmleç bağdaştırıcıları göstermek için basit bir SQLite veritabanı uygulamasını gerektirir. Kodda **SimpleCursorTableAdapter/VegetableDatabase.cs** bir tablo oluşturun ve bazı verilerle doldurmak için SQL ve kod içerir.
Tam `VegetableDatabase` sınıfı burada gösterilmiştir:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
   public static readonly string create_table_sql =
       "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
   public static readonly string DatabaseName = "vegetables.db";
   public static readonly int DatabaseVersion = 1;
   public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
   public override void OnCreate(SQLiteDatabase db)
   {
       db.ExecSQL(create_table_sql);
       // seed with data
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
   }
   public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
   {   // not required until second version :)
       throw new NotImplementedException();
   }
}
```

`VegetableDatabase` Sınıfı içinde başlatılamaz `OnCreate` yöntemi `HomeScreen` etkinlik. `SQLiteOpenHelper` Temel sınıf veritabanı dosyasının Kurulum yönetir ve SQL sağlar, `OnCreate` yöntemi yalnızca çalıştırıldığında kez. Bu sınıf için aşağıdaki iki örneklerde kullanılan `SimpleCursorAdapter` ve `CursorAdapter`.

İmleç sorgusu *gerekir* bir tamsayı sütunu `_id` için `CursorAdapter` çalışmak için. Temel tablo adlı bir tamsayı sütunu yoksa `_id` başka bir benzersiz tamsayı olarak bir sütun diğer adı kullanılmaya `RawQuery` imleci yukarı yapar. Başvurmak [Android belgeleri](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/) daha fazla bilgi için.


### <a name="creating-the-cursor"></a>İmleci oluşturma

Örnekler bir `RawQuery` içine bir SQL sorgusunu açmak için bir `Cursor` nesnesi. İmleci döndürülen sütun listesi imleç bağdaştırıcısı görüntülemek için kullanılabilir veri sütunlarını tanımlar. Veritabanında oluşturan kodu **SimpleCursorTableAdapter/HomeScreen.cs** `OnCreate` yöntemi burada gösterilmiştir:

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

Çağıran herhangi bir kod `StartManagingCursor` da çağırmalıdır `StopManagingCursor`. Örneklerde `OnCreate` başlatmak için ve `OnDestroy` imleci kapatın. `OnDestroy` Yöntemi bu kodu içerir:

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

Bir uygulama kullanılabilir bir SQLite veritabanı yoktur ve gösterildiği gibi bir imleç nesnesi oluşturdu sonra ya da kullanabileceği bir `SimpleCursorAdapter` veya bir alt sınıfı `CusorAdapter` satırları görüntülemek için bir `ListView`.


## <a name="using-simplecursoradapter"></a>SimpleCursorAdapter kullanma

`SimpleCursorAdapter` benzer `ArrayAdapter`, ancak özel SQLite ile kullanım için. Değil sınıflara gerektiren – bazı basit parametreleri nesne oluşturma sırasında ayarlamanız yeterlidir ve ona atayın bir `ListView`'s `Adapter` özelliği.

SimpleCursorAdapter oluşturucusu için Parametreler şunlardır:

 **Bağlam** – içeren etkinliği için bir başvuru.

 **Düzen** – kullanılacak satır görünümü kaynak kimliği.

 **ICursor** – verilerin görüntülemek SQLite sorgu içeren bir imleç.

 **Gelen** dize dizisi – imleci sütunlarının adlarını karşılık gelen bir dizeler dizisi.

 **İçin** tamsayı dizisi – satır düzeninde denetimleri karşılık düzeni kimlikleri bir dizi. Belirtilen sütunun değeri `from` dizi bağlı aynı dizinde bu dizideki belirtilen ControlId.

`from` Ve `to` diziler düzen denetimleri için veri kaynağından bir eşleme görünümünde oluşturuyorlar çünkü aynı sayıda girişe sahip olmalıdır.

**SimpleCursorTableAdapter/HomeScreen.cs** yukarı kod örnek bir `SimpleCursorAdapter` şöyle:

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` SQLite verileri görüntülemek için hızlı ve kolay bir yolu bir `ListView`. Ana kısıtlamadır yalnızca denetimlerini görüntülemek için sütun değerlerini bağlayabilirsiniz, onu diğer yönlerini (örneğin, denetimleri gösterme/gizleme veya özelliklerini değiştirme) satır düzenini değiştirmek izin vermez.


## <a name="subclassing-cursoradapter"></a>CursorAdapter alt sınıf yapma

A `CursorAdapter` bir alt kümesi olan olarak aynı performans avantajı `SimpleCursorAdapter` için SQLite, ancak verileri görüntüleme ayrıca oluşturulması ve her satır görünüm düzenini üzerinde tam denetim sağlar. `CursorAdapter` Uygulamasıdır sınıflara gelen çok farklı `BaseAdapter` geçersiz olduğundan `GetView`, `GetItemId`, `Count` veya `this[]` dizin oluşturucu.

Bir çalışma verilen SQLite veritabanı oluşturmak için iki yöntem geçersiz kılmak için yeterlidir bir `CursorAdapter` bir alt kümesi:

- **BindView** – bir görünüm verildiğinde, sağlanan imlecin verileri görüntülemek için güncelleştirin.

- **NewView** – çağrılır `ListView` görüntülemek için yeni bir görünüm gerektirir. `CursorAdapter` Görünümleri geri dönüştürme dikkatli (aksine `GetView` normal bağdaştırıcılarında yöntemi).

Önceki örneklerde bağdaştırıcısı alt sınıfların satır sayısını döndürür ve geçerli öğesi – almak için yöntemleri sahip `CursorAdapter` bu bilgileri imleci konusunda çünkü bu yöntemleri gerektirmez. Oluşturma ve bu iki yöntem her görünüme popülasyonunu bölme tarafından `CursorAdapter` görünümü yeniden kullanımını zorunlu kılar. Buna karşılık normal bir bağdaştırıcıya yoksay mümkün olduğu budur `convertView` parametresinin `BaseAdapter.GetView` yöntemi.


### <a name="implementing-the-cursoradapter"></a>CursorAdapter uygulama

Kodda **CursorTableAdapter/HomeScreenCursorAdapter.cs** içeren bir `CursorAdapter` bir alt kümesi. Böylece erişebildiğinizi oluşturucuya geçirilen bir bağlam başvurularını depolayan bir `LayoutInflater` içinde `NewView` yöntemi. Tüm sınıf şöyle görünür:

```csharp
public class HomeScreenCursorAdapter : CursorAdapter {
   Activity context;
   public HomeScreenCursorAdapter(Activity context, ICursor c)
       : base(context, c)
   {
       this.context = context;
   }
   public override void BindView(View view, Context context, ICursor cursor)
   {
       var textView = view.FindViewById<TextView>(Android.Resource.Id.Text1);
       textView.Text = cursor.GetString(1); // 'name' is column 1 in the cursor query
   }
   public override View NewView(Context context, ICursor cursor, ViewGroup parent)
   {
       return this.context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, parent, false);
   }
}
```


### <a name="assigning-the-cursoradapter"></a>CursorAdapter atama

İçinde `Activity` , görüntüler `ListView`, imleci oluşturmak ve `CursorAdapter` liste görünümüne atayın.

Bu eylem gerçekleştiren kod **CursorTableAdapter/HomeScreen.cs** `OnCreate` yöntemi burada gösterilmiştir:

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

`OnDestroy` Yöntemini içeren `StopManagingCursor` daha önce açıklanan yöntem çağrısı.



## <a name="related-links"></a>İlgili bağlantılar

- [SimpleCursorTableAdapter (sample)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (örnek)](https://developer.xamarin.com/samples/CursorTableAdapter/)
