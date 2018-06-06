---
title: WatchOS Xamarin Gezinti ile çalışma
description: Bu belge watchOS uygulamasında Gezinti ile nasıl çalışılacağını açıklar. Kalıcı arabirimleri, hiyerarşik gezinti ve sayfa tabanlı arabirimler açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c9bcfc388164060549ca7010d11671abfa8230ac
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790646"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>WatchOS Xamarin Gezinti ile çalışma

En basit Gezinti kullanılabilir izleme üzerinde basit bir seçenektir [kalıcı açılan](#modal) geçerli Sahne en üstünde görünür.

Çok Sahne izleme için iki Gezinti örneklerinde var. uygulamalar şunlardır:

- [Hiyerarşik Gezinme](#Hierarchical_Navigation)
- [Sayfa tabanlı arabirimleri](#Page-Based_Interfaces)

<a name="modal"/>

## <a name="modal-interfaces"></a>Kalıcı arabirimleri

Kullanım `PresentController` yöntemi bir arabirim denetleyicisi kalıcı olarak açın. Arabirim Denetleyicisi zaten tanımlanmalıdır **Interface.storyboard**.

```csharp
PresentController ("pageController","some context info");
```

Kalıcı olarak sunulan denetleyicileri (önceki Sahne kapsayan) ekranın tamamını kullanın. Varsayılan başlığı ayarlamak **iptal** ve onu dokunarak denetleyicisi kapatmak.

Program aracılığıyla kalıcı olarak sunulan denetleyicisi kapatmak için arama `DismissController`.

```csharp
DismissController();
```

Kalıcı ekranlar tek Sahne ya da kullanım sayfa tabanlı yerleşim olabilir.

<a name="Hierarchical_Navigation"/>

## <a name="hierarchical-navigation"></a>Hiyerarşik gezinme

Planda üzerinden geri gittiğinizde, benzer bir biçimde olabilir bir yığın gibi sunar `UINavigationController` iOS üzerinde çalışır. Planda Gezinti yığına gönderilir ve (programlı olarak veya kullanıcı seçime göre) Sil'i devre dışı.

![](navigation-images/hierarchy-1.png "Planda Gezinti yığına gönderilen") ![ ] (navigation-images/hierarchy-2.png "planda dışına Gezinti yığını Sil'i")

İOS gibi sol-edge-manyetik geri hiyerarşik Gezinti yığınında üst denetleyici gider.

Her iki [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog) ve [WatchTables](https://developer.xamarin.com/samples/WatchTables) örnekleri hiyerarşik Gezinti içerir.

### <a name="pushing-and-popping-in-code"></a>İletme ve kodda pencerelerinin

Kit bir "Gezinti denetleyici" aşırı arching gerektirmez izleme iOS yaptığı gibi - oluşturulması için yalnızca bir denetleyici push kullanılarak `PushController` yöntemi ve bir gezinti yığını otomatik olarak oluşturulur.

```csharp
PushController("secondPageController","some context info");
```

İzleme'nin ekran içerecek bir **geri** sol üst ancak düğmesini Program aracılığıyla da kaldırabilirsiniz Sahne kullanarak gezinti yığını `PopController`.

```csharp
PopController();
```

İOS, ayrıca Gezinti yığını kullanarak kök dizinine döndürmeyi mümkün olduğu gibi `PopToRootController`.

```csharp
PopToRootController();
```

### <a name="using-segues"></a>Kullanarak Segues

Segues arasındaki hiyerarşik gezinti tanımlamak için film şeridi görünümlerde oluşturulabilir. İşletim sistemi çağrıları hedef Sahne için içerik almak için `GetContextForSegue` Yeni Arabirim Denetleyicisi başlatılamadı.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier)
{
  if (segueIdentifier == "mySegue") {
    return new NSString("some context info");
  }
  return base.GetContextForSegue (segueIdentifier);
}
```
<a name="Page-Based_Interfaces"/>

## <a name="page-based-interfaces"></a>Sayfa tabanlı arabirimleri

Sayfa tabanlı arabirimler, soldan sağa, benzer şekilde içeri doğru çekin `UIPageViewController` iOS üzerinde çalışır. Gösterge nokta hangi sayfa şu anda görüntülenen göstermek için ekranın altındaki görüntülenir.

![](navigation-images/paged-1.png "Örnek ilk sayfasına") ![ ] (navigation-images/paged-2.png "örnek ikinci sayfasında") ![ ] (navigation-images/paged-5.png "örnek beşinci sayfası")


Gözcü uygulamanız için ana UI sayfasında tabanlı bir arabirim sağlamak için kullanın `ReloadRootControllers` bir dizi arabirim denetleyicilerini ve bağlamları ile:

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

Kök olmayan sayfa tabanlı bir denetleyicisi ortaya çıkarabilir kullanarak `PresentController` uygulama diğer görünümlerde birinden.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```



## <a name="related-links"></a>İlgili bağlantılar

- [WatchKitCatalog (örnek)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WatchTables (örnek)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchTables/)
