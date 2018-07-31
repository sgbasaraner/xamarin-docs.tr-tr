---
title: Bilinen sorunlar ve geçici çözümler
description: Bu belgede, Xamarin çalışma kitapları için bilinen sorunlar ve geçici çözümler açıklanmıştır. CultureInfo sorunları, JSON sorunları ve diğer ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: d362698d2844ae6d96bba4929d509f5373742578
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351001"
---
# <a name="known-issues--workarounds"></a>Bilinen sorunlar ve geçici çözümler

## <a name="persistence-of-cultureinfo-across-cells"></a>Hücre arasında CultureInfo kalıcılığı

Ayarı `System.Threading.CurrentThread.CurrentCulture` veya `System.Globalization.CultureInfo.CurrentCulture` çalışma kitabında hücreleri Mono tabanlı çalışma kitapları hedeflerde (Mac, iOS ve Android) arasında nedeniyle kalıcı olmayacak bir [Mono'nın hatada `AppContext.SetSwitch` ] [ appcontext-bug] uygulama .

### <a name="workarounds"></a>Geçici Çözümler

* Uygulama etki alanı yerel ayarla `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* Veya, çalışma kitaplarına 1.2.1 güncelleştirme ya da daha yeni olan atamaları yeniden `System.Threading.CurrentThread.CurrentCulture` ve `System.Globalization.CultureInfo.CurrentCulture` (Mono hataya çalışma) istenen davranışı sağlamak için.

## <a name="unable-to-use-newtonsoftjson"></a>Newtonsoft.Json kullanılamıyor

### <a name="workaround"></a>Geçici Çözüm

* Newtonsoft.Json 9.0.1 yükleyecek olan çalışma kitaplarını 1.2.1, güncelleştirin.
  Alfa kanalı şu anda çalışma kitapları 1.3, 10 ve daha yeni sürümleri destekler.

### <a name="details"></a>Ayrıntılar

Newtonsoft.Json 10 bırakıldığını, çakışan sürüm çalışma kitapları ile birlikte gelen desteklemek için Microsoft.CSharp bağımlı indirgenmesine `dynamic`. Bu çalışma kitaplarını 1.3 Önizleme sürümünde giderilen, ancak şimdilik bu sorunu tarafından özellikle sürümüne 9.0.1 sabitleme Newtonsoft.Json çalıştık.

NuGet paketlerini açıkça Newtonsoft.Json 10 bağlı olarak veya yalnızca çalışma kitapları 1.3 şu anda alfa kanalına desteklenir.

## <a name="code-tooltips-are-blank"></a>Kod araç ipuçları şunlardır: boş

Var olan bir [Monaco düzenleyicisine hatada] [ monaco-bug] Safari/Mac çalışma kitapları uygulamada kullanılan WebKit içinde sonuçları içinde metin olmayan kod araç ipuçları oluşturma.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Geçici Çözüm

* Araç ipucu görünür sonra tıklayarak metin işlemek için zorlar.

* Veya güncelleştirme çalışma kitaplarına 1.2.1 veya daha yeni

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>Çalışma kitapları 1.3 içinde SkiaSharp oluşturucu yok

İçinde çalışma kitapları 1.3 başlayarak, biz sevk SkiaSharp oluşturucular çalışma kitaplarında 0.99.0, oluşturucu kendisini sağlanması, kullanarak SkiaSharp yerine kaldırdık bizim [SDK](~/tools/workbooks/sdk/index.md).

### <a name="workaround"></a>Geçici Çözüm

* SkiaSharp en son sürüm NuGet ile güncelleştirin. Makalenin yazıldığı sırada 1.57.1 budur.

## <a name="related-links"></a>İlgili bağlantılar

- [Hata Raporlama](~/tools/workbooks/install.md#reporting-bugs)
