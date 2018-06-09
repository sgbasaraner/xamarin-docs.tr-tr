---
title: Xamarin.Forms yerelleştirme
description: Yerleşik .NET yerelleştirme framework Xamarin.Forms ile platformlar arası çok dilli uygulamaları oluşturmak için kullanılabilir. Metin ve görüntüler yerelleştirilmiş ve bir sağdan sola akış yönü uygulamaları destekleyebilir.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/13/2018
ms.openlocfilehash: 78731924324a1ddd34c0d197070699e2998c1513
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240599"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms yerelleştirme

_Yerleşik .NET yerelleştirme framework Xamarin.Forms ile platformlar arası çok dilli uygulamaları oluşturmak için kullanılabilir._

## <a name="string-and-image-localizationtextmd"></a>[Dize ve Görüntü Yerelleştirme](text.md)

.NET uygulamaları kullanan yerelleştirme için yerleşik mekanizması [RESX dosyaları](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) ve sınıflarda `System.Resources` ve `System.Globalization` ad alanları. Çevrilen dizelerin bulunduğu RESX dosyaları Xamarin.Forms derlemesindeki çevirileri kesin türü belirtilmiş erişim sağlayan derleyici tarafından üretilen bir sınıf birlikte katıştırılır. Çevrilmiş metni ardından kodda alınabilir.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Sağdan Sola Yerelleştirme](right-to-left.md)

Akış yönü kullanıcı Arabirimi öğeleri sayfada göz tarafından taranır yönüdür. Sağdan sola yerelleştirme Xamarin.Forms uygulamalarına sağdan sola akış yönü için destek ekler.
