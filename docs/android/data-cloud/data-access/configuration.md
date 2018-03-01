---
title: "Yapılandırma"
ms.topic: article
ms.prod: xamarin
ms.assetid: 44526226-4E4E-4FFF-9A16-CA7B1E01BB8F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/11/2016
ms.openlocfilehash: dd4b4c13fd860a13f97ca93435399ed952b1ed0e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="configuration"></a>Yapılandırma

Xamarin.Android uygulamanıza SQLite kullanmak için veritabanı dosyası için doğru dosya konumu belirlemek gerekir.

## <a name="database-file-path"></a>Veritabanı dosya yolu

Kullandığınız veri erişim yöntemine bağımsız olarak, veriler ile SQLite depolanabilir önce bir veritabanı dosyası oluşturmanız gerekir. Dosya konumu, hedeflediğiniz hangi platformu bağlı olarak farklı olacaktır. Android için geçerli bir yol oluşturmak için aşağıdaki kod parçacığında gösterildiği gibi ortam sınıfını kullanabilirsiniz:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Veritabanı dosyasının depolanacağı karar verirken dikkate edilecek diğer noktalar vardır. Örneğin, Android, iç veya dış depolama kullanmayı seçebilirsiniz.

Platformlar arası uygulamanızdaki her platformda farklı bir konum kullanmak istiyorsanız, bir derleme yönergesi gösterildiği gibi her platform için farklı bir yol oluşturmak için kullanabilirsiniz:

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Android dosya sistemi kullanılarak hakkında ipuçları için bkz [dosyalara Gözat](https://developer.xamarin.com/recipes/android/data/Files/Browse_Files) tarif. Bkz: [Çapraz Platform uygulamaları oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) belge her platform için özel kod yazmanıza olanak derleyici yönergeleri kullanma hakkında daha fazla bilgi için.

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
- [Android veri tarif](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
