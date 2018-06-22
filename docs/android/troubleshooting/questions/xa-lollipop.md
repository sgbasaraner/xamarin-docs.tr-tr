---
title: Xamarin.Android hangi sürümünün Lolipop desteği eklendi mi?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 065c68a373f67bb352b59dc88ef89daec8b51ef8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764475"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>Xamarin.Android hangi sürümünün Lolipop desteği eklendi mi?

**Not:** bu kılavuzda başlangıçta Android M Önizleme için yazılmıştır.

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) Android M Önizleme desteği eklendi.
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) Android Lolipop desteği eklendi.

Xamarin yalnızca etkin olarak Xamarin araçlarının geçerli kararlı sürümünü destekler. Aşağıdaki bilgiler sağlanır "olarak-olan" araçlarının eski sürümleri için. Xamarin sürümlerinde en son bilgiler için lütfen denetleyin [burada](http://releases.xamarin.com/).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>"Android.jar API düzeyi 21 Android M önizlemede eksik"

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

(Veya benzeri) aşağıdaki hata iletisini görünebilir:

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Bu ileti Android SDK platformu için API düzey 21 yüklü olmadığını anlamına gelir. Android SDK Yöneticisi'nde yüklemek ya da (Araçlar > Open Android SDK Manager...), ya da yüklü bir API sürümü hedeflemek için Xamarin.Android projenizi değiştirin.

Bu sorun için birkaç geçici çözümler vardır:

1. Projenizi API 19 hedefler şekilde değiştirin veya azaltın.

2. Android-21, android 21 klasörünüze android L. yeniden adlandırma (En iyi bu geçici bir düzeltme olarak kullanılmalıdır ve çok iyi hiç çalışmayabilir.)

   **% LOCALAPPDATA %\\Android\\android sdk\\platformları\\android 21**

3. Android API düzeyi 21 "M" Önizleme geri [1] geçici olarak düşürmek:

    1.  Silme **LOCALAPPDATA %\\Android\\android sdk\\platformları\\android 21** 
    2.  [1] uygulamasına ayıklamak **C:\\kullanıcılar\\<username>\\AppData\\yerel\\Android\\android sdk\\platformları** oluşturmak için bir **android m** klasör.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

(Veya benzeri) aşağıdaki hata iletisini görünebilir:

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

Başka bir deyişle Android SDK platformu için API düzey 21 yüklü değil. Android SDK Yöneticisi'nde yüklemek ya da (Araçlar > SDK Yöneticisi...), ya da yüklü bir API sürümü hedeflemek için Xamarin.Android projenizi değiştirin.

Bu sorun için birkaç geçici çözümler vardır:

1. Projenizi API 19 hedefler şekilde değiştirin veya azaltın.

2. Android-21, android 21 klasörünüze android L. yeniden adlandırma (En iyi bu geçici bir düzeltme olarak kullanılmalıdır ve çok iyi hiç çalışmayabilir.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Android API düzeyi 21 "M" Önizleme geri [1] geçici olarak düşürmek:

    1.  Silme **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  [1] uygulamasına ayıklamak **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** oluşturmak için bir **android m** klasör.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
