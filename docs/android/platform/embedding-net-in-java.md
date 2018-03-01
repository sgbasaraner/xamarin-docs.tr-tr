---
title: ".NET Java katıştırma"
description: "Java tabanlı yerel Android projesinde Xamarin .NET kitaplığı kullanma"
ms.topic: article
ms.prod: xamarin
ms.assetid: A489EEF3-1008-4257-BF63-FE21D8C23821
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 1a25f4bc39e39ce58a07ed399082bf13284c16e9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="embedding-net-in-java"></a>.NET Java katıştırma

Bazı durumlarda, bir Xamarin .NET kitaplığı varolan yerel Android projesine eklemek isteyebilirsiniz. Bunu yapmak için kullanabileceğiniz [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/) bir yerel Java tabanlı Android uygulamaya birleştirilebilir yerel bir kitaplığı .NET kitaplığı dönüştürmek için aracı.

 
## <a name="requirements"></a>Gereksinimler

Android üzerinde Java ile Embeddinator 4000 kullanmak için aşağıdakiler gerekir:

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/preview/index.html) veya sonrası yüklü olmalıdır.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.4.99](https://jenkins.mono-project.com/view/Xamarin.Android/job/xamarin-android/lastSuccessfulBuild/Azure/) or later must be installed.

-   **Java Geliştirme Seti** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya sonrası yüklü olmalıdır.

-   **Mono** &ndash; [Mono 5.0](http://www.mono-project.com/download/) veya sonrası yüklü olmalıdır.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

C# kodunuzu derleyin ve düzenlemek için Visual Studio'yu kullanabilirsiniz.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio, düzenlemek ve C# kodu derlemek için kullanabilirsiniz.

-----

 
## <a name="using-the-embeddinator-4000"></a>Embeddinator 4000 kullanma

Yerel bir Android projesi .NET kitaplıkta kullanmak için aşağıdaki adımları kullanın:

1.  Bir C# Android kitaplığı projesi oluşturun.

2.  NuGet aracılığıyla Embeddinator 4000 yükleyin.

3.  Embeddinator Android Kitaplık derlemesi üzerinde çalıştırın.

4.  Android Studio'da bir Java projesi oluşturulan AAR dosyasında kullanın.

Bu adımları ayrıntılı olarak açıklanmıştır [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/getting-started-java-android.html) belgeleri.
