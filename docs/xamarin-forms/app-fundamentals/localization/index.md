---
title: Xamarin.Forms yerelleştirme
description: Yerleşik .NET yerelleştirme framework Xamarin.Forms ile platformlar arası çok dilli uygulamaları oluşturmak için kullanılabilir.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/13/2018
ms.openlocfilehash: 29624eeccc8542b3296774f6b77480b664bee478
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="localization"></a>Yerelleştirme

_Yerleşik .NET yerelleştirme framework Xamarin.Forms ile platformlar arası çok dilli uygulamaları oluşturmak için kullanılabilir._

## <a name="string-and-image-localizationtextmd"></a>[Dize ve görüntü yerelleştirme](text.md)

.NET uygulamaları kullanan yerelleştirme için yerleşik mekanizması [RESX dosyaları](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) ve sınıflarda `System.Resources` ve `System.Globalization` ad alanları. Çevrilen dizelerin bulunduğu RESX dosyaları Xamarin.Forms derlemesindeki çevirileri kesin türü belirtilmiş erişim sağlayan derleyici tarafından üretilen bir sınıf birlikte katıştırılır. Çevrilmiş metni ardından kodda alınabilir.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Sağdan sola yerelleştirme](right-to-left.md)

Akış yönü kullanıcı Arabirimi öğeleri sayfada göz tarafından taranır yönüdür. Sağdan sola yerelleştirme Xamarin.Forms uygulamalarına sağdan sola akış yönü için destek ekler.
