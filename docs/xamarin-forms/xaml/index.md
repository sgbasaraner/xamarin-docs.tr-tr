---
title: "Genişletilebilir uygulama biçimlendirme dili (XAML)"
description: "XAML kullanıcı arabirimleri tanımlamak için kullanılan bir tanımlayıcı biçimlendirme dilidir. Kullanıcı arabirimi, çalışma zamanı davranışı ayrı arka plan kodu dosyasında tanımlanan XAML sözdizimi kullanırken bir XML dosyasında tanımlanır."
ms.topic: article
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/24/2016
ms.openlocfilehash: a6e31fa9da7a5764d9a7fd04aa73d7d246143384
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="extensible-application-markup-language-xaml"></a>Genişletilebilir uygulama biçimlendirme dili (XAML)

_XAML kullanıcı arabirimleri tanımlamak için kullanılan bir tanımlayıcı biçimlendirme dilidir. Kullanıcı arabirimi, çalışma zamanı davranışı ayrı arka plan kodu dosyasında tanımlanan XAML sözdizimi kullanırken bir XML dosyasında tanımlanır._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**2016 gelişmesi: XAML asıl olma**

> [!NOTE]
> Denemenin [XAML Standard Önizleme](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML temelleri](xaml-basics/index.md)

XAML geliştiricilerin kodu yerine biçimlendirme Xamarin.Forms uygulamalarda kullanıcı arabirimleri tanımlamanızı sağlar. XAML hiçbir zaman bir Xamarin.Forms programında gerekli ancak oluşturulabildiğinden ve genellikle daha büyük görsel olarak tutarlı ve eşdeğer kodu daha kısa. XAML popüler Model View ViewModel (MVVM) uygulama mimarisi ile kullanım için uygun özellikle: XAML XAML tabanlı veri bağlamaları ViewModel koduna bağlı Görünüm tanımlar.

## <a name="xaml-compilationxamlcmd"></a>[XAML derleme](xamlc.md)

XAML isteğe bağlı olarak ara dile (IL) XAML derleyici (XAMLC) ile doğrudan derlenebilir. Bu makaleler XAMLC ve onun avantajlarını nasıl kullanılacağını açıklar.

## <a name="xaml-previewerxaml-previewermd"></a>[XAML Previewer](xaml-previewer.md)

[XAML Önizleyicisi](~/xamarin-forms/xaml/xaml-previewer.md) adresindeki duyurdu Xamarin gelişmesi 2016 alfa kanal test etmek için kullanılabilir.

## <a name="xaml-namespacesnamespacesmd"></a>[XAML Ad Alanları](namespaces.md)

XAML kullanan `xmlns` ad alanı bildirimleri için XML özniteliği. Bu makalede XAML ad uzayı söz dizimi tanıtır ve bir tür erişmek için XAML ad uzayı bildirmek gösterilmiştir.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[XAML işaretleme uzantıları](markup-extensions/index.md)

XAML biçimlendirme uzantıları değerleri veya basit dizelerle ifade edilebilir ötesinde nesnelere öznitelikleri ayarlamak için içerir. Bunlar, sabitleri, statik özellikleri ve alanları, kaynak sözlüklerindeki ve veri bağlamaları başvuran içerir.

## <a name="passing-argumentspassing-argumentsmd"></a>[Bağımsız değişkenleri geçirme](passing-arguments.md)

XAML, varsayılan olmayan oluşturucular veya Fabrika yöntemleri bağımsız değişkenler geçirmek için kullanılabilir. Bu makalede, Fabrika yöntemlerini çağırmaya ve genel bir bağımsız değişken türünü belirtmek için oluşturucular için bağımsız değişkenler geçirmek için kullanılan XAML öznitelikleri kullanma gösterilmektedir.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[Bağlanabilir özellikleri](bindable-properties.md)

Xamarin.Forms içinde ortak dil çalışma zamanı (CLR) özellikleri işlevselliğini bağlanabilir özellikleri tarafından genişletilir. Bir bağlanabilirse özel türde bir özellik, özelliğin değerini Xamarin.Forms özellik sistemi tarafından nerede izlenen özelliğidir. Bu makalede bağlanabilirse özelliklerine bir giriş sağlar ve oluşturmak ve bunları kullanmak gösterilmiştir.

## <a name="attached-propertiesattached-propertiesmd"></a>[Ekli özellikler](attached-properties.md)

Ekli özellik bir sınıf tarafından tanımlanan diğer nesnelere bağlı bağlanabilirse özelliği, ancak, özel bir tür ve bir sınıfı içeren bir öznitelik olarak XAML'de tanınabilir ve bir noktayla bir özellik adı ayrılmış. Bu makalede, ekli özellikler bir giriş sağlar ve oluşturmak ve bunları kullanmak gösterilmiştir.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[Kaynak sözlükleri](resource-dictionaries.md)

XAML, birden çok kez kullanılabilir nesne tanımları kaynaklardır. A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) tek bir konumda tanımlanmış ve bir Xamarin.Forms uygulaması yeniden kullanılabilir kaynaklar sağlar. Bu makalede oluşturmak ve kullanmak nasıl gösteren bir `ResourceDictionary`ve bir birleştirme nasıl `ResourceDictionary` başka içine.
