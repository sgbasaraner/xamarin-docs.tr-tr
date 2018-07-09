---
title: Amaç Sharpie Araçlar ve komutlar
description: Bu belgenin amacı Sharpie ve komut satırı bağımsız değişkenleri ile birlikte kullanabilmeniz için dahil edilen araçlara genel bakış sağlar.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 718b5104ddc4593d080b88b062c42d371d9e8e2e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855074"
---
# <a name="objective-sharpie-tools--commands"></a>Amaç Sharpie Araçlar ve komutlar

_Hedefi Sharpie ve komut satırı bağımsız değişkenleri ile bunları kullanmak için dahil edilen araçlara genel bakış._

<style type="text/css"> .Terminal mavi {color: rgb(10,96,254);} .terminal yeşil {renk: rgb(12,156,26);} .terminal Eflatun {renk: rgb(152,12,103);} </style>


Hedefi Sharpie başarılı olduktan sonra [yüklü](~/cross-platform/macios/binding/objective-sharpie/get-started.md), bir terminal açın ve ile kendinizi alıştırın <em>komutları</em> hedefi Sharpie sahip sunmak:

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
|**xcode**|Geçerli bir Xcode yüklemesi ve iOS ve Mac kullanılabilir SDK'lar sürümleri hakkında bilgi sağlar. Size sunduğumuz bağlamaları oluşturduğunuzda size daha sonra bu bilgileri kullanacaklardır.|
|**pod**|Arar ve yapılandırır, (bir yerel dizine) yükler ve Objective-C bağlar [CocoaPod](https://cocoapods.org/) ana özel depodan kitaplıkları. Bu araç otomatik olarak geçirilecek girişin doğru çıkarmaya yüklü CocoaPod değerlendirir `bind` aşağıdaki aracı. 3.0 içinde yeni!|
|**bağlama**|Üst bilgi dosyalarını ayrıştırma (`*.h`) içinde ilk Objective-C kitaplığına [ApiDefinition.cs ve StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) dosyaları.|
|**update**|Hedefi Sharpie'nın daha yeni sürümleri için denetler ve indirir ve varsa yükleyici başlatır.|
|**docs doğrulayın**|Hakkında ayrıntılı bilgileri gösterir `[Verify]` öznitelikleri.|
|**docs**|Varsayılan web tarayıcınızda bu belgeye gider.|

Belirli bir amaç Sharpie aracı hakkında Yardım almak için aracının adını girin ve `-help` seçeneği. Örneğin, `sharpie xcode -help` aşağıdaki çıktı döndürür:

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Bağlama işlemi başlatmadan önce terminale aşağıdaki komutu girerek geçerli yüklü Larımız hakkında bilgi almak ihtiyacımız `sharpie xcode -sdks`. Çıkış, yüklediğiniz Xcode hangi sürümüne bağlı olarak farklı olabilir. Amaç Sharpie birinde yüklü SDK'ları arar `Xcode*.app` altında `/Applications` dizini:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

Yukarıdakilerden, biz olduğunu görebiliriz `iphoneos9.1` SDK bizim makinede yüklü olan ve atanmış `arm64` mimari desteği. Bu bölümdeki tüm örnekleri için Biz bu değeri kullanıyor olacaksınız. Bu bilgileri bir yerde bir Objective-C Kitaplığı başlık dosyaları içinde ilk ayrıştırmak hazırız `ApiDefinition.cs` ve `StructsAndEnums.cs` bağlama projesi için.

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)