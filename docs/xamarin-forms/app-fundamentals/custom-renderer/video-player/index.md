---
title: Bir video oynatıcı uygulama
description: Bu makalede, Xamarin.Forms kullanarak bir video oynatıcı uygulaması uygulamak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 0CE9BEE7-4F81-4A00-B9B3-5E2535CD3050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 00697ca0adf3a34abec90c2f96d9fd9c273d06bb
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239790"
---
# <a name="implementing-a-video-player"></a>Bir video oynatıcı uygulama

Bazen, bir Xamarin.Forms uygulaması video dosyalarında yürütmek için tercih edilir. Bu makale dizisinde, iOS, Android ve adlı bir Xamarin.Forms sınıf Evrensel Windows Platformu (UWP) için özel Oluşturucu nasıl yazılacağından `VideoPlayer`.

İçinde [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) örnek, uygulayan ve destekleyen tüm dosyaları `VideoPlayer` adlı klasörlerde bulunan `FormsVideoLibrary` ve ad alanı ile tanımlanan `FormsVideoLibrary` veya ad alanları başlamak `FormsVideoLibrary`. Bu kuruluş ve adlandırma kolaylaştırır kendi Xamarin.Forms çözüme video oynatıcı dosyaları kopyalayın.

`VideoPlayer` üç tür kaynaktan video dosyalarından yürütebilirsiniz:

- Bir URL kullanarak Internet
- Platform uygulaması içinde katıştırılmış bir kaynağı
- Cihazın video kitaplığı

Video oynatıcı gerektiren *Aktarım denetimleri*, yürütme ve video duraklatmak için düğmeler olduğu ve çubuğunu konumlandırma video ilerlemeyi gösterir ve hızlı bir şekilde farklı bir konuma Atla olanak tanır. `VideoPlayer` platform (aşağıda gösterildiği gibi) veya sizin tarafından sağlanan konumlandırma çubuğu özel Aktarım denetimleri ve konumlandırma çubuğu sağlayabilir ve Aktarım denetimleri kullanabilirsiniz. İOS, Android ve evrensel Windows platformu altında çalışan program şöyledir:

[![Web çalmasına](web-videos-images/playwebvideo-small.png "Web Video Oynat")](web-videos-images/playwebvideo-large.png#lightbox "Web Video Oynat")

Elbette, telefon yandan daha büyük bir görünüm için etkinleştirebilirsiniz.

Daha karmaşık bir video oynatıcı ses denetimi, bir telefon çağrısı geldiğinde video kesmek için bir mekanizma ve ekran kayıttan yürütme sırasında etkin tutma yöntemi gibi bazı ek özellikler gerekir.

Aşağıdaki makaleleri dizi aşamalı olarak destekleyen sınıfları ve platform Oluşturucu nasıl oluşturulur gösterir:

## <a name="creating-the-platform-video-playersplayer-creationmd"></a>[Platform video oynatıcı oluşturma](player-creation.md)

Her platform gerektiren bir `VideoPlayerRenderer` oluşturur ve platformu tarafından desteklenen bir video oynatıcı denetimi bulundurur sınıfı. Bu makalede Oluşturucu yapısını sınıfları ve oyuncu nasıl oluşturulacağını gösterir.

## <a name="playing-a-web-videoweb-videosmd"></a>[Bir Web video oynatma](web-videos.md)

Büyük olasılıkla en yaygın bir video oynatıcı için videolar Internet kaynağıdır. Bu makalede, nasıl bir Web video kullanılabilir başvurulan ve video oynatıcı için bir kaynak olarak kullanılıyorsa açıklanmaktadır.

## <a name="binding-video-sources-to-the-playersource-bindingsmd"></a>[Player video kaynaklarına bağlama](source-bindings.md)

Bu makalede kullanan bir `ListView` yürütmek için videolar koleksiyonu sunmak için. Bir program, arka plan kod dosyasına video oynatıcı video kaynağının nasıl ayarlayacağınızı gösterir, ancak ikinci bir program arasındaki bağlama verileri nasıl kullanabileceğinizi gösterir `ListView` ve video oynatıcı.

## <a name="loading-application-resource-videosloading-resourcesmd"></a>[Uygulama kaynak videoları yükleniyor](loading-resources.md)

Videolar kaynaklar olarak platformu projelerin katıştırılabilir. Bu makalede, bu kaynakları depolamak ve daha sonra tarafından video oynatıcı çalınacak programa yük gösterilmektedir.

## <a name="accessing-the-devices-video-libraryaccessing-librarymd"></a>[Cihazın video kitaplığı erişimi](accessing-library.md)

Bir video cihazın kamera kullanılarak oluşturulduğunda, video dosyası cihazın Resim Kitaplığı'nda depolanır. Bu makalede, cihazın Resmi Seçici video seçin ve ardından yürütmek video oynatıcı kullanarak erişmek gösterilmiştir.

## <a name="custom-video-transport-controlscustom-transportmd"></a>[Özel görüntü Aktarım denetimleri](custom-transport.md)

Video oynatıcı her platformda kendi Aktarım denetimleri için düğmeler biçiminde sunmasına karşın **Yürüt** ve **duraklatma**, bu düğmeleri görüntülenmesine ve kendi sağlayın. Bu makale size nasıl gösterir.

## <a name="custom-video-positioningcustom-positioningmd"></a>[Özel görüntü konumlandırma](custom-positioning.md)

Her platform video oynatıcı video ilerlemesini gösterir ve İleri veya geri belirli bir konuma atlamanızı sağlar bir konum çubuğu vardır. Bu makalede, bu konum çubuğu sahip özel bir denetim nasıl değiştirebileceğiniz gösterilmektedir.





## <a name="related-links"></a>İlgili bağlantılar

- [Video oynatıcı gösterilerini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
