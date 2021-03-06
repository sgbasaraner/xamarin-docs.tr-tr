---
title: Bölüm 9 özeti. Platforma özgü API çağrıları
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 9 özeti. Platforma özgü API çağrıları'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: e7b2eea22758155db7d79fa26f3376e16cf16a45
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39157022"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Bölüm 9 özeti. Platforma özgü API çağrıları

> [!NOTE] 
> Bu sayfadaki notları kitapta tanıtılan malzeme gelen Xamarin.Forms nerede ayrıldığını alanları gösterir.

Bazen, platforma göre değişen bazı kodlar çalıştırmak gereklidir. Bu bölümde teknikleri inceler.

## <a name="preprocessing-in-the-shared-asset-project"></a>Paylaşılan varlık projede ön işleme

Xamarin.Forms paylaşılan bir varlık projesine C# önişlemci yönergeleri kullanarak her platform için farklı bir kod yürütebilir `#if`, `#elif`, ve `endif`. Bu gösterilmiştir [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Üç ekran değişkeninin biçimlendirilmiş paragraf](images/ch09fg01-small.png "cihaz modeli ve işletim sistemi")](images/ch09fg01-large.png#lightbox "cihaz modeli ve işletim sistemi")

Ancak, sonuç kodu çirkin ve okunması zor olabilir.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Paylaşılan varlık projesine paralel sınıfları

SAP'de, platforma özgü kod yürüten daha yapılandırılmış bir yaklaşım gösterilmiştir [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) örnek. Platformu projelerin her biri aynı adlı bir sınıf ve yöntemler içeriyor, ancak bu belirli bir platform için uygulanır. SAP sonra yalnızca sınıfın örneğini oluşturur ve yöntemini çağırır.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService ve taşınabilir sınıf kitaplığı

> [!NOTE] 
> Taşınabilir sınıf kitaplıkları, .NET standart kitaplıkları tarafından değiştirilmiştir. Kayıt defterinden tüm örnek kod, .NET standart kitaplıkları kullanmak için dönüştürüldü.

Bir kitaplık, normalde uygulama projelerindeki sınıfların erişemez. Bu kısıtlama gösterilen teknik önlemek için gibi görünüyor **PlatInfoSap2** kitaplığa kullanılmasını. Ancak, Xamarin.Forms adlı bir sınıf içeren [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) uygulama projesinde ortak Sınıf Kitaplığı'ndan erişmek için .NET, yansıtma kullanan.

Kitaplık tanımlamalıdır bir `interface` platformlarının her birinde kullanmak için ihtiyaç duyduğu üyelere sahip. Ardından, her platform bu arabirimi uygulaması içerir. Arabirimini uygulayan sınıf ile tanımlanmalıdır bir [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) derleme düzeyinde.

Kitaplığı daha sonra genel kullanır [ `Get` ](xref:Xamarin.Forms.DependencyService.Get*) yöntemi `DependencyService` arabirimi uygulayan platform sınıfının bir örneği elde edilir.

Bu gösterilmiştir [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) örnek.

## <a name="platform-specific-sound-generation"></a>Platforma özel ses oluşturma

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) örnek ekler bip sesi için **MonkeyTap** ses oluşturma özelliklerini her platformdaki erişerek program.

## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 9 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Bölüm 9 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Bağımlı hizmet](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
