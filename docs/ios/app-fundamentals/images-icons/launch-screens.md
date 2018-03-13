---
title: "Ekranlar başlatma"
description: "Bu makalede, tüm iOS cihazları için uygulama başlatma ekranı herhangi çözümleme ve yönü, tek bir birleşik film şeridi kullanarak oluşturun açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/19/2018
ms.openlocfilehash: 54ec41636f491708ea72585d3889fbbca85c8eb1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="launch-screens"></a>Ekranlar başlatma

_Bu makalede, tüm iOS cihazları için uygulama başlatma ekranı herhangi çözümleme ve yönü, tek bir birleşik film şeridi kullanarak oluşturun açıklanmaktadır._

İOS 8 önce bir iOS uygulaması için bir başlatma ekranı oluşturma geliştirici bir görüntü varlığı her çeşitli aygıt form faktörleri ve uygulama çalıştırma çözümleri sağlamak gereklidir. İOS 8 yayımlandıktan sonra ancak bu her durumda doğru görünen başlatma ekran oluşturmak için tek bir birleşik film şeridi kullanmak mümkün olmuştur.

Bu kısa izlenecek başlatma ekranı el ile varolan bir projeye eklenmiş film şeridi veya varsayılan olarak yeni bir proje sağlanan ya da bir görsel taslak haline getirme ile oluşturmayı açıklar. Ardından, iOS Tasarımcısı film şeridi, bu görünümler kısıtlamalar ayarlayın ve film şeridi çeşitli cihazlar ve yönler için doğru görünüyorsa doğrulamak için bir resim görünümü ve bir etiket eklemek için nasıl kullanılacağını gösterir.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>Film şeritleri ile başlatma ekranları yönetme

İOS 8 (ve üstü), geliştirici özel birleşik bir veya daha fazla statik başlatma görüntüleri kullanmak yerine başlatma ekranı sağlamak için film şeridi oluşturabilirsiniz. Başlatma film şeridi iOS Tasarımcısı oluştururken boyutu sınıfları ve Otomatik Yerleşim farklı görüntü ortamlar için farklı düzenleri tanımlamak için kullanın. Geliştirici, boyutu sınıfları ve otomatik düzenini kullanarak, tüm cihazlarda iyi benzeyen bir tek başlatma ekranı oluşturun ve ortamlar görüntüleyin.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'da seçerek yeni bir proje oluşturun **Dosya > Yeni bir çözüm** ve ardından **Single View uygulaması**: 

    ![Yeni Proje penceresinin seçili tek görünüm uygulaması ile](launch-screens-images/launch01.png)

    - Varsayılan olarak, yeni bir proje içeren bir **LaunchScreen.storyboard** başlatma ekranı arabirimi tanımlayan dosyası. 
    - Bunun yerine var olan bir projeye başlatma ekranı film şeridi eklemek için proje adına sağ tıklayın **çözüm paneli** ve **Ekle > yeni dosya...**  ve ardından **başlatma ekranı**:

    ![Yeni dosya penceresiyle iOS seçili ekran başlatma](launch-screens-images/launch01b.png)

    - Dosya adı **LaunchScreen** veya seçtiğiniz başka bir ad.

2. Uygun film şeridi, başlatma ekranı olarak kullanmak için projeyi yapılandırın:

    - Çift **Info.plist** dosyasını **çözüm paneli** düzenlemek için açın.
    - İçinde **başlatma görüntüleri** bölümünde, olduğundan emin olun **başlatma ekranı** uygun film şeridi adına ayarlayın:

    ![Info.plist başlatma ekranı seçicide](launch-screens-images/launch02.png)

    - Varsayılan olarak, yeni bir proje kullanacak şekilde yapılandırılmış **LaunchScreen.storyboard** başlatma, ekranı olarak.

3. Görüntüye ekleme **Assets.xcassets** varlık katalog böylece Başlat ekranında kullanmak için kullanılabilir. Daha fazla bilgi için bkz: [ekleme görüntülere bir varlık Kataloğu resmi ayarlama](~/ios/app-fundamentals/images-icons/displaying-an-image.md) bölümünü [görüntü görüntüleme](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Kılavuzu.

4. Açık **LaunchScreen.storyboard** çift tıklatarak düzenlemek için **çözüm paneli**.

5. Bir cihaz ve başlatma ekranı film şeridi iOS Tasarımcısı Önizleme bağlanacağı yönünü seçin. Alt kısımdaki araç çubuğunda aygıt Seçim Bölmesi'ni açın ve seçin **iPhone 4S** ve **dikey**.

    ![Aygıt seçimi araç çubuğu](launch-screens-images/launch05.png)

    - Bir cihaz seçip yönü yalnızca nasıl tasarım iOS Tasarımcısı Önizleme değiştiğine dikkat edin. Burada yaptığınız seçimi bağımsız olarak, kısıtlamalar uygulanır tüm cihazlar ve yönler sürece yeni eklenen **Düzenle nitelikler** düğmesi kullanıldığından aksi belirtmek için. 

6. Ayarlama **arka plan** görünüm denetleyicisinin ana görünümü rengi. Görünüm denetleyicisini ortasında tıklayarak görünümünü seçin ve kullanarak arka plan rengini ayarlama **özellikleri paneli**:

    ![Tek bir görünüm mor arka plan rengiyle](launch-screens-images/launch06.png)

7. Ekleme bir **resim görünümü** Başlat ekranına ve kaynağı ayarla **görüntü**:

    - Sürükleme bir **resim görünümü** gelen **araç paneli** görünümün Merkezi.
    - İle **resim görünümü** , buna seçili **pencere öğesi** bölümünü **özellikleri paneli** ayarlamak **görüntü** resmi ayarlama zaten özelliğine eklenen **Assets.xcassets** varlık Kataloğu. Yeniden konumlandırma ve boyutu **resim görünümü** gerektiği gibi:
    
    ![Kendi görüntü özellik kümesine sahip bir resim görünümü](launch-screens-images/launch07.png)

8. Ekleme bir **etiket** aşağıda **resim görünümü** ve **özellikleri paneli** özniteliklerini ayarlamak için: 

    ![Metin ve renk kümesi etiketle](launch-screens-images/launch08.png)

9. Sağ taraftaki düğmesini kullanarak kısıtlaması düzenleme moduna geç **kısıtlamaları araç**:
    
    ![Kısıtlama düzenleme moduna düğmesi](launch-screens-images/launch09.png)

10. Kısıtlama eklemek **resim görünümü**, yüksekliğini ve genişliğini ayarlama ve yatay ve dikey olarak ortalama:

    ![Düzen kısıtlamaları olan bir resim görünümü](launch-screens-images/launch10.png)

    - Kısıtlamaları ekleme hakkında daha fazla ayrıntı için bkz: [iOS için Xamarin Tasarımcısı otomatik düzeniyle](~/ios/user-interface/designer/designer-auto-layout.md).

11. Kısıtlama eklemek **etiket**yatay ortalama, yüksekliğini ve genişliğini vermek ve bir sabit konumlandırma gelen dikey uzaklığı **resim görünümü**:

    ![Düzen kısıtlamaları olan bir etiketi](launch-screens-images/launch11.png)

12. Test diğer aygıtlar ve yönler tasarım tüm senaryolarda beklendiği gibi göründüğünü doğrulayın. Burada ayarlamalar gereken belirli bir cihaza veya yönlendirme için yapılması durumlarda kullanmanızı **Düzenle nitelikler** constaints belirli boyutu sınıfları için Ekle düğmesini:

    ![Yatay yönlendirme kullanarak X iPhone olarak çizilir başlatma ekranı](launch-screens-images/launch12.png)

13. Film şeridi için değişiklikleri kaydedin. Bir simulator veya aygıt uygulamayı çalıştırın ve uygulama başlatma gibi başlatma ekranı görünür olur.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Yeni bir proje oluşturun. Visual Studio'da seçin **Dosya > Yeni > Proje**ve ardından **Single View uygulaması (iPhone)**:
    
    ![Seçili yeni proje penceresinin Single View uygulaması (iPhone)](launch-screens-images/launch01-vs.png)

    - Proje adı, bir konum seçin ve Seç **Tamam**.

2. Varsa **Kaynakları > LaunchScreen.xib** bulunmaktadır **Çözüm Gezgini**, dosya üzerinde sağ tıklayıp seçerek silme **silmek**. Bu dosya, bir sonraki adımda film şeridi ile değiştirilecek.

3. Başlatma ekranı kullanmak için bir film şeridi oluşturun. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle > Yeni öğe...**  arkasından **boş film şeridi**. Bu film şeridi ad **LaunchScreen.storyboard** tıklatıp **Ekle**:

    ![Yeni Öğe Ekle penceresiyle boş seçili film şeridi](launch-screens-images/launch03-vs.png)

4. Kullanmak üzere proje yapılandırma **LaunchScreen.storyboard** başlatma ekranı şeridini olarak:

    - Çift **Info.plist** dosyasını **Çözüm Gezgini** düzenlemek için açın. 
    - Üzerinde **görsel varlıklar** sekmesinde, ayarlamak **başlatma ekranı** için **LaunchScreen**.

    ![Info.plist başlatma ekranı seçicide](launch-screens-images/launch04-vs.png)

5. Böylece Başlat ekranında kullanmak için kullanılabilir görüntü proje varlık kataloğunda ekleyin:

    - İçinde **Çözüm Gezgini**, sağ tıklayın **varlık kataloglar** seçip **Add varlık Catalog**. Bu yeni varlık katalog adı **varlıklar**:

    ![Yeni Öğe Ekle penceresiyle seçili varlık Kataloğu](launch-screens-images/launch05-vs.png)

    - Yeni bir ayarlayın görüntü ekleme **varlıklar** varlık açıklandığı gibi katalog [ekleme görüntülere bir varlık Kataloğu resmi ayarlama](~/ios/app-fundamentals/images-icons/displaying-an-image.md) bölümünü [görüntü görüntüleme](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Kılavuzu.

6. Açık **LaunchScreen.storyboard** çift tıklatarak düzenlemek için **Çözüm Gezgini**.

    - Film şeridi dosya düzenlemek için Visual Studio Mac yapı barındırmak için etkin bir bağlantı gerekiyor. Bkz: [Mac bilgisayara bağlayarak](~/ios/get-started/installation/windows/connecting-to-mac/index.md) ayrıntılı bilgi için Kılavuzu.

7. Bir cihaz ve başlatma ekranı film şeridi iOS Tasarımcısı Önizleme bağlanacağı yönünü seçin. Alt kısımdaki araç çubuğunda aygıt Seçim Bölmesi'ni açın ve seçin **iPhone 4S** ve **dikey**: 
 
    ![Aygıt seçimi araç çubuğu](launch-screens-images/launch07-vs.png)

    - Bir cihaz seçip yönü yalnızca nasıl tasarım iOS Tasarımcısı Önizleme değiştiğine dikkat edin. Burada yaptığınız seçimi bağımsız olarak, kısıtlamalar uygulanır tüm cihazlar ve yönler sürece yeni eklenen **Düzenle nitelikler** düğmesi kullanıldığından aksi belirtmek için. 

8. Ekleme bir **View Controller** birinden sürükleyerek film şeridi için **araç** tasarım yüzeyine: 

    ![Boş bir View Controller tasarım yüzeyine eklenir.](launch-screens-images/launch08-vs.png)

9. Ayarlama **arka plan** görünüm denetleyicisinin ana görünümü rengi. Görünüm denetleyicisini ortasında tıklayarak görünümünü seçin ve kullanarak arka plan rengini ayarlama **Özellikler penceresini**:
    
    ![Tek bir görünüm mor arka plan rengiyle](launch-screens-images/launch09-vs.png)

10. Ekleme bir **resim görünümü** Başlat ekranına ve kaynağı ayarla **görüntü**:

    - Sürükleme bir **resim görünümü** gelen **araç** görünümün Merkezi.
    - İle **resim görünümü** halen seçiliyken, buna **pencere öğesi** bölümünü **Özellikler penceresini** ayarlamak **görüntü** özelliği resmi ayarlamak için zaten eklendi **varlıklar** varlık Kataloğu. Yeniden konumlandırma ve boyutu **resim görünümü** gerektiği gibi:
    
    ![Kendi görüntü özellik kümesine sahip bir resim görünümü](launch-screens-images/launch10-vs.png)

11. Ekleme bir **etiket** aşağıda **görüntü Görünüm**:

    - Sürükleme bir **etiket** gelen **araç** tasarım yüzeyine aşağıdaki yerleştirme **görüntü Görünüm**.
    - Ayarlamak için öznitelikler **etiket** kullanarak **Özellikler penceresini**:

    ![Metin ve renk kümesi etiketle](launch-screens-images/launch11-vs.png) 

12. Sağ taraftaki düğmesini kullanarak kısıtlaması düzenleme moduna geç **kısıtlamaları araç**:
    
    ![Kısıtlama düzenleme moduna düğmesi](launch-screens-images/launch12-vs.png) 

13. Kısıtlama eklemek **resim görünümü**, yüksekliğini ve genişliğini ayarlama ve yatay ve dikey olarak ortalama:

    ![Düzen kısıtlamaları olan bir resim görünümü](launch-screens-images/launch13-vs.png) 

    - Kısıtlamaları ekleme hakkında daha fazla bilgi için bkz: [iOS için Xamarin Tasarımcısı otomatik düzeniyle](~/ios/user-interface/designer/designer-auto-layout.md).

14. Kısıtlama eklemek **etiket**yatay ortalama, yüksekliğini ve genişliğini vermek ve bir sabit konumlandırma gelen dikey uzaklığı **resim görünümü**:
    
    ![Düzen kısıtlamaları olan bir etiketi](launch-screens-images/launch14-vs.png) 

15. Test diğer aygıtlar ve yönler tasarım tüm senaryolarda beklendiği gibi göründüğünü doğrulayın. Burada ayarlamalar gereken belirli bir cihaza veya yönlendirme için yapılması durumlarda kullanmanızı **Düzenle nitelikler** constaints belirli boyutu sınıfları için Ekle düğmesini:

    ![Yatay yönlendirme kullanarak X iPhone olarak çizilir başlatma ekranı](launch-screens-images/launch15-vs.png) 

16. Film şeridi için değişiklikleri kaydedin. Bir simulator veya aygıt uygulamayı çalıştırın ve uygulama başlatma gibi başlatma ekranı görünür olur.

-----

> [!NOTE]
> **Not**: A Başlat ekranında kullanılan film şeridi _gerekir_ yalnızca basit, yerleşik kullanıcı Arabirimi öğeleri içerir ve **olamaz** tüm hesaplamalar yapabilir veya özel bir sınıftan türetilen.

Birleşik bir film şeridi ile başlatma ekranı oluşturma hakkında daha fazla bilgi için lütfen bkz [dinamik başlatma ekranlar](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) bölümünü [birleşik film şeritleri](~/ios/user-interface/storyboards/unified-storyboards.md) Kılavuzu.

## <a name="migrating-to-launch-screen-storyboards"></a>Ekran film şeritleri başlatmak için geçirme

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Film şeritleri kendi başlatma ekranları için kullanılacak var olan bir uygulama güncelleştirilirken sağ tıklayın **proje adı** içinde **Çözüm Gezgini** seçip **Ekle**  >  **Yeni dosya...** . Seçin **iOS** > **başlatma ekranı** tıklatıp **yeni** düğmesi:

![](launch-screens-images/storyboard02.png "Başlatma ekranı iOS seçin")

Ardından, çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın. Altında **başlatma ekranı**, yukarıda oluşturduğunuz yeni bir film şeridi dosya seçin.

![](launch-screens-images/storyboard09.png "Yukarıda oluşturduğunuz yeni film şeridi dosyasını seçin")


Yeni film şeridi başlatma ekranı kullanmak için aşağıdakileri yapın:

1. Çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
2. Kaydırma **Evrensel başlatma görüntüleri** açık Düzenleyicisi bölümünü **başlatma ekranı** film şeridi adını oluşturulan açılır ve select yukarıda: 

    ![](launch-screens-images/storyboard08.png "Başlatma ekranı film şeridi için ayarlama")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Proje adına sağ tıklayın **Çözüm Gezgini** seçip **Ekle** > **yeni dosya...** : 

    ![](launch-screens-images/image012.png "Yeni dosya ekleme")
2. Başlatma ekranı için bir ad girin ve tıklayın **Ekle** düğmesi: 

    ![](launch-screens-images/image013.png "Başlatma ekranı için bir ad girin")
3. İçinde **Çözüm Gezgini**, yeni oluşturulan film şeridi dosya düzenlemek üzere açmak için çift tıklayın.
4. Emin **boyutu sınıfı** ayarlanır **herhangi: tüm** ve **görünüm olarak** olan **genel**: 

    ![](launch-screens-images/image016.png "Boyutu sınıfı için ayarlandığından emin olun: tüm ve görünüm olarak genel")
5. Derleme başlatma ekranı boyutu sınıflardan basit kullanıcı Arabirimi öğeleri (gibi `UIImageView`) ve uygulamanın pakete eklenen görüntüler: 

    ![](launch-screens-images/image017.png "Derleme Tasarımcısı iOS başlatma ekranı")
6. Film şeridi için değişiklikleri kaydedin.

-----

## <a name="related-links"></a>İlgili bağlantılar

- [Dinamik başlatma ekranlar (örnek)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Birleşik Görsel Taslaklar](~/ios/user-interface/storyboards/unified-storyboards.md)
- [Temel iOS Designer Bilgileri](~/ios/user-interface/designer/index.md)
- [Bir varlık Kataloğu görüntüsüne ekleme görüntüleri ayarlama](~/ios/app-fundamentals/images-icons/displaying-an-image.md#asset-catalogs)
- [İOS için Xamarin Tasarımcısı ile otomatik Düzenle](~/ios/user-interface/designer/designer-auto-layout.md)
- [İnsan Arabirimi yönergelerine: Başlatma ekranı](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
