---
title: İOS kullanmaya başlama
description: Bu belge .NET ekleme ile iOS kullanmaya başlama işlemini açıklamaktadır. Bu gereksinimleri açıklanır ve yönetilen bir derleme bağlama ve çıkış Xcode projesinde kullanma göstermek için örnek uygulama sunar.
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: d61eb8f1ad1def764c8552b2f047aa46cd712018
ms.sourcegitcommit: ef04a4ae1b19c1854a8e4e8315516d4030f4bbd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39654834"
---
# <a name="getting-started-with-ios"></a>İOS kullanmaya başlama

## <a name="requirements"></a>Gereksinimler

Alınan gereksinimlerine ek olarak sunduğumuz [Objective-C ile çalışmaya başlama](~/tools/dotnet-embedding/get-started/objective-c/index.md) Kılavuzu da kullanmanız gerekir:

* [Xamarin.iOS 10.11](https://visualstudio.microsoft.com/xamarin/) veya üzeri

## <a name="hello-world"></a>Merhaba Dünya

İlk olarak, basit bir hello world örnek C# oluşturun.

### <a name="create-c-sample"></a>C# örneği oluşturma

Mac için Visual Studio'yu açın, yeni bir iOS sınıf kitaplığı projesi oluşturun, adlandırın **csharp gelen hello**ve kaydetmesi **~/Projects/hello-from-csharp**.

Değiştirin **MyClass.cs** aşağıdaki kod parçacığı dosyası:

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

Projeyi oluşturmak ve elde edilen derlemeyi şu adla kaydedilecek **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Yönetilen bütünleştirilmiş kod bağlama

Yönetilen bir derleme oluşturduktan sonra .NET ekleme çağırarak bağlayın.

Bölümünde anlatıldığı gibi [yükleme](~/tools/dotnet-embedding/get-started/install/install.md) Kılavuzu, bu yapılabilir, projenizdeki derleme sonrası adımı olarak özel bir MSBuild hedefi veya el ile:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Framework yerleştirileceği **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode projesinde oluşturulan çıktıda kullanın

Xcode açın, yeni bir iOS tek görünüm uygulaması oluşturma, adlandırın **csharp gelen hello**seçip **Objective-C** dili.

Açık **~/Projects/hello-from-csharp/output** dizin Finder'da seçin **csharp.framework gelen hello**, Xcode projesine sürükleyin ve bunu açılan hemen üzerinde **csharp gelen Merhaba**  proje klasöründe.

![Sürükle ve bırak framework](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

Emin **gerekirse öğeleri Kopyala** açılır iletişim kutusunda denetlenir ve tıklayın **son**.

![Gerekirse öğeleri kopyala](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Seçin **csharp gelen hello** gidin ve proje **csharp gelen hello** hedefin **Genel sekmesinde**. İçinde **katıştırılmış ikili** bölümünde, eklemek **csharp.framework gelen hello**.

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

.NET ekleme şu anda bitcode'u bazı Xcode proje şablonları için etkinleştirilmiş olan iOS desteklemez. 

Proje ayarlarınızdaki devre dışı bırakın:

![Bitcode seçeneği](../../images/ios-bitcode-option.png)

Son olarak, bir Xcode projesini çalıştırın ve aşağıdaki gibi görünür:

![Örnekten simülatörde çalıştırılan C# Merhaba](ios-images/hello-from-csharp-ios.png)
