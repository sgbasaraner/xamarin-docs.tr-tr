---
title: Genişletilebilir uygulama biçimlendirme dili (XAML)
description: XAML kullanıcı arabirimlerini tanımlamak için kullanılan bir bildirim temelli bir biçimlendirme dilidir. Kullanıcı arabirimi, çalışma zamanı davranışı ayrı arka plan kod dosyasında tanımlanır sırasında XAML söz dizimi kullanılarak bir XML dosyasında tanımlanır.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/18/2018
ms.openlocfilehash: f593e5d084d8cd7071d17195663478d430d994b7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995488"
---
# <a name="extensible-application-markup-language-xaml"></a>Genişletilebilir uygulama biçimlendirme dili (XAML)

_XAML kullanıcı arabirimlerini tanımlamak için kullanılan bir bildirim temelli bir biçimlendirme dilidir. Kullanıcı arabirimi, çalışma zamanı davranışı ayrı arka plan kod dosyasında tanımlanır sırasında XAML söz dizimi kullanılarak bir XML dosyasında tanımlanır._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**2016 evrim Geçiren: XAML asıl olma**

> [!NOTE]
> Denemenin [XAML Standard Önizleme](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[HTML Temelleri](xaml-basics/index.md)

XAML, geliştiricilerin kod yerine biçimlendirme kullanarak Xamarin.Forms uygulamalarında kullanıcı arabirimlerini tanımlamanızı sağlar. XAML, bir Xamarin.Forms programda hiç gereklidir ancak oluşturulabildiğinden ve genellikle büyük görsel olarak tutarlı ve eşdeğer kod daha Sözün. XAML popüler Model-View-ViewModel (MVVM) uygulama mimarisi ile kullanım için uygun özellikle: XAML ViewModel kodla XAML tabanlı veri bağlamaları ile bağlantılı görünüm tanımlar.

## <a name="xaml-compilationxamlcmd"></a>[XAML Derlemesi](xamlc.md)

XAML, Ara dil (IL) XAML derleyicisi (XAMLC) ile doğrudan isteğe bağlı olarak derlenebilir. Bu makaleler XAMLC ve aboneliğin avantajları nasıl kullanılacağını açıklar.

## <a name="xaml-previewerxaml-previewermd"></a>[XAML Previewer](xaml-previewer.md)

[XAML Önizleyiciyi](~/xamarin-forms/xaml/xaml-previewer.md) sırasında duyurulan Xamarin evrim Geçiren 2016 alfa kanalı test etmek için kullanılabilir.

## <a name="xaml-namespacesnamespacesmd"></a>[XAML Ad Alanları](namespaces.md)

XAML kullanan `xmlns` XML özniteliği için ad alanı bildirimi. Bu makalede, XAML ad alanı sözdizimi tanıtır ve bir tür erişmek için XAML ad alanı bildirmek gösterilmektedir.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[XAML Biçimlendirme Uzantıları](markup-extensions/index.md)

XAML biçimlendirme uzantıları değerler veya nesneler ile basit dizeler belirtilebilir ötesinde öznitelikleri ayarlamaya yönelik içerir. Bunlar, sabitleri, statik özellikler ve alanları, kaynak sözlükleri ve veri bağlamalarını başvuru içerir.

## <a name="field-modifiersfield-modifiersmd"></a>[Alan Değiştiricileri](field-modifiers.md)

`x:FieldModifier` Ad alanı özniteliği, oluşturulmuş alanlar adlandırılmış XAML öğeleri için erişim düzeyini belirtir.

## <a name="passing-argumentspassing-argumentsmd"></a>[Bağımsız Değişkenleri Geçirme](passing-arguments.md)

XAML, varsayılan olmayan bir oluşturucu veya Fabrika yöntemleri bağımsız değişkenleri geçirmek için kullanılabilir. Bu makalede, Oluşturucular, Fabrika yöntemleri çağırmak için ve genel bir bağımsız değişken türünü belirtmek için bağımsız değişkenleri geçirmek için kullanılan XAML öznitelikleri kullanmayı gösterir.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[Bağlanabilir Özellikler](bindable-properties.md)

Xamarin.Forms içinde bağlanabilir özellikler tarafından ortak dil çalışma zamanı (CLR) özellikleri işlevselliği genişletilir. Bağlanılabilir özellik bir özel özellik, özelliğin değerini Xamarin.Forms özellik sistemi tarafından izlenen burada türüdür. Bu makalede bağlanabilir özellikler tanıtır ve oluşturmak ve bunları kullanma işlemi gösterilmektedir.

## <a name="attached-propertiesattached-propertiesmd"></a>[Ekli Özellikler](attached-properties.md)

Ekli özelliği bir özel bir sınıf tarafından tanımlanan, ancak diğer nesnelere bağlı bağlanılabilir özellik türüdür, tanınan bir öznitelik olarak XAML içinde bir sınıf içeren ve bir özellik adı noktayla ayrılmış. Bu makalede ekli özelliklerini tanıtır ve oluşturmak ve bunları kullanma işlemi gösterilmektedir.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[Kaynak Sözlükler](resource-dictionaries.md)

XAML, birden çok kez kullanılabilir nesnelerin tanımlarını kaynaklardır. A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) tek bir konumda tanımlanmış ve bir Xamarin.Forms uygulamanın tamamında yeniden kullanılan kaynaklar sağlar. Bu makale oluşturma ve tüketme yapmayı gösteren bir `ResourceDictionary`ve bir birleştirme nasıl `ResourceDictionary` diğeriyle birleştirmek.
