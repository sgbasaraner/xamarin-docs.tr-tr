---
title: Xamarin.iOS uygulamaları için başlatma ekranları
description: Bu makalede, tüm çözüm ve yönü, tek bir birleşik görsel taslak kullanarak, tüm iOS cihazları için uygulama başlatma ekranı oluşturun açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2018
ms.openlocfilehash: 40b8c38e89e96223bbf657ff06356d9fb2e9d9b3
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780566"
---
# <a name="launch-screens-for-xamarinios-apps"></a>Xamarin.iOS uygulamaları için başlatma ekranları

_Bu makalede, tüm çözüm ve yönü, tek bir birleşik görsel taslak kullanarak, tüm iOS cihazları için uygulama başlatma ekranı oluşturun açıklanmaktadır._

İOS 8 önce başlatma ekran için bir iOS uygulaması oluşturma, görüntü varlık her çeşitli cihaz büyüklükleri ve uygulamayı çalıştırarak çözümleri sağlamak Geliştirici gereklidir. İOS 8 yayımlandıktan sonra ancak bu, her durumda yanlışlık başlatma ekran oluşturmak için tek bir birleşik görsel taslak kullanmak mümkün olmamıştı.

Bu kısa kılavuzda bir başlatma ekranı varsayılan olarak yeni bir proje tarafından sağlanan herhangi bir görsel taslak veya varolan bir projeye el ile eklenen bir görsel taslak oluşturmayı açıklar. Ardından bu görünümleri kısıtlamaları ayarlamak ve görsel taslak çeşitli cihazlar ve yönlendirmelerine yanlışlık doğrulamak için görsel taslak için bir görüntü görünümü ve bir etiket eklemek için iOS Designer nasıl yapılacağı açıklanır.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>Başlatma ekranları görsel Taslaklar ile yönetme

İOS 8 (ve üzeri), geliştirici özel birleşik bir veya daha fazla statik başlatma görüntüleri kullanmak yerine başlatma ekranı sağlamak için film şeridi oluşturabilirsiniz. İOS Designer'daki başlatmadan görsel taslak oluşturma, boyut sınıfları ve otomatik düzen görünen farklı ortamlar için farklı düzenleri tanımlamak için kullanın. Boyut sınıflarını ve Otomatik Düzen'i kullanarak, geliştirici, tüm cihazlarda güzel görünümlü bir tek başlatma ekranı oluşturun ve ortamları görüntülemek.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'da seçerek yeni bir proje oluşturun **Dosya > Yeni Çözüm** seçip **tek görünüm uygulaması**: 

    ![Yeni Proje penceresinin tek görünüm uygulama seçildi](launch-screens-images/launch01.png)

    - Varsayılan olarak, yeni bir proje içeren bir **LaunchScreen.storyboard** tanımlayan başlatma ekranı arabirim dosyası. 
    - Bunun yerine bir başlatma ekranı görsel taslak var olan bir projeye eklemek için proje adına sağ **çözüm bölmesi** ve **Ekle > yeni dosya...**  seçip **başlatma ekranı**:

    ![Yeni dosya penceresiyle iOS seçili ekran başlatma](launch-screens-images/launch01b.png)

    - Dosya adı **LaunchScreen** veya seçtiğiniz başka bir ad.

2. Projeyi Başlat ekranı için uygun bir görsel taslak kullanacak şekilde yapılandırın:

    - Çift **Info.plist** dosyası **çözüm bölmesi** düzenlemek üzere açın.
    - İçinde **başlatma resimleri** bölümünde, emin **başlatma ekranı** uygun film şeridi adına ayarlanır:

    ![Info.plist dosyasında başlatma ekranı Seçici](launch-screens-images/launch02.png)

    - Varsayılan olarak, yeni bir proje kullanmak için yapılandırılmış **LaunchScreen.storyboard** başlatma ekranı olarak.

3. Görüntüye ekleme **Assets.xcassets** varlık kataloğunu Başlat ekranında için kullanılabilir. Daha fazla bilgi için [bir varlık Kataloğu görüntü kümesi ekleme görüntüleri](~/ios/app-fundamentals/images-icons/displaying-an-image.md) bölümünü [bir görüntüyü görüntüleme](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Kılavuzu.

4. Açık **LaunchScreen.storyboard** çift tıklayarak düzenleme için **çözüm bölmesi**.

5. Bir cihaz ve başlatma ekranı görsel taslak iOS Designer'daki Önizleme bağlanacağı yönünü seçin. Cihaz seçim paneli alt araç çubuğunda açın ve seçin **iPhone 4S** ve **dikey**.

    ![Cihaz seçimi araç çubuğu](launch-screens-images/launch05.png)

    - Bir cihaz seçip yönü yalnızca iOS Designer tasarım nasıl Önizleme değiştiğine dikkat edin. Kısıtlamalar uygulanır tüm cihazları ve yönlendirmelerine sürece burada yapılan seçim bağımsız olarak, yeni eklenen **nitelikleri Düzenle** düğmesi, aksi takdirde belirtmek için kullanılmıştı. 

6. Ayarlama **arka plan** görünümü denetleyicinin ana görünüm rengi. Görünümün ortasında görünüm denetleyicisi tıklayarak seçin ve kullanarak arka plan rengi ayarlamak **özellikler panelinde**:

    ![Mor renkli arka plan rengi ile tek bir görünüm](launch-screens-images/launch06.png)

7. Ekleme bir **resim görünümü** Başlat ekranına ve kendi kaynağı **görüntü**:

    - Sürükleme bir **resim görünümü** gelen **araç kutusu paneli** görünümün Merkezi.
    - İle **resim görünümü** seçiliyken **pencere öğesi** bölümünü **özellikler panelinde** ayarlamak **görüntü** görüntü kümesi zaten özelliği eklenen **Assets.xcassets** varlık Kataloğu. Yeniden konumlandırma ve boyutlandırma **resim görünümü** gerektiği gibi:
    
    ![Kendi görüntü özelliği ayarlanmış bir resim görünümü](launch-screens-images/launch07.png)

8. Ekleme bir **etiket** aşağıda **resim görünümü** ve **özellikler panelinde** özniteliklerini ayarlamak için: 

    ![Bir etiket, metin ve rengini ayarlama](launch-screens-images/launch08.png)

9. Sağ düğmeye kullanarak kısıtlaması düzenleme moduna geçin **kısıtlamalar araç çubuğu**:
    
    ![Kısıtlama düzenleme modu düğmesi](launch-screens-images/launch09.png)

10. İçin kısıtlama Ekle **resim görünümü**yüksekliğini ve genişliğini ayarlamak ve yatay ve dikey olarak ortalama:

    ![Düzen kısıtlamalarıyla bir resim görünümü](launch-screens-images/launch10.png)

    - Kısıtlamaları ekleme hakkında daha fazla ayrıntı için [iOS için Xamarin Tasarımcısı ile otomatik düzen](~/ios/user-interface/designer/designer-auto-layout.md).

11. İçin kısıtlama Ekle **etiket**, yatay olarak ortalama, yükseklik ve genişlik vermek ve sabit konumlandırma gelen dikey uzaklığı **resim görünümü**:

    ![Düzeni kısıtlamaları içeren bir etiket](launch-screens-images/launch11.png)

12. Diğer cihazlar ve tasarım tüm senaryolarda beklendiği gibi göründüğünü doğrulamak için yönleri test edin. Belirli bir cihaza veya yönlendirme için yapılacak ayarlamalar gereken yere durumlarda **nitelikleri Düzenle** constaints belirli boyut sınıfları için Ekle düğmesini:

    ![İPhone X kullanarak yatay olarak işlenen başlatma ekranı](launch-screens-images/launch12.png)

13. Görsel taslak için değişiklikleri kaydedin. Bir simülatörü veya Cihazınızda uygulamayı çalıştırın ve uygulama başlatma olarak Başlat ekranı görünür.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Yeni bir proje oluşturun. Visual Studio'da **Dosya > Yeni > Proje > Visual C# > iPhone & iPad > iOS uygulaması (Xamarin)**:

    ![Seçili yeni proje penceresinin, iOS uygulaması (Xamarin)](launch-screens-images/launch01.w157.png)

    Seçin **tek görünüm uygulaması** şablonu ve ardından **Tamam**:

    ![Tek görünüm uygulaması şablonu](launch-screens-images/launch01-2.w157.png)

2. Varsa **kaynaklar > LaunchScreen.xib** var. **Çözüm Gezgini**, dosyaya sağ tıklayıp seçerek silme **Sil**. Bu dosya, sonraki adımda bir görsel taslak ile değiştirilecek.

3. Başlatma ekranı olarak kullanmak üzere bir film şeridi oluşturun. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle > Yeni öğe...**  ardından **boş görsel taslak**. Bu film şeridini ad **LaunchScreen.storyboard** tıklatıp **Ekle**:

    ![Seçili boş görsel taslak ile yeni öğe Ekle penceresi](launch-screens-images/launch03.w157.png)

4. Projeyi kullanacak şekilde yapılandırma **LaunchScreen.storyboard** kendi başlatma ekranı görsel taslak olarak:

    - Çift **Info.plist** dosyası **Çözüm Gezgini** düzenlemek üzere açın. 
    - Üzerinde **görsel varlıklar** sekmesinde, belirleyin **başlatma ekranı** için **LaunchScreen**.

    ![Info.plist dosyasında başlatma ekranı Seçici](launch-screens-images/launch04-vs.png)

5. Böylece Başlat ekranında kullanmak için kullanılabilir görüntü projedeki bir varlık Kataloğu ekleyin:

    - İçinde **Çözüm Gezgini**, sağ **varlık katalogları** seçip **varlık Kataloğu ekleyin**. Bu yeni bir varlık kataloğu adı **varlıklar**:

    ![Seçilen varlık Kataloğu ile yeni öğe Ekle penceresi](launch-screens-images/launch05.w157.png)

    - Yeni bir ayarlayın görüntü ekleme **varlıklar** varlık Kataloğu açıklandığı gibi [bir varlık Kataloğu görüntü kümesi ekleme görüntüleri](~/ios/app-fundamentals/images-icons/displaying-an-image.md) bölümünü [bir görüntüyü görüntüleme](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Kılavuzu.

6. Açık **LaunchScreen.storyboard** çift tıklayarak düzenleme için **Çözüm Gezgini**.

    - Bir görsel taslak dosyası düzenlemek için Visual Studio etkin bir Mac derleme konağı bağlantısı gerekir. Bkz: [Mac bilgisayara bağlayarak](~/ios/get-started/installation/windows/connecting-to-mac/index.md) ayrıntıları Kılavuzu.

7. Bir cihaz ve başlatma ekranı görsel taslak iOS Designer'daki Önizleme bağlanacağı yönünü seçin. Cihaz seçim paneli alt araç çubuğunda açın ve seçin **iPhone 4S** ve **dikey**: 
 
    ![Cihaz seçimi araç çubuğu](launch-screens-images/launch07-vs.png)

    - Bir cihaz seçip yönü yalnızca iOS Designer tasarım nasıl Önizleme değiştiğine dikkat edin. Kısıtlamalar uygulanır tüm cihazları ve yönlendirmelerine sürece burada yapılan seçim bağımsız olarak, yeni eklenen **nitelikleri Düzenle** düğmesi, aksi takdirde belirtmek için kullanılmıştı. 

8. Ekleme bir **görünüm denetleyicisi** diğerine sürükleyerek film şeridi için **araç kutusu** tasarım yüzeyine: 

    ![Boş bir görünüm denetleyicisi tasarım yüzeyine eklenir.](launch-screens-images/launch08-vs.png)

9. Ayarlama **arka plan** görünümü denetleyicinin ana görünüm rengi. Görünümün ortasında görünüm denetleyicisi tıklayarak seçin ve kullanarak arka plan rengi ayarlamak **Özellikler penceresi**:
    
    ![Mor renkli arka plan rengi ile tek bir görünüm](launch-screens-images/launch09-vs.png)

10. Ekleme bir **resim görünümü** Başlat ekranına ve kendi kaynağı **görüntü**:

    - Sürükleme bir **resim görünümü** gelen **araç kutusu** görünümün Merkezi.
    - İle **resim görünümü** halen seçiliyken **pencere öğesi** bölümünü **Özellikler penceresi** ayarlamak **görüntü** özelliğini resmi ayarlama önceden eklenen **varlıklar** varlık Kataloğu. Yeniden konumlandırma ve boyutlandırma **resim görünümü** gerektiği gibi:
    
    ![Kendi görüntü özelliği ayarlanmış bir resim görünümü](launch-screens-images/launch10-vs.png)

11. Ekleme bir **etiket** aşağıda **görüntü Görünüm**:

    - Sürükleme bir **etiket** gelen **araç kutusu** tasarım yüzeyine sürükleyin, aşağıdaki yerleştirme **resim görünümü**.
    - Ayarlamak için öznitelikler **etiket** kullanarak **Özellikler penceresi**:

    ![Bir etiket, metin ve rengini ayarlama](launch-screens-images/launch11-vs.png) 

12. Sağ düğmeye kullanarak kısıtlaması düzenleme moduna geçin **kısıtlamalar araç çubuğu**:
    
    ![Kısıtlama düzenleme modu düğmesi](launch-screens-images/launch12-vs.png) 

13. İçin kısıtlama Ekle **resim görünümü**yüksekliğini ve genişliğini ayarlamak ve yatay ve dikey olarak ortalama:

    ![Düzen kısıtlamalarıyla bir resim görünümü](launch-screens-images/launch13-vs.png) 

    - Kısıtlamaları ekleme hakkında daha fazla bilgi için bkz: [iOS için Xamarin Tasarımcısı ile otomatik düzen](~/ios/user-interface/designer/designer-auto-layout.md).

14. İçin kısıtlama Ekle **etiket**, yatay olarak ortalama, yükseklik ve genişlik vermek ve sabit konumlandırma gelen dikey uzaklığı **resim görünümü**:
    
    ![Düzeni kısıtlamaları içeren bir etiket](launch-screens-images/launch14-vs.png) 

15. Diğer cihazlar ve tasarım tüm senaryolarda beklendiği gibi göründüğünü doğrulamak için yönleri test edin. Belirli bir cihaza veya yönlendirme için yapılacak ayarlamalar gereken yere durumlarda **nitelikleri Düzenle** constaints belirli boyut sınıfları için Ekle düğmesini:

    ![İPhone X kullanarak yatay olarak işlenen başlatma ekranı](launch-screens-images/launch15-vs.png) 

16. Görsel taslak için değişiklikleri kaydedin. Bir simülatörü veya Cihazınızda uygulamayı çalıştırın ve uygulama başlatma olarak Başlat ekranı görünür.

-----

> [!NOTE]
> Başlatma ekranı olarak kullanılan bir görsel taslak _gerekir_ yalnızca basit, yerleşik kullanıcı Arabirimi öğeleri ekleyin ve **olamaz** tüm hesaplamalar yapabilir veya özel bir sınıf türetin.

Bir başlatma ekranı ile birleştirilmiş bir görsel taslak oluşturma hakkında daha fazla bilgi için lütfen bkz [dinamik başlatma ekranları](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) bölümünü [birleşik görsel Taslaklar](~/ios/user-interface/storyboards/unified-storyboards.md) Kılavuzu.

## <a name="migrating-to-launch-screen-storyboards"></a>Başlatma ekranı görsel taslak için geçirme

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Görsel Taslaklar, başlatma ekranları için kullanılacak mevcut bir uygulamayı güncelleştirirken, sağ tıklayın **proje adı** içinde **Çözüm Gezgini** seçip **Ekle**  >  **Yeni dosya...** . Seçin **iOS** > **başlatma ekranı** tıklatıp **yeni** düğmesi:

![](launch-screens-images/storyboard02.png "Bir iOS başlatma ekranı seçin")

Ardından, çift `Info.plist` dosyası **Çözüm Gezgini** düzenlemek üzere açın. Altında **başlatma ekranı**, yukarıda oluşturulan yeni görsel taslak dosyası seçin.

![](launch-screens-images/storyboard09.png "Yukarıda oluşturulan yeni görsel taslak dosyası seçin")


Yeni bir film şeridi bir başlatma ekranı kullanmak için aşağıdakileri yapın:

1. Çift `Info.plist` dosyası **Çözüm Gezgini** düzenlemek üzere açın.
2. Kaydırma **Evrensel başlatma resimleri** Düzenleyici, açık bir bölümünü **başlatma ekranı** açılır ve seçin, görsel taslağın adını oluşturulan yukarıda: 

    ![](launch-screens-images/storyboard08.png "Başlatma ekranı görsel taslak için ayarlama")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Proje adına sağ tıklayın **Çözüm Gezgini** seçip **Ekle** > **yeni dosya...** : 

    ![](launch-screens-images/image012.png "Yeni bir dosya ekleyin")
2. Başlatma ekranı için bir ad girin ve tıklayın **Ekle** düğmesi: 

    ![](launch-screens-images/image013.png "Başlatma ekranı için bir ad girin")
3. İçinde **Çözüm Gezgini**, düzenlemek üzere açmak için yeni oluşturulan görsel taslak dosyasına çift tıklayın.
4. Emin **boyut sınıfına** ayarlanır **herhangi: tüm** ve **görünümü olarak** olduğu **genel**: 

    ![](launch-screens-images/image016.png "Boyut sınıfına birine ayarlanmış olduğundan emin olun: tüm ve görünüm olarak genel")
5. Derleme başlatma ekranı boyutu sınıflardan basit kullanıcı Arabirimi öğeleri (gibi `UIImageView`) ve uygulama paketi grubuna dahil ettiğiniz görüntüler: 

    ![](launch-screens-images/image017.png "Derleme iOS Designer'daki başlatma ekranı")
6. Görsel taslak için değişiklikleri kaydedin.

-----

## <a name="related-links"></a>İlgili bağlantılar

- [Dinamik başlatma ekranları (örnek)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Birleşik Görsel Taslaklar](~/ios/user-interface/storyboards/unified-storyboards.md)
- [Temel iOS Designer Bilgileri](~/ios/user-interface/designer/index.md)
- [Bir varlık Kataloğu görüntü ekleme görüntüleri ayarlama](~/ios/app-fundamentals/images-icons/displaying-an-image.md#adding-images-to-an-asset-catalog-image-set)
- [İOS için Xamarin Tasarımcısı ile otomatik düzen](~/ios/user-interface/designer/designer-auto-layout.md)
- [İnsan Arabirimi yönergelerine: Başlatma ekranı](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
