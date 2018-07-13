---
title: Kullanılabilir derlemeler
description: Bu belge, Xamarin.iOS ve Xamarin.Android Xamarin.Mac kullanılmaya derlemeleri listeler. Ayrıca .NET Standard kitaplıkları ve taşınabilir sınıf kitaplıkları hakkındaki belgelere bağlar.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: d005d6c5e1dcfe7e9bcff44b308cea0ce7ab73e9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998660"
---
# <a name="available-assemblies"></a>Kullanılabilir derlemeler

Xamarin.iOS, Xamarin.Android ve Xamarin.Mac tüm üzerinde bir düzine derlemeleri ile gönderin. Yalnızca Silverlight Masaüstü .NET derlemelerini genişletilmiş bir alt kümesi, Xamarin platformlar olduğundan ayrıca çeşitli Silverlight ve Masaüstü .NET derlemelerini genişletilmiş bir alt kümesi.

Xamarin platformlar ABI için farklı bir profil derlenmiş mevcut derlemeler ile uyumlu değildir. (Yalnızca, Silverlight ve .NET 3.5 ayrı olarak hedeflemek için kaynak kodu yeniden derlemeniz gerektiği gibi), doğru profilin hedefleyen derlemeleri oluşturmak için kaynak kodu yeniden derlemeniz gerekir.

Xamarin.Mac uygulamaları üç modlarında derlenmiş: Xamarin kullanan bir mobil profili seçkin sağlayan Xamarin.Mac .NET 4.5 Framework hedefleyen var olan tam masaüstü derlemeleri ve desteklenmeyen bir .NET API kullanan bir sistemde Mono bulunamadı yükleme. Daha fazla bilgi için lütfen bkz. bizim [hedef çerçeve](~/mac/platform/target-framework.md) belgeleri.


## <a name="net-standard-libraries"></a>.NET standart kitaplıkları

İOS, Android ve Mac yanı sıra bağlamaları, Xamarin projeleri kullanabileceği [.NET standart kitaplıkları](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Taşınabilir sınıf kitaplıkları
 
Xamarin projeleri de kullanabileceği [.NET taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md), ancak bu teknoloji .NET Standard ile değiştiriliyor kullanımdan kaldırılıyor.

## <a name="supported-assemblies"></a>Desteklenen derlemeler

> [!div class="mx-tdCol2BreakAll"]
> |Derleme|API uyumluluğu|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|CJK, diğer, nadir MidEast Batı içerir|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|ADO.NET sağlayıcısı sqlite; sınırlamalar bakın.|✓|✓|✓|
> |Mono.Data.Tds.dll|TDS protokol desteği; için kullanılan [System.Data.SqlClient](xref:System.Data.SqlClient) içinde destek [System.Data](xref:System.Data).|✓|✓|✓|
> |Mono.Dynamic. &#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|Şifreleme API'leri.|✓|✓|✓|
> |monotouch.dll|Bu derleme, C# bağlama CocoaTouch API'sine içeriyor. Bu yalnızca klasik iOS projeleri içinde kullanılabilir.|✓| | |
> |MonoTouch. &#8203;İletişim 1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|İPhone cihaz desteği sağlamak için genişletilmiş API'leri, OpenGL/OpenAL nesne yönelimli.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), şu ad alanlarından türlerinden ayrıca:<br />System.Collections.Specialized<br />Sistemi. &#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime. &#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security. &#8203;AccessControl<br />System.Security.Authentication<br />System.Security. &#8203;Şifreleme<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |Sistemi. &#8203;ComponentModel. &#8203;Composition.dll| |✓|✓|✓|
> |Sistemi. &#8203;ComponentModel. &#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx) , ile [bazı işlevler kaldırıldı](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|Tam oData istemcisi.|✓|✓|✓|
> |System.IO. &#8203;Sıkıştırma| |✓|✓|✓|
> |System.IO. &#8203;Sıkıştırma. &#8203;Dosya sistemi| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |Sistemi. &#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime. &#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|Mevcut olarak WCF yığın [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), şu ad alanlarından türlerinden ayrıca: <br />Sistem<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |Sistemi. &#8203;Transactions.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); parçası [System.Data](~/ios/data-cloud/system.data.md) destekler.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|.NET 3.5 profilinden kaldırılan sunucu özellikler ile temel Web Hizmetleri.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml. &#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Bu derleme, C# bağlama CocoaTouch API'sine içeriyor. Bu, yalnızca Birleşik iOS projelerinde kullanılır.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android. &#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices. &#8203;SymbolWriter.dll|Derleyici yazıcılar için.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |Sistemi. &#8203;Drawing.dll|System.Drawing API'si - yalnızca klasik API. System.Drawing birleşik API, Xamarin.Mac, .NET 4.5 veya mobil çerçeveleri için desteklenmiyor. System.Drawing destek eklenebilir, iOS ve OS X kullanarak [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) kitaplığı|✓| |✓|
