---
title: Yapılandırma
ms.prod: xamarin
ms.assetid: 44526226-4E4E-4FFF-9A16-CA7B1E01BB8F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/11/2016
ms.openlocfilehash: b3a7858361d25f26807ea328e8bfdd30ca8d483b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241885"
---
# <a name="configuration"></a>Yapılandırma

SQLite Xamarin.Android kullanmak için veritabanı dosyası için doğru dosya konumu belirlemeniz gerekir.

## <a name="database-file-path"></a>Veritabanı dosya yolu

SQLite ile veri depolanabilir önce kullandığınız hangi veri erişim yönteminden bağımsız olarak, bir veritabanı dosyası oluşturmanız gerekir. Dosya konumu, hedeflediğiniz platformdan bağlı olarak farklı olacaktır. Android için geçerli bir yol oluşturmak için aşağıdaki kod parçacığında gösterildiği gibi ortam sınıfını kullanabilirsiniz:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Veritabanı dosyasının depolanacağı karar verirken dikkate gereken diğer noktalar vardır. Örneğin, Android, iç veya dış depolama kullanılıp kullanılmayacağını seçebilirsiniz.

Platformlar arası uygulamanızdaki her platformda farklı bir konum kullanmak istiyorsanız, bir derleyici yönergesi gösterildiği gibi her platform için farklı bir yol oluşturmak için kullanabilirsiniz:

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

Dosya sistemi Android kullanma hakkında ipuçları için başvurmak [dosyalara Gözat](https://github.com/xamarin/recipes/tree/master/Recipes/android/data/files/browse_files) tarif. Bkz: [Çapraz Platform uygulamaları oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) belge her platform için özel kod yazmak için derleyici yönergeleri kullanma hakkında daha fazla bilgi için.

## <a name="threading"></a>İş Parçacığı Oluşturma

Çoklu iş parçacıkları arasında aynı SQLite veritabanı bağlantısı kullanmamalıdır. Açın, kullanmak ve aynı iş parçacığında oluşturduğunuz tüm bağlantıları kapatın konusunda dikkatli olun.

Kodunuzu SQLite veritabanındaki kullanıcıyla aynı anda birden fazla iş parçacığından erişmek çalışırken değil emin olmak için şunun gibi bir veritabanına erişmek için oluşturacağınız her bir kilit el ile uygulayın:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Tüm veritabanı erişimi (okuma, yazma, güncelleştirmeler vb.) ile aynı kilit alınmalı. Kilit yan tümcesi içinde iş basit tutulur ve ayrıca bir kilit sürebilir diğer yöntemler ile çağırmaz sağlayarak bir kilitlenme durumdan kaçınmak için dikkatli olunması gerekir!


## <a name="related-links"></a>İlgili bağlantılar

- [DataAccess Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android veri tarifleri](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
