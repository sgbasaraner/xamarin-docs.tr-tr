---
title: Xamarin.Forms eklentileri oluşturma ve kullanma
description: Bu makalede, kullanma ve Xamarin.Forms eklentileri oluşturma açıklanmaktadır. Eklentileri, genellikle kolayca yerel platform özellikleri göstermek için kullanılır.
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/05/2018
ms.openlocfilehash: 4d121c2dfcca380e1735da1a4ca47c42d1957b8a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854746"
---
# <a name="consuming-and-creating-xamarinforms-plugins"></a>Xamarin.Forms eklentileri oluşturma ve kullanma

Tüm platformlarda mevcut birçok yerel platform özellikleri vardır, ancak biraz farklı bir API'ye sahiptir. Soyut bir platformlar arası arabirim oluşturarak ve ardından bu arabirimi çeşitli platformlarda uygulama bu özellikleri kullanmak için geliştiricilere bir yol olduğu. Xamarin.Forms uygulama daha sonra bu platform uygulamalarını kullanarak erişir [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

Geliştiriciler, yazarak bu çalışmayı paylaşabilirsiniz bir _eklentisi_ ve NuGet için yayımlama.

> [!NOTE]
> Yalnızca eklentileri ile önceden kullanılabilen birçok platformlar arası özellik artık açık kaynaklı bir parçası olan **[Xamarin.Essentials](~/essentials/index.md)** kitaplığı. Bu özellikler şunları içerir: stav baterie, compass, hareket algılayıcılar, coğrafi konum, metin okuma ve çok daha fazlası. Gelecekte **Xamarin.Essentials** Xamarin.Forms uygulamaları için platformlar arası özellikleriyle birincil kaynağı olacaktır. Geliştiriciler, yine de oluşturabilir ve eklentileri yayımlama olsa da, katkıda bulunan göz önünde bulundurun **Xamarin.Essentials**.

## <a name="finding-and-adding-plugins"></a>Bulma ve eklentiler ekleme

Xamarin topluluk birçok platformlar arası eklenti Xamarin.Forms ile uyumlu oluşturdu. En büyük koleksiyon bulunabilir:

[**Xamarin eklentileri**](https://github.com/xamarin/XamarinComponents)

Bizim izlenecek yol kılavuzu için NuGet paketlerini projenize ekleme, bakın [NuGet paketini projenize dahil olmak üzere](/visualstudio/mac/nuget-walkthrough/).

## <a name="creating-plugins"></a>Eklentiler oluşturma

Oluşturma ve Nuget paketleri (ve Xamarin bileşenleri) kendi eklentileri yayımlama mümkündür. Birçok mevcut eklentiler açık kaynaklı olduğundan nasıl writtern silinmiş anlamak için kodu gözden geçirebilirsiniz.

Örneğin, aşağıdaki eklentiler listesi olan tüm açık kaynak ve bazı örnekleri için karşılık gelen [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) bölümü:

- **Metin okuma** James Montemagno tarafından &ndash; [GitHub](https://github.com/jamesmontemagno/TextToSpeechPlugin) ve [NuGet  ](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech)
- **Stav Baterie** James Montemagno tarafından &ndash; [GitHub](https://github.com/jamesmontemagno/BatteryPlugin) ve [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)

Bu yönergeler için olduğu gibi bu Github projeleri iyi bir başlangıç noktası kendi platformlar arası eklenti oluşturmak için sağlayabilir [bir eklenti oluşturmak için Xamarin](https://github.com/xamarin/XamarinComponents#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Platformlar arası eklenti projeleri yapılandırma

Bir NuGet paketi tasarlamak için belirli bir gereksinimi yoktur olsa da, platformlar arası uygulamalar için paket oluşturmaya yönelik bazı yönergeler vardır.

Geçmişte, platformlar arası eklenti genellikle aşağıdaki bileşenlerden almıştır:

- PCL Eklentisi için API temsil eden bir arabirim ile
- iOS, Android ve evrensel Windows Platformu (UWP) arabiriminin kitaplıklarıyla sınıfı.

Okuma James Montemagno'nın [blog gönderisi](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) açıklayan eklentileri için Xamarin oluşturma işlemi.

Daha yakın bir tarihte eklentileri olması oluşturulabilir ile tek bir çoklu hedeflenen platform. Bu yaklaşım, James Montemagno kişinin açıklanan [blog gönderisi](https://montemagno.com/converting-xamarin-libraries-to-sdk-style-multi-targeted-projects/). Bu yaklaşım, yukarıda bağlantı James Montemagno'nın Eklentileri kullanılır ve biçimini de kullanılır **Xamarin.Essentials**.

Bu, doğrudan bir eklentiyi Xamarin.Forms başvuran önlemek için bir tercih edilir.
Diğer geliştiricilerin eklentisini kullanmaya çalıştığınızda, bu sürüm çakışması sorunlar oluşturabilir. Bunun yerine API herhangi bir Xamarin veya .NET uygulama tarafından kullanılabilir olacağı şekilde tasarlayın deneyin.

### <a name="publishing-nuget-packages"></a>NuGet paketleri yayımlama

NuGet paketlerini sahip bir **nuspec** dosyasını projenize hangi parçalarının pakette yayımlanan tanımlayan bir xml dosyasıdır. **Nuspec** dosya kimliği, başlık ve yazarlar gibi paketi hakkında bilgileri de içerir.

Bkz: [NuGet belgeleri](/nuget/create-packages/creating-a-package.md) oluşturma ve NuGet paketleri yayımlama hakkında daha fazla bilgi için.

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms için yeniden kullanılabilir eklentiler oluşturma](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Xamarin (video) için kullanma ve geliştirme eklentileri](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
