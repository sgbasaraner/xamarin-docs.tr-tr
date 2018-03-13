---
title: Bir uygulamada veri kullanma
ms.topic: article
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: aefec1992a5f061f9d50d499b3594601a2651b17
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="using-data-in-an-app"></a>Bir uygulamada veri kullanma

**DataAccess_Adv** örnek kullanıcı girişini sağlar ve çalışan bir uygulama gösterir ve *CRUD* (oluşturma, okuma, güncelleştirme ve silme) veritabanı işlevselliği. İki ekranda uygulama oluşur: bir listesi ve veri girişi formu. Tüm veri erişim kodu değişiklik yapılmadan iOS ve Android yeniden kullanılabilir.

Bazı veriler ekledikten sonra uygulama ekranlar İos'ta şöyle:

 ![](using-data-in-an-app-images/image9.png "iOS örnek listesi")

 ![](using-data-in-an-app-images/image10.png "iOS örnek ayrıntısı")

İOS projesi aşağıda gösterilen – Bu bölümde gösterilen kodu kapsamında yer alan **Orm** dizini:

 ![](using-data-in-an-app-images/image13.png "iOS proje ağacı")

İOS içinde ViewControllers için yerel kullanıcı Arabirimi kod bu belgenin kapsamı dışındadır.
Başvurmak [iOS tabloları ve hücreleriyle çalışma](~/ios/user-interface/controls/tables/index.md) UI denetimleri hakkında daha fazla bilgi için Kılavuzu.

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

iOS veri işler farklı olarak bir `UITableView`.

## <a name="create-and-update"></a>Oluşturma ve güncelleştirme

Uygulama kodu basitleştirmek için tek bir yöntem kaydetme, INSERT sağlanan veya PrimaryKey olup ayarlanmış bağlı olarak güncelleştirmedir. Çünkü `Id` özelliği ile işaretlenmiş bir `[PrimaryKey]` , ayarlı değil, kodunuzda özniteliği.
Bu yöntem değeri (birincil anahtar özelliği denetleyerek) kaydedilen önceki olup olmadığını algılar ve eklemek veya nesne uygun şekilde güncelleştirin:

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



Gerçek dünya uygulamalar genellikle bazı doğrulama (örneğin, gerekli alanlar, minimum uzunluk veya diğer iş kuralları) gerektirir.
Doğrulama hataları platformun yetenekleri göre görüntülemek için kullanıcı Arabirimi yedekle geçirme paylaşılan kodda mümkün olduğunca mantıksal doğrulama çok iyi platformlar arası uygulamalar uygular.

## <a name="delete"></a>Sil

Farklı `Insert` ve `Update` yöntemleri, `Delete<T>` yöntemi yalnızca birincil anahtar değeri yerine bir tam kabul edebileceği `Stock` nesnesi.
Bu örnekte bir `Stock` nesne yönteme geçirilen ancak yalnızca kimliği özelliği için geçirilen `Delete<T>` yöntemi.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Önceden girilmiş bir SQLite veritabanı dosyasını kullanarak

Bazı uygulamalar, verilerle zaten doldurulmuş bir veritabanı ile aktarılır.
Ayrıca, uygulamanızı olan mevcut bir SQLite veritabanı dosya aktarma ve erişmeden önce yazılabilir bir dizine kopyalama tarafından mobil uygulamanızda bu kolayca gerçekleştirebilirsiniz. SQLite birçok platformlarda kullanılan standart bir dosya biçimi olduğundan, bir dizi bir SQLite veritabanı dosyasını oluşturmak için kullanılabilen araçlar vardır:

-  **SQLite Yöneticisi Firefox uzantısı** – iOS ve Android ile uyumlu olan Mac ve Windows ve üretir dosyaları çalışır.
-  **Komut satırı** – bkz [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .


Dağıtım için bir veritabanı dosyası ile uygulamanızı oluştururken, tablolar ve sütunlar bunların eşleşmesi ne kodunuzu bekler, özellikle, C# sınıfları ve özellikleri eşleşecek şekilde adları beklediği SQLite.NET kullanıyorsanız emin olmak için adlandırma ile ilgilenebilmek (veya ilişkili özel öznitelikler).

İOS, uygulamanızda sqlite dosya ekleyin ve işaretli ile emin **yapı eylemi: içerik**. Kodda yerleştirin `FinishedLaunching` yazılabilir bir dizine dosya kopyalamak için *önce* herhangi bir veri yöntem çağrısı. Aşağıdaki kod adlı varolan bir veritabanını kopyalayacak **data.sqlite**, yalnızca zaten yoksa.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Tüm veri erişim kodu (olup olmadığını ADO.NET veya SQLite.NET kullanarak) bu önceden doldurulmuş haldedir verilere erişimi tamamlanmış olacaktır sahip olduktan sonra yürütür.


## <a name="related-links"></a>İlgili bağlantılar

- [ADO'da Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [ADO'da Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS veri tarif](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
