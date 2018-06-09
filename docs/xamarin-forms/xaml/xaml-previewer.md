---
title: Xamarin.Forms için XAML Önizleyicisi
description: Bu makalede XAML genele gitmeyi yazarken çizilir, Xamarin.Forms düzenlerini görmek için nasıl kullanılacağı açıklanmaktadır. Visual Studio 2017 ve Mac için Visual Studio XAML genele gitmeyi kullanılabilir
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/31/2018
ms.openlocfilehash: 25c8e1a34f8be5ab2f8491e75fa5aac470d55bc8
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245865"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms için XAML Önizleyicisi

_Siz yazarken çizilir, Xamarin.Forms düzenleri bakın!_

## <a name="requirements"></a>Gereksinimler

Projeleri en son çalışmak XAML genele gitmeyi Xamarin.Forms NuGet paketini gerektirir. Android uygulamaları Önizleme gerektirir [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Daha fazla bilgi bulunmaktadır [sürüm notları](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## <a name="getting-started"></a>Başlarken

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kullanım **Görünüm > Diğer Pencereler > Xamarin.Forms Önizleyicisi** Önizleme penceresini açmak için Visual Studio menüsünde. Kullanım **Pencere > Yeni dikey sekme grubu** menü yan yana getirin.

[![ListView denetimi Önizleme Visual Studio'da](xaml-previewer-images/xamlp-list-vs-sml.png "Visual Studio Forms önizlemesinde")](xaml-previewer-images/xamlp-list-vs.png#lightbox "Visual Studio Forms önizlemesinde")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

**Önizleme** düğmesi XAML dosyası sağ tıklayıp seçerek Editor'ü görüntülenebilir **birlikte Aç > Forms Önizleyicisi**. Önizleme bölmesini sonra gösterilen veya tuşlarına basarak gizli **Önizleme** herhangi bir XAML belge pencerenin sağ üst köşedeki düğmesi:

[![Mac için Visual Studio ListView denetimi Önizleme](xaml-previewer-images/xamlp-list-sml.png "Mac için Visual Studio Forms önizlemesinde")](xaml-previewer-images/xamlp-list.png#lightbox "Mac için Visual Studio Forms önizlemesinde")

-----

## <a name="xaml-preview-options"></a>XAML Önizleme seçenekleri

Önizleme bölmesini üstünde Seçenekler şunlardır:

* **Telefon** – bir telefon boyutu ekran oluşturma
* **Tablet** – bir tablet boyutu ekranda işleme (bölmesinin sağ taraftaki yakınlaştırma denetimleri vardır unutmayın)
* **Android** – ekran Android sürümü göster
* **iOS** – ekran iOS sürümünü Göster
* Portait (simgesi) – önizleme için dikey yönde kullanır
* Yatay (simgesi) – kullanır yatay yönde Önizleme

## <a name="adding-design-time-data"></a>Tasarım zamanı veri ekleme

Bazı düzenleri için kullanıcı arabirimi denetimlerini bağlı herhangi bir veri olmadan görselleştirmek zor olabilir. Önizleme daha kullanışlı hale getirmek için bazı statik verileri denetimlere bir bağlama bağlamı (ya da arka plan kodu veya XAML kullanılarak) cmdlet'e kod atayın.

Ahmet Montemagno için 's başvuran [tasarım zamanı veri ekleme blog gönderisi](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) statik ViewModel XAML'de bağlamak nasıl görmek için.

## <a name="detecting-design-mode"></a>Tasarım modunu algılama

Statik [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) özelliği incelenmesi uygulama önizlemesinde çalışıp çalışmadığını belirlemek için. Bu uygulama önizlemesinde çalışırken yalnızca yürütülür kodu belirtmenize olanak tanır:

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki sorunları denetleme ve [Xamarin forumları](https://forums.xamarin.com/categories/xamarin-forms), sorunlarla karşılaşırsanız.

### <a name="xaml-preview-isnt-showing"></a>XAML Önizleme gösteren değil

Aşağıdakileri denetleyin:

* Proje (derlenmiş) XAML dosyaları Önizleme denemeden önce oluşturulmalıdır.
* Tasarımcı Aracısı XAML dosyası Önizleme ilk kez kurulum olmalıdır - bu hazır olana kadar genele gitmeyi ilerleme durumu iletileri ile birlikte, bir İlerleme göstergesi görünür.
* Try Kapanış ve XAML dosyası'nı yeniden açmayı.

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>Geçersiz XAML: Android projesi yerleşik Önizleme oluşturulmadan önce gerekiyor

XAML genele gitmeyi proje bir sayfa işlemeden önce oluşturulması gerekir.
Aşağıdaki hata önizleme bölmesinde en üstte görünüyorsa, uygulamayı yeniden oluşturun ve yeniden deneyin.

![Hata iletisi: Proje gerekir oluşturulur ilk](xaml-previewer-images/error-not-built-sml.png "hata iletisi: projeyi yeniden")
