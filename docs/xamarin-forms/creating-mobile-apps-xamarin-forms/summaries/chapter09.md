---
title: "Bölüm 9 özeti. Platforma özgü API çağrıları"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 637096d3ebb7fb90321f7f459e0ca9e51572d935
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Bölüm 9 özeti. Platforma özgü API çağrıları

Bazen, platforma göre değişir bazı kodlar çalıştırmak gereklidir. Bu bölümde teknikleri inceler.

## <a name="preprocessing-in-the-shared-asset-project"></a>Paylaşılan varlık projesinde ön işleme

Xamarin.Forms paylaşılan varlık proje C# önişlemci yönergeleri kullanarak her platform için farklı bir kod yürütebilir `#if`, `#elif`, ve `endif`. Bu, gösterilmiştir [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Üçlü ekran değişkenin biçimlendirilmiş paragraf](images/ch09fg01-small.png "cihaz modeli ve işletim sistemi")](images/ch09fg01-large.png "cihaz modeli ve işletim sistemi")

Ancak, sonuç kodu çirkin ve okunması zor olabilir.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Paylaşılan varlık projesinde paralel sınıfları

SAP platforma özgü kod yürütmek için daha fazla yapılandırılmış bir yaklaşım örneklerde gösterildiği [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) örnek. Platformu projelerin her biri aynı adlı bir sınıf ve yöntemleri içerir, ancak, belirli bir platform için uygulanır. SAP sonra sadece sınıfı başlatır ve yöntemini çağırır.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService ve taşınabilir sınıf kitaplığı

Bir kitaplık normalde uygulama projeleri sınıflarda erişemiyor. Bu kısıtlama gösterilen teknik önlemek görünüyor **PlatInfoSap2** bir PCL kullanılmasını. Ancak, Xamarin.Forms adlı bir sınıf içeriyor [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) PCL uygulama projesi ortak sınıflarda erişmek için .NET yansıma kullanan.

PCL tanımlamalısınız bir `interface` üyeleriyle platformlarının her birinde kullanmanız gerekir. Ardından, her biri platformları bu arabirim uygulaması içerir. Arabirimini uygulayan sınıf ile tanımlanmalıdır bir [DependencyAttribute](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyAttribute/) derleme düzeyi.

PCL sonra genel kullanır [ `Get` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DependencyService.Get{T}/p/Xamarin.Forms.DependencyFetchTarget/) yöntemi `DependencyService` arabirimini uygulayan platform sınıfının bir örneği elde edilir.

Bu, gösterilmiştir [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) örnek.

## <a name="platform-specific-sound-generation"></a>Platforma özgü ses oluşturma

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) örnek ekler için bip **MonkeyTap** her platform ses nesil tesislerde erişerek program.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 9 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Bölüm 9 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Bağımlılık hizmeti](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
