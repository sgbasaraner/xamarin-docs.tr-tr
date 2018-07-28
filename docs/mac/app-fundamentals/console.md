---
title: Konsol uygulamaları için Xamarin.Mac bağlamaları kullanma
description: Yerel macOS API'leri erişmek için Xamarin.Mac kullanarak bir başsız konsol uygulaması oluşturacaksınız.
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: 135ef06cd044235023c3acc970c8e7a33f144b47
ms.sourcegitcommit: d0da5ce4158239abd2b36f67550e9b475055766a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320815"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>Konsol uygulamalarında Xamarin.Mac bağlamaları

Apple yerel API'lerden bazılarını C# dilinde gözetimsiz bir uygulama oluşturmak için kullanmak istediğiniz bazı senaryolar vardır &ndash; kullanıcı arabirimine sahip olmayan bir &ndash; C# kullanarak.

Mac uygulaması için proje şablonları bir çağrı ekleyin `NSApplication.Init()` bir çağrı tarafından izlenen `NSApplication.Main(args)`, genellikle şu şekilde görünür:

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

Çağrı `Init` Xamarin.Mac çalışma zamanı, çağrı hazırlar `Main(args)` klavye ve fare olayları alma ve uygulamanızın ana pencereyi gösterme uygulamanın hazırlar Cocoa uygulaması ana döngü başlatır.   Çağrı `Main` ayrıca dener Cocoa kaynaklarını bulmak için bir toplevel penceresi hazırlama ve programın bir uygulama paketi parçası olmasını bekliyor (bir dizinde dağıtılmış programlar `.app` uzantısı ve belirli bir düzeni).

Gözetimsiz uygulamalar gerekli bir kullanıcı arabirimi ve bir uygulama paketi bir parçası çalıştırmak gerekmez.

## <a name="creating-the-console-app"></a>Konsol uygulaması oluşturma

Bu nedenle normal bir .NET konsol projesi türü ile başlamanız iyidir.

Bazı şeyler yapmanız gerekir:

- Boş bir proje oluşturun.
- Başvuru Xamarin.Mac.dll kitaplığı.
- Yönetilmeyen projenize getirin.

Bu adımlar aşağıda daha ayrıntılı olarak açıklanmıştır:

### <a name="create-an-empty-console-project"></a>Boş bir konsol projesi oluşturma

Yeni bir .NET konsol projesi oluşturmak için .NET ve Xamarin.Mac.dll olarak .NET Core değil, .NET Core çalışma zamanı altında çalışmaz, yalnızca Mono çalışma zamanı ile çalışır olmasını sağlayın.

### <a name="reference-the-xamarinmac-library"></a>Xamarin.Mac Kitaplığı Başvurusu

Kodunuzu derlemek için başvurmak istediğiniz `Xamarin.Mac.dll` bu dizinden derleme: `/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

Bunu yapmak için Proje referansı gidin **.NET bütünleştirilmiş kodu** sekmesine ve tıklayın **Gözat** düğmesini dosya sistemindeki dosyasını bulun.  Yukarıdaki yoluna gidin ve ardından **Xamarin.Mac.dll** bu dizinden.

Bu işlem, derleme zamanında Cocoa API'lere erişim sağlar.   Bu noktada, ekleyebileceğiniz `using AppKit` dosya ve çağrı üstüne `NSApplication.Init()` yöntemi.   Uygulamanızı çalıştırmadan önce yalnızca tek bir adım yoktur.

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>Yönetilmeyen destek kitaplığı projenize Getir

Uygulamanızın çalışması için önce getirin gerekiyor `Xamarin.Mac` destek kitaplığı projenize.   Bunu yapmak için projenize yeni bir dosya ekleyin (Proje seçenekleri belirleyin **Ekle**, ardından **mevcut Dosya Ekle**) ve bu dizine gidin:

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

Burada, dosyayı seçin **libxammac.dylib**.   Kopyalama, bağlama veya taşıma seçeneği sunulacaktır.   Kişisel bağlama istiyorum, ancak kopyalama de çalışır.    Sonra dosyayı seçmeniz gerekir ve özellik panelinde (seçin **Görünümü > doldurmalar > Özellikleri** özellik paneli görünür değilse) gidin **derleme** bölümünde ve ayarlamak **çıktıya Kopyala Dizin** ayarını **yeniyse Kopyala**.

Xamarin.Mac uygulamanız artık çalıştırabilirsiniz.

Bin dizinindeki sonucu şöyle görünür:

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

Bu uygulamayı çalıştırmak için bu dosyaları ile aynı dizinde gerekir.

## <a name="building-a-standalone-application-for-distribution"></a>Dağıtım için bir tek başına uygulama oluşturma

Kullanıcılarınız için tek bir yürütülebilir dosya dağıtmak isteyebilirsiniz.  Bunu yapmak için kullanabileceğiniz `mkbundle` araç kendi içinde bir yürütülebilir dosyanın içine çeşitli dosyaları açmak için.

İlk olarak, uygulamanızı derler ve çalıştırır emin olun.   Sonuçlardan memnun olduğunuzda, komut satırından aşağıdaki komutu çalıştırabilirsiniz:

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

Yukarıdaki komut satırı çağrısı seçeneği `-o` olan oluşturulan çıktı belirtmek için kullanılan, bu durumda, biz geçirilen `/tmp/consoleapp`.   Bu artık dağıtabileceğiniz tek başına bir uygulamadır ve Mono veya Xamarin.Mac dış bağımlılıkları yoktur, bir tam olarak kendi çalıştırılabilir.

El ile belirtilen komut satırı **machine.config** kullanılacak dosya ve sistem genelinde kitaplık eşleme yapılandırma dosyası.   Tüm uygulamalar için gerekli değildir, ancak daha fazla .NET özelliklerini kullandığınızda kullanılabilir, paket uygun olan

## <a name="project-less-builds"></a>Proje olmayan yapılar

Kendi içinde bir Xamarin.Mac uygulamasını oluşturmak için tam bir proje gerekmez, işlerinizi tamamlamanız için basit UNIX derleme görevleri dosyalarını da kullanabilirsiniz.   Aşağıdaki örnek, bir basit bir komut satırı uygulaması için bir derleme görevleri dosyası Kur nasıl gösterir:

```
XAMMAC_PATH=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full/
DYLD=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib
MONODIR=/Library/Frameworks/Mono.framework/Versions/Current/etc/mono

all: consoleapp.exe

consoelapp.exe: consoleapp.cs Makefile
    mcs -g -r:$(XAMMAC_PATH)/Xamarin.Mac.dll consoleapp.cs
    
run: consoleapp.exe
    MONO_PATH=$(XAMMAC_PATH) DYLD_LIBRARY_PATH=$(DYLD) mono --debug consoleapp.exe $(COMMAND)


bundle: consoleapp.exe
    mkbundle --simple consoleapp.exe -o ncsharp -L $(XAMMAC_PATH) --library $(DYLD)/libxammac.dylib --config $(MONODIR)/config --machine-config $(MONODIR)/4.5/machine.config
```

Yukarıdaki `Makefile` üç hedefleri sağlar:

- `make` program oluşturacaksınız
- `make run` derleme ve geçerli dizinde programı çalıştırın
- `make bundle` kendi içinde bir yürütülebilir dosya oluşturur
