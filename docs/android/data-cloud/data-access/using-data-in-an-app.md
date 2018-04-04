---
title: Bir uygulamada veri kullanma
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: dd2a0a58a3a8c10671609aa385629d4754ca5378
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="using-data-in-an-app"></a>Bir uygulamada veri kullanma

**DataAccess_Adv** örnek kullanıcı girişini ve CRUD (Oluştur, okuma, güncelleştirme ve silme) veritabanı işlevselliği sağlar ve çalışan bir uygulama gösterir. İki ekranda uygulama oluşur: bir listesi ve veri girişi formu. Tüm veri erişim kodu değişiklik yapılmadan iOS ve Android yeniden kullanılabilir.

Bazı veriler ekledikten sonra uygulama ekranlar Android şöyle:

![Android örnek listesi](using-data-in-an-app-images/image11.png "Android örnek listesi")

![Android örnek ayrıntı](using-data-in-an-app-images/image12.png "Android örnek ayrıntısı")

Android projesi aşağıda gösterilen &ndash; Bu bölümde gösterilen kodu kapsamında yer alan **Orm** dizini:

![Android projesi ağaç](using-data-in-an-app-images/image14.png "Android projesi ağacı")

Yerel kullanıcı Arabirimi kodu Android etkinlikler için bu belgenin kapsamı dışındadır. Başvurmak [Android ListViews ve bağdaştırıcıları](~/android/user-interface/layouts/list-view/index.md) UI denetimleri hakkında daha fazla bilgi için Kılavuzu.

## <a name="read"></a>Oku

Birkaç örnek okuma işlemleri şunlardır:

-  Listenin okuma
-  Tek tek kayıtları okuma

Öğesinde iki yöntem `StockDatabase` sınıfı bulunur:

```csharp
public IEnumerable<Stock> GetStocks ()
{
    lock (locker) {
        return (from i in Table<Stock> () select i).ToList ();
    }
}
public Stock GetStock (int id)
{
    lock (locker) {
        return Table<Stock>().FirstOrDefault(x => x.Id == id);
    }
}
```

Android veri olarak işleyen bir `ListView`.

## <a name="create-and-update"></a>Oluşturma ve güncelleştirme

Uygulama kodu basitleştirmek için tek bir yöntem kaydetme, INSERT sağlanan veya PrimaryKey olup ayarlanmış bağlı olarak güncelleştirmedir. Çünkü `Id` özelliği ile işaretlenmiş bir `[PrimaryKey]` , ayarlı değil, kodunuzda özniteliği. Bu yöntem değeri (birincil anahtar özelliği denetleyerek) kaydedilen önceki olup olmadığını algılar ve eklemek veya nesne uygun şekilde güncelleştirin:

```csharp
public int SaveStock (Stock item)
{
    lock (locker) {
        if (item.Id != 0) {
            Update (item);
            return item.Id;
    } else {
            return Insert (item);
        }
    }
}
```

Gerçek dünya uygulamalar genellikle bazı doğrulama (örneğin, gerekli alanlar, minimum uzunluk veya diğer iş kuralları) gerektirir. Doğrulama hataları platformun yetenekleri göre görüntülemek için kullanıcı Arabirimi yedekle geçirme paylaşılan kodda mümkün olduğunca mantıksal doğrulama çok iyi platformlar arası uygulamalar uygular.

## <a name="delete"></a>Sil

Farklı `Insert` ve `Update` yöntemleri, `Delete<T>` yöntemi yalnızca birincil anahtar değeri yerine bir tam kabul edebileceği `Stock` nesnesi. Bu örnekte bir `Stock` nesne yönteme geçirilen ancak yalnızca kimliği özelliği için geçirilen `Delete<T>` yöntemi.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Önceden girilmiş bir SQLite veritabanı dosyasını kullanarak

Bazı uygulamalar, verilerle zaten doldurulmuş bir veritabanı ile aktarılır. Ayrıca, uygulamanızı olan mevcut bir SQLite veritabanı dosya aktarma ve erişmeden önce yazılabilir bir dizine kopyalama tarafından mobil uygulamanızda bu kolayca gerçekleştirebilirsiniz. SQLite birçok platformlarda kullanılan standart bir dosya biçimi olduğundan, bir dizi bir SQLite veritabanı dosyasını oluşturmak için kullanılabilen araçlar vardır:

-   **SQLite Yöneticisi Firefox uzantısı** &ndash; iOS ve Android ile uyumlu olan Mac ve Windows ve üretir dosyaları çalışır.

-   **Komut satırı** &ndash; bkz [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .

Dağıtım için bir veritabanı dosyası ile uygulamanızı oluştururken, tablolar ve sütunlar bunların eşleşmesi ne kodunuzu bekler, özellikle, C# sınıfları ve özellikleri eşleşecek şekilde adları beklediği SQLite.NET kullanıyorsanız emin olmak için adlandırma ile ilgilenebilmek (veya ilişkili özel öznitelikler).

Biraz kod Android uygulamanızda başka bir şey önce çalıştığından emin olmak için yüklemek için ilk etkinliğin yerleştirin veya oluşturabileceğiniz bir `Application` hiç etkinlik önce yüklenen bir alt kümesi. Aşağıdaki kodu gösterir bir `Application` var olan bir veritabanı dosyası kopyalar bir alt **data.sqlite** dışı **/Resources/Raw/** dizin.

```csharp
[Application]
public class YourAndroidApp : Application {
    public override void OnCreate ()
    {
        base.OnCreate ();
        var docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
        Console.WriteLine ("Data path:" + Database.DatabaseFilePath);
        var dbFile = Path.Combine(docFolder, "data.sqlite"); // FILE NAME TO USE WHEN COPIED
        if (!System.IO.File.Exists(dbFile)) {
            var s = Resources.OpenRawResource(Resource.Raw.data);  // DATA FILE RESOURCE ID
            FileStream writeStream = new FileStream(dbFile, FileMode.OpenOrCreate, FileAccess.Write);
            ReadWriteStream(s, writeStream);
        }
    }
    // readStream is the stream you need to read
    // writeStream is the stream you want to write to
    private void ReadWriteStream(Stream readStream, Stream writeStream)
    {
        int Length = 256;
        Byte[] buffer = new Byte[Length];
        int bytesRead = readStream.Read(buffer, 0, Length);
        // write the required bytes
        while (bytesRead > 0)
        {
            writeStream.Write(buffer, 0, bytesRead);
            bytesRead = readStream.Read(buffer, 0, Length);
        }
        readStream.Close();
        writeStream.Close();
    }
}
```


## <a name="related-links"></a>İlgili bağlantılar

- [ADO'da Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [ADO'da Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android veri tarif](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
