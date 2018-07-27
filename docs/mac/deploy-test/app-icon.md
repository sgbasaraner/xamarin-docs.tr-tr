---
title: Xamarin.Mac uygulamaları için uygulama simgesi
description: Bu makalede, görüntüleri .icns dosyasına paketleme ve Xamarin.Mac projede simgesi dahil olmak üzere bir Xamarin.Mac uygulama simgesi, gerekli görüntü oluşturmayı kapsar.
ms.prod: xamarin
ms.assetid: 675b9405-d9a7-49f0-94ad-417f10a71d11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9c81138aea57c0027ad0f53e3116878ffb800eae
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276047"
---
# <a name="application-icon-for-xamarinmac-apps"></a>Xamarin.Mac uygulamaları için uygulama simgesi

_Bu makalede, görüntüleri .icns dosyasına paketleme ve Xamarin.Mac projede simgesi dahil olmak üzere bir Xamarin.Mac uygulama simgesi, gerekli görüntü oluşturmayı kapsar._


## <a name="overview"></a>Genel Bakış

C# ve .NET ile bir Xamarin.Mac uygulamasında çalışırken, bir geliştirici aynı görüntü erişimi olan ve simge, içinde çalışmakta olan bir geliştirici araçları *Objective-C* ve *Xcode* yapar.

Harika bir simge ana amacı, kullanıcı bir uygulama kullanırken beklenmesi bir Xamarin.Mac uygulamasını ve ipucu deneyimi iletmek. Bu makalede tüm görüntü halinde bu varlıkları paketleme simge için gerekli varlıkları oluşturmak için gerekli adımları ele alınmaktadır bir `AppIcon.appiconset` dosya ve dosyada bulunan bir Xamarin.Mac uygulamasını kullanma.

![AppIcon.appiconset Düzenleyicisi](app-icon-images/intro01.png "AppIcon.appiconset Düzenleyicisi")


## <a name="application-icon"></a>Uygulama simgesi

Harika bir simge ana amacı, kullanıcı bir uygulama kullanırken beklenmesi bir Xamarin.Mac uygulamasını ve ipucu deneyimi iletmek. Her macOS uygulama simgesini görüntülemek için çeşitli boyutlarda Bulucu, Dock, Launchpad ve başka konumlara bilgisayar genelinde içermesi gerekir.


## <a name="designing-the-icon"></a>Simge tasarlama

Apple, uygulama simgesine tasarlarken aşağıdaki ipuçlarını önerir:

- Gerçekçi ve benzersiz bir şekil simgesi vermeyi düşünün.
- MacOS uygulamanın bir iOS karşılığı varsa, iOS uygulamanın simgesi yeniden kullanmayın.
- Kişiler bir kolayca tanıyabilirsiniz Evrensel tanımayı kullanın.
- Kolaylık olması için çaba harcar.
- Renk ve gölge Yardım uygulamanın hikayenizi simgesi tedbirli şekilde kullanın.
- Gerçek metinle karıştırmaktan kaçının _greeked_ metin veya metin önermek için satır.
- Simgenin konu gerçek bir fotoğraf kullanmak yerine bir görünümüdür sürümünü oluşturun.
- MacOS kullanıcı Arabirimi öğeleri simgeler kullanmaktan kaçının.
- Çoğaltmaları Apple simgelerin simge kullanmayın.

Okuyun [uygulama simgesi Galerisi](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Gallery.html#//apple_ref/doc/uid/20000957-CH88-SW1) ve [tasarlama uygulama simgeleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Designing.html#//apple_ref/doc/uid/20000957-CH87-SW1) Apple'nın bölümlerini [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) Xamarin.Mac uygulamanın simgesi tasarlama önce.


## <a name="required-image-sizes-and-filenames"></a>Gerekli resim boyutları ve dosya adları

Diğer resim geliştirici bir Xamarin.Mac uygulamasını kullanmak için giden gibi herhangi bir kaynakta, uygulamanın simgesi sağlanan hem standart hem de Retina çözüm sürümü gerekir. Diğer herhangi bir görüntü gibi yeniden kullanmak bir `@2x` biçimlendirme adlandırırken simge dosyaları:

- **Standart çözünürlükte**  - _IMAGENAME_**.** _dosya adı uzantısı_ (örnek: **icon_512x512.png**)
- **Yüksek çözünürlüklü**  - _IMAGENAME_**@2x.** _dosya adı uzantısı_ (örnek: **icon_512x512@2x.png**)

Örneğin, uygulamanın simgesi 512 x 512 sürümünü sağlamak için dosya sayfadayken **icon_512x512.png** ve **icon_512x512@2x.png**.

Simge kullanıcıları nasıl görmek her yerde harika göründüğünden emin olmak için aşağıda listelenen boyutlarda kaynakları sağlayın:

|Dosya adı|Piksel cinsinden boyutu|
|---|---|
|icon_512x512@2x.png|1024 x 1024|
|icon_512x512.png|512 x 512|
|icon_256x256@2x.png|512 x 512|
|icon_256x256.png|256 x 256|
|icon_128x128@2x.png|256 x 256|
|icon_128x128.png|128 x 128|
|icon_32x32@2x.png|64 x 64|
|icon_32x32.png|32 x 32|
|icon_16x16@2x.png|32 x 32|
|icon_16x16.png|16 x 16|

Daha fazla bilgi için bkz. Apple'nın [sağlayan yüksek çözünürlüklü sürümleri, tüm uygulama grafik kaynaklarını](https://developer.apple.com/library/mac/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html#//apple_ref/doc/uid/TP40012302-CH7-SW3) belgeleri.


## <a name="packaging-the-icon-resources"></a>Simge kaynakları paketleme

Simgesiyle tasarlanmış ve çıkış adları ve gerekli dosya boyutları için kaydettiğiniz Mac için Visual Studio görüntü varlıkları kullanılmak üzere Xamarin.Mac atamak kolaylaştırır.

Aşağıdakileri yapın:

1. İçinde **çözüm bölmesi**açın **Assets.xcassets** > **AppIcons.appiconset**: 

    ![AppIcon.appiconset düzenleme](app-icon-images/intro01.png "AppIcon.appiconset düzenleme")
2. Gerekli, her bir simge boyutu için simgeye tıklayın ve yukarıda oluşturulan karşılık gelen bir resim dosyası seçin: 

    [![Bir simge görüntüsü seçme](app-icon-images/intro02.png "bir simge görüntüsü seçme")](app-icon-images/intro02-large.png#lightbox)
3. Değişikliklerinizi kaydedin.


## <a name="using-the-icon"></a>Simgesini kullanarak

Bir kez `AppIcon.appiconset` dosya derlendiğinden, Mac için Visual Studio'da Xamarin.Mac projeye atamanız gerekir

Aşağıdakileri yapın:

1. Çift **Info.plist** içinde **çözüm bölmesi** açmak için **proje seçenekleri**.
2. İçinde **Mac OS X uygulaması hedefi** tıklayın ve bölüm **uygulama simgeleri** seçmek için `AppIcon.appiconset` dosyası: 

    [![Simge kümesi ayarlama](app-icon-images/icon01.png "simge kümesi ayarlama")](app-icon-images/icon01-large.png#lightbox)
3. Değişiklikleri kaydedin.

Uygulama çalıştırıldığında, yeni simgesine dock'ta görüntülenir:

![Uygulama simgesi macos'deki örneği dock](app-icon-images/icon04.png "macos'deki uygulama simgesi örneği Yerleştir")


## <a name="summary"></a>Özet

Bu makalede macOS uygulama simgesi, oluşturmak için gereken görüntüleri bir simge paketleme ve Xamarin.Mac projesinde bir simge içeren çalışma ayrıntılı bilgi duruma getirdi.


## <a name="related-links"></a>İlgili bağlantılar

- [MacImages (örnek)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Görüntülerle Çalışma](~/mac/app-fundamentals/image.md)
- [macOS İnsan Arabirimi yönergelerine - simgelerle ve görüntülerle](https://developer.apple.com/macos/human-interface-guidelines/icons-and-images/image-size-and-resolution/)
- [OS X için yüksek çözünürlüklü hakkında](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
- [Icns Oluşturucusu](https://itunes.apple.com/us/app/icns-builder/id554660130?mt=12)
