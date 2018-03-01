---
title: "Neden my Android yayın derlemesi Internet'e bağlanamıyor musunuz?"
ms.topic: article
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 60da469001f8a9641aff7e014add5f58bdb389cb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Neden my Android yayın derlemesi Internet'e bağlanamıyor musunuz?

## <a name="cause"></a>Sebep

Bu sorunun en yaygın nedeni olan **Internet** izni olan bir hata ayıklama derlemesi otomatik olarak eklenir, ancak yayın derlemesi için el ile ayarlamanız gerekir. Internet izin işlemine eklemek üzere bir hata ayıklayıcısı "DebugSymbols için" açıklandığı şekilde izin vermek için kullanıldığından budur [burada](~/android/deploy-test/building-apps/build-process.md).


## <a name="fix"></a>Düzeltme

Sorunu çözmek için Android derleme bildirimi Internet izin gerektirebilir. Bu bildirim Düzenleyicisi'ni veya bildirim sourcecode yoluyla yapılabilir:

-   Düzenleyicide düzeltin: Android projenize Git **Özellikler -> AndroidManifest.xml gerekli izinler ->** ve denetleyin **Internet**

-   İçinde Sourcecode düzeltin: AndroidManifest kaynak düzenleyicisinde açın ve izni etiketinin içine ekleyin `<Manifest>` etiketler:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
