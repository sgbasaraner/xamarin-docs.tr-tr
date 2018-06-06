---
title: Xamarin.iOS uygulama mağazası simgeleri
description: Bu belge, bir Xamarin.iOS uygulaması için bir uygulama mağazası simge yönetmek için varlık kataloglar kullanmayı açıklar. Daha önce uygulama mağazası simgeler Bağlan iTunes ile yönetiliyordu.
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/26/2017
ms.openlocfilehash: 749dbf01af382a54fe24652706f6a605ac7b20b4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783616"
---
# <a name="app-store-icons-in-xamarinios"></a>Xamarin.iOS uygulama mağazası simgeleri

Xcode 9 önce tüm App Store simgeleri Bağlan iTunes eklendi. Ancak, bu artık bir durumdur. Uygulama mağazası simgeler şimdi gerekir proje paketin bir parçası olarak dahil edilen ve bir varlık Kataloğu içinde eklendi. Uygulama mağazası simge içermeyen uygulamalar Apple tarafından reddedilir.

Uygulama mağazası uygulamanızı etkileyici olması gerekir böylece kullanıcılar ve görüntü küçük bir boyuta en iyi yüz simgedir. Etkileyici simgeler temiz, basit ve hemen tanımlanabilir.

Apple uygulamaları simge tasarlarken aşağıdaki yönergeleri önerir:

- Simge, uygulamanız için uygun olun.
- Uygulamanızı tasarım ile tutarlı olan basit bir simge oluşturun.
- Sözcükler, simgesini kullanmaktan kaçının.
- Genel olarak düşünün: bir tek uygulama simgesi tüm deposu bölgeler kullanılır.

1024 x 1024 piksel görüntü uygulama Mağazası'nda görüntülenen uygulama simgesi için gereklidir.  Apple varlık kataloğunda uygulama mağazası simgesini saydam olamaz veya bir alfa kanal içeren belirtilmiş.

Daha fazla bilgi için Apple'nın bkz [iOS İnsan Arabirimi yönergelerine](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/).

## <a name="adding-an-app-store-icon"></a>Uygulama mağazası simge ekleme

Uygulama mağazasına simgeleri şimdi bir varlık Kataloğu tarafından teslim. 

Eklemek için uygulama mağazası simge aşağıdakileri yapın:

1. Bulun **AppIcon** görüntü kümesinde **Assets.xcassets** projenizin dosya. 
    - Tüm yeni projeler ile alınması gereken bir bir **Assets.xcassets** AppIcon görüntü kümesini içeren dosya.
    - Yeni bir varlık katalog eklemek için projeyi sağ tıklatın ve **Ekle > Yeni Dosya > varlık Kataloğu**.
    - Yeni bir uygulama simge görüntüsü kümesi eklemek için simgesi kümesi alanı seçin sağ tıklayın ve **uygulama simgeleri & başlatma resimleri > Yeni uygulama simgesi**:
    
    ![Yeni bir görüntü kümesi seçeneği Ekle](app-store-icon-images/image1.png)

2. Kaydırma **App Store** listedeki simgeleri:

    ![Uygulama mağazası simgesi](app-store-icon-images/image2.png)

3. Simgesine tıklayın ve 1024 x 1024 piksel görüntünüzü için göz atın. Varlık Kataloğu kaydedin.




## <a name="related-links"></a>İlgili bağlantılar

- [Varlık kataloglar simgelerle yönetme](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
