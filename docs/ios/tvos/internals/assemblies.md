---
title: Derlemeler
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 7d0ee27cfa2ae153ef481f943402f5fcfc5d04e4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="assemblies"></a>Derlemeler

## <a name="supported-assemblies"></a>Desteklenen derlemeler

Xamarin.tvOS uygulamaları için Xamarin tarafından desteklenen bir derleme listesi budur. Bu ayrıntılı listesi aşağıda listelenmiştir.  Bazı önemli atlamalar dahil `System.EnterpriseServices`, ASP.NET yığını ve Windows.Forms.

|Derleme|Eklenen|API Uyumluluk|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Derleyici yazarların.|
|Mono.Data.Sqlite.dll|1.2|ADO.NET sağlayıcısı için SQLite; bkz: [sınırlamalar](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|TDS protokol desteği; için kullanılan [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) içinde destek [System.Data](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|Şifreleme API'leri.|
|monotouch.dll|1.0|Bu derleme içeren [C# CocoaTouch API bağlama](https://developer.xamarin.com/api/root/ios-unified/).|
|mscorlib.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|API, OpenGL/OpenAL nesne yönelimli [iPhone cihaz desteği sağlamak için Genişletilmiş](https://developer.xamarin.com/api/namespace/OpenGLES/).|
|System.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), şu ad alanlarından türlerinden artı: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx), [kaldırılan bazı işlevler ile](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Tam oData istemcisi.|
|System.Drawing|1.0|System.Drawing API - yalnızca klasik API.<br />_System.Drawing Unified API Xamarin.Mac .NET 4.5 veya mobil çerçeveler için desteklenmiyor._|
|System.Json.dll|1.1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) yığın mevcut olarak [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), şu ad alanlarından türlerinden artı: <ul><li>Sistem</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); parçası [System.Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) destekler.|
|System.Web.Services|1.1|[Temel Web Hizmetleri](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) kaldırılan sunucu özellikler ile .NET 3.5 profilinden.|
|System.Xml.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Taşınabilir sınıf kitaplıkları

Mac bağlamaları yanı sıra Xamarin.tvOS tüketebileceği [.NET taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md).



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
