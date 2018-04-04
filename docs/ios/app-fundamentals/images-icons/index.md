---
title: Görüntüleri ve simgeler
description: Bu bölümde simgelerle kullanarak gibi bir Xamarin.iOS uygulaması görüntülerle çalışma kapak, ekranlar başlatma makalelerin veya bunları dahil olmak üzere çeşitli denetimleri ve özel belge türleri için simgeleri sağlama içerir.
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fd191c898d5bb015d2d394d42db1049bb0128fb7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="images-and-icons"></a>Görüntüleri ve simgeler

_Bu bölümde simgelerle kullanarak gibi bir Xamarin.iOS uygulaması görüntülerle çalışma kapak, ekranlar başlatma makalelerin veya bunları dahil olmak üzere çeşitli denetimleri ve özel belge türleri için simgeleri sağlama içerir._

Varlıklar bir iOS uygulaması içinde kullanılan, görüntü birkaç yolu vardır. Yalnızca bir kullanıcı Arabirimi denetim gibi atama görüntünün bir uygulamanın kullanıcı Arabiriminde, bir parçası olarak görüntülenmesini bir `UIButton` veya `UIImageView`, simgeler ve başlatma ekranlar sağlamayı Xamarin.iOS harika resmi aşağıdaki yollarla bir iOS uygulaması eklemek kolaylaştırır: 

- **Çözümleme bağımsız görüntüleri** – iOS'ın yerleşik destek farklı cihaz çözümler ve türleri (iPhone, iPad, vb.) görüntülerle çalışma için kullanın.
- **Varlık Kataloğu görüntü kümeleri** -kullanım **varlık Kataloğu görüntü kümeleri** yönetmek ve tüm gerekli bir uygulama tarafından verilen görüntü varlık sürümü grup.
- **İOS Tasarımcı'da görüntü** -iOS Tasarımcısı denetimleri için görüntüleri ayarlamak için kullanın.
- **Kod görüntülerinde** – kullanım `UIImage` yük ve görüntü varlıklarla çalışmaya ve C# kodunda UI denetimleri atamanız sınıfının yöntemleri.
- **Uygulama simgesi** -uygulama simgesi her iOS uygulamanız tarafından gerekli tanımlayın. Bu kullanıcı dokunun simgedir ekranından uygulamayı başlatmak için IOS home. Ayrıca, bu simge varsa oyun Merkezi tarafından kullanılır.
- **Sahne Işığı simgesi** -uygulamanın Spotlight simgesi tanımlayın. Spotlight aramasının içinde bir uygulama adı kullanıcının girdiği zaman bu simgeyi görüntülenir.
- **Ayarlar simgesine** -uygulamanın tanımlamak **ayarları** simgesi. Kullanıcı girerse **ayarları** uygulama iOS cihazında, bu simge, uygulama ayarları listesi sonunda görüntülenir. 
- **Ekranlar başlatma** -uygulamanın başlatma ekranı tanımlayın. Kullanıcı uygulama simgesini dokunur sonra ve ilk görünüm görünmeden önce boş bir ekran gösterilir. Neyse ki, iOS film şeridi kullanarak boş ekran yerine bir görüntü görüntüleme için destek içerir. 
- **iTunes simgesi** -iTune simge sağlayın. Bir uygulama (veya kurumsal kullanıcılar için beta gerçek cihazlarda test etmek için) teslim etmek için geçici yöntemini kullanıyorsanız, geliştirici de 512 x 512 ve iTunes uygulamada temsil etmek için kullanılan bir 1024 x 1024 görüntüsü içermesi gerekir.
- **Belge simgeleri** -bir görüntü, bir Xamarin.iOS uygulaması oluşturur veya destekler herhangi belirli bir belge türü için bir simge olarak kullanın.

Bu varlıkları nerede kullanılacak çeşitli yerlerde yanı sıra, bir iOS uygulaması için görüntü varlıkların oluştururken dikkate alınması gereken bazı noktalar vardır. Bunların her biri yalnızca kaç görüntü varlıklarının gerekli olacaktır, ancak bu varlıkları nasıl oluşturulduğunu üzerinde bir etkisi vardır. Aşağıdaki konular gerekli olacak görüntüleri varlıklar, bu varlıkları uygulamanın pakette nasıl bulunur ve görüntü varlıklarının gerekli işlevselliği sağlamak için nasıl tüketilen türlerini kapsar:


## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[Bir Görüntüyü Görüntüleme](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

Bu makalede, bir Xamarin.iOS uygulaması ve C# kodu kullanarak veya iOS Tasarımcısı denetiminde atama, görüntü görüntüleme bir görüntü varlığı dahil olmak üzere yer almaktadır.

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[Uygulama Simgeleri](~/ios/app-fundamentals/images-icons/app-icons.md)

Bu makale, bir uygulama simgesi olarak kullanılacak bir görüntü varlığı bir Xamarin.iOS uygulaması yönetme ve dahil kapsar.

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[Alternatif Uygulama Simgeleri](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple bazı geliştirmeler simgesini yönetmek bir uygulama sağlayan iOS 10.3 ekledi:

 - `ApplicationIconBadgeNumber` -Alır veya uygulama simgesini rozet Springboard ayarlar.
 - `SupportsAlternateIcons` -İf `true` simgeleri alternatif bir dizi uygulama vardır.
 - `AlternateIconName` -Şu anda seçili alternatif simgenin adını döndürür veya `null` birincil simgesi kullanıyorsanız.
 - `SetAlternameIconName` -Uygulamanın simgesi verilen alternatif simgesi geçiş yapmak için bu yöntemi kullanın.


## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[Başlatma Ekranları](~/ios/app-fundamentals/images-icons/launch-screens.md)

Bu makalede her iOS aygıtı boyutu ve çözümleme için bir evrensel başlatma ekranı sağlamak üzere özel türde bir film şeridi kullanmayı ele alır.

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[Özel Belge Türleri](~/ios/app-fundamentals/images-icons/custom-document-types.md)

Bu makale, bir özel belge türü simgesi olarak kullanılacak bir görüntü varlığı bir Xamarin.iOS uygulaması yönetme ve dahil kapsar.



## <a name="related-links"></a>İlgili bağlantılar

- [Görüntüleri (örnek) ile çalışma](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Özel bir simge ve görüntü oluşturma yönergeleri](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
