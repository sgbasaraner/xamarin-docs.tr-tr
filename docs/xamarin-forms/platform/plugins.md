---
title: Eklentileri
description: "Xamarin.Forms uygulamalarına yerel işlevselliği ekleyin"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2016
ms.openlocfilehash: ad338e655c1aeb475122c837ca5c805e491f84bc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="plugins"></a>Eklentileri

Tüm platformlarda mevcut birçok yerel platform özellikleri vardır, ancak biraz farklı API'leri sahip. Geliştiriciler de başkalarıyla paylaşabilir bu özellikleri için bir Özet platformlar arası arabirimi oluşturmak üzere eklenti yazma.

Bu özellikler şunlardır: pil durumu, pusula, hareket algılayıcılar, coğrafi konuma, okuma ve çok daha fazlasını. Eklentileri bu özellikler Xamarin.Forms uygulamaları tarafından kolayca erişilmesini sağlar.

## <a name="finding-and-adding-plugins"></a>Bulma ve eklenti ekleme

Xamarin topluluk birçok platformlar arası eklentileri Xamarin.Forms - uyumlu oluşturdu büyük bir koleksiyon yolda bulunabilir:

[**Xamarin eklentileri**](https://github.com/xamarin/plugins)

NuGet paketlerini projenize eklemek için bir kılavuz için bizim izlenecek bakın [NuGet paketini projenize dahil olmak üzere](/visualstudio/mac/nuget-walkthrough/).


## <a name="creating-plugins"></a>Eklentiler oluşturma

Oluşturma ve Nuget paketleri (ve Xamarin bileşenleri) kendi eklentileri yayımlama mümkündür. Birçok mevcut eklentileri açık kaynaklı olduğundan, writtern nasıl kaldıktan anlamak için bunların kodu gözden geçirebilirsiniz.

Örneğin, aşağıdaki eklenti listesi olan tüm açık kaynaklı ve örnekleri için karşılık gelen [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) bölümü:

- **Metin okuma** Ahmet Montemagno tarafından &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/TextToSpeech) ve [NuGet  ](https://www.nuget.org/packages/Xam.Plugin.Battery)
- **Pil durumu** Ahmet Montemagno tarafından &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery) ve [NuGet](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech/)

Bu yönergeler için yaptığınız gibi bu Github projeleri iyi bir başlangıç noktası kendi platformlar arası eklenti oluşturmak için sağlayabilir [bir eklenti için Xamarin oluşturma](https://github.com/xamarin/plugins#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Platformlar arası eklentisi projeleri yapılandırma

Bir NuGet paketi tasarlamak için belirli bir gereksinimi olsa da, platformlar arası uygulamalar için paket oluşturmak için bazı kurallar vardır.

Platformlar arası eklentisi genellikle aşağıdaki bileşenlerden oluşur:

- PCL Eklentisi için API temsil eden bir arabirim ile
- iOS, Android ve Windows arabirimini uygulaması kitaplıklarıyla sınıfı.

Okuma Ahmet Montemagno'nın [blog gönderisi](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) eklentileri için Xamarin oluşturma işlemi açıklayan.

Bir tercih Xamarin.Forms doğrudan bir eklentiden başvuran önlemek için edilir.
Diğer geliştiriciler eklenti kullanmaya çalıştığınızda bu sürüm çakışması sorunlar oluşturabilir. Böylece herhangi bir Xamarin ya da .NET uygulama tarafından kullanılan API tasarlamak bunun yerine deneyin.

### <a name="publishing-nuget-packages"></a>NuGet paketleri yayımlama

NuGet paketlerini sahip bir **nuspec** projenizi hangi kısımlarının pakette yayımlanan tanımlayan bir xml dosyasıdır dosya. **Nuspec** dosyası, ayrıca paket kimliği, başlık ve yazarlar gibi ilgili bilgileri içerir.

Bkz: [NuGet belgelerine](http://docs.nuget.org/create/creating-and-publishing-a-package) oluşturma ve NuGet paketleri yayımlama hakkında daha fazla bilgi için.


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms için yeniden kullanılabilir eklentileri oluşturma](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Geliştirme & kullanarak eklentileri xamarin (video)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
