---
title: Amaç Sharpie Araçlar ve komutlar
description: Bu belgenin amacı Sharpie ve komut satırı bağımsız değişkenleri ile birlikte kullanabilmeniz için içerdiği araçlarına genel bakış sağlar.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 9ef566559249caca75281d9490d5314e08e26d44
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781071"
---
# <a name="objective-sharpie-tools--commands"></a>Amaç Sharpie Araçlar ve komutlar

_Hedefi Sharpie ve komut satırı bağımsız değişkenleri ile bunları kullanmaya dahil araçlarına genel bakış._

<style type="text/css"> .Terminal mavi {color: rgb(10,96,254);} .terminal yeşil {renk: rgb(12,156,26);} .terminal macenta {renk: rgb(152,12,103);} </style>


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

|Aracı|Açıklama|
|--- |--- |
|**Xcode**|Geçerli Xcode yükleme ve iOS ve Mac kullanılabilen SDK'ları sürümleri hakkında bilgi sağlar. Biz bizim bağlamaları oluşturduğunuzda biz daha sonra bu bilgileri kullanarak.|
|**pod**|Arar ve yapılandırır, (bir yerel dizine) yükler ve Objective-C bağlar [CocoaPod](https://cocoapods.org/) ana belirtim depodan kitaplıkları. Bu araç otomatik olarak geçirilecek girişin doğru türetme için yüklü CocoaPod değerlendirir `bind` aracı aşağıdaki. Yeni 3.0!|
|**Bağlama**|Üstbilgi dosyaları ayrıştırır (`*.h`) ilk içine Objective-C Kitaplığı'nda [ApiDefinition.cs ve StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) dosyaları.|
|**update**|Hedefi Sharpie daha yeni sürümleri denetler ve yükler ve varsa yükleyici başlatır.|
|**doğrulama belgeleri**|Hakkında ayrıntılı bilgiler gösterilmektedir `[Verify]` öznitelikleri.|
|**Belgeleri**|Bu belgede, varsayılan web tarayıcısı gider.|

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

