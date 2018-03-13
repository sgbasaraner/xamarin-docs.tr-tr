---
title: "Özel ContentProvider oluşturma"
description: "Önceki bölümde yerleşik bir ContentProvider uygulaması verileri kullanma gösterilmektedir. Bu bölüm, özel ContentProvider oluşturmak ve verileri kullanmak nasıl anlatılmıştır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 9fac4a233cecd9332602047bc83830d145b5fb08
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="creating-a-custom-contentprovider"></a>Özel ContentProvider oluşturma

_Önceki bölümde yerleşik bir ContentProvider uygulaması verileri kullanma gösterilmektedir. Bu bölüm, özel ContentProvider oluşturmak ve verileri kullanmak nasıl anlatılmıştır._

## <a name="about-contentproviders"></a>ContentProviders hakkında

İçerik sağlayıcı sınıfı öğesinden devralmalıdır `ContentProvider`. Sorgularını yanıtlamak için kullanılan bir iç veri deposu oluşmalıdır ve kod tüketen yardımcı olmak için sabitleri veriler için geçerli istekleri yaparken, URI ve MIME türleri maruz bırakmamalısınız.

### <a name="uri-authority"></a>URI (yetkilisi)

`ContentProviders` Android URI kullanılarak erişilir. Kullanıma sunan bir uygulama bir `ContentProvider` içeri yanıt URI ayarlar kendi **AndroidManifest.xml** dosya. Uygulama yüklendiğinde bu URI kayıtlı diğer uygulamaların bunlara erişebilmesi için.

Android için mono olarak içerik sağlayıcı sınıfı olmalıdır bir `[ContentProvider]` özniteliğini URI (veya URI'ler) belirtmek için eklenmesi için **AndroidManifest.xml**.


### <a name="mime-type"></a>MIME türü

MIME türleri için normal biçim iki bölümden oluşur. Android `ContentProviders` yaygın olarak şu iki dizeyi ilk parçası MIME türü için kullanın:

1. `vnd.android.cursor.item` &ndash; tek bir satır temsil etmek için kullanmak `ContentResolver.CursorItemBaseType` kodda sabit.

1. `vnd.android.cursor.dir` &ndash; birden çok satır için kullanmak `ContentResolver.CursorDirBaseType` kodda sabit.

MIME türü ikinci bölümü uygulamanıza özeldir ve bir geriye doğru DNS standart kullanması gereken bir `vnd.` öneki. Örnek kod kullanır `vnd.com.xamarin.sample.Vegetables`.


### <a name="data-model-metadata"></a>Veri modeli meta verileri

Farklı veri türlerini erişmek için URI sorgular oluşturmak tüketen uygulamayı gerekir. Taban URI verilerin belirli bir tabloya başvurmak için genişletilmiş ve sonuçları filtrelemek için parametreler de içerebilir. Sütunları ve sonuçta elde edilen imleç verileri görüntülemek için kullanılan yan tümceleri de bildirilmesi gerekir.

Yalnızca geçerli URI sorgu oluşturulur emin olmak için geçerli dize sabit değerleri olarak sağlamak için yaygın bir uygulamadır. Bu, erişim kolaylaştırır `ContentProvider` çünkü değerleri kod tamamlama aracılığıyla bulunabilirlik yapar ve yazım hatalarını dizelerinde engeller.

Önceki örnekte `android.provider.ContactsContract` sınıfı sunulan kişiler veri için meta veriler. Bizim özel `ContentProvider` biz yalnızca sabitleri sınıfı üzerinde açığa çıkarır.


## <a name="implementation"></a>Uygulama

Üç adım vardır oluşturma ve özel bir tüketen `ContentProvider`:

1. **Veritabanı sınıf oluşturma** &ndash; uygulama `SQLiteOpenHelper`.

2. **Oluşturma bir `ContentProvider` sınıfı** &ndash; uygulama `ContentProvider` veritabanının bir örneğiyle metaveri sabit değerleri ve verilere erişmek için yöntemler.

3. **Erişim `ContentProvider` URI'sini aracılığıyla** &ndash; Populate bir `CursorAdapter` kullanarak `ContentProvider`, URI'sini aracılığıyla erişilen.

Daha önce açıklandığı gibi `ContentProviders` burada tanımlanan dışındaki uygulamalardan tüketilebilir. Bu örnekte aynı uygulamada veriler tüketiliyor ancak URI ve (genellikle sabit değerler sunulur) şema hakkında bilgi bildikleri sürece diğer uygulamalar da erişebildiğinizi göz önünde bulundurun.


## <a name="create-a-database"></a>Bir veritabanı oluşturun

Çoğu `ContentProvider` uygulamaları temel alınacak bir `SQLite` veritabanı. Örnek veritabanı kodda **SimpleContentProvider/VegetableDatabase.cs** gösterildiği gibi çok basit bir iki sütun veritabanı oluşturur:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

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
  {
    throw new NotImplementedException();
  }
}
```

Veritabanı uygulaması ile açığa çıkarılması herhangi diğer noktalar gerekmez bir `ContentProvider`, ancak bağlamak istiyorsanız, `ContentProvider's` verileri bir `ListView` adlı bir benzersiz tamsayı sütunu kontrol `_id` parçası olmalıdır Sonuç kümesi. Bkz: [ListViews ve bağdaştırıcıları](~/android/user-interface/layouts/list-view/index.md) kullanma hakkında daha fazla ayrıntı için belge `ListView` denetim.


## <a name="create-the-contentprovider"></a>ContentProvider oluşturma

Bu bölümde rest yönergeler üzerinde nasıl verir **SimpleContentProvider/VegetableProvider.cs** örnek sınıf yapılmış.


### <a name="initialize-the-database"></a>Veritabanını başlatılamadı

Bir alt kümesi için ilk adımdır `ContentProvider` ve kullanacağı veritabanı ekleyin.

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

Kod kalan verilerin bulunan ve sorgulanan izin verir gerçek içerik sağlayıcı uygulaması oluşturacak.



## <a name="add-metadata-for-consumers"></a>Tüketiciler için meta verileri ekleme

Üzerinde kullanıma sunmak için uygulayacağınız meta verileri dört farklı türde vardır `ContentProvider` sınıfı. Yalnızca yetkili gereklidir, rest kurala göre yapılır.

- **Yetkilisi** &ndash; `ContentProvider` özniteliği *gerekir* eklenmesi sınıfa böylece uygulama yüklendiğinde Android ile kaydedilir.

- **URI** &ndash; `CONTENT_URI` böylece kodu kullanımı kolay bir sabit değer olarak sunulur. Yetkilisi eşleşen, ancak düzeni ve temel yolu içerir.

- **MIME türleri** &ndash; biz bunları temsil etmek için iki MIME türlerini tanımlamak için sonuçları ve tek tek sonuçlar listelerini farklı içerik türlerini olarak kabul edilir.

- **InterfaceConsts** &ndash; böylece kodu tüketen kolayca bulmak ve bunlara yazım hataları riski olmadan başvurmak için her veri sütunu adı, sabit bir değer sağlayın.

Bu kodu nasıl bu öğelerin her birini uygulanır, gösterir önceki adımdan veritabanı tanımına ekleme:

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```


## <a name="implement-the-uri-parsing-helper"></a>Yardımcı ayrıştırma URI uygulama

Kod kullanan URI'ler isteklerinin yapmak için kullandığı için bir `ContentProvider`, geri dönmek için hangi verilerin belirlemek için bu istekleri ayrıştırma olması gerekir. `UriMatcher` URI'ler ayrıştırmak için yardımcı sınıfı, ile başlatıldı sonra URI, düzenleri `ContentProvider` destekler.

`UriMatcher` Örnekte iki URI ile başlatılır:

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; request to return the full list of vegetables.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; where the \# is a placeholder for a numeric parameter (the `_id` of the row in the database). Bir yıldız işareti yer tutucu ("\*") bir metin parametre eşleştirmek için de kullanılabilir.

Kod içinde sabitleri yetkilisi ve temel gibi meta veri değerlerinin başvurmak için kullanırız\_yolu. Ayrıştırma döndürmek için hangi verilerin belirlemek için URI yapmak yöntemlerinde dönüş kodları kullanılır.

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

Bu kod tüm özel `ContentProvider` sınıfı. Başvurmak [Google UriMatcher belgelerine](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/) daha fazla bilgi için.


## <a name="implement-the-querymethod"></a>Uygulama QueryMethod

En basit `ContentProvider` uygulamak için yöntem `Query` yöntemi. Uygulama kullanır `UriMatcher` ayrıştırmak için `uri` parametre ve doğru veritabanı yöntemini çağırın. Varsa `uri` ID parametresi tamsayı çıkışı ayrıştırılır sonra içerir (kullanarak `LastPathSegment`) ve veritabanı sorguda kullanılan.

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

`GetType` Yöntemi gerekir ayrıca geçersiz kılındı. Bu yöntem için verilen URI döndürülecek içerik türü belirlemek için çağrılabilir.
Bu, kullanıcı uygulama verileri nasıl ele alınacağını söyleyebilir.

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```


## <a name="implement-the-other-overrides"></a>Diğer geçersiz kılmalarını

Basit örneğimizde düzenleme veya verilerin silinmesini izin vermez ancak ekleme, güncelleştirme ve silme yöntemleri uygulanması gerekir böylece bunları bir uygulama ekleyin:

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

Temel tamamlanan `ContentProvider` uygulaması. Uygulama yüklendikten sonra onu sunan verilerin kullanılabilir olacağını hem uygulama aynı zamanda, başvuru için URI bilir herhangi bir uygulama için.


## <a name="access-the-contentprovider"></a>Erişim ContentProvider

Bir kez `VegetableProvider` bırakıldı uygulanan erişmekte bu belge başlangıcı kişiler sağlayıcısındaki aynı şekilde yapılır: bir imleç Belirtilen URI kullanılarak elde edilir ve verilere erişmek için bir bağdaştırıcı kullanacak.


## <a name="bind-a-listview-to-a-contentprovider"></a>ListView bir ContentProvider bağlama

Doldurmak için bir `ListView` verilerle et filtrelenmemiş listesine karşılık gelen URI kullanırız. Sabit değer kullanırız kodda `VegetableProvider.CONTENT_URI`, hangi biliyoruz çözümler `com.xamarin.sample.vegetableprovider/vegetables`. Bizim `VegetableProvider.Query` uygulama sonra bağlanabilen bir imleç döndürecektir `ListView`.

Kodda `SimpleContentProvider/HomeScreen.cs` verileri görüntülemek için nasıl basit olduğunu gösteren bir `ContentProvider`:

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

Sonuçta elde edilen uygulama şöyle görünür:

[![Listeleme et, Fruits, çiçek tıkaçları, Legumes, Bulbs, Tubers uygulamasının ekran görüntüsü](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)



## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Tek bir öğe ContentProvider alma

Kullanıcı bir uygulama için belirli bir satırdan (örneğin) başvuruyor farklı bir URI oluşturarak yapılabilir veri tek satırı erişmek de isteyebilirsiniz.

Kullanım `ContentResolver` tek bir öğe gerekli olan bir URI oluşturma tarafından doğrudan erişmek için `Id`.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

Tam yöntem şöyle görünür:

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```


## <a name="related-links"></a>İlgili bağlantılar

- [SimpleContentProvider (örnek)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
