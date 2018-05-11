---
title: İOS ile çalışmaya başlama
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: ab841c461356bd435db0c68e82c5e30d398d806a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-ios"></a>İOS ile çalışmaya başlama

## <a name="requirements"></a>Gereksinimler

Gereksinimlerden yanı sıra bizim [Objective-C ile çalışmaya başlama](~/tools/dotnet-embedding/get-started/objective-c/index.md) Kılavuzu, ayrıca gerekir:

* [Xamarin.iOS 10.11 sürümünü](https://www.visualstudio.com/xamarin/) veya daha yenisi

## <a name="hello-world"></a>Merhaba Dünya

İlk olarak, C# basit Merhaba Dünya örneği oluşturun.

### <a name="create-c-sample"></a>C# örnek oluşturma

Mac için Visual Studio'yu açın, yeni bir iOS sınıf kitaplığı proje oluşturma, adlandırın **csharp gelen hello**ve kaydetmesi **~/Projects/hello-from-csharp**.

Kodla **MyClass.cs** aşağıdaki kod parçacığıyla dosyası:

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

Projeyi derlemek ve elde edilen derlemeyi olarak kaydedilecek **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Yönetilen derleme bağlama

Yönetilen bir derleme sahip olduğunda, .NET katıştırma çağırarak bağlayın.

Bölümünde açıklandığı gibi [yükleme](~/tools/dotnet-embedding/get-started/install/install.md) Kılavuzu, bu yapılabilir, projenizin oluşturma sonrası adımı olarak özel bir MSBuild hedef veya el ile:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Framework yerleştirilecek **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode projesinde oluşturulan çıktı kullanın

Xcode açın, yeni bir iOS tek görünüm uygulaması oluşturma, adlandırın **csharp gelen hello**seçip **Objective-C** dili.

Açık **~/Projects/hello-from-csharp/output** Finder, select içinde dizin **csharp.framework gelen hello**, Xcode projeye sürükleyin ve bırakın yukarıdaki **csharp gelen hello**  proje klasöründe.

! [Sürükle ve bırak framework] Images/Hello-from-CSharp-iOS-Drag-DROP-Framework.png)

Emin olun **gerekirse öğeleri Kopyala** açılır iletişim denetlenir ve tıklayın **son**.

![Gerekirse öğeleri kopyala](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Seçin **csharp gelen hello** proje ve gidin **csharp gelen hello** hedefin **Genel sekmesinde**. İçinde **katıştırılmış ikili** bölümünde **csharp.framework gelen hello**.

![Katıştırılmış ikili dosyalar](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Açık **ViewController.m**ve içeriği ile değiştirin:

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

.NET katıştırma şu anda bitcode'u bazı Xcode proje şablonları etkin iOS desteklemiyor. 

Proje ayarlarınızı devre dışı bırakın:

![Bitcode'u seçeneği](../../images/ios-bitcode-option.png)

Son olarak, Xcode projesini çalıştırın ve şunun gibi görünür:

![Merhaba örnekten benzeticisinde çalıştıran C#](ios-images/hello-from-csharp-ios.png)
