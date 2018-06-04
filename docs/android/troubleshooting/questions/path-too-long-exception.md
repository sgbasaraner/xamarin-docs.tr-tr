---
title: PathTooLongException hata nasıl giderebilirim?
description: Bu makalede, bir uygulama oluştururken oluşabilir PathTooLongException gidermek açıklanmaktadır.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/29/2018
ms.openlocfilehash: 8303e71d516fcec8d1136bd99adf2eb0797a9a40
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34562792"
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

Bu bayrak ayarlıyorsanız değil düzeltme **PathTooLongException** hatası, başka bir yaklaşım olduğunu belirtmek için bir [ortak Ara çıkış kök](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/) ayarlayarak çözümünüzdeki projeleri için `IntermediateOutputPath` içinde Proje **.csproj** dosya. Görece kısa bir yol kullanmayı deneyin. Örneğin:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

Yapı özelliklerini ayarlama hakkında daha fazla bilgi için bkz: [derleme işlemi](~/android/deploy-test/building-apps/build-process.md).
