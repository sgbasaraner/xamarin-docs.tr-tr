---
title: Derlemeler
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 0a435e80173ca3ccb76dba0719ec2eea6eb72611
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="assemblies"></a>Derlemeler

## <a name="supported-assemblies"></a>Desteklenen derlemeler

Xamarin.tvOS uygulamaları için Xamarin tarafından desteklenen bir derleme listesi budur. Bu ayrıntılı listesi aşağıda listelenmiştir.  Bazı önemli atlamalar dahil `System.EnterpriseServices`, ASP.NET yığını ve Windows.Forms.

<table align="center" border="1" cellpadding="1" cellspacing="1" width="90%">
      <tbody>
        <tr>
          <td>
            <strong>Derleme</strong>
          </td>
          <td>
            <strong>Eklenen</strong>
          </td>
          <td>
            <strong>API Uyumluluk</strong>
          </td>
        </tr>
        <tr>
          <td valign="top">
Mono.CompilerServices.SymbolWriter.dll </td>
          <td align="center" valign="top">
1.0 </td>
          <td valign="top">
Derleyici yazarların.
          </td>
        </tr>
        <tr>
          <td valign="top">
Mono.Data.Sqlite.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
ADO.NET sağlayıcısı için SQLite; bkz:&nbsp;<a href="~/ios/data-cloud/system.data.md">sınırlamalar</a>.
          </td>
        </tr>
        <tr>
          <td valign="top">
Mono.Data.Tds.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
TDS protokol desteği; için kullanılan&nbsp;<a href="https://developer.xamarin.com/api/namespace/System.Data.SqlClient/" target="_blank">System.Data.SqlClient</a>&nbsp;içinde destek&nbsp;<a href="~/ios/data-cloud/system.data.md">System.Data</a>.
          </td>
        </tr>
        <tr>
          <td>
Mono.Security.dll </td>
          <td align="center" valign="top">
1.0 </td>
          <td>
Şifreleme API'leri.
          </td>
        </tr>
        <tr>
          <td valign="top">
monotouch.dll </td>
          <td align="center" valign="top">
1.0 </td>
          <td>
Bu derleme içeren&nbsp;<a href="https://developer.xamarin.com/api/root/ios-unified/" target="_blank">C# CocoaTouch API bağlama</a>.
          </td>
        </tr>
        <tr>
          <td valign="top">
mscorlib.dll </td>
          <td align="center" valign="top">
1.0 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
OpenTK.dll </td>
          <td align="center" valign="top">
1.0 </td>
          <td>
API, OpenGL/OpenAL nesne yönelimli&nbsp;<a href="https://developer.xamarin.com/api/namespace/OpenGLES/" target="_blank">iPhone cihaz desteği sağlamak için Genişletilmiş</a>.
          </td>
        </tr>
        <tr>
          <td align="left" valign="top">
System.dll </td>
          <td align="center" valign="top">
1.0 </td>
          <td>
            <p><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>, şu ad alanlarından türlerinden artı:</p>
    
            <ul>
              <li>System.Collections.Specialized</li>
              <li>System.ComponentModel</li>
              <li>System.ComponentModel.Design</li>
              <li>System.Diagnostics</li>
              <li>System.IO.Compression</li>
              <li>System.Net</li>
              <li>System.Net.Cache</li>
              <li>System.Net.Mail</li>
              <li>System.Net.Mime</li>
              <li>System.Net.NetworkInformation</li>
              <li>System.Net.Security</li>
              <li>System.Net.Sockets</li>
              <li>System.Security.Authentication</li>
              <li>System.Security.Cryptography</li>
              <li>System.Timers</li>
            </ul>
    
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Core.dll </td>
          <td align="center" valign="top">
1.0 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Data.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>&nbsp;,&nbsp;<a href="~/ios/data-cloud/system.data.md">kaldırılan bazı işlevler ile</a>.
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Data.Service.Client.dll </td>
          <td align="center" valign="top">
3.x </td>
          <td>
Tam oData istemcisi.
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Drawing </td>
          <td align="center" valign="top">
1.0 </td>
          <td>
            <p>System.Drawing API - yalnızca klasik API.</p>
            <p><i>System.Drawing Unified API Xamarin.Mac .NET 4.5 veya mobil çerçeveler için desteklenmiyor.</i></p>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Json.dll </td>
          <td align="center" valign="top">
1.1 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Runtime.Serialization.dll </td>
          <td align="center" valign="top">
?
          </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.ServiceModel.dll </td>
          <td align="center" valign="top">
1.1 </td>
          <td>
            <a href="http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services" target="_blank">WCF</a>&nbsp;yığın mevcut olarak&nbsp;<a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.ServiceModel.Web.dll </td>
          <td align="center" valign="top">
?
          </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>, şu ad alanlarından türlerinden artı: <p>&nbsp;</p>
    
            <ul>
              <li>Sistem</li>
              <li>System.ServiceModel.Channels</li>
              <li>System.ServiceModel.Description</li>
              <li>System.ServiceModel.Web</li>
            </ul>
    
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Transactions.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>; parçası&nbsp;<a href="~/ios/data-cloud/system.data.md">System.Data</a>&nbsp;destekler.
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Web.Services </td>
          <td align="center" valign="top">
1.1 </td>
          <td>
            <a href="http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services" target="_blank">Temel Web Hizmetleri</a>&nbsp;kaldırılan sunucu özellikler ile .NET 3.5 profilinden.
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Xml.dll </td>
          <td align="center" valign="top">
1.0 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Xml.Linq.dll </td>
          <td align="center" valign="top">
1.0 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a><br>
            <br>
&nbsp; </td>
        </tr>
      </tbody>
    </table>

<a name="Summary" />

## <a name="portable-class-libraries"></a>Taşınabilir sınıf kitaplıkları

Mac bağlamaları yanı sıra Xamarin.tvOS tüketebileceği [.NET taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md).



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
