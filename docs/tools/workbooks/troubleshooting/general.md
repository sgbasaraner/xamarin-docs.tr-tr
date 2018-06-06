---
title: Bilinen sorunlar ve geçici çözümler
description: Bu belgede, Xamarin çalışma kitapları için bilinen sorunlar ve geçici çözümler açıklanmaktadır. CultureInfo sorunları, JSON sorunları ve daha fazlasını açıklanır.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: topgenorth
ms.author: toopge
ms.openlocfilehash: b6dc3b119d3e85369a71638f2519b2ef0c85446c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794039"
---
# <a name="known-issues--workarounds"></a>Bilinen sorunlar ve geçici çözümler

## <a name="persistence-of-cultureinfo-across-cells"></a>CultureInfo Kalıcılık hücreler boyunca

Ayarı `System.Threading.CurrentThread.CurrentCulture` veya `System.Globalization.CultureInfo.CurrentCulture` çalışma kitabı hücreler Mono tabanlı çalışma kitaplarını hedeflerde (Mac, iOS ve Android) boyunca nedeniyle devam etmez bir [Mono'nın hatada `AppContext.SetSwitch` ] [ appcontext-bug] uygulama .

### <a name="workarounds"></a>Geçici Çözümler

* Uygulama etki alanı yerel ayarla `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* Veya, çalışma kitaplarına 1.2.1 güncelleştirme ya da daha yeni olan atamaları yeniden `System.Threading.CurrentThread.CurrentCulture` ve `System.Globalization.CultureInfo.CurrentCulture` (Mono hata çalışma) istenen davranışı sağlamak için.

## <a name="unable-to-use-newtonsoftjson"></a>Newtonsoft.Json kullanılamıyor

### <a name="workaround"></a>Geçici Çözüm

* Hangi Newtonsoft.Json 9.0.1 yükleyecek çalışma kitaplarına 1.2.1, güncelleştirin.
  Çalışma kitapları 1.3, şu anda alfa kanal içinde 10 ve daha yeni sürümleri destekler.

### <a name="details"></a>Ayrıntılar

Newtonsoft.Json 10, sürüm çalışma kitapları ile çakışan gelir desteklemek için Microsoft.CSharp bağımlı indirgenmesine yayımlanmış `dynamic`. Bu çalışma kitaplarını 1.3 Önizleme sürümünde ele, ancak şu an için bu sorunu özellikle sürümüne 9.0.1 sabitleme Newtonsoft.Json tarafından çalıştık.

NuGet paketlerini açıkça Newtonsoft.Json 10 bağlı olarak veya yalnızca çalışma kitaplarını 1.3 içinde şu anda alfa kanal desteklenir.

## <a name="code-tooltips-are-blank"></a>Kod araç ipuçları şunlardır: boş

Var olan bir [Monaco düzenleyicisine hatada] [ monaco-bug] Safari/Mac çalışma kitaplarını uygulamada kullanılan WebKit içinde sonuçları metin olmadan kod araç ipuçları oluşturma.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Geçici Çözüm

* Araç İpucu üzerinde göründükten sonra tıklatarak işlemek için metin zorlar.

* Veya güncelleştirme çalışma kitaplarına 1.2.1 ya da daha yeni

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>Çalışma kitapları 1.3 SkiaSharp oluşturucu yok

Çalışma kitapları 1.3 başlayarak, biz sevk SkiaSharp Oluşturucu Oluşturucu kendisini sağlamak, kullanarak SkiaSharp lehinde 0.99.0, çalışma kitaplarındaki kaldırdık bizim [SDK](~/tools/workbooks/sdk/index.md).

### <a name="workaround"></a>Geçici Çözüm

* SkiaSharp NuGet en son sürüme güncelleştirin. Yazma zaman 1.57.1 budur.

## <a name="related-links"></a>İlgili bağlantılar

- [Raporlama hataları](~/tools/workbooks/install.md#reporting-bugs)
