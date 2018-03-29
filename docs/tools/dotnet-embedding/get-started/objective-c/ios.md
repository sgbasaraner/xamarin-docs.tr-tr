---
title: İOS ile çalışmaya başlama
ms.topic: article
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 3921f57847bbe095f01ea9246ffe19f9bc5c5014
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="getting-started-with-ios"></a>İOS ile çalışmaya başlama


## <a name="requirements"></a>Gereksinimler

Gereksinimlerden yanı sıra bizim [Objective-C ile çalışmaya başlama](~/tools/dotnet-embedding/get-started/objective-c/index.md) Kılavuzu de gerekir:

* [Xamarin.iOS 10.11 sürümünü](https://www.visualstudio.com/xamarin/) veya daha yenisi

## <a name="hello-world"></a>Merhaba Dünya

İlk şimdi basit hello world örnek C# derleme.

### <a name="create-c-sample"></a>C# örnek oluşturma

Mac için Visual Studio'yu açın, yeni bir iOS sınıf kitaplığı proje oluşturma, adlandırın `hello-from-csharp`ve kaydetmesi `~/Projects/hello-from-csharp`.

Kodla `MyClass.cs` aşağıdaki kod parçacığıyla dosyası:

```csharp
using UIKit;
public class MyUIView : UITextView
{
    public MyUIView ()
    {
        Text = "Hello from C#";
    }
}
```

Proje derleme, elde edilen derlemeyi olarak kaydedilecek `~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll`.

### <a name="bind-the-managed-assembly"></a>Yönetilen derleme bağlama

Yönetilen derleme için doğal bir çerçeve oluşturmak için embeddinator çalıştırın:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Framework yerleştirilecek `~/Projects/hello-from-csharp/output/hello-from-csharp.framework`.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode projesinde oluşturulan çıktı kullanın

Xcode açın ve yeni bir iOS tek görünüm uygulaması oluşturma, adlandırın `hello-from-csharp` seçip **Objective-C** dili.

Açık `~/Projects/hello-from-csharp/output` Finder, select içinde dizin `hello-from-csharp.framework`, Xcode projeye sürükleyin ve bırakın yukarıdaki `hello-from-csharp` proje klasöründe.

! [Sürükle ve bırak framework] Images/Hello-from-CSharp-iOS-Drag-DROP-Framework.png)

Emin olun `Copy items if needed` açılır iletişim denetlenir ve tıklayın `Finish`.

![Gerekirse öğeleri kopyala](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Seçin `hello-from-csharp` proje ve gidin `hello-from-csharp` hedefin **Genel sekmesinde**. İçinde **katıştırılmış ikili** bölümünde `hello-from-csharp.framework`.

![Katıştırılmış ikili dosyalar](ios-images/hello-from-csharp-ios-embedded-binaries.png)

ViewController.m açın ve içeriği ile değiştirin:

```objective-c
#import "ViewController.h"
#include "hello-from-csharp/hello-from-csharp.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    MyUIView *view = [[MyUIView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}
@end
```

Son olarak Xcode projesini çalıştırın ve şunun gibi görünür:

![Merhaba örnekten benzeticisinde çalıştıran C#](ios-images/hello-from-csharp-ios.png)
