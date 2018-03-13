---
title: "Daha Akıllı Xamarin Android v4 destek / v13 NuGet paketleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: ab8d90ebd938a8417e5a48155347e03191ef9ca4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>Daha Akıllı Xamarin Android v4 destek / v13 NuGet paketleri

## <a name="about-the-android-support-libraries"></a>Android desteği kitaplıkları hakkında

Google yeni özellikler Android eski sürümleri kullanılabilir hale getirmek için destek kitaplıkları oluşturdu. Genel olarak, destek kitaplıkları bir sürüm numarası en düşük Android API ile uyumlu olduklarından düzeyi kullanıcıların adı verilmiştir (örn: v4 destek, yalnızca API düzey 4 ve üzeri kullanılabilir. Daha fazla bilgi bu [yığın taşması tartışma](http://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

İki destek kitaplıkları: `Support-v4` ve `Support-v13` kullanılamaz birlikte aynı uygulamada, diğer bir deyişle, bunlar karşılıklı olarak birbirini dışlar. Bunun nedeni, `Support-v13` gerçekten tüm türleri ve uyarlamasını içeren `Support-v4`. Çalışırsanız ve her ikisi de aynı projede başvuru yinelenen türü hatalarla karşılaşır.

## <a name="problems-with-referencing"></a>Başvuran sorunları

Bu yana `Support-v4` 3 taraf kitaplıkların çok şimdi bağımlı üzerinde böylece popüler hale gelmiştir. Bunlar üzerinde destek v13 yerine bağımlı seçtiniz, ancak daha fazla işlem bağımlı için ortak _v4_ , bu 3 taraf kitaplıklar kullanılarak uygulamalardan API düzeylerini 4 kadar destekleme seçeneğiniz sağlandığından.

Xamarin 3. taraf kitaplık başvuruyorsa `Xamarin.Android.Support.v4.dll` bağlama `Support-v4`, bu kitaplığı kullanan tüm uygulamalarda da başvurmalıdır `Xamarin.Android.Support.v4.dll`. Aynı uygulama aynı zamanda bazı işlevlerini kullanmak istediğinde bu bir sorun haline `Xamarin.Android.Support.v13.dll` bağlama `Support-v13`. Her iki bağlamaları başvurursanız, yinelenen türü hatalarla karşılaşır.

## <a name="type-forwarded-v4-binding-assembly"></a>Türü iletilen v4 bağlama derleme

Bu sorunla karşılaşmamak için özel bir oluşturduk `Xamarin.Android.Support.v4.dll` uygulaması, ancak yalnızca sahip derleme `[assembly: TypeForwardedTo (..)]` tüm iletmek öznitelikler `Support-v4` içinde uygulama türlerine `Xamarin.Android.Support.v13.dll` derleme.

Bir geliştirici bu başvuru bu anlamına gelir _türü iletilen_ referansı yerine getirecek, uygulama derlemede `Xamarin.Android.Support.v4.dll` hala verirken herhangi 3 taraf kitaplıklar tarafından `Xamarin.Android.Support.v13.dll` uygulamada kullanılacak.

## <a name="nuget-assistance"></a>NuGet Yardım

Bir geliştirici doğru başvuruları gerekli el ile ekleyebilirsiniz, ancak biz sağ derleme seçmenize yardımcı olması için NuGet kullanabilirsiniz (ya da normal _v4_ bağlama veya türü iletilen _v4_ derleme) olduğunda NuGet paketi yüklendi.

Bu nedenle, `Xamarin.Android.Support.v4` NuGet paketi şimdi aşağıdaki mantık içerir:

Uygulamanızı bir API düzeyi 13 (Zencefilli kurabiye 3.2) hedeflediği veya üstü:

*   `Xamarin.Android.Support.v13` NuGet bağımlılık olarak otomatik olarak eklenir
*   _Türü iletilen_ `Xamarin.Android.Support.v4.dll` projede başvurulacak

Uygulamanızı API düzeyi 13 düşük bir şey hedefliyorsa, normal alırsınız `Xamarin.Android.Support.v4.dll` projenizde başvurulan bağlama.

## <a name="do-i-have-to-use-support-v13"></a>Destek v13 kullanmak zorunda mıyım?

Uygulamanızı 13 veya üzeri API düzeyinde hedefleme ve kullanmayı seçerseniz `Xamarin Android Support-v4` NuGet paketi, sonra `Xamarin Android Support v13` NuGet paketi olduğundan gerekli bir bağımlılık.

Uyumluluk ve onu sonuçlanır daha az zorlukların değerinde (iki .jar dosyalar tarafından 17 kb farklı) uygulama boyutu çok küçük artış olduğunu düşündüğünüz.

Kullanma hakkında adamant varsa `Support-v4` API düzeyi 13 hedefleyen bir uygulamada veya üzeri, her zaman el ile yükleyebilirsiniz `.nupkg`ayıklayın ve derleme başvurusu.
