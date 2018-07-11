---
title: 'Xamarin.Essentials: Dosya sistemi Yardımcıları'
description: Xamarin.Essentials dosya sistemi sınıfı Yardımcıları uygulamanın önbellek ve veri dizinleri bulma ve uygulama paketi dosyalarını açmak için bir dizi içeriyor.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 13293ec05261cbdc1e70fd278002d1af18654851
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815624"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: Dosya sistemi Yardımcıları

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Dosya sistemi** sınıfı Yardımcıları uygulamanın önbellekte dizinleri bulmak ve uygulama paketi dosyalarını açmak için bir dizi içerir.

## <a name="using-file-system-helpers"></a>Dosya sistemi Yardımcıları kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Depolamak için uygulamanın dizin almak için **veriyi önbelleğe alma**. Önbellek verilerini, geçici verileri daha uzun kalıcı hale getirilmesi, ancak düzgün çalışması için gerekli veri olmaması gereken tüm veriler için kullanılabilir.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Kullanıcı veri dosyalarını olmayan tüm dosyalar için en üst düzey uygulamanın dizinine geçin. Bu dosyalar işletim sistemi framework eşitleniyor yedeklenir. Platform uygulaması özellikleri bakın.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Uygulama paket sunulur bir dosyayı açmak için:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Platform uygulaması özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** – döndürür [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) geçerli bağlam.
- **AppDataDirectory** – döndürür [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) olan ve geçerli bağlam yedeklenen kullanarak [otomatik yedekleme](https://developer.android.com/guide/topics/data/autobackup.html) API 23 ve yukarıdaki başlatılıyor.

Herhangi bir dosyaya ekleme **varlıklar** Android klasöründe proje ve derleme eylemi olarak işaretlemek **AndroidAsset** ile kullanmak için `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – döndürür [kitaplığı/önbellekler](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) dizin.
- **AppDataDirectory** – döndürür [Kitaplığı](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) iTunes ve iCloud tarafından yedeklenir dizin.

Herhangi bir dosyaya ekleme **kaynakları** iOS klasöründe proje ve derleme eylemi olarak işaretlemek **BundledResource** ile kullanmak için `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** – döndürür [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) dizin...
- **AppDataDirectory** – döndürür [localFolder'daki](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) buluta yedeklendiğinden dizin.

Kök dizininde UWP projesi içine herhangi bir dosya ekleyin ve derleme eylemi olarak işaretlemek **içerik** ile kullanmak için `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>API

- [Dosya sistemi Yardımcıları kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Dosya sistemi API belgeleri](xref:Xamarin.Essentials.FileSystem)
