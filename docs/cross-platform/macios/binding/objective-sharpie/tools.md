---
title: "Araçlar ve komutlar"
description: "Hedefi Sharpie ve komut satırı bağımsız değişkenleri ile bunları kullanmaya dahil araçlarına genel bakış."
ms.topic: article
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 0d6e953a6a45b78d470c7ff73e1d6faa7444a683
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="tools--commands"></a>Araçlar ve komutlar

_Hedefi Sharpie ve komut satırı bağımsız değişkenleri ile bunları kullanmaya dahil araçlarına genel bakış._

<style type="text/css"> .terminal-blue { color: rgb(10,96,254); } .terminal-green { color: rgb(12,156,26); } .terminal-magenta { color: rgb(152,12,103); } </style>


Hedefi Sharpie başarıyla olduğunda [yüklü](~/cross-platform/macios/binding/objective-sharpie/get-started.md), bir terminal açın ve ile öğrenmeniz <em>komutları</em> hedefi Sharpie sahip sunmak:

<pre>$ <b>sharpie -help</b>
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation</pre>

Amaç Sharpie aşağıdaki araçları sağlar:

<table>
  <thead>
    <tr><td>Aracı</td><td>Açıklama</td>
  </thead>
  <tbody>
    <tr><td><b>xcode</b></td><td>Geçerli Xcode yükleme ve iOS ve Mac kullanılabilen SDK'ları sürümleri hakkında bilgi sağlar. Biz bizim bağlamaları oluşturduğunuzda biz daha sonra bu bilgileri kullanarak.</td></tr>
    <tr><td><b>pod</b></td><td>Arar ve yapılandırır, (bir yerel dizine) yükler ve Objective-C bağlar <a href="https://cocoapods.org">CocoaPod</a> ana belirtim depodan kitaplıkları. Bu araç otomatik olarak geçirilecek girişin doğru türetme için yüklü CocoaPod değerlendirir <code>bind</code> aracı aşağıdaki. <em><strong>Yeni 3.0!</strong></em></td></tr>
    <tr><td><b>Bağlama</b></td><td>Üstbilgi dosyaları ayrıştırır (<code>*.h</code>) Objective-C Kitaplığı'nda <a href="~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md">ilk <i>ApiDefinition.cs</i> ve <i>StructsAndEnums.cs</i> dosyaları</a>.</td></tr>
    <tr><td><b>update</b></td><td>Hedefi Sharpie daha yeni sürümleri denetler ve yükler ve varsa yükleyici başlatır.</td></tr>
    <tr><td><b>verify-docs</b></td><td>Hakkında ayrıntılı bilgiler gösterilmektedir <code>[Verify]</code> öznitelikleri.</td></tr>
    <tr><td><b>Belgeleri</b></td><td>Bu belgede, varsayılan web tarayıcısı gider.</td></tr>
  </tbody>
</table>

Belirli bir amaç Sharpie aracını kullanarak Yardım almak için aracın adını girin ve `-help` seçeneği. Örneğin, `sharpie xcode -help` aşağıdaki çıktıyı döndürür:

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Bağlama işlemi başlayabilmeniz için önce terminale aşağıdaki komutu girerek bizim geçerli yüklü SDK'ları hakkında bilgi almak ihtiyacımız `sharpie xcode -sdks`. Çıktı yüklediğiniz hangi sürümler, Xcode bağlı olarak değişebilir. Amaç Sharpie birinde yüklü SDK'ları arar `Xcode*.app` altında `/Applications` dizini:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

Yukarıdaki biz olduğunu görebiliriz `iphoneos9.1` SDK bizim makinede yüklü ve sahip `arm64` mimarisi desteği. Biz bu değer bu bölümdeki tüm örnekleri için kullanır. Yerinde bu bilgilerle biz ilk Objective-C Kitaplık üstbilgi dosyaları ayrıştırmak hazır `ApiDefinition.cs` ve `StructsAndEnums.cs` bağlama projesi için.

