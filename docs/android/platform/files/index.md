---
title: Xamarin.Android ile dosya erişimi
description: Bu kılavuzda dosya erişim Xamarin.Android açıklayacak
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 07/23/2018
ms.openlocfilehash: 5a4ddf606bb71bef10cf99660c198c5a8fdb1b69
ms.sourcegitcommit: 9bb9e8297d3edd9a50585f4ba53c1b4f0bcd1d3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39212162"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>Dosya depolama ve Xamarin.Android ile erişim

Dosyaları yönetmek için Android uygulamaları için ortak bir gereksinim olan &ndash; resimleri kaydetme, belgeler indiriliyor veya diğer programlarla paylaşmak için verileri dışarı aktarma. (Bu, Linux tabanlı) android için dosya depolama alanı sağlayarak destekler. Android, iki farklı depolama türlerini dosya grupları:

* **İç Depolama** &ndash; yalnızca uygulama veya işletim sistemi tarafından erişilebilen dosya sisteminin bir bölümünü budur.
* **Dış depolama** &ndash; tüm uygulamalar, kullanıcı ve diğer cihazlar tarafından büyük olasılıkla erişilebilir dosya depolama için bir bölüm budur. Bazı cihazlar, dış depolama (SD kartı gibi) çıkarılabilir olabilir.

Koruyabilmesini yalnızca kavramsal ve mutlaka tek bölüm veya cihazdaki dizinine başvuru yok. Bir Android cihazı, her zaman iç depolama ve harici depolama için bölüm sağlayacaktır. Bu, belirli cihazları dış depolama olarak değerlendirilir, birden çok bölüm olabilir mümkündür. Bölüm bağımsız olarak okumak için API'ler, yazma veya dosyaları oluşturma aynıdır. Bir Xamarin.Android uygulaması dosya erişimi için kullanabileceği API'leri iki tür vardır:

1. **.NET (Mono tarafından sağlanan ve Xamarin.Android tarafından Sarmalanan) API'lerini** &ndash; bunlar içerir [dosya sistemi Yardımcıları](~/essentials/file-system-helpers.md?context=xamarin/android) tarafından sağlanan [Xamarin.Essentials](~/essentials/index.md?context=xamarin/android). .NET API'lerini en iyi platformlar arası uyumluluğu sağlamak ve bu nedenle, bu kılavuzun odak bu API'leri olacaktır.
1. **Yerel bir Java dosya erişimi (Java tarafından sağlanan ve Xamarin.Android tarafından Sarmalanan) API'leri** &ndash; Java dosyaları okuma ve yazma için kendi API'ler sağlar. Bunlar tamamen kabul edilebilir bir alternatif .NET API'lerine ancak Android özgüdür ve platformlar arası olmayı amaçlayan uygulamalar için uygun değildir.

Diğer herhangi bir .NET uygulamasında olduğu için dosyaları okuma ve yazma Xamarin.Android neredeyse aynıdır. Xamarin.Android uygulaması değiştirilecek, ardından kullanımlar standart .NET deyimleri dosya erişimi için dosya yolunu belirler. Android sürümünden Android sürümüne veya cihazı başka bir cihaz gerçek iç ve dış depolama yolları farklılık gösterebileceğinden dosyalarının yolunu sabit kod için önerilmez. Bunun yerine, dosya yolunu belirlemek için Xamarin.Android API'lerini kullanın. Bu sayede, dosyaları okuma ve yazma için .NET API'lerini yerel Android dahili ve harici depolama dosyalarının yolunu belirleme ile yardımcı olacak API'lerini kullanıma sunar.

Dosya erişimi ile ilgili API'leri ele almadan önce dahili ve harici depolama çevreleyen ayrıntılarının bazılarını anlamak önemlidir. Bu sonraki bölümde açıklanmıştır.

## <a name="internal-vs-external-storage"></a>İç ve dış depolama

İç Depolama ve harici depolama kavramsal olarak, çok benzer &ndash; oldukları her iki yerde, bir Xamarin.Android uygulaması dosyaları kaydedebilir. Bu uygulama iç depolama vs dış depolama kullandığınızda, açık olmadığından Android ile tanıdık olmayan geliştiriciler için kafa karıştırıcı olabilir.

İç Depolama Apk'lar, işletim sistemine ve tek tek uygulamalar için Android ayırır geçici olmayan belleği ifade eder. Bu alan işletim sistemi veya uygulama tarafından dışında erişilemez. Android her uygulama için dahili depolama bölüm bir dizindeki ayırır. Uygulama kaldırıldığında, bu dizine iç depolamada tutulan tüm dosyalar da silinir. İç Depolama alanı, yalnızca uygulamaya erişilebilir olduğundan ve diğer uygulamalarla paylaşılmaz veya uygulama kaldırıldıktan sonra çok az değerine sahip olacaktır dosyaları için idealdir. Android 6.0 veya üzeri sürümde, iç depolama dosyaları otomatik olarak Google kullanarak yedeklenmeyebilir [otomatik yedekleme özelliği Android 6.0](https://developer.android.com/guide/topics/data/autobackup). İç depolama alanına aşağıdaki dezavantajları bulunur:

* Dosyaları paylaşılamaz.
* Uygulama kaldırıldığında dosyalar silinecek.
* Belki de sınırlı iç depolama alanında mevcut olan alanı.

Dış depolama iç depolama değil dosya depolama alanına başvuruyor ve bir uygulama için özel olarak erişilebilir. Dış depolama birincil amacı, uygulamalar arasında paylaşılacak yöneliktir veya iç depolama alanında sığmayacak kadar büyük olan dosyaları koyabileceğimiz bir yer sağlamaktır. Dış depolama avantajı, genellikle dosyaları için daha fazla alan iç depolamaya sahip olur. Bununla birlikte, dış depolama her zaman bir cihazda mevcut olması garanti edilmez ve ona erişmek için özel izin kullanıcıdan gerektirebilir.

> [!NOTE]
> Birden çok kullanıcı destekleyen cihazlar için Android her kullanıcının kendi dizin iç ve dış depolama alanı sağlar. Bu dizin, cihazdaki diğer kullanıcılar tarafından erişilemez. İç veya dış depolama dosyalarda değil sabit kodlamayın yolları yaptıkları sürece bu ayrımı uygulamalara görünmez durumdadır.

Bir kural karşısında, Xamarin.Android uygulamaları makul iç depolama dosyalarını kaydederken tercih ve diğer uygulamalarla paylaşılması gerekiyor, çok büyük veya uygulama kaldırılır bile korunması gereken dosyaları dış depolama alanında kullanan gerekir. Dışında hiçbir önem oluşturduğu bir uygulamaya sahip olduğundan, bir yapılandırma dosyası bir iç depolama alanı için idealdir.  Buna karşılık, fotoğraflar dış depolama için iyi bir adaydır. Çok büyük olabilir ve bunları paylaşabilir veya uygulama kaldırılır bile bunları erişmek kullanıcı çoğu durumda isteyebilirsiniz.

Bu kılavuz, iç depolama alanına odaklanır. Lütfen kılavuzuna bakın [dış depolama](~/android/platform/files/external-storage.md) bir Xamarin.Android uygulamasına dış depolama kullanma hakkında bilgi.

## <a name="working-with-internal-storage"></a>İç Depolama ile çalışma

Bir uygulama için dahili depolama dizini işletim sistemi tarafından belirlenir ve Android uygulamaları tarafından kullanıma sunulan `Android.Content.Context.FilesDir` özelliği. Bu döndürür bir `Java.IO.File` Android uygulaması için özel olarak ayrılmış dizin temsil eden nesne.  Örneğin, paket adı ile bir uygulama **com.companyname** iç depolama dizini olabilir:

```bash
/data/user/0/com.companyname/files
```

Bu belge, iç depolama dizine başvuracaktır _dahili\_depolama_.

> [!IMPORTANT]
> İç Depolama dizinine tam yolun CİHAZDAN cihaz ve Android sürümleri arasında değişebilir. Bu nedenle, uygulamaları sabit olmayan iç dosyalar depolama dizini yolunu kod ve bunun yerine Xamarin.Android API'leri gibi kullanmanız gerekir `System.Environment.GetFolderPath()`.

Kod paylaşımını en üst düzeye çıkarmak için Xamarin.Android uygulamaları (veya Xamarin.Forms uygulamaları hedefleyen Xamarin.Android) kullanmanız gerekir [ `System.Environment.GetFolderPath()` ](xref:System.Environment.GetFolderPath*) yöntemi. Xamarin.Android bu yöntem ile aynı konumda bir dizin için bir dize döndürür `Android.Content.Context.FilesDir`. Bu yöntem bir Sıralayıcı alır `System.Environment.SpecialFolder`, işletim sistemi tarafından kullanılan özel klasörler yollarını temsil eden numaralandırılmış sabitler kümesini tanımlamak için kullanılır. Tüm `System.Environment.SpecialFolder` değerleri Xamarin.Android üzerinde geçerli bir dizin eşlemeniz. Aşağıdaki tablo, hangi yolu için belirtilen değeri beklenebilir açıklar `System.Environment.SpecialFolder`:

| System.Environment.SpecialFolder | Yol  |
|----------------------|---|
| `ApplicationData` | **_İÇ\_depolama_/.config** |
| `Desktop` | **_İÇ\_depolama_  /Masaüstü** |
| `LocalApplicationData` | **_İÇ\_depolama_/.local/share** |
| `MyComputer` | **_İÇ\_depolama_/.local/share** |
| `MyDocuments` | **_İÇ\_DEPOLAMA_** |
| `MyMusic` | **_İÇ\_depolama_/Music** |
| `MyPictures` | **_İÇ\_depolama_/Music** |
| `MyVideos` | **_İÇ\_depolama_/Videos** |
| `Personal` | **_İÇ\_DEPOLAMA_** |


### <a name="reading-or-writing-to-files-on-internal-storage"></a>Okuma veya iç depolama alanına ve dosyalara yazma

Herhangi bir [yazmak için C# API](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) bir dosya için gerekli olan tek şey ayrılan dizinde yer dosyaya olan yolu almak için yeterli; olan uygulama. Zaman uyumsuz sürümlerini .NET API'lerini olabilecek sorunları en aza indirmek için kullanılan ana iş parçacığı engelleme ile dosya erişimi ilişkilendirmek önemle tavsiye edilir.

Bu kod parçacığı bir tamsayı uygulamanın iç depolama dizinine UTF-8 metin dosyasına yazmanın bir örnek verilmiştir:

```csharp
public async Task SaveCountAsync(int count)
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");
    using (var writer = File.CreateText(backingFile))
    {
        await writer.WriteLineAsync(count.ToString());
    }
}
```

Aşağıdaki kod parçacığı bir metin dosyasında saklanan bir tamsayı değeri okumak için bir yol sağlar:

```csharp
public async Task<int> ReadCountAsync()
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");

    if (backingFile == null || !File.Exists(backingFile))
    {
        return 0;
    }

    var count = 0;
    using (var reader = new StreamReader(backingFile, true))
    {
        string line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (int.TryParse(line, out var newcount))
            {
                count = newcount;
            }
        }
    }

    return count;
}
```

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Xamarin.Essentials kullanarak &ndash; dosya sistemi Yardımcıları

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android) platformlar arası uyumlu kod yazmak için API'ler kümesidir. [Dosya sistemi Yardımcıları](~/essentials/file-system-helpers.md?context=xamarin/android) Yardımcıları uygulamanın önbellekte dizinleri bulma basitleştirmek için bir dizi içeren bir sınıftır. Bu kod parçacığı, bir uygulama için dahili depolama dizini ve önbellek dizin bulmak nasıl bir örnek sağlar:

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>Dosyalardan gizleme `MediaStore`

`MediaStore` Medya dosyalarını (video, müzik, resim) ilgili meta verileri toplayan bir Android bir Android cihazında bileşendir. Amacı olan cihazdaki tüm Android uygulamalarında bu dosyaların paylaşımını basitleştirin.

Özel dosyaları paylaşılabilir ortam olarak gösterilmez. Uygulama, resim, özel bir dış depolama birimine kaydederse, örneğin, ardından bu dosyayı medya tarayıcı tarafından seçilir değil (`MediaStore`).

Ortak dosyalar toplanmış tarafından `MediaStore`. Sıfır bayt dosya adı dizinleri **. NOMEDIA** tarafından taranmaz `MediaStore`.

## <a name="related-links"></a>İlgili bağlantılar

* [Harici Depolama](~/android/platform/files/external-storage.md)
* [Cihaz depolama dosyaları kaydetme](https://developer.android.com/training/data-storage/files)
* [Xamarin.Essentials dosya sistemi Yardımcıları](~/essentials/file-system-helpers.md?context=xamarin/android)
* [Yedekleme kullanıcı verileri ile otomatik yedekleme](https://developer.android.com/guide/topics/data/autobackup)
* [Adoptable depolama](https://source.android.com/devices/storage/adoptable)
