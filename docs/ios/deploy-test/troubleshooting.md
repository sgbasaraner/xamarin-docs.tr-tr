---
title: Sorun giderme
description: "İpuçları ve püf noktaları kesintisiz dağıtımı oluşturmak için"
ms.topic: article
ms.prod: xamarin
ms.assetid: 65286D09-F74D-4F22-B6CD-D1BCD7FC7992
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: 174d1cf974c39420b932d494d5b28c62d7fd1eb1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="troubleshooting"></a>Sorun giderme

## <a name="code-signing--provisioning"></a>Kod imzalama & sağlama

Kod imzalama & iOS hazırlama oldukça garip olabilir ve bu nedenle kod imzalama sertifikası ve sağlama profilleri sırada olduğundan emin olmak önemlidir.

* Büyük ekiplere burada gösterilen Xcode'da "sorunu düzeltin" düğmesini kullanarak ayarladıysa:

    [![](troubleshooting-images/fixissue.png "Sorunları gidermek iletişim kutusu")](troubleshooting-images/fixissue.png#lightbox)

    Bu, yeni sağlama profilleri ve sertifikaları oluşturur. En iyi bir ekip üyesine, Profiller disorganization neden tıklar her zaman bu bir sağlama profili oluşturur. En bu sertifikaları uygulamalarını çalışmayı durdurmasına neden şirketteki başka herkes için İptal.

* Canlı Anahtarlık erişimi düzenlenir ve süresi dolan sertifikaları ve profillerini silin. Son üç başkalarının yalnızca bir yılın son sırada yıl için kuruluş sertifikaları. Yalnızca eskilerle sona ermeden önce yeni sertifikalar oluşturmak için gerekli olacak sertifikaları yenilenemez. İptal etme ve eski sertifikaları silmek emin olun ve yeni sertifikalar uygulamalarla yeniden imzalayın.

* Yenilerini yüklü olarak eski sağlama profilleri kaldırın. Bu, Mac için Visual Studio kullanmak için hangi profilin karar vermek için bulunduğu bir konumda olmadığı anlamına gelir. Bunun için önce Apple developer Center'da profilini silmek istediğinizden emin olun ve sonra gidin *Tercihler > hesabınız > ayrıntıları görüntüle...* . Sağlama profili seçin ve tıklatın **Göster Finder**. Mac dosya sisteminde, bu profilin konumunu gösterecek Bulucu kullanılarak sonra silinebilir burada.

* Tüm gerekli sertifikaları ve ilgili özel anahtarları kullanılabilir olduğundan emin olun. Her ekip için bir geliştirici sertifikası (kendi cihazda uygulama yüklemek için) ve (diğer cihazlara yüklemek için) bir dağıtım sertifika gerekir

* Yeni bir sağlama profili veya sertifika yüklendiğinde, Xcode ve Visual Studio for Mac / Visual Studio yeniden başlatın.


## <a name="testflight"></a>TestFlight

Bazı durumlarda, test planlanan sorunsuz olarak oldukça gideceği anlamına gelmez.  Aşağıdaki adımlar TestFlight ile çözümleme sorunları yardımcı olabilir:

- TestFlight, yalnızca yukarıda ve iOS 8 hedefleyen uygulamalar için kullanılabilir.

- Olmalıdır bir *App Store dağıtım profili* beta yetkilendirme ile.

- **Yeni iOS uygulaması gönderme** penceresi, uygulamanın tam olarak aynı bilgileri içermelidir **Info.plist**, ve tüm bölümleri doldurulması gerekir. Simgeler TestFlight için karşıya yükleme öncesinde uygulama için belirtilmelidir.

- Yeni bir yapı karşıya yüklenirken yapı iTunes Bağlan görünene kadar 1 ile 5 dakika arasında bir yerde olur.

- [TestFlight Beta test anahtar](~/ios/deploy-test/testflight.md#beta-testing) uygulamanızı her sürümü için açık olması gerekir.

- Ayrıca bir iç tester Geliştirici ekip her bir üyesi olmalıdır **iç Tester** anahtar açık.

- Ait veya başka bir iTunes Bağlan hesabı sahibi kullanıcılar iç sınayıcılar olamaz. Bunlar yalnızca dış sınayıcılar eklenebilir.

- Dahili ve harici kullanıcılar seçili, eklenen ve ayrı olarak davet. Ayrıca yönetilen her bir listesi olmalıdır.

- Apple dış edenlere dağıtılacak her yapı onaylamanız gerekir. Derleme sürümü değişirse, Apple tarafından yeni bir beta İnceleme gereklidir. Yapı numarası değişirse gözden isteğe bağlıdır.

- Meta veriler için dış sınayıcılar dağıtılmış derlemeleri eklenmesi gerekir. Bu yapı numarası tıklayarak erişilebilir **My uygulamaları > yayın öncesi**.

- Her gün gözden geçirilmek üzere yalnızca iki derlemeleri gönderilebilir. Sürüm değiştiğinde bir gözden geçirme zorlar olduğundan, bu sürüm numaraları yalnızca günde iki kez değiştirilebilir olduğunu anlamına gelir.

<a name="Automatically_copy_app_bundles_back_to_Windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Windows geri .app paketleri otomatik olarak kopyalama

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]
