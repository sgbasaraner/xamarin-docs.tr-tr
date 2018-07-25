---
title: Bir Android uygulamasını kullanarak verileri
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 563c04ef1c8eec00108844894c5f9bdc0e9950e3
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241896"
---
# <a name="using-data-in-an-app"></a>Bir uygulamada verileri kullanarak

**DataAccess_Adv** örnek kullanıcı girişini ve CRUD (oluşturma, okuma, güncelleştirme ve silme) veritabanı işlevselliğini sağlar ve çalışan bir uygulama gösterilmektedir. Uygulama iki ekran oluşur: bir listesi ve bir veri giriş formunu. Tüm veri erişim kodunun değiştirilmeden iOS ve Android yeniden kullanılabilir.

Bazı veriler ekledikten sonra uygulama ekranları Android'de şuna benzer:

![Android örnek listesi](using-data-in-an-app-images/image11.png "Android örnek listesi")

![Android örnek ayrıntı](using-data-in-an-app-images/image12.png "Android örnek ayrıntısı")

Android projesine aşağıda gösterilen &ndash; Bu bölümde gösterilen kodu içinde yer alan **Orm** dizini:

![Android projesi ağaç](using-data-in-an-app-images/image14.png "Android proje ağacı")

Android etkinlikler için yerel kullanıcı Arabirimi kodu, bu belgenin kapsamı dışındadır. Başvurmak [Android ListViews ve bağdaştırıcıları](~/android/user-interface/layouts/list-view/index.md) kullanıcı Arabirimi denetimleri hakkında daha fazla bilgi için Kılavuzu.

## <a name="read"></a>Oku

Birkaç örnek okuma işlemlerinde vardır:

-  Listenin okuma
-  Tek tek kayıtları okuma

İki metot `StockDatabase` sınıfı:

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

## <a name="create-and-update"></a>Oluştur ve güncelleştir

Uygulama kodu basitleştirmek için tek bir yöntem kaydetme ekleme yapan sağlanan veya PrimaryKey olup ayarlandı bağlı olarak güncelleştirmedir. Çünkü `Id` özelliği ile işaretlenmiş bir `[PrimaryKey]` , ayarlı değil, kodunuzda özniteliği. Bu yöntem, değeri (birincil anahtar özelliği kontrol ederek) kaydedilmiş önceki olup olmadığını algılar ve ekleme veya nesne uygun şekilde güncelleştirin:

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

Gerçek dünya uygulamaları genellikle bazı doğrulama (örneğin, gerekli alanları, en düşük uzunlukta veya diğer iş kuralları) gerektirir. Doğrulama mantıksal paylaşılan kod, doğrulama hataları platformun özelliklerinden göre görüntülemek için kullanıcı Arabirimi yede geçirme mümkün olduğunca çok iyi platformlar arası uygulamalar uygular.

## <a name="delete"></a>Sil

Farklı `Insert` ve `Update` yöntemleri `Delete<T>` yöntemi yalnızca birincil anahtar değer yerine eksiksiz bir kabul edebilir `Stock` nesne. Bu örnekte bir `Stock` nesne yönteme geçirilir, ancak yalnızca kimlik özelliği için geçirilen `Delete<T>` yöntemi.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Önceden doldurulmuş bir SQLite veritabanı dosyası kullanma

Bazı uygulamalar, zaten doldurulmuş bir veritabanı ile gönderilir. Ayrıca, uygulamanızı mevcut bir SQLite veritabanı dosyasıyla sevkiyat ve erişmeden önce yazılabilir bir dizine kopyalanıyor mobil uygulamanızda bu kolayca gerçekleştirebilirsiniz. SQLite birçok platformunda kullanılan standart bir dosya biçimi olduğundan, bir SQLite veritabanı dosyası oluşturmak kullanılabilen araçlar vardır:

-   **SQLite Firefox yöneticisini** &ndash; iOS ve Android ile uyumlu olan Mac ve Windows ve oluşturan dosyalar üzerinde çalışır.

-   **Komut satırı** &ndash; bkz [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .

Dağıtım için bir veritabanı dosyası, uygulamanızı oluştururken, tablolar ve sütunlar bunların eşleşmesi ne kodunuzu bekliyor, özellikle, C# sınıfları ve özellikleri ile eşleşecek biçimde adlarını beklediği SQLite.NET kullanıyorsanız emin olmak için adlandırma ile ilgileniriz (veya ilişkili özel öznitelikler).

Bazı kod önce Android uygulamanızda diğer her şeyin çalıştığından emin olmak için yüklemek için ilk etkinlik yerleştirebilirsiniz veya oluşturabileceğiniz bir `Application` herhangi bir etkinlik önce yüklenen bir alt sınıfı. Aşağıdaki kod gösterir bir `Application` mevcut veritabanı dosyasına kopyalar alt **data.sqlite** tanesi **/Resources/ham/** dizin.

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

- [DataAccess Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android veri tarifleri](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
