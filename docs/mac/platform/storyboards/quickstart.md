---
title: Film şeritleri hızlı başlangıç
description: Film şeritleri ile kullanıcı arabirimleri başlatılan yapı macOS alınıyor.
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: 0c366de844b119736763f7d419d6bc9bb3fbfc0a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="starting-a-new-storyboard-based-project"></a>Proje dayalı yeni bir film şeridi başlatılıyor

Film şeritleri Xamarin.Mac uygulamanın kullanıcı arabirimi tanımlamak için kullanmak için bir giriş, yeni bir Xamarin.Mac projesi başlayalım. Seçin **Mac** > **uygulama** > **Cocoa uygulama** tıklatıp **sonraki** düğmesi:

[![](quickstart-images/qs01.png "Yeni bir Cocoa uygulama ekleme")](quickstart-images/qs01.png#lightbox)

Kullanım **App Name** , `MacStoryboard` tıklatıp **sonraki** düğmesi:

[![](quickstart-images/qs02.png "Uygulama adı ayarlama")](quickstart-images/qs02.png#lightbox)

Varsayılan kullanmak **proje adı** ve **çözüm adı** tıklatıp **oluşturma** düğmesi:

[![](quickstart-images/qs03.png "Proje ve çözüm adları")](quickstart-images/qs03.png#lightbox)

İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosyayı Xcode'nın arabirimi Oluşturucusu'nda düzenlemek için açın:

[![](quickstart-images/qs04.png "Xcode'da film şeridi düzenleme")](quickstart-images/qs04.png#lightbox)

Yukarıda gördüğünüz gibi varsayılan film şeridi uygulamanın menü çubuğu ve ana kendi penceresiyle görünümü denetleyici ve görünüm tanımlar. Bizim örnek uygulamamız için size bir ana sahip bir kullanıcı Arabirimi oluşturmak zorunda kalacaklarını _içerik görünümü_ bir tarafında ve bir _denetçisi Görünüm_ ikinci.

Bunu yapmak için biz öncelikle varsayılan görünüm denetleyicisi kaldırmanız gerekir ve görsel taslak haline getirme ile birlikte görünümü seçin, bunu, arabirim oluşturucusu ve tuşlarına basarak **silmek** anahtarı:

[![](quickstart-images/qs05.png "Varsayılan görünüm denetleyicisini kaldırma")](quickstart-images/qs05.png#lightbox)

Sonra aşağıdakileri yazın `split` içine **filtre** alan, Dikey bölme görünüm denetleyicisini seçin ve sürüklediğinizde _tasarım yüzeyi_:

[![](quickstart-images/qs06.png "Bölme görünüm denetleyicisi aranıyor")](quickstart-images/qs06.png#lightbox)

Denetleyici görünüm denetleyicileri (ve bunların ilgili görünümler) kablolu yukarı sol ve sağ tarafa bölünmüş görünümün iki alt otomatik olarak dahil edilen dikkat edin. Bölünmüş Görünümü kendi üst penceresine bağlamanın basın **denetim** anahtar, pencere denetleyici (mavi daire penceresi denetleyicinin çerçevesinde) tıklayın ve bir satır bölme görünüm denetleyiciye sürükleyin. Seçin **pencere içeriğinin** açılan gelen:

[![](quickstart-images/qs07.png "Windows içerik görünümü ayarlama")](quickstart-images/qs07.png#lightbox)

Bu, bir Segue birlikte kullanarak iki arabirim öğe tie:

[![](quickstart-images/qs08.png "Pencerenin ve içeriği arasındaki Segue")](quickstart-images/qs08.png#lightbox)

Metin görünümü Bölünmüş Görünüm sol taraftaki yerleştirin ve sahip penceresi veya bölme görünüm boyutlandırıldığında otomatik olarak kullanılabilir alanı dolduracak istiyoruz. Metin görünümü View Controller bağlı bölme görünümüne üst sürükleyin ve tıklatın **PIN** Otomatik Yerleşim kısıtlaması (sağ alt kısmındaki tasarım yüzeyine ikinci simgesinden).

[![](quickstart-images/qs09.png "Kısıtlamalarını yapılandırma")](quickstart-images/qs09.png#lightbox)

Buradan dört öğesini **ı ışını** sınırlayıcı geçici simgeler kutusuna kısıtlamaları Popover en üstte ve tıklatın **4 kısıtlamalar eklemek** gerektirdiği kısıtlamaları eklemek için alttaki düğmesi.

Mac için Visual Studio'ya geri dönün ve projeyi çalıştırın, bölme Görünüm penceresi olarak sol tarafındaki doldurmak için metin görünümü otomatik olarak yeniden boyutlandırır veya bölme boyutlandırılır dikkat edin:

[![](quickstart-images/qs10.png "Çalışan uygulama örneği")](quickstart-images/qs10.png#lightbox)

Daha küçük bir boyuta sahiptir ve daraltılmış olabilir olanak tanımak için istiyoruz sağ taraftaki bölünmüş görünümün denetçisi alanı olarak kullanılmasını kalacaklarını beri. Dönmek için Xcode ve tasarım yüzeyine seçerek ve tıklayarak sağ tarafı için görünümü düzenleyin **boyutu denetçisi**. Buradan girin bir **genişliği** , `250`:

[![](quickstart-images/qs11.png "Genişliği ayarlama")](quickstart-images/qs11.png#lightbox)

Sonraki seçin sağ tarafında temsil eden bölünmüş öğesi ayarlamak daha yüksek **bulunduran öncelik** tıklatıp **kullanıcı yapabilirsiniz Daralt** onay kutusu:

[![](quickstart-images/qs12.png "Bekletme öncelik düzenleme")](quickstart-images/qs12.png#lightbox)

Mac için Biz Visual Studio'ya geri dönün ve projeyi Şimdi Çalıştır, sağ tarafında, tutar bildirimi daha küçük ise boyutu ve pencere boyutu:

[![](quickstart-images/qs13.png "Çalışan uygulama örneği")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>Sunu tanımlama ü

Biz düzene sağ taraftaki seçili metnin özellikleri için bir denetleyici olarak davranacak şekilde bölünmüş görünümün adımıdır. Biz bazı denetimler alt görünümü denetçisinin UI düzene sürükleyin. Son denetim için dört hazır karakter stilleri seçmesini sağlayan bir popover görüntülenecek istiyoruz.

Denetleyici ve görünüm denetleyicisine tasarım yüzeyine bir düğme ekleyeceğiz. Boyutu Görünüm Denetleyicisi'ne yeniden boyutlandırılacak olması ve dört düğme eklemek için bizim Popover istiyoruz. Sonraki biz gerekir **denetim** anahtarı-Inspector görünümünde düğmeye tıklayın ve bizim popover temsil edecek görünüm denetleyiciye sürükleyin:

[![](quickstart-images/qs14.png "Yeni bir segue oluşturmak için sürükleme")](quickstart-images/qs14.png#lightbox)

Açılır menüden biz seçersiniz **Popover**: 

[![](quickstart-images/qs15.png "Segue türü seçme")](quickstart-images/qs15.png#lightbox)

Son olarak, biz Segue tasarım yüzeyine seçip ayarlayın **tercih edilen kenar** için **sol**. Ardından, biz bir satırından sürükleyin **bağlantı görünümü** eklenmiş popover istiyoruz düğmesine:

[![](quickstart-images/qs16.png "Yeni bir segue oluşturmak için sürükleme")](quickstart-images/qs16.png#lightbox)

Visual Studio için Mac döndürürse uygulamayı çalıştırın ve tıklayın **hiçbiri** popover denetçisi düğmesi görüntülenir:

[![](quickstart-images/qs17.png "Çalışan segue örneği")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>Uygulama Tercihleri oluşturma

Çoğu standart macOS uygulamalar sağlayan bir _tercih iletişim_ görünüm veya kullanıcı hesapları gibi uygulama çeşitli yönlerini kontrol çeşitli seçenekler tanımlamak kullanıcı sağlar.

Bir standart tercih iletişim kutusu penceresinin tanımlamak için önce sekmesini View Controller tasarım yüzeyine sürükleyin:

[![](quickstart-images/qs18.png "Xcode'da film şeridi düzenleme")](quickstart-images/qs18.png#lightbox)

Yeniden, bu görünüm denetleyicileri bağlı iki alt ile otomatik olarak gelir. Örneğin artırmak amacıyla, bir etiket bunun içinde merkezi her görünümüne ekleyeceğiz:

[![](quickstart-images/qs19.png "Kısıtlamalar ayarlama")](quickstart-images/qs19.png#lightbox)

Ardından, kullanıcı seçtiğinde Tercihler penceresini görüntülemek istiyoruz **tercihleri...**  menü öğesi. Tercihler menü öğesini menü çubuğundan seçin **denetim** anahtar'ı tıklatın ve bir satır bizim sekmesini görünüm denetleyiciye sürükleyin:

[![](quickstart-images/qs20.png "Bir segue oluşturmak için sürükleme")](quickstart-images/qs20.png#lightbox)

Açılır penceresinden biz seçersiniz **kalıcı** Bu pencere kalıcı bir iletişim kutusu olarak göstermek için:

[![](quickstart-images/qs21.png "Segue türü seçme")](quickstart-images/qs21.png#lightbox)

Biz değişikliklerimizi, Visual Studio Mac dönün kaydederseniz, uygulamayı çalıştırın ve seçin **tercihleri...**  menü öğesi, bizim yeni Tercihleri iletişim kutusu görüntülenir:

[![](quickstart-images/qs22.png "Çalışan segue örneği")](quickstart-images/qs22.png#lightbox)

Bu standart macOS uygulama tercih iletişim kutusu penceresinin gibi görünmüyor fark edebilirsiniz. Bu sorunu gidermek için Xamarin.Mac uygulamanın iki görüntü dosyaları dahil `Resources` klasöründe **Çözüm Gezgini** ve Xcode'nın arabirimi oluşturucuya döndürür.

Sekme View Controller ve anahtarı seçin, **stili** için **araç**: 

[![](quickstart-images/qs23.png "Sekme çubuğu stili ayarlanıyor")](quickstart-images/qs23.png#lightbox)

Her sekmesini seçin ve bu verin bir **etiket** ve onu temsil etmek için görüntülerden birini seçin:

[![](quickstart-images/qs24.png "Her sekme Xcode'da yapılandırma")](quickstart-images/qs24.png#lightbox)

Biz değişikliklerimizi, Visual Studio Mac dönün kaydederseniz, uygulamayı çalıştırın ve seçin **tercihleri...**  menü öğesi, iletişim kutusu artık gibi standart macOS uygulamasının görüntülenir:

[![](quickstart-images/qs25.png "Çalışan Tercihler penceresi örneği")](quickstart-images/qs25.png#lightbox)

Daha fazla bilgi için lütfen bkz bizim [görüntülerle çalışma](~/mac/app-fundamentals/image.md), [menüleri](~/mac/user-interface/menu.md), [Windows](~/mac/user-interface/window.md) ve [iletişim kutularını](~/mac/user-interface/dialog.md) belgeleri.

## <a name="related-links"></a>İlgili bağlantılar

- [MacStoryboard (örnek)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows ile birlikte çalışma](~/mac/user-interface/window.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
