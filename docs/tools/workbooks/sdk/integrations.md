---
title: "Gelişmiş tümleştirme konuları"
ms.topic: article
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 5e51aa9ab9d4d63d16b3a68d24084c872d831975
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="external-integrations"></a>Dış tümleştirmeler

Tümleştirme derlemeleri başvuruda bulunmalıdır [ `Xamarin.Workbooks.Integrations` NuGet][nuget]. Kullanıma bizim [hızlı başlangıç belgelerine](~/tools/workbooks/sdk/index.md) NuGet paketi ile çalışmaya başlama hakkında daha fazla bilgi için.

İstemci tümleştirmeler de desteklenir ve JavaScript veya CSS dosyaları Aracısı tümleştirme derleme aynı adla aynı dizinde yerleştirme tarafından başlatılır. Örneğin, (NuGet başvuruyor) aracısı tümleştirme bütünleştirilmiş adlı `SampleExternalIntegration.dll`, ardından `SampleExternalIntegration.js` ve `SampleExternalIntegration.css` varsa istemci tümleşik. İstemci tümleştirmeler isteğe bağlıdır.

Dış tümleştirme kullanılabilir NuGet paketlenmiş, sağlanan ve doğrudan aracıyı barındıran ya da yalnızca yanında yerleştirilen uygulama içinde başvurulan bir `.workbook` bunu kullanmak istediği dosya.

Çalışma kitabı sevk tümleştirme derlemeler olarak şekilde başvuruda bulunmanız gerekir sırada paket, hızlı başlangıç belgelerine göredir başvurulduğunda NuGet paketleri, dış tümleştirmeler (aracısı ve istemci) otomatik olarak yüklenir:

```csharp
#r "SampleExternalIntegration.dll"
```

Bir tümleştirme bu şekilde başvururken, istemci tarafından hemen yüklenmeyecek&mdash;entegrasyonunu yüklemek için bazı kod çağırmak gerekir. Biz bu hatanın ileride adresleme.

`Xamarin.Interactive` PCL birkaç önemli tümleştirme API'ler sağlar. Her tümleştirme en az bir tümleştirme giriş noktası sağlamanız gerekir:

```csharp
using Xamarin.Interactive;

[assembly: AgentIntegration (typeof (AgentIntegration))]

class AgentIntegration : IAgentIntegration
{
    const string TAG = nameof (AgentIntegration);

    public void IntegrateWith (IAgent agent)
    {
        // hook into IAgent APIs
    }
}
```

Bu noktada, tümleştirme derlemesi başvurulan sonra istemci örtük JavaScript ve CSS tümleştirme dosyaları yükler.

## <a name="apis"></a>API'ler

Oturum sahip bir çalışma kitabı başvuruda veya dinamik derleme incelemek gibi kendi ortak API'ler hiçbirini oturuma erişilebilir. Bu nedenle keşfetmek kullanıcılar için güvenli ve duyarlı API yüzeyi olması önemlidir.

Tümleştirme etkin bir uygulama veya ilgi SDK ve oturum arasında bir köprü derlemesidir. Oturum, inceleyebilir veya hiçbir ortak API'ler sağlar ve yalnızca "arka planda" nesnesi oluşturan gibi görevleri gerçekleştirebilir özellikle bağlamında çalışma kitabının veya canlı anlamlı yeni API'leri sağlayabilir [Beyanları](~/tools/workbooks/sdk/representations.md).

> [!NOTE]
> Genel olmalı, ancak IntelliSense ortaya çıkması değil API'leri işaretlenir normal ile `[EditorBrowsable (EditorBrowsableState.Never)]` özniteliği.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
