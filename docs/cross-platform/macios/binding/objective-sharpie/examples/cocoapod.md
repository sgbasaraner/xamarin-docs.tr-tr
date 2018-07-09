---
title: CocoaPods kullanarak gerçek örneği
description: Bu belge otomatik olarak bir CocoaPod C# bağlama tanımları oluşturmak için Objective Sharpie nasıl yapılacağı açıklanır.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: bac34f662e24c6b08a67cd8da1f41b37b43b3faf
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855214"
---
# <a name="real-world-example-using-cocoapods"></a>CocoaPods kullanarak gerçek örneği

> [!NOTE]
> Bu örnekte [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

Yeni sürüm 3.0, hedefi Sharpie CocoaPods bağlama destekler ve hatta bir komut içerir (`sharpie pod`) yükleme, yapılandırma ve oluşturma CocoaPods çok kolay hale getirmek için. Yapmanız gerekenler [CocoaPods ile kendinizi alıştırın](https://cocoapods.org) genel önce bu özelliği kullanarak.

## <a name="creating-a-binding-for-a-cocoapod"></a>Bağlama için bir CocoaPod oluşturma

`sharpie pod` Komutu, bir genel seçenek ve iki alt komutları sahiptir:

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

`init` Alt bazı yararlı bir Yardım de vardır:

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Birden çok CocoaPod ve subspec adları için sağlanabilir `init`.

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

Bağlama artık, CocoaPod ayarlandıktan sonra oluşturabilirsiniz:

```bash
$ sharpie pod bind
```

Bu yerleşik olarak değerlendirilir ve ardından hedefi Sharpie tarafından ayrıştırılan CocoaPod Xcode projesi neden olur. Konsol çıktısı birçok oluşturulur ancak bağlama tanımında en sonda sağlamalıdır:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma sonrasında **ApiDefinitions.cs** ve **StructsAndEnums.cs** dosyaları, uygulamalarınızda kullanmak için bir derleme oluşturmak için aşağıdaki belgelere göz atın:

- [Bağlama Objective-C genel bakış](~/cross-platform/macios/binding/overview.md)
- [Objective-C kitaplıklarını bağlama](~/cross-platform/macios/binding/objective-c-libraries.md)
- [İzlenecek yol: bir iOS Objective-C kitaplığını bağlama](~/ios/platform/binding-objective-c/walkthrough.md)
- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
