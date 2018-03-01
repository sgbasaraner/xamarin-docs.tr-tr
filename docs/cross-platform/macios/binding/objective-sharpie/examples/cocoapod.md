---
title: "Gerçek dünya CocoaPods kullanarak örnek"
ms.topic: article
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: ae92b491e6186371f1fc1ead835f918a94f18f86
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="real-world-example-using-cocoapods"></a>Gerçek dünya CocoaPods kullanarak örnek


**Bu örnekte [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).**

Yeni sürüm 3.0, hedefi Sharpie CocoaPods bağlama destekler ve hatta bir ön uç komutu vardır (`sharpie pod`) yükleme, yapılandırma ve CocoaPods çok kolay derleme yapma. Yapmanız gerekenler [faimilarize CocoaPods kendinizle](https://cocoapods.org) genel önce bu özelliği kullanarak.

`sharpie pod` Komutu, bir genel seçenek ve iki subcommands sahiptir:

```csharp
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

`init` Alt ayrıca bazı yararlı Yardım sahiptir:

```csharp
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

İçin birden çok CocoaPod ve subspec adlarını sağlanabilir `init`.

<pre>$ <b>sharpie pod init ios AFNetworking</b>
<span class="terminal-green">**</span> Setting up CocoaPods master repo ...
   (this may take a while the first time)
<span class="terminal-green">**</span> Searching for requested CocoaPods ...
<span class="terminal-green">**</span> Working directory:
<span class="terminal-green">**</span>   - Writing Podfile ...
<span class="terminal-green">**</span>   - Installing CocoaPods ...
<span class="terminal-green">**</span>     (running `<span class="terminal-blue">pod install --no-integrate --no-repo-update</span>`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
<span class="terminal-green">**</span> 🍻 Success! You can now use other `<span class="terminal-green">sharpie pod</span>`  commands.</pre>

Bağlama artık, CocoaPod ayarlandığına sonra oluşturabilirsiniz:

<pre>$ <b>sharpie pod bind</b></pre>

Bu yerleşik ve sonra değerlendirilir ve hedefi Sharpie tarafından ayrıştırılan CocoaPod Xcode projesi neden olur. Konsol çıktısı çok oluşturulur ancak bağlama tanımında sonunda neden olur:

<pre><em>(... lots of build output ...)</em>

<span class="terminal-blue">Parsing 19 header files...</span>

<span class="terminal-magenta">Binding...</span>
  <span class="terminal-magenta">[write]</span> ApiDefinitions.cs
  <span class="terminal-magenta">[write]</span> StructsAndEnums.cs

<span class="terminal-green">Done.</span></pre>

