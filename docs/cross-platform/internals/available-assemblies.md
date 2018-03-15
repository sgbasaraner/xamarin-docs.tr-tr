---
title: "Kullanılabilir derlemeler"
description: "Xamarin.iOS, Xamarin.Android ve Xamarin.Mac kullanılabilir derlemeler"
ms.topic: article
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: 09df3b5e203281070e21b8c18ee4ff42245c0e46
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="available-assemblies"></a>Kullanılabilir derlemeler

_Xamarin.iOS, Xamarin.Android ve Xamarin.Mac kullanılabilir derlemeler_

Xamarin.iOS, Xamarin.Android ve Xamarin.Mac tüm üzerinde bir düzine Derlemelerle sevk. Yalnızca Silverlight Masaüstü .NET derlemelerini genişletilmiş bir alt kümesidir, Xamarin platformları aynıdır ayrıca çeşitli Silverlight ve Masaüstü .NET derlemelerini genişletilmiş bir kısmı.

Xamarin platformları ABI farklı bir profil derlenmiş varolan derlemeler ile uyumlu değildir. (Yalnızca ayrı olarak hedef Silverlight ve .NET 3.5 için kaynak kodunu yeniden derleyin gerektiği gibi) doğru profili hedefleme derlemeleri oluşturmak için kaynak kodunuzu derleyin gerekir.

Xamarin.Mac uygulamaları üç modda derlenmiş: Xamarin kullanan bir seçkin mobil profili, böylece Xamarin.Mac .NET 4.5 Framework hedefleyen varolan tam masaüstü derlemeler ve .NET API kullanır desteklenmeyen bir sistemde Mono bulundu yükleme. Daha fazla bilgi için lütfen bkz bizim [hedef çerçeveyi](~/mac/platform/target-framework.md) belgeleri.


## <a name="net-standard-libraries"></a>.NET standart kitaplıkları

İOS, Android ve Mac yanı sıra bağlamaları, projeler tüketebileceği Xamarin [.NET standart kitaplıkları](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Taşınabilir sınıf kitaplıkları
 
Xamarin projeleri de tüketebileceği [.NET taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md), ancak bu teknoloji .NET standart lehinde onaylanmaz.

## <a name="supported-assemblies"></a>Desteklenen derlemeler

> [!div class="mx-tdCol2BreakAll"]
> |Derleme|API Uyumluluk|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Includes CJK, MidEast, Other, Rare, West|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|ADO.NET sağlayıcısı için SQLite; sınırlamalar bakın.|✓|✓|✓|
> |Mono.Data.Tds.dll|TDS protokol desteği; için kullanılan [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) içinde destek [System.Data](https://developer.xamarin.com/api/namespace/System.Data/).|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|Şifreleme API'leri.|✓|✓|✓|
> |monotouch.dll|Bu derleme C# bağlama CocoaTouch API içerir. Bu yalnızca klasik iOS projeleri içinde kullanılabilir.|✓| | |
> |MonoTouch. &#8203;İletişim 1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|OpenGL/OpenAL Nesne API'leri, iPhone cihaz desteği sağlamak için genişletilmiş yönelimli.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx), şu ad alanlarından türlerinden artı:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx) , ile [işlevselliğinin kaldırılan](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|Tam oData istemcisi.|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|WCF yığını mevcut olarak [Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx), şu ad alanlarından türlerinden artı: <br />Sistem<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx); parçası [System.Data](~/ios/data-cloud/system.data.md) destekler.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|Temel Web hizmetlerinden .NET 3.5 profiliyle kaldırılan sunucu özellikler.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Bu derleme C# bağlama CocoaTouch API içerir. Bu, yalnızca Unified iOS projelerinde kullanılır.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|Derleyici yazarların.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|System.Drawing API - yalnızca klasik API. System.Drawing Unified API Xamarin.Mac .NET 4.5 veya mobil çerçeveler için desteklenmiyor. İOS ve OS X kullanarak System.Drawing destek eklenebilir [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) kitaplığı|✓| |✓|
