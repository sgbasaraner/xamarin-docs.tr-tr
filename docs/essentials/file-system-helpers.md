---
title: Xamarin.Essentials dosya sistemi Yardımcıları
description: Dosya sistemi sınıfı Yardımcıları uygulamanın önbellek ve veri dizinleri bulmak ve uygulama paketi dosyalarını açmak için bir dizi içerir.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 14aabc319fefdbad86f29a9d27ce39b59da35e3e
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2018
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials dosya sistemi Yardımcıları

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**FileSystem** sınıfı Yardımcıları uygulamanın önbellek ve veri dizinleri bulmak ve uygulama paketi dosyalarını açmak için bir dizi içerir.

## <a name="using-file-system-helpers"></a>Dosya sistemi Yardımcıları kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Depolamak için uygulamanın dizinine almak için **veriyi önbelleğe**. Önbellek verilerini, geçici verileri daha uzun kalıcı olması gerekiyor, ancak düzgün çalışması için gerekli veriler olmamalıdır tüm veriler için kullanılabilir.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Uygulamanın en üst düzey dizinine kullanıcı veri dosyaları için tüm dosyaları almak için. Bu dosyalar framework eşitleniyor işletim sistemi ile yedeklenir. Platform uygulaması özellikleri aşağıdaki bakın.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Uygulama paketinin paketlenmiş bir dosyayı açmak için:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Platform uygulama özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** – döndürür [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) geçerli bağlamın.
- **AppDataDirectory** – döndürür [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) olan ve geçerli bağlamı yedeklenen kullanarak [otomatik yedekleme](https://developer.android.com/guide/topics/data/autobackup.html) API 23 ve üzeri sürümlerde başlatılıyor.

Her dosya içine ekleme **varlıklar** Android klasöründe proje ve yapı eylem olarak işaretlemek **AndroidAsset** ile kullanmak için `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – döndürür [kitaplığı/önbellekleri](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) dizin.
- **AppDataDirectory** – döndürür [Kitaplığı](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) iTunes ve iCloud tarafından yedeklenir dizin.

Her dosya içine ekleme **kaynakları** iOS klasöründe proje ve yapı eylem olarak işaretlemek **BundledResource** ile kullanmak için `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** – döndürür [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) dizin...
- **AppDataDirectory** – döndürür [localFolder'daki](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) buluta yedeklendiğinden dizin.

UWP projesini kökte herhangi bir dosya ekleyin ve yapı eylem olarak işaretlemek **içerik** ile kullanmak için `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>API

- [Dosya sistemi Yardımcıları kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Dosya sistemi API belgeleri](xref:Xamarin.Essentials.FileSystem)