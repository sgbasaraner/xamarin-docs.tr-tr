---
title: "PathTooLongException hata nasıl giderebilirim?"
description: "Bu makalede, bir uygulama oluştururken oluşabilir PathTooLongException gidermek açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/20/2018
ms.openlocfilehash: f4554178648d1257049d1c21cdc3e141acdb3de7
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2018
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>PathTooLongException hata nasıl giderebilirim?

## <a name="cause"></a>Sebep

Xamarin.Android projesinde oluşturulan yol adları çok uzun olabilir.
Örneğin, aşağıdaki gibi bir yol, bir derleme sırasında oluşturulabilir:

**C:\\bazı\\Directory\\çözüm\\proje\\obj\\hata ayıklama\\__library_projects__ \\ Xamarin.Forms.Platform.Android\\library_project_imports\\varlıklar**

Windows (bir yol için en fazla uzunluk olduğu [260 karakter](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), bir **PathTooLongException** oluşturulan bir yol uzunluk üst sınırını aşarsa proje oluşturulurken üretilen. 

## <a name="fix"></a>Düzeltme

Xamarin.Android 8.0 ile başlayan `UseShortFileNames` MSBuild özelliği, bu hata aşmak için ayarlanabilir. Bu özellik ayarlandığında `True` (varsayılan `False`), oluşturan olasılığını azaltmak için daha kısa yol adları oluşturma işlemi kullanan bir **PathTooLongException**.
Örneğin, `UseShortFileNames` ayarlanır `True`, yukarıdaki yol şuna benzer bir yola kısaltılır:

**C:\\bazı\\Directory\\çözüm\\proje\\obj\\hata ayıklama\\lp\\1\\jl\\varlıklar**

Bu özelliği ayarlamak için aşağıdaki MSBuild özelliği projeye ekleyin **.csproj** dosyası:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Yapı özelliklerini ayarlama hakkında daha fazla bilgi için bkz: [derleme işlemi](~/android/deploy-test/building-apps/build-process.md).
