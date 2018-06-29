---
title: .NET Android katıştırma
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: topgenorth
ms.author: toopge
ms.date: 06/15/2018
ms.openlocfilehash: e90d1e6258d4cfd9c918c566c9e18c358ee7668a
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067199"
---
# <a name="net-embedding-on-android"></a>.NET Android katıştırma

Bazı durumlarda, bir Xamarin .NET kitaplığı varolan yerel Android projesine eklemek isteyebilirsiniz. Bunu yapmak için kullanabileceğiniz [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/) bir yerel Java tabanlı Android uygulamaya birleştirilebilir yerel bir kitaplığı .NET kitaplığı dönüştürmek için aracı.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android gereksinimleri

.NET katıştırma ile çalışmak Xamarin.Android için aşağıdakiler gerekir:

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) veya sonrası yüklü olmalıdır.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) veya sonrası yüklü olmalıdır.

-   **Java Geliştirme Seti** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya sonrası yüklü olmalıdır.


## <a name="using-embeddinator-4000"></a>Embeddinator 4000 kullanma

Yerel bir Android projesi .NET kitaplıkta kullanmak için aşağıdaki adımları kullanın:

1.  Bir C# Android kitaplığı projesi oluşturun.

2.  Yükleme [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Bulun **Embeddinator 4000.exe** ve ekleyin, **yolu**. Örneğin:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  Embeddinator 4000 Kitaplığı derleme üzerinde çalıştırın. Örneğin:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Android Studio'da bir Java projesi oluşturulan AAR dosyasında kullanın.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android gereksinimleri

.NET katıştırma ile çalışmak Xamarin.Android için aşağıdakiler gerekir:

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) veya sonrası yüklü olmalıdır.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) veya sonrası yüklü olmalıdır.

-   **Java Geliştirme Seti** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya sonrası yüklü olmalıdır.

-   **Mono** &ndash; [Mono 5.0](http://www.mono-project.com/download/) veya sonrası yüklü olmalıdır (mono yüklü Visual Studio ile Mac için).


## <a name="using-embeddinator-4000"></a>Embeddinator 4000 kullanma

Yerel bir Android projesi .NET kitaplıkta kullanmak için aşağıdaki adımları kullanın:

1.  Bir C# Android kitaplığı projesi oluşturun.

2.  Yükleme [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Bulun **Embeddinator 4000.exe** ve ekleme **mono** yolunuz için. Örneğin:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  Embeddinator 4000 Kitaplığı derleme üzerinde çalıştırın. Örneğin:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Android Studio'da bir Java projesi oluşturulan AAR dosyasında kullanın.

-----

Kullanımı ve komut satırı seçenekleri açıklanmıştır [Embeddinator 4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) belgeleri.


## <a name="callbacks"></a>Geri aramalar

Hakkında bilgi edinin [C# ve Java arasında çağrıları yapma](callbacks.md).

## <a name="samples"></a>Örnekler

* [Hava durumu örnek uygulaması](https://github.com/jamesmontemagno/embeddinator-weather)
