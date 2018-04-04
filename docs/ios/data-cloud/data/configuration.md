---
title: Yapılandırma
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: f4a1bc65fbba68b59196702978633eaf46bc44c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="configuration"></a>Yapılandırma

Xamarin.iOS uygulamanızı SQLite kullanmak için veritabanı dosyası için doğru dosya konumu belirlemek gerekir.

## <a name="database-file-path"></a>Veritabanı dosya yolu

Kullandığınız veri erişim yöntemine bağımsız olarak, veriler ile SQLite depolanabilir önce bir veritabanı dosyası oluşturmanız gerekir. Dosya konumu, hedeflediğiniz hangi platformu bağlı olarak farklı olacaktır. İOS için geçerli bir yol oluşturmak için aşağıdaki kod parçacığında gösterildiği gibi ortam sınıfını kullanabilirsiniz:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Veritabanı dosyasının depolanacağı karar verirken dikkate edilecek diğer noktalar vardır. İos'ta yedeklenen otomatik olarak (veya değil) veritabanına isteyebilirsiniz.

Platformlar arası uygulamanızdaki her platformda farklı bir konum kullanmak istiyorsanız, bir derleme yönergesi gösterildiği gibi her platform için farklı bir yol oluşturmak için kullanabilirsiniz:

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Başvurmak [dosya sistemi ile çalışma](~/ios/app-fundamentals/file-system.md) İos'ta kullanmak için hangi dosya konumları hakkında daha fazla bilgi için makalenin. Bkz: [Çapraz Platform uygulamaları oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) belge her platform için özel kod yazmanıza olanak derleyici yönergeleri kullanma hakkında daha fazla bilgi için.

## <a name="threading"></a>İş Parçacığı Oluşturma

Çoklu iş parçacıkları arasında aynı SQLite veritabanı bağlantısı kullanmamalısınız. Açmak için kullanın ve sonra aynı iş parçacığı üzerinde oluşturduğunuz tüm bağlantıları kapatın dikkatli olun.

Kodunuzu aynı anda birden çok iş parçacığından SQLite veritabanı erişmeye çalışan değil emin olmak için bu gibi bir veritabanına erişmek için kalacaklarını her bir kilit el ile alın:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Tüm veritabanı erişimi (okuma, yazma, güncelleştirmeler, vb.) ile aynı kilit alınmalı. Kilit yan tümcesi içinde iş basit tutulur ve ayrıca bir kilit sürebilir diğer yöntemleri çağırmaz sağlayarak bir kilitlenme durumdan kaçınmak için dikkatli olunması gerekir!


## <a name="related-links"></a>İlgili bağlantılar

- [ADO'da Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [ADO'da Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS veri tarif](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
