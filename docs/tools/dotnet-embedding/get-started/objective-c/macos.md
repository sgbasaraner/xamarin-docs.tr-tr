---
title: MacOS ile çalışmaya başlama
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: c129079aad14ac9e8aad6f73670ce9a43a36f222
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-macos"></a>MacOS ile çalışmaya başlama


## <a name="what-you-will-need"></a>İhtiyacınız olacak

* ' Ndaki yönergeleri izleyin bizim [Objective-C ile çalışmaya başlama](~/tools/dotnet-embedding/get-started/objective-c/index.md) Kılavuzu.

* İle kullanmak için bir .NET derlemesi **Embeddinator 4000**.

* MacOS Cocoa uygulama

' Ndaki yönergeleri izlediğinizden sonra lütfen devam bizim [Objective-C ile çalışmaya başlama](~/tools/dotnet-embedding/get-started/objective-c/index.md) Kılavuzu. Bir .NET derlemesi zaten varsa, doğrudan atlayabilirsiniz **kullanarak Embeddinator 4000** bölümü.

## <a name="creating-a-net-assembly"></a>Bir .NET derlemesi oluşturma

Açmak için ihtiyacınız bir .NET derlemesi oluşturmak için [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/) ve yeni bir **.NET kitaplığı projesi** yapılması tarafından *Dosya > Yeni bir çözüm > Diğer > .NET > Kitaplığı*. Sonraki tıklatın ve verin *hava durumu* olarak *proje adı*ve tıklayın *oluşturma*.

Sonraki adımları izleyin:

1. Silme **MyClass.cs** dosya ve **özellikleri** klasör.

2. Sağ tıklayın *hava durumu Proje > Ekle > yeni dosya.*

3. Seçin *boş sınıfı* ve **XAMWeatherFetcher** adıyla yeni'ye tıklayın.

4. Değiştir *XAMWeatherFetcher.cs* aşağıdaki kod ile:

```csharp
using System;
using System.Json;
using System.Net;

public class XAMWeatherFetcher {

    static string urlTemplate = @"https://query.yahooapis.com/v1/public/yql?q=select%20item.condition%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text%3D%22{0}%2C%20{1}%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys";
    public string City { get; private set; }
    public string State { get; private set; }

    public XAMWeatherFetcher (string city, string state)
    {
        City = city;
        State = state;
    }

    public XAMWeatherResult GetWeather ()
    {
        try {
            using (var wc = new WebClient ()) {
                var url = string.Format (urlTemplate, City, State);
                var str = wc.DownloadString (url);
                var json = JsonValue.Parse (str)["query"]["results"]["channel"]["item"]["condition"];
                var result = new XAMWeatherResult (json["temp"], json["text"]);
                return result;
            }
        }
        catch (Exception ex) {
            // Log some of the exception messages
            Console.WriteLine (ex.Message);
            Console.WriteLine (ex.InnerException?.Message);
            Console.WriteLine (ex.InnerException?.InnerException?.Message);

            return null;
        }

    }
}

public class XAMWeatherResult {
    public string Temp { get; private set; }
    public string Text { get; private set; }

    public XAMWeatherResult (string temp, string text)
    {
        Temp = temp;
        Text = text;
    }
}
```

Göreceksiniz `Using System.Json;` ihtiyacımız aşağıdakileri yapmak için bu durumu düzeltmek için hata; sağlar:

1. Çift tıklatın **başvuruları** klasör.

2. Tıklayın **paketleri** sekmesi.

3. Denetleme **System.Json**.

4. Click **Ok**.

Yukarıdaki tüm yapmak için ihtiyacımız yaptıktan sonra bizim .NET derlemesi tıklayarak yapıdır *yapı menüsüyle > Yapı tüm* veya ⌘ + b. İleti üst durum çubuğunda görünmelidir yürütmeye oluşturun.

Şimdi sağ tıklayın *hava durumu* proje düğümüne ve select *Finder ortaya*. Finder Git *bin/Debug* ; klasörü iç, bulacaksınız **Weather.dll.**

## <a name="using-embeddinator-4000"></a>Embeddinator 4000 kullanma

Embeddinator 4000 bizim pkg Yükleyicisi'ni kullanarak yüklediyseniz ve yüklendikten sonra yeni terminal oturumu başlatıldı, size kullanabilmek için **objcgen** komutu (Aksi halde, mutlak yolu kullanabilirsiniz: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`); **objcgen** .NET derlemesinden yerel kitaplık oluşturmak için ihtiyacımız aracıdır.

Terminali açın, `cd` Weather.dll, içeren klasöre ve yürütme **objcgen** aşağıda gösterilen bağımsız değişkenlerle:

```shell
cd /Users/Alex/Projects/Weather/Weather/bin/Debug

objcgen --debug --outdir=output -c Weather.dll
```

Her şeyi, yerleştirilecek içine **çıkış** yanındaki dizin *Weather.dll*. Kendi .NET derlemesi varsa Değiştir *Weather.dll* onu ve yukarıdaki aynı adımları izleyin.

## <a name="using-the-generated-output-in-an-xcode-project"></a>Xcode projesinde oluşturulan çıktı kullanma

Xcode açın ve yeni bir **macOS Cocoa uygulama** ve adlandırın **MyWeather**. Sağ tıklayın *MyWeather proje düğümüne*seçin *"MyWeather" için dosyaları Ekle*, gitmek **çıkış** tarafından oluşturulan dizin *Embeddinator 4000* , aşağıdaki dosyaları ekleyin:

* bindings.h
* embeddinator.h
* glib.h
* Mono support.h
* mono_embeddinator.h
* objc-support.h
* libWeather.dylib
* Weather.dll

Emin olun **gerekirse öğeleri Kopyala** dosya iletişim kutusu seçenekleri panelinde denetlenir.

Emin olmak ihtiyacımız artık **libWeather.dylib** ve **Weather.dll** uygulama paket alın:

* Tıklayın *MyWeather proje düğümüne*.
* Seçin *derleme aşamaları* sekmesi.
* Yeni bir ekleme *kopyalama dosyaları aşaması*.
* Üzerinde *hedef* seçin **çerçeveler** ve ekleme **libWeather.dylib**.
* Yeni bir ekleme *kopyalama dosyaları aşaması*.
* Üzerinde *hedef* seçin **yürütülebilir dosyalar**, ekleme **Weather.dll** ve emin olun *kod oturum kopyasında* denetlenir.

Şimdi açmak **ViewController.m** ve içeriği ile değiştirin:

```objective-c
#import "ViewController.h"
#import "bindings.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    XAMWeatherFetcher * fetcher = [[XAMWeatherFetcher alloc] initWithCity:@"Boston" state:@"MA"];
    XAMWeatherResult * weather = [fetcher getWeather];

    NSString * result;
    if (weather)
        result = [NSString stringWithFormat:@"%@ °F - %@", weather.temp, weather.text];
    else
        result = @"An error occured";

    NSTextField * textField = [NSTextField labelWithString:result];
    [self.view addSubview:textField];
}

@end
```

Son olarak Xcode projesini çalıştırın ve şunun gibi görünür:

![MyWeather örnek çalışıyor](macos-images/weather-from-csharp-macos.png)

Daha kapsamlı ve daha iyi görünümlü bir örnek kullanılabilir [burada](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
