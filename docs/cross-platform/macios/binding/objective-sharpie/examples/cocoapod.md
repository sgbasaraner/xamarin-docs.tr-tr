---
title: CocoaPods kullanarak gerçek dünya örneği
description: Bu belgenin amacı Sharpie CocoaPod C# bağlama tanımları otomatik olarak oluşturmak için nasıl kullanılacağını gösterir.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: 026b2c46f7c294d4ac4a110376131ec83c7c112e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33947400"
---
# <a name="real-world-example-using-cocoapods"></a>CocoaPods kullanarak gerçek dünya örneği

> [!NOTE]
> Bu örnekte [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

Yeni sürüm 3.0, hedefi Sharpie CocoaPods bağlama destekler ve hatta bir komut içerir (`sharpie pod`) yükleme, yapılandırma ve CocoaPods çok kolay derleme yapma. Yapmanız gerekenler [CocoaPods ile öğrenmeniz](https://cocoapods.org) genel önce bu özelliği kullanarak.

## <a name="creating-a-binding-for-a-cocoapod"></a>Bağlama için bir CocoaPod oluşturma

`sharpie pod` Komutu, bir genel seçenek ve iki subcommands sahiptir:

```bash
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

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

İçin birden çok CocoaPod ve subspec adlarını sağlanabilir `init`.

```bash
$ sharpie pod init ios AFNetworking
** Setting up CocoaPods master repo ...
   (this may take a while the first time)
** Searching for requested CocoaPods ...
** Working directory:
**   - Writing Podfile ...
**   - Installing CocoaPods ...
**     (running `pod install --no-integrate --no-repo-update`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
** 🍻 Success! You can now use other `sharpie podn`  commands.
```

Bağlama artık, CocoaPod ayarlandığına sonra oluşturabilirsiniz:

```bash
$ sharpie pod bind
```

Bu yerleşik ve sonra değerlendirilir ve hedefi Sharpie tarafından ayrıştırılan CocoaPod Xcode projesi neden olur. Konsol çıktısı çok oluşturulur ancak bağlama tanımında sonunda neden olur:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma sonrasında **ApiDefinitions.cs** ve **StructsAndEnums.cs** dosyalarının, uygulamalarınızı kullanmak için bir derleme oluşturmak için aşağıdaki belgelere göz alabilir:

- [Bağlama Objective-C genel bakış](~/cross-platform/macios/binding/overview.md)
- [Objective-C kitaplıkları bağlama](~/cross-platform/macios/binding/objective-c-libraries.md)
- [İzlenecek yol: bir iOS Objective-C Kitaplığı bağlama](~/ios/platform/binding-objective-c/walkthrough.md)

