---
title: Uygulama simgesi
description: "Bu makalede, görüntüleri .icns dosyasına paketleme ve Xamarin.Mac projesinde simgesi dahil olmak üzere bir Xamarin.Mac uygulamanın simgesi için gerekli görüntü oluşturulması ele alınmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 675b9405-d9a7-49f0-94ad-417f10a71d11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 28218bcbf6527c818fdf2988375d1c353e9f1d27
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="application-icon"></a>Uygulama simgesi

_Bu makalede, görüntüleri .icns dosyasına paketleme ve Xamarin.Mac projesinde simgesi dahil olmak üzere bir Xamarin.Mac uygulamanın simgesi için gerekli görüntü oluşturulması ele alınmaktadır._


## <a name="overview"></a>Genel Bakış

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, bir geliştirici aynı görüntü erişimi ve simge, çalışan bir geliştirici araçları *Objective-C* ve *Xcode* yapar.

Harika bir simge ana amacı, kullanıcının uygulamayı kullanırken beklemesi gereken bir Xamarin.Mac uygulama ve ipucu deneyimi iletmek. Bu makalede bu varlıkları halinde paketleme simge için gereken görüntü varlıklar oluşturmak için gerekli adımların tümünü kapsayan bir `AppIcons.appiconset` dosyası ve bu dosyayı bir Xamarin.Mac uygulamasında kullanma.

![AppIcons.appiconset Düzenleyicisi'ni](app-icon-images/intro01.png "AppIcons.appiconset Düzenleyicisi")


## <a name="application-icon"></a>Uygulama simgesi

Harika bir simge ana amacı, kullanıcının bir uygulamayı kullanırken beklemesi gereken bir Xamarin.Mac uygulama ve ipucu deneyimi iletmek. Her macOS uygulama simgesini görüntülemek için çeşitli boyutlarda Bulucu, yerleştirme, Git'i ve bilgisayar genelinde başka konumlara eklemeniz gerekir.


## <a name="designing-the-icon"></a>Simge tasarlama

Apple, bir uygulamanın simgesini tasarlarken aşağıdaki ipuçları önerir:

- Simge gerçekçi ve benzersiz bir şekli vermeye çalışın.
- MacOS uygulamanın bir iOS karşılık gelen varsa, iOS uygulamanın simgesini yeniden yok.
- Kişiler kolayca tanıyabilirsiniz Evrensel görüntülerin kullanın.
- Kolaylık sağlamak için çalışmalarımızı.
- Renk ve gölge uygulamanın olay anlatın simgesi yardımcı olması için olabildiğince az kullanın.
- Gerçek metinle karıştırma kaçının _greeked_ metin veya metin önermek için satır.
- Simgenin konu gerçek bir fotoğraf kullanmak yerine görünümüdür bir sürümünü oluşturur.
- MacOS kullanıcı Arabirimi öğeleri simgeleri kullanmaktan kaçının.
- Apple simgeler çoğaltmalarının simgeleri kullanmayın.

Lütfen okuyun [uygulama simgesi Galerisi](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Gallery.html#//apple_ref/doc/uid/20000957-CH88-SW1) ve [tasarlama uygulama simgeleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Designing.html#//apple_ref/doc/uid/20000957-CH87-SW1) Apple'nın bölümlerini [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) Xamarin.Mac uygulamanın simgesini tasarlama önce.


## <a name="required-image-sizes-and-filenames"></a>Gereken görüntü boyutları ve dosya adları

Diğer görüntü Geliştirici Xamarin.Mac uygulamasında kullanma edecek gibi herhangi bir kaynakta, uygulama simgesine sağlanan hem standart hem de Retina çözüm sürüm gerekir. Yeniden, diğer herhangi bir görüntü gibi kullanmak bir `@2x` simge dosyaları adlandırırken Biçimlendir:

- **Standart çözümleme**  - _görüntü adı_**.** _dosya adı uzantısı_ (örnek: **icon_512x512.png**)
- **Yüksek çözünürlüklü**  - _görüntü adı_**@2x.** _dosya adı uzantısı_ (örnek:  **icon_512x512@2x.png** )

Örneğin, uygulamanın simgesi 512 x 512 sürümünü temin etmek için dosyanın sayfadayken **icon_512x512.png** ve  **icon_512x512@2x.png** .

Simge kullanıcılar gördüğünüzde tüm konumları harika görünüyor emin olmak için aşağıda listelenen boyutları kaynaklarında sağlar:

<table width="100%" border="1px">
<tr>
    <td>Dosya adı</td>
    <td>Piksel cinsinden boyutu</td>
</tr>
<tr>
    <td>icon_512x512@2x.png</td>
    <td>1024 x 1024</td>
</tr>
<tr>
    <td>icon_512x512.png</td>
    <td>512 x 512</td>
</tr>
<tr>
    <td>icon_256x256@2x.png</td>
    <td>512 x 512</td>
</tr>
<tr>
    <td>icon_256x256.png</td>
    <td>256 x 256</td>
</tr>
<tr>
    <td>icon_128x128@2x.png</td>
    <td>256 x 256</td>
</tr>
<tr>
    <td>icon_128x128.png</td>
    <td>128 x 128</td>
</tr>
<tr>
    <td>icon_32x32@2x.png</td>
    <td>64 x 64</td>
</tr>
<tr>
    <td>icon_32x32.png</td>
    <td>32 x 32</td>
</tr>
<tr>
    <td>icon_16x16@2x.png</td>
    <td>32 x 32</td>
</tr>
<tr>
    <td>icon_16x16.png</td>
    <td>16 x 16</td>
</tr>
</table>

Apple'nın daha fazla bilgi için bkz: [sağlamak yüksek çözünürlüklü sürümleri, tüm uygulama grafik kaynakları](https://developer.apple.com/library/mac/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html#//apple_ref/doc/uid/TP40012302-CH7-SW3) belgeleri.


## <a name="packaging-the-icon-resources"></a>Simge kaynakları paketleme

Tasarlanmış ve çıkış adları ve gerekli dosya boyutları için kaydedilen içeren simge, Mac için Visual Studio Xamarin.Mac kullanmak için görüntü varlıklarının atamak kolaylaştırır.

Aşağıdakileri yapın:

1. İçinde **çözüm paneli**, açık **Assets.xcassets** > **AppIcons.appiconset**: 

    ![AppIcons.appiconset düzenleme](app-icon-images/intro01.png "AppIcons.appiconset düzenleme")
2. Gerekli her simge boyutu için simgesine tıklayın ve yukarıda oluşturulan karşılık gelen görüntü dosyasını seçin: 

    [![Bir simge görüntüsü seçme](app-icon-images/intro02.png "bir simge görüntüsü seçme")](app-icon-images/intro02-large.png#lightbox)
3. Değişikliklerinizi kaydedin.


## <a name="using-the-icon"></a>Simgesini kullanarak

Bir kez `AppIcons.appiconset` dosyası derlendikten, Mac için Visual Studio Xamarin.Mac projeye atamanız gerekir

Aşağıdakileri yapın:

1. Çift **Info.plist** içinde **çözüm paneli** açmak için **proje seçenekleri**.
2. İçinde **Mac OS X uygulaması hedefi** 'ye tıklayın **uygulama simgeleri** seçmek için `AppIcons.appiconset` dosyası: 

    [![Simge ayarlanmasını](app-icon-images/icon01.png "simge kümesi ayarlama")](app-icon-images/icon01-large.png#lightbox)
3. Değişiklikleri kaydedin.

Uygulama çalıştırıldığında yeni simgesine yerleştirme görüntülenir:

![Bir uygulama simgesine macOS örneği yerleştirme](app-icon-images/icon04.png "macOS bir uygulama simgesine örneği yerleştirme")


## <a name="summary"></a>Özet

Bu makalede, bir simge paketleme macOS uygulama simgesini oluşturmak için gereken görüntülerle ve simge Xamarin.Mac projesinde dahil olmak üzere çalışma ayrıntılı bir bakış duruma getirdi.


## <a name="related-links"></a>İlgili bağlantılar

- [MacImages (örnek)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Görüntülerle Çalışma](~/mac/app-fundamentals/image.md)
- [macOS İnsan Arabirimi yönergelerine - simgeleri ve görüntüleri](https://developer.apple.com/macos/human-interface-guidelines/icons-and-images/image-size-and-resolution/)
- [OS X için yüksek çözünürlüğü hakkında](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
- [Icns Oluşturucusu](https://itunes.apple.com/us/app/icns-builder/id554660130?mt=12)
