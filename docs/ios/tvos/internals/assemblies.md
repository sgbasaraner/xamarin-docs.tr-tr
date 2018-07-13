---
title: TvOS için Xamarin tarafından desteklenen derlemeler
description: TvOS uygulamaları için kullanılabilir özellikler netleştirmeye yardımcı olması için bu belge, tvOS geliştirme için Xamarin tarafından desteklenen derlemelerin bir listesini sağlar.
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 89f2d4b1a4b58f49ab859d3603433427d05c7393
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996567"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>TvOS için Xamarin tarafından desteklenen derlemeler

## <a name="supported-assemblies"></a>Desteklenen derlemeler

Xamarin.tvOS uygulamalarınız için Xamarin tarafından desteklenen derlemelerin bir listesini budur. Bu ayrıntılı listesi aşağıda verilmiştir.  Bazı önemli atlamalar dahil `System.EnterpriseServices`, ASP.NET yığını ve Windows.Forms.

|Derleme|Eklendi|API uyumluluğu|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Derleyici yazıcılar için.|
|Mono.Data.Sqlite.dll|1.2|ADO.NET sağlayıcısı sqlite; bkz: [sınırlamaları](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|TDS protokol desteği; için kullanılan [System.Data.SqlClient](xref:System.Data.SqlClient) içinde destek [System.Data](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|Şifreleme API'leri.|
|monotouch.dll|1.0|Bu derlemeyi içeren [C# CocoaTouch API bağlama](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8).|
|mscorlib.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|API'leri, OpenGL/OpenAL nesne yönelimli [iPhone cihaz desteği sağlamak için Genişletilmiş](https://developer.xamarin.com/api/namespace/OpenGLES/).|
|System.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), şu ad alanlarından türlerinden ayrıca: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx), [kaldırılan bazı işlevler ile](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Tam oData istemcisi.|
|System.Drawing|1.0|System.Drawing API'si - yalnızca klasik API.<br />_System.Drawing birleşik API, Xamarin.Mac, .NET 4.5 veya mobil çerçeveleri için desteklenmiyor._|
|System.Json.dll|1.1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) yığın olarak mevcut [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), şu ad alanlarından türlerinden ayrıca: <ul><li>Sistem</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
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
- [Tvos uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
