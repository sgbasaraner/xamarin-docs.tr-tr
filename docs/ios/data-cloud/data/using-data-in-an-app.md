---
title: Verileri kullanarak bir iOS uygulaması
description: Bu belge, kullanıcı girişini toplamak ve gerçekleştirmek nasıl gösterir ve örnek oluşturma DataAccess_Adv açıklar, okuma, güncelleştirme ve silme (CRUD) veritabanı işlemleri bir Xamarin.iOS uygulaması.
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 35caae657700e321a7560d1e95c8551b7b10a5ca
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242111"
---
# <a name="using-data-in-an-ios-app"></a>Verileri kullanarak bir iOS uygulaması

**DataAccess_Adv** örnek, kullanıcı girişine izin çalışan bir uygulama gösterir ve *CRUD* (oluşturma, okuma, güncelleştirme ve silme) veritabanı işlevleri. Uygulama iki ekran oluşur: bir listesi ve bir veri giriş formunu. Tüm veri erişim kodunun değiştirilmeden iOS ve Android yeniden kullanılabilir.

Bazı veriler ekledikten sonra uygulama ekranları İos'ta şöyle:

 ![](using-data-in-an-app-images/image9.png "iOS örnek listesi")

 ![](using-data-in-an-app-images/image10.png "iOS örnek ayrıntısı")

İOS projesi aşağıda gösterilmiştir: Bu bölümde gösterilen kodu içinde yer alan **Orm** dizini:

 ![](using-data-in-an-app-images/image13.png "iOS projesi ağacı")

İos'ta ViewControllers yerel UI kodunu bu belgenin kapsamı dışındadır.
Başvurmak [iOS tablolar ve hücreler çalışma](~/ios/user-interface/controls/tables/index.md) kullanıcı Arabirimi denetimleri hakkında daha fazla bilgi için Kılavuzu.

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

iOS veri işleyen olarak farklı bir `UITableView`.

## <a name="create-and-update"></a>Oluştur ve güncelleştir

Uygulama kodu basitleştirmek için tek bir yöntem kaydetme ekleme yapan sağlanan veya PrimaryKey olup ayarlandı bağlı olarak güncelleştirmedir. Çünkü `Id` özelliği ile işaretlenmiş bir `[PrimaryKey]` , ayarlı değil, kodunuzda özniteliği.
Bu yöntem, değeri (birincil anahtar özelliği kontrol ederek) kaydedilmiş önceki olup olmadığını algılar ve ekleme veya nesne uygun şekilde güncelleştirin:

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



Gerçek dünya uygulamaları genellikle bazı doğrulama (örneğin, gerekli alanları, en düşük uzunlukta veya diğer iş kuralları) gerektirir.
Doğrulama mantıksal paylaşılan kod, doğrulama hataları platformun özelliklerinden göre görüntülemek için kullanıcı Arabirimi yede geçirme mümkün olduğunca çok iyi platformlar arası uygulamalar uygular.

## <a name="delete"></a>Sil

Farklı `Insert` ve `Update` yöntemleri `Delete<T>` yöntemi yalnızca birincil anahtar değer yerine eksiksiz bir kabul edebilir `Stock` nesne.
Bu örnekte bir `Stock` nesne yönteme geçirilir, ancak yalnızca kimlik özelliği için geçirilen `Delete<T>` yöntemi.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Önceden doldurulmuş bir SQLite veritabanı dosyası kullanma

Bazı uygulamalar, zaten doldurulmuş bir veritabanı ile gönderilir.
Ayrıca, uygulamanızı mevcut bir SQLite veritabanı dosyasıyla sevkiyat ve erişmeden önce yazılabilir bir dizine kopyalanıyor mobil uygulamanızda bu kolayca gerçekleştirebilirsiniz. SQLite birçok platformunda kullanılan standart bir dosya biçimi olduğundan, bir SQLite veritabanı dosyası oluşturmak kullanılabilen araçlar vardır:

-  **SQLite Firefox yöneticisini** – iOS ve Android ile uyumlu olan Mac ve Windows ve oluşturan dosyalar üzerinde çalışır.
-  **Komut satırı** – bkz [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .


Dağıtım için bir veritabanı dosyası, uygulamanızı oluştururken, tablolar ve sütunlar bunların eşleşmesi ne kodunuzu bekliyor, özellikle, C# sınıfları ve özellikleri ile eşleşecek biçimde adlarını beklediği SQLite.NET kullanıyorsanız emin olmak için adlandırma ile ilgileniriz (veya ilişkili özel öznitelikler).

İOS, uygulamanızda sqlite dosyasını dahil edin ve ile işaretlenmiş olun **derleme eylemi: içerik**. Kodda yerleştirin `FinishedLaunching` dosyayı yazılabilir bir dizine kopyalamak için *önce* herhangi bir veri yöntem çağrısı. Aşağıdaki kodu adlı varolan bir veritabanını kopyalayacak **data.sqlite**, yalnızca zaten mevcut değilse.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Tüm veri erişim kodu (olmadığını ADO.NET veya SQLite.NET kullanarak) bu önceden doldurulmuş veri erişimi tamamlanmış olacaktır sahip olduktan sonra yürütür.


## <a name="related-links"></a>İlgili bağlantılar

- [DataAccess Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS veri tarifleri](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
