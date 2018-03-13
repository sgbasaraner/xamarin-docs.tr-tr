---
title: "Mac App Store için karşıya yükleme"
description: "Bu kılavuzda, Xamarin.Mac uygulama yayının Mac App Store karşıya aracılığıyla anlatılmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 30224db257af4de8dd13f362761ecd369b682dda
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="upload-to-mac-app-store"></a>Mac App Store için karşıya yükleme

_Bu kılavuzda, Xamarin.Mac uygulama yayının Mac App Store karşıya aracılığıyla anlatılmaktadır._

Uygulamaları Mac App Store onaya gönderildiğinde [iTunes Bağlan](http://itunesconnect.apple.com/).

1. Seçin bir **macOS uygulama** oluşturmak için: 

    [![](uploading-images/image65.png "iTunes Connect")](uploading-images/image65.png#lightbox)

2. Uygulamanın adını ve başka ayrıntılar girin. Geliştirici, yalnızca mevcut bir seçebilirsiniz **paket kimliği** , oluşturuldu daha önce: 

    [![](uploading-images/image66.png "Paket kimliği seçme")](uploading-images/image66.png#lightbox)

3. Fiyat ve kullanılabilirlik tarihi seçin. Bunu onaylandıktan sonra Geliştirici seçer kullanılabilirlik tarihi bağımsız olarak, uygulamayı yalnızca satış için kullanılabilir hale gelir. Geliştirici gerçek kullanılabilirlik tarihi üzerinde daha fazla denetim istiyorsa, bu değer kadar gelecekte ayarlayabilirsiniz: 

    [![](uploading-images/image67.png "Fiyat ve kullanılabilir tarihi ayarlama")](uploading-images/image67.png#lightbox)

4. In ait olduğu uygulama mağazası kategori uygulamanın bilgilerini girin: 

    [![](uploading-images/image68.png "Uygulama bilgilerini girme")](uploading-images/image68.png#lightbox) 

    Uygulama derecelendirme seçin: 

    [![](uploading-images/image69.png "Uygulama derecelendirme ayarlama")](uploading-images/image69.png#lightbox) 

    Açıklama, anahtar sözcükleri ve iletişim URL'ler: 

    [![](uploading-images/image70.png "Açıklama, anahtar sözcükleri düzenleme ve URL'leri başvurun")](uploading-images/image70.png#lightbox) 

    Kişi bilgileri ve öneriler App Store gözden geçirenler için: 

    [![](uploading-images/image71.png "Kişi bilgileri ve öneriler için App Store gözden geçirenler düzenleme")](uploading-images/image71.png#lightbox) 

    Ve son olarak, ekran görüntüleri: 

    [![](uploading-images/image72.png "Gerekli ekran görüntüleri ekleme")](uploading-images/image72.png#lightbox) 

    Ekran görüntüleri JPG, TIF veya PNG biçiminde, 1280 x 800, 1440 x 900, 2880 x 1800 veya 2560 x 1600 piksel boyutunda olmalıdır. Tuşuna **kaydetmek** tamamlamak için.

5. Uygulama bilgilerini gözden geçirme için gösterilir. Tıklatın **ayrıntıları görüntüle** durumu değiştirmek için: 

    [![](uploading-images/image73.png "Uygulama ayrıntılarını görüntüleme")](uploading-images/image73.png#lightbox)

6. Ayrıntılar Görünümü'nde uygulama paketi dosyası göndermek için ikili karşıya yüklemek için hazır tıklatın: 

    [![](uploading-images/image74.png "İkili yüklemeye hazır seçme")](uploading-images/image74.png#lightbox)

7. Şifreleme soruyu yanıtlayın: 

    [![](uploading-images/image75.png "Şifreleme soru yanıtlama")](uploading-images/image75.png#lightbox)

8. Uygulama paketi dosyası kabul etmeye hazır olduğunda site önerilerde: 

    [![](uploading-images/image76.png "Kabul bildirim")](uploading-images/image76.png#lightbox)

9. Uygulama Yükleyicisi başlatın ve Apple kimliğiniz ile oturum açmanız için emin olun
Seçin **uygulamanızı teslim** devam etmek için: 

    [![](uploading-images/image77.png "Uygulama Yükleyicisi arabirimi")](uploading-images/image77.png#lightbox)

10. Uygulamaları listeden seçin **hazır karşıya ikili** durumunu ve tıklatın **sonraki**: 

    [![](uploading-images/image78.png "Yüklemek için uygulama seçme")](uploading-images/image78.png#lightbox)

11. Uygulama meta verileri gözden geçirin ve tıklatın **seçin...**  paket dosyasını bulmak için: 

    [![](uploading-images/image79.png "Uygulama meta verileri gözden geçirme")](uploading-images/image79.png#lightbox)

12. Oluşturulan paket dosyasının Visual Studio'da Uygulama mağazası yapı yapılandırması'nı kullanarak Mac için bulun: 

    [![](uploading-images/image80.png "Karşıya yüklenecek dosyayı seçme")](uploading-images/image80.png#lightbox)

13. Tuşuna **Gönder**: 

    [![](uploading-images/image81.png "Uygulama gönderme")](uploading-images/image81.png#lightbox)

14. Paket doğrulanacak ve herhangi bir hata bildirdi. Bu hataları düzeltin ve yeniden yükleyin. Karşıya yükleme başarıyla tamamlandıktan sonra uygulamayı otomatik olarak gözden geçirme için uygulama mağazası ekibi tarafından gönderilir: 

    [![](uploading-images/image82.png "Karşıya yükleme hataları örneği")](uploading-images/image82.png#lightbox)

Uygulama onaylandığında, indirme veya Mac uygulama Mağazası'ndan satın almak için kullanılabilir olacaktır.

## <a name="related-links"></a>İlgili bağlantılar

- [Yükleme](~//mac/get-started/installation.md)
- [Merhaba, Mac örnek](~//mac/get-started/hello-mac.md)
- [Mac App Store, uygulamaları dağıtma](https://developer.apple.com/devcenter/mac/checklist/)
- [Araçlar Kılavuzu: Kod uygulamanızı imzalama](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Geliştirici kimliği ve ağ geçidi](https://developer.apple.com/resources/developer-id/)
