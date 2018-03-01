---
title: "Canlı uygulamaları inceleniyor"
description: "Görselleştirme ve canlı uygulamanızın hatalarını ayıklama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: bd6d47f98435cc68ecf4156423526c31dbac09da
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="inspecting-live-applications"></a>Canlı uygulamaları inceleniyor

Dinamik uygulama İnceleme Kurumsal müşteriler için kullanılabilir.


1. [Xamarin çalışma kitaplarını & Denetçisi yükleyin.](~/tools/inspector/install.md)

1. Tüm açık [uygulama projesi desteklenen](~/tools/inspector/install.md#supported-platforms) Mac veya Visual Studio için Visual Studio.
1. Uygulamanızı hata ayıklama modunda çalıştırın.
1. Tıklatın **incele** IDE araç çubuğu düğmesini (Visual Studio'da **geçerli uygulamayı inceleyin...**  menü öğesi edinilebilir de **Araçları** veya **hata ayıklama** menüsü).



[ ![](inspect-images/mac-heres-the-button.png "IDE araç çubuğunda denetle düğmesine tıklayın")](inspect-images/mac-heres-the-button.png)

Yeni bir REPL istemiyle yeni bir Xamarin denetçisi istemci penceresi açılır.

[ ![](inspect-images/inspector-0.7.0-map-inspect-small.png "Yeni bir Xamarin denetçisi istemci penceresi açılacaktır, yeni bir REPL istemiyle")](inspect-images/inspector-0.7.0-map-inspect.png)

Bu pencere göründükten sonra yürütme ve C# deyimleri ve ifadeler değerlendirmek için kullanabileceğiniz etkileşimli bir C# istemi sahip. Bu benzersiz kılan kod hedef işlem bağlamında değerlendirilir olur. Bu durumda, biz görüntülenen iOS uygulamaya karşı çalışan kodu gösteriliyor.

Uygulama durumunu yaptığınız tüm değişiklikler gerçekte hedef işlem yapılmazsa, kullanabilmek için Canlı uygulama değiştirmek için C# veya dinamik uygulama durumunu denetleyebilirsiniz.

Örneğin, iOS, biz (burada çok sayıda uygulama durumu depolarız) bizim ana sürücüsü bizim Uıapplication temsilci sınıfı bulmak isteyebilirsiniz:

    var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
    del.Database.GetAllCustomers ()
    ...
    del.Database.AddCustomer (...)

(Not: her gönderme çok satırlı bir düzenleyicide oluşur. `Shift + Enter` Yeni bir satır oluşturur ve `Cmd + Enter` (`Ctrl + Enter` Windows'da) değerlendirme kodu gönderir. `Enter` Güvenli olduğunda otomatik olarak gönderir.)

Uygulamanız için görsel öğeleri almak için daha kolay bir yol "Denetle" düğmesine kullanmaktır. Bu tuşa basın sonra uygulamanızı tıklayarak bir kullanıcı Arabirimi öğesi seçebilirsiniz. Değişkeni `selectedView` ekranında gerçek öğesine işaret edecek şekilde atanır. Yukarıdaki ekran görüntüsünde nasıl biz erişilen ve sonra düzenlenebilir görebilirsiniz `selectedView.BarTintColor` üzerinde `UISearchBar` seçtik.

Canlı görsel ağaç de faydalıdır. Bu, o anki anlık görünüm hiyerarşinizin temsil eder. Ayarlamak için satırları seçin `selectedView` REPL ve Görünüm'ün özellik değerleri görmek için. Mac üzerinde ayrılmış katmanlı görünümleri görselleştirme 3B ile etkileşim kurabilirsiniz. Windows, bir görünümün özellik değerlerini görsel olarak düzenleyebilirsiniz.

## <a name="known-limitations"></a>Bilinen sınırlamaları

 - Görünüm seçimi yalnızca ana ekranınızda desteklenir.
 - Kılavuz özellik düzenleme Mac için kullanılabilir değil ve Windows birkaç veri türleri ile sınırlıdır. REPL daha güçlü düzenlemek için kullanın.
 - Hata ayıklama modunda her başlatıldığında denetçisi eklentisi/uzantısı yüklü ve IDE içinde etkin olduğu sürece, biz kodu uygulamanıza injecting. Uygulamanızda herhangi garip davranış fark ederseniz, lütfen deneyin devre dışı bırakma veya Inspector eklentisi/uzantısını kaldırma, IDE yeniden başlatmayı ve yeniden denetleniyor. Ve lütfen [dosya hataları](~/tools/inspector/install.md#reporting-bugs) bize bildirin!
 - Bir kullanıcı Arabirimi öğesi inceleyerek, yine de değiştirmek neden olursa, lütfen [bize bildirin](~/tools/inspector/install.md#reporting-bugs)gibi bu hatayı gösteriyor olabilir.

