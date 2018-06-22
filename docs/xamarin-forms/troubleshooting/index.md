---
title: Sorun giderme
description: Sık karşılaşılan hata koşulları ve bunların nasıl çözüleceği
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fbe4fb6fce52636b59a9637ee0150c4c19fcc9da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30788529"
---
# <a name="troubleshooting"></a>Sorun giderme

_Sık karşılaşılan hata koşulları ve bunların nasıl çözüleceği_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>Hata: "Xamarin.Forms sürümü ile uyumlu bulunamıyor... kurulamıyor"

Aşağıdaki hataları görüntülenebilir **paketi Konsolu** Xamarin.Forms çözümünü veya bir Xamarin.Forms Android uygulama projesi ilgili tüm Nuget paket güncelleştirilirken penceresi:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>Bu hatanın nedeni nedir?

Visual Studio Mac (veya Visual Studio) için güncelleştirmelerinin Xamarin.Forms Nuget packge için kullanılabilir olduğunu belirtebilir *ve tüm bağımlılıklarını*. Xamarin Studio'da, çözüm 's **paketleri** düğümü şuna benzeyebilir (sürüm numaraları farklı olabilir):

![](images/updates-available.png "Android projesi paketler klasörü")

Güncelleştirme çalışırsanız, bu hata oluşabilir _tüm_ paketler.

Android projeleri Android 6.0 (API 23) hedef/derleme sürümüne ayarlayın veya Xamarin.Forms sıkı bir bağımlılık aşağıda sahiptir çünkü *belirli* Android sürümlerini paketlerini destekler. Bu paketleri güncelleştirilmiş sürümlerini kullanılabilir olmakla birlikte, Xamarin.Forms bunlarla mutlaka uyumlu değil.

Bu durumda güncelleştirmelidir _yalnızca_ **Xamarin.Forms** paket bağımlılıkları uyumlu sürümlerinde kalmasını sağlamak üzere. Güncelleştirme için Android desteği paketlerini oluşturmayın sürece projenize eklediğiniz diğer paketleri de ayrı ayrı güncelleştirilebilir.


> [!NOTE]
> Xamarin.Forms 2.3.4 kullanıyorsanız ya da daha yüksek **ve** Android 7.0 (API 24) veya üzeri, Android projenizin hedef/derleme sürümü Ayarla sonra artık yukarıdaki sabit bağımlılıkları uygulamak ve Destek güncelleştirebilir Xamarin.Forms paket bağımsız olarak paketler.


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Düzeltme: tüm paketleri kaldırın ve yeniden Xamarin.Forms ekleyin

Varsa **Xamarin.Android.Support** paketleri uyumsuz sürümleri için güncelleştirildi, en basit düzeltme kullanmaktır:

1. El ile Android projedeki tüm Nuget paketlerini silin
2. Yeniden ekleyin **Xamarin.Forms** paket.

Bu otomatik olarak yükleyecek *doğru* diğer paketleri sürümleri.

Bu işlemin bir video görmek için bu bakın [forumları post](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
