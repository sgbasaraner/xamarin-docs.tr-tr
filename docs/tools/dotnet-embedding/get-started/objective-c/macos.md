---
title: MacOS ile çalışmaya başlama
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 85510168f37e724563eae59dfca438177b4feffe
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-macos"></a>MacOS ile çalışmaya başlama

## <a name="what-you-will-need"></a>İhtiyacınız olacak

* ' Ndaki yönergeleri izleyin [Objective-C ile çalışmaya başlama](~/tools/dotnet-embedding/get-started/objective-c/index.md) Kılavuzu.

## <a name="hello-world"></a>Merhaba Dünya

İlk olarak, C# basit Merhaba Dünya örneği oluşturun.

### <a name="create-c-sample"></a>C# örnek oluşturma

Mac için Visual Studio'yu açın, adlı yeni bir Mac sınıf kitaplığı proje oluşturma **csharp gelen hello**ve kaydetmesi **~/Projects/hello-from-csharp**.

Kodla **MyClass.cs** aşağıdaki kod parçacığıyla dosyası:

```csharp
using AppKit;
public class MyNSView : NSTextView
{
    public MyNSView ()
    {
        Value = "Hello from C#";
    }
}
```

Projeyi oluşturun. Elde edilen derlemeyi olarak kaydedilecek **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Yönetilen derleme bağlama

Yönetilen bir derleme sahip olduğunda, .NET katıştırma çağırarak bağlayın.

Bölümünde açıklandığı gibi [yükleme](~/tools/dotnet-embedding/get-started/install/install.md) Kılavuzu, bu yapılabilir, projenizin oluşturma sonrası adımı olarak özel bir MSBuild hedef veya el ile:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

Framework yerleştirilecek **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode projesinde oluşturulan çıktı kullanın

Xcode açın ve yeni bir Cocoa uygulaması oluşturun. Adlandırın **csharp gelen hello** seçip **Objective-C** dili.

Açık **~/Projects/hello-from-csharp/output** Finder, select içinde dizin **csharp.framework gelen hello**, Xcode projeye sürükleyin ve bırakın yukarıdaki **csharp gelen hello**  proje klasöründe.

![Sürükle ve bırak framework](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

Emin olun **gerekirse öğeleri Kopyala** açılır iletişim denetlenir ve tıklayın **son**.

![Gerekirse öğeleri kopyala](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

Seçin **csharp gelen hello** proje ve gidin **csharp gelen hello** hedefin **genel** sekmesi. İçinde **katıştırılmış ikili** bölümünde **csharp.framework gelen hello**.

![Katıştırılmış ikili dosyalar](macos-images/hello-from-csharp-mac-embedded-binaries.png)

Açık **ViewController.m**ve içeriği ile değiştirin:

```objc
#import "ViewController.h"

#include "hello-from-csharp/hello-from-csharp.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    MyNSView *view = [[MyNSView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}

@end
```

Son olarak, Xcode projesini çalıştırın ve şunun gibi görünür:

![Merhaba örnekten benzeticisinde çalıştıran C#](macos-images/hello-from-csharp-mac.png)

Daha eksiksiz ve sonrasının bir örnek [buradan kullanılabilir](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
