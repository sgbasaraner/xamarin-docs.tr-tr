---
title: Xamarin.Mac, araç çubukları
description: Bu makalede bir Xamarin.Mac uygulamasını araç çubuklarında ile çalışma. Xcode ve arabirim Oluşturucu, bunları için kodu gösterme ve program aracılığıyla çalışma araç çubukları oluşturma ve bakımını yapmak kapsar.
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 06faaf16ffd0adc64063bfa5a264c1895b9ca9cb
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780569"
---
# <a name="toolbars-in-xamarinmac"></a>Xamarin.Mac, araç çubukları

_Bu makalede bir Xamarin.Mac uygulamasını araç çubuklarında ile çalışma. Xcode ve arabirim Oluşturucu, bunları için kodu gösterme ve program aracılığıyla çalışma araç çubukları oluşturma ve bakımını yapmak kapsar._

Mac için Visual Studio ile çalışan Xamarin.Mac geliştiriciler kullanılabilir araç çubuğu denetimi de dahil olmak üzere, Xcode ile çalışan macOS geliştiriciler için aynı kullanıcı Arabirimi denetimleri erişebilir. Xamarin.Mac Xcode ile doğrudan tümleştirilir, oluşturmak ve araç çubuğu öğelerini korumak için Xcode'un arabirim Oluşturucu kullanmak mümkün olmasıdır. Bu araç çubuğu öğelerini de C# ' ta oluşturulabilir.

MacOS araç çubuklarını, pencerenin üst kısmında için eklenir ve işlevselliği için ilgili komutları kolay erişim sağlar. Araç çubukları gizli, gösterilen veya bir uygulamanın kullanıcı tarafından özelleştirilebilir ve araç çubuğu öğeleri çeşitli şekillerde sunabilir.

Bu makalede, araç çubukları ve araç çubuğu öğeleri bir Xamarin.Mac uygulamasını ile çalışmanın temel kavramları kapsar. 

Devam etmeden önce okumak [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale — özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) bölümleri — haliyle temel kavramları ve bu kılavuzda kullanılır teknikler kapsar.

Ayrıca göz atın [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) belge. Açıklar `Register` ve `Export` Objective-C sınıflar için C# sınıfları bağlanmak için kullanılan öznitelikleri.

## <a name="introduction-to-toolbars"></a>Araç çubukları giriş

MacOS uygulama herhangi bir pencerede bir araç şunları içerebilir:

![Bir örnek penceresi araç çubuğu ile](toolbar-images/info01.png "bir örnek pencere bir araç çubuğu")

Araç çubukları, önemli hızlıca erişmek için uygulamanızın kullanıcıları veya sık kullanılan özellikler için kolay bir yol sağlar. Örneğin, bir belge düzenleme uygulaması metin rengini ayarlama, yazı tipini değiştirmek veya geçerli belge yazdırma için araç çubuğu öğelerini sağlayabilir.

Araç çubukları, üç yolla öğeleri görüntüleyebilir:

1. **Simge ve metin** 

     ![Simge ve metin ile bir araç çubuğu](toolbar-images/info02.png "simge ve metin ile bir araç çubuğu")

2. **Yalnızca simgesi** 

     ![Bir simge yalnızca araç](toolbar-images/info03.png "simgesi yalnızca bir araç çubuğu")

3. **Yalnızca metin** 

     ![Yalnızca metin araç](toolbar-images/info04.png "salt metin araç çubuğu")

Araç çubuğunun sağ tıklayıp bağlam menüsünden bir görüntü modu seçme bu modları arasında geçiş yapma:

![Araç çubuğu için bağlamsal menü](toolbar-images/info05.png "bağlamsal bir araç çubuğunun menüsü")

Aynı menüyü, küçük bir boyutta araç çubuğunu görüntülemek için kullanın:

![Küçük simgeleri içeren bir araç çubuğu](toolbar-images/info06.png "küçük simgeleri içeren bir araç çubuğu")

Araç çubuğunu özelleştirme için menü da sağlar:

![Araç çubuğu özelleştirmek için kullanılan iletişim](toolbar-images/info07.png "araç özelleştirmek için kullanılan iletişim kutusu")

Xcode'un arabirim oluşturucu araç çubuğu ayarlarken bir geliştirici varsayılan yapılandırması parçası olmayan ek araç çubuğu öğeleri sağlar. Uygulama kullanıcılarının, ardından gerekli olarak önceden tanımlanmış bu öğeleri ekleme ve kaldırma araç özelleştirebilirsiniz. Elbette, araç, varsayılan yapılandırmasında sıfırlanabilir.

Araç için otomatik olarak bağlanır. **görünümü** gizlemek göstermek ve özelleştirmek kullanıcılara menüsünde:

![Görünüm menüsü araç çubuğu ile ilgili öğeleri](toolbar-images/info08.png "Görünüm menüsü araç çubuğu ile ilgili öğeleri")

Bkz: [yerleşik bir menüsünü işlevi](~/mac/user-interface/menu.md) daha fazla ayrıntı için belgeleri.

Ayrıca, araç arabirimi Oluşturucusu'nda düzgün şekilde yapılandırılırsa, uygulama araç çubuğu özelleştirmeleri otomatik olarak genelinde birden çok uygulama başlatılan kalıcı.

Bu kılavuzun sonraki bölümleri oluşturmak ve arabirim Oluşturucu Xcode'un çubuklarıyla sürdürmek nasıl ve bunlarla çalışan kodda nasıl açıklar.

## <a name="setting-a-custom-main-window-controller"></a>Özel ana pencere denetleyicisi ayarlanıyor

Kullanıcı Arabirimi öğeleri için C# kodu çıkışlar ve Eylemler, Xamarin.Mac uygulamasını aracılığıyla kullanıma sunmak için bir özel pencere denetleyicisi kullanmanız gerekir:

1. Uygulamanın görsel taslak Xcode'un arabirimi Oluşturucusu'nda açın.
2. Tasarım yüzeyinde penceresi denetleyicisi seçin.
3. Geçiş **kimlik denetçisi** "WindowController" olarak girin **sınıf adı**: 

    [![Pencere denetleyicisi için özel bir sınıf adı ayarlama](toolbar-images/windowcontroller01.png "penceresi denetleyicisi için özel bir sınıf adı ayarlama")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. Değişikliklerinizi kaydetmek ve eşitlemek Mac için Visual Studio geri dönün.
5. A **WindowController.cs** dosyası projenize eklenecek **çözüm bölmesi** Mac için Visual Studio'da: 

    ![Çözüm panelinde WindowController.cs seçerek](toolbar-images/windowcontroller02.png "WindowController.cs seçme içinde çözüm bölmesi")

6. Xcode'un arabirim oluşturucu görsel taslağı yeniden açın.
7. **WindowController.h** dosya kullanım için kullanılabilir olacak: 

    [![WindowController.h dosya](toolbar-images/windowcontroller03.png "WindowController.h dosyası")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Oluşturulması ve bakımının yapılması xcode'da araç çubukları

Araç çubukları oluşturulur ve Xcode'un arabirim Oluşturucu ile korunur. Bir araç bir uygulamaya eklemek için uygulamanın birincil görsel taslak Düzenle (Bu durumda **Main.storyboard**) içinde çift tıklayarak **çözüm bölmesi**:

![Çözüm panelinde Main.Storyboard açma](toolbar-images/edit01.png "çözüm panelinde Main.storyboard açma")

İçinde **kitaplığı denetçisi**, girin "aracı" **arama kutusuna** tüm kullanılabilir araç çubuğu öğeleri görmek daha kolay hale getirmek için:

![Kitaplık denetçisi filtrelenmiş araç çubuğu öğelerini göstermek için](toolbar-images/edit02.png "araç çubuğu öğelerini gösterecek şekilde kitaplığı denetçisi")

Araç penceresine sürükleyin **Arayüzü Düzenleyicisi**. Araç seçili özellikleri ayarlayarak davranışını yapılandırma **öznitelikleri denetçisi**:

![Öznitelikleri denetleyici için bir araç çubuğu](toolbar-images/edit04.png "öznitelikleri Inspector'ı için bir araç çubuğu")

Aşağıdaki özellikler mevcuttur:

1. **Görüntü** -araç simgeler, metin ya da her ikisini de görüntüler denetler
2. **Başlatma sırasında görünür** -seçtiyseniz, araç varsayılan olarak görünür.
3. **Özelleştirilebilir** -seçtiyseniz, kullanıcıların düzenleyebilir ve araç çubuğunu özelleştirme.
4. **Ayırıcı** -seçtiyseniz, ince bir yatay çizgi pencerenin içeriği araç ayırır.
5. **Boyutu** -araç boyutunu ayarlar.
6. **Otomatik kaydetme** -seçtiyseniz, uygulama genelinde başlatılan uygulama kullanıcının araç yapılandırma değişiklikleri açık kalır.

Seçin **otomatik kaydetme** seçenek ve diğer tüm özellikler, varsayılan ayarlarında bırakın. 

Araç çubuğunda açıp sonra **arabirimi hiyerarşi**, bir araç çubuğu öğesini seçerek özelleştirme iletişim kutusunu Getir:

![Araç çubuğunu özelleştirme](toolbar-images/edit05.png "araç çubuğunu özelleştirme")

Tasarım, uygulama için varsayılan araç çubuğu için araç çubuğunda bir parçası olan öğeleri özelliklerini ayarlamak ve araç özelleştirirken seçmek bir kullanıcı için ek araç çubuğu öğelerini sağlamak için bu iletişim kutusunu kullanın. Araç çubuğuna öğeleri eklemek için sürükleyin **kitaplığı denetçisi**:

![Kitaplık denetçisi](toolbar-images/edit06.png "kitaplığı denetçisi")

Aşağıdaki araç çubuğu öğelerini eklenebilir:

- **Araç çubuğu öğesi resim** -simge olarak özel bir görüntü ile bir araç çubuğu öğesi.
- **Esnek alanı araç çubuğu öğesi** -sonraki araç çubuğu öğelerini yaslamak için kullanılan esnek alanı. Örneğin, bir veya daha fazla araç çubuğu öğelerini esnek alanı araç çubuğu öğesi tarafından izlenen ve başka bir araç çubuğu öğesi araç çubuğunun sağ tarafındaki son öğeye sabitler.
- **Araç çubuğu öğesi alanı** -araç çubuğundaki öğeler arasındaki boşluk düzeltildi
- **Ayırıcı araç çubuğu öğesi** -iki veya daha fazla araç çubuğu öğeleri için gruplandırma arasında görünür bir ayırıcı
- **Özelleştirme araç çubuğu öğesi** -kullanıcılara araç çubuğunu özelleştirme
- **Araç çubuğu öğesi yazdırma** -açık belge yazdırma olanağı sağlar.
- **Renkleri araç çubuğu öğesi Göster** -standart sistem renk seçici görüntülenir: 

     ![Sistem Renk Seçici](toolbar-images/edit07.png "sistem renk seçici")

- **Yazı tipi araç çubuğu öğesi Göster** -standart sistem yazı tipi iletişim kutusu görüntülenir: 

     ![Yazı tipi Seçici](toolbar-images/edit08.png "yazı tipi Seçici")

> [!IMPORTANT]
> Daha sonra görülür gibi bir araç çubuğuna arama alanları, bölümlenmiş denetimler ve yatay kaydırma çubuklarını gibi birçok standart Cocoa UI denetimleri de eklenebilir.

### <a name="adding-an-item-to-a-toolbar"></a>Bir araç çubuğuna bir öğe ekleme

Araç çubuğuna bir öğe eklemek için araç çubuğunda seçin **arabirimi hiyerarşi** ve özelleştirme iletişim kutusunun görüntülenmesine neden öğelerinden birine tıklayın. Ardından, yeni bir öğe sürüklemeden **kitaplığı denetçisi** için **izin araç çubuğu öğelerini** alan:

![Araç çubuğu özelleştirme iletişim kutusunda izin araç çubuğu öğelerini](toolbar-images/add01.png "araç çubuğu özelleştirme iletişim kutusunda izin araç çubuğu öğeleri")

Yeni bir öğe varsayılan araç bir parçası olduğundan emin olmak için sürükleyin **varsayılan araç çubuğu öğelerini** alan: 

![Araç çubuğu öğesi sürükleyerek yeniden sıralama](toolbar-images/add02.png "sürükleyerek bir araç çubuğu öğesi yeniden sıralama")

Varsayılan araç çubuğu öğeleri yeniden sıralamak için sola veya sağa sürükleyin.

Ardından, **öznitelikleri denetçisi** öğesi varsayılan özelliklerini ayarlamak için:

![Öznitelikleri Inspector'ı kullanarak bir araç çubuğu öğesi özelleştirme](toolbar-images/add03.png "öznitelikleri Inspector'ı kullanarak bir araç çubuğu öğesi özelleştirme")

Aşağıdaki özellikler mevcuttur:

- **Görüntü adı** -öğe için simge kullanılacak resim
- **Etiket** -araç çubuğu öğesi için görüntülenecek metin
- **Palet etiket** -öğesi için görüntülenecek metin **izin araç çubuğu öğelerini** alan
- **Etiket** -kod öğesinde tanımlamanıza yardımcı olan isteğe bağlı, benzersiz tanımlayıcı.
- **Tanımlayıcı** -tanımlar araç çubuğu öğe türü. Özel bir değer kodda bir araç çubuğu öğesini seçmek için kullanılabilir.
- **Seçilebilir** -işaretlediyseniz, öğeyi bir açma/kapatma düğmesi gibi davranacak.

> [!IMPORTANT]
> Öğe ekleme **izin araç çubuğu öğelerini** alan ancak olmayan kullanıcılar için özelleştirme seçenekleri sağlamak için varsayılan araç. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>Diğer kullanıcı Arabirimi denetimleri için araç çubuğu ekleme

Arama alanları gibi çeşitli Cocoa kullanıcı Arabirimi öğeleri ve bölümlenmiş denetimler araç da eklenebilir.

Bunu denemek için araç çubuğunda açın **arabirimi hiyerarşi** ve özelleştirme iletişim kutusunu açmak için bir araç çubuğu öğesi seçin. Sürükleme bir **arama alanı** gelen **kitaplığı denetçisi** için **izin araç çubuğu öğelerini** alan:

![Araç çubuğu özelleştirme iletişim kutusunu kullanarak](toolbar-images/add05.png "araç çubuğu özelleştirme iletişim kutusunu kullanarak")

Buradan, arama alanına yapılandırın ve kod bir eylem veya çıkış aracılığıyla kullanıma sunmak için arabirim Oluşturucu kullanın.

## <a name="built-in-toolbar-item-support"></a>Yerleşik araç çubuğu öğesi desteği

Birkaç Cocoa kullanıcı Arabirimi öğeleri varsayılan olarak standart araç çubuğu öğeleri ile etkileşim kurun. Örneğin, sürükleyin bir **metni görünümü** uygulama penceresinin üzerine ve içerik alanı dolduracak şekilde yerleştir:

[![Bir metin görünümünü uygulamaya ekleme](toolbar-images/edit09.png "metni görünümü uygulamaya ekleme")](toolbar-images/edit09-large.png#lightbox)

Xcode ile eşitleyin, uygulamayı çalıştırmak, metinleri girin, seçin ve tıklayın Mac için Visual Studio geri dönüp belgeyi kaydedin **renkleri** araç çubuğu öğesi. Metin görünümünü otomatik olarak bir renk seçici ile çalıştığını fark:

![Metin görünümü ve Renk Seçici ile yerleşik araç işlevselliği](toolbar-images/edit10.png "yerleşik araç işlevi bir metin görünümünü ve Renk Seçici")

## <a name="using-images-with-toolbar-items"></a>Görüntüleri araç çubuğu öğeleri ile kullanma

Kullanarak bir **görüntü araç çubuğu öğesi**, herhangi bir bit eşlem görüntüsüne eklenecek **kaynakları** klasörü (ve bir yapı eylemi, belirtilen **paket kaynak**) simge olarak araç çubuğunda görüntülenen:

1. Mac için Visual Studio içinde **çözüm bölmesi**, sağ tıklayın **kaynakları** klasörü ve select **Ekle** > **Add Files** .
2. Gelen **Add Files** iletişim kutusunda, istenen görüntülerin gidin, bunları seçin ve tıklayın **açık** düğmesi: 

    [![Eklemek için görüntüleri seçme](toolbar-images/edit11.png "eklemek için görüntüleri seçme")](toolbar-images/edit11-large.png#lightbox)

3. Seçin **kopyalama**, kontrol **aynı eylem seçilen tüm dosyaları için kullanan**, tıklatıp **Tamam**:

    ![Eklenen görüntüleri için kopyalama eylemini seçerek](toolbar-images/edit12.png "eklenen görüntüleri için kopyalama eylemini seçerek")

4. İçinde **çözüm bölmesi**, çift **MainWindow.xib** Xcode'da açın.

5. Araç çubuğunda seçin **arabirimi hiyerarşi** ve özelleştirme iletişim kutusunu açmak için öğelerinden birine tıklayın.

6. Sürükleme bir **görüntü araç çubuğu öğesi** gelen **kitaplığı denetçisi** araç çubuğunun için **izin araç çubuğu öğelerini** alan: 

    ![Araç çubuğu öğelerini izin alan bir görüntü araç çubuğu öğesi eklenen](toolbar-images/edit14.png "bir görüntü araç çubuğu öğesi için araç çubuğu öğelerini izin verilen alanı eklendi")

7. İçinde **öznitelikleri denetçisi**, Mac için Visual Studio'da yeni eklenen görüntüyü seçin: 

    ![Araç çubuğu öğesi için özel bir görüntü ayarlama](toolbar-images/edit15.png "araç çubuğu öğesi için özel bir görüntü ayarlama")

8. Ayarlama **etiket** "Çöp" için ve **palet etiket** "Belge silmek için": 

    ![Araç ayarı öğesi etiketi ve etiket palet](toolbar-images/edit16.png "araç ayarı öğesi etiketi ve etiket paleti")

9. Sürükleme bir **ayırıcı araç çubuğu öğesi** gelen **kitaplığı denetçisi** araç çubuğunun için **izin araç çubuğu öğelerini** alan: 

    [![Ayırıcı araç çubuğu öğesi izin verilen araç çubuğu öğelerini alanına eklenen](toolbar-images/edit17.png "bir ayırıcı araç çubuğu öğesi için araç çubuğu öğelerini izin verilen alanı eklendi")](toolbar-images/edit17-large.png#lightbox)

10. Ayırıcı öğe ve "Çöp" öğesine sürükleyin **varsayılan araç çubuğu öğelerini** alan ve sırasını araç çubuğu öğelerini gelen ayarlı soldan sağa (renkleri, yazı tipleri, ayırıcı, çöp, esnek bir boşluk, yazdırma) aşağıdaki gibi: 

    ![Varsayılan araç çubuğu öğelerini](toolbar-images/edit18.png "varsayılan araç çubuğu öğeleri")

11. Değişiklikleri kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Yeni araç çubuğu varsayılan olarak görüntülendiğini doğrulamak için uygulamayı çalıştırın:

![Özelleştirilmiş varsayılan öğeleri ile bir araç çubuğu](toolbar-images/edit19.png "özelleştirilmiş varsayılan öğeleri ile bir araç çubuğu")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Çıkışlar ve Eylemler ile araç çubuğu öğeleri gösterme

Bir araç veya araç çubuğu öğesi kodda erişmek için bir çıkış veya bir eylem eklenmesi gerekir:

1. İçinde **çözüm bölmesi**, çift **Main.storyboard** Xcode'da açın.
2. "WindowController", ana pencereyi denetleyicisine atanmış özel bir sınıf emin **kimlik denetçisi**:

    [![Özel bir sınıf için pencere denetleyicisi ayarlamak için kimlik Inspector'ı kullanarak](toolbar-images/edit20a.png "özel bir sınıf için pencere denetleyicisi ayarlamak için kimlik denetçisini kullanma")](toolbar-images/edit20a-large.png#lightbox)

3. Ardından, araç çubuğu öğesi içindeki seçin **arabirimi hiyerarşi**: 

    ![Arabirim hiyerarşisinde araç çubuğu öğesini seçerek](toolbar-images/edit20.png "araç çubuğu öğesi arabirim hiyerarşisinde seçme")  

4. Açık **Yardımcısı görünümü**seçin **WindowController.h** dosya ve araç çubuğu öğesi için Denetim Sürükle **WindowController.h** dosya.
5. Ayarlama **bağlantı** için yazın **eylem**, "trashDocument" girin **adı**, tıklatıp **Connect** düğmesi: 

    [![Araç çubuğu öğesi için bir eylem yapılandırma](toolbar-images/edit23.png "araç çubuğu öğesi için bir eylem yapılandırma")](toolbar-images/edit23-large.png#lightbox)

6. Kullanıma sunma **metni görünümü** "documentEditor" adlı bir çıkış olarak **ViewController.h** dosyası: 

    [![Metin görünümü için bir çıkış yapılandırma](toolbar-images/edit24.png "metni görünümü için bir çıkış yapılandırma")](toolbar-images/edit24-large.png#lightbox)

7. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Mac için Visual Studio'da Düzenle **ViewController.cs** dosyasını açıp aşağıdaki kodu ekleyin:

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

Ardından, Düzenle **WindowController.cs** altına aşağıdaki kodu ekleyin ve dosya `WindowController` sınıfı:

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

Uygulama çalışırken **çöp** araç çubuğu öğesi etkin olacaktır:

![Etkin çöp öğesi ile bir araç çubuğu](toolbar-images/edit25.png "etkin çöp öğesi ile bir araç çubuğu")

Dikkat **çöp** araç çubuğu öğesi artık metni silmek için kullanılabilir.

## <a name="disabling-toolbar-items"></a>Araç çubuğu öğelerini devre dışı bırakma

Öğenin araç çubuğunda devre dışı bırakmak için özel bir oluşturma `NSToolbarItem` sınıf ve geçersiz kılma `Validate` yöntemi. Ardından, arabirim Oluşturucu'da özel bir tür etkinleştir/devre dışı bırakmak istediğiniz öğeye atayın.

Özel bir oluşturmak için `NSToolbarItem` sınıfı, projeye sağ tıklayıp **Ekle** > **yeni dosya...** . Seçin **genel** > **boş sınıf**, "ActivatableItem" girin **adı**, tıklatıp **yeni** düğmesi: 

![Boş bir sınıf, Mac için Visual Studio'da ekleme](toolbar-images/custom01.png "boş bir sınıf, Mac için Visual Studio'da ekleme")

Ardından, Düzenle **ActivatableItem.cs** şu şekilde okunacak dosya:

```csharp
using System;

using Foundation;
using AppKit;

namespace MacToolbar
{
    [Register("ActivatableItem")]
    public class ActivatableItem : NSToolbarItem
    {
        public bool Active { get; set;} = true;

        public ActivatableItem ()
        {
        }

        public ActivatableItem (IntPtr handle) : base (handle)
        {
        }

        public ActivatableItem (NSObjectFlag  t) : base (t)
        {
        }

        public ActivatableItem (string title) : base (title)
        {
        }

        public override void Validate ()
        {
            base.Validate ();
            Enabled = Active;
        }
    }
}
```

Çift **Main.storyboard** Xcode'da açın. Seçin **çöp** araç çubuğu öğesi yukarıda oluşturulan ve "İçin ActivatableItem" değiştirmek sınıfıyla **kimlik denetçisi**:

![Araç çubuğu öğesi için özel bir sınıf ayarlama](toolbar-images/custom02.png "araç çubuğu öğesi için özel bir sınıf ayarlama")

Adlı bir çıkış oluşturma `trashItem` için **çöp** araç çubuğu öğesi. Değişiklikleri kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün. Son olarak, açık **MainWindow.cs** ve güncelleştirme `AwakeFromNib` gibi görünecek şekilde yöntemi:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

Uygulamayı çalıştırmak ve unutmayın **çöp** öğesi artık araç çubuğunda devre dışı:

![Etkin olmayan çöp öğesi ile bir araç çubuğu](toolbar-images/custom03.png "etkin olmayan çöp öğesi ile bir araç çubuğu")

## <a name="summary"></a>Özet

Bu makalede, araç çubukları ve araç çubuğu öğeleri bir Xamarin.Mac uygulamasını çalışma ayrıntılı bir bakış duruma getirdi. Bunu açıklanan, oluşturma ve arabirim Oluşturucu Xcode'un araç çubuklarında korumak, bazı kullanıcı Arabirimi denetimleri ile araç çubuğu öğelerini otomatik olarak nasıl çalıştığını, C# kodunda araç çubukları ile nasıl çalışılacağını ve nasıl etkinleştirileceği ve araç çubuğu öğelerini devre dışı bırakın.


## <a name="related-links"></a>İlgili bağlantılar

- [MacToolbar (örnek)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Araç çubukları için İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [Araç çubukları giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
