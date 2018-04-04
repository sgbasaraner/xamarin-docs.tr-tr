---
title: Araç Çubukları
description: Bu makalede araç çubukları Xamarin.Mac uygulamada çalışmaya açıklanmaktadır. Xcode ve arabirimi kodu gösterme ve bunlarla program aracılığıyla çalışma Oluşturucu Oluşturma ve bakımını yapmak araç çubukları kapsar.
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 729c5c69d80c52047585d1026d7c675f3267f34e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="toolbars"></a>Araç Çubukları

_Bu makalede araç çubukları Xamarin.Mac uygulamada çalışmaya açıklanmaktadır. Xcode ve arabirimi kodu gösterme ve bunlarla program aracılığıyla çalışma Oluşturucu Oluşturma ve bakımını yapmak araç çubukları kapsar._

Mac için Visual Studio ile çalışan Xamarin.Mac geliştiriciler araç çubuğu denetimi dahil olmak üzere Xcode ile çalışma macOS geliştiricilerin kullanımına aynı kullanıcı Arabirimi denetimlerini erişimi. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü oluşturmak ve araç çubuğu öğeleri korumak için Xcode'nın arabirimi Oluşturucu kullanmak da mümkündür. Bu araç çubuğu öğeleri ayrıca C# ' ta oluşturulabilir.

MacOS çubuklarında penceresinin üst bölümüne eklenir ve işlevselliği için ilgili komutları kolay erişim sağlar. Araç çubukları gizli, gösterilen veya bir uygulamanın kullanıcı tarafından özelleştirilmiş ve çeşitli şekillerde araç çubuğu öğeleri sunabilir.

Bu makalede, araç çubukları ve Xamarin.Mac uygulama araç çubuğu öğeleri ile çalışmanın temelleri yer almaktadır. 

Devam etmeden önce okuyun [Hello, Mac](~/mac/get-started/hello-mac.md) makale — özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) bölümleri — şekliyle temel kavramları ve bu kılavuzda kullanılan teknikleri kapsar.

Ayrıca bir göz atalım [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) belge. Açıklar `Register` ve `Export` Objective-C sınıfları için C# sınıfları bağlanmak için kullanılan öznitelikleri.

## <a name="introduction-to-toolbars"></a>Araç çubukları giriş

MacOS uygulama herhangi bir pencerede bir araç şunları içerebilir:

![Araç çubuğu örnek penceresiyle](toolbar-images/info01.png "bir araç çubuğu örnek penceresiyle")

Araç çubukları, hızlı bir şekilde önemli erişmek için uygulamanızın kullanıcıları veya yaygın olarak kullanılan özellikler için kolay bir yol sağlar. Örneğin, uygulama düzenleme belgesi metin rengini ayarlama, yazı tipini değiştirme ya da geçerli belge yazdırma için araç çubuğu öğeleri sağlayabilir.

Araç çubukları öğeleri üç yolla görüntüleyebilirsiniz:

1. **Simge ve metin** 

     ![Simgeler ve metin içeren bir araç çubuğu](toolbar-images/info02.png "simgeler ve metin içeren bir araç çubuğu")

2. **Yalnızca simgesi** 

     ![Yalnızca simgesi araç](toolbar-images/info03.png "yalnızca simgesi araç çubuğu")

3. **Yalnızca metin** 

     ![Yalnızca metin araç](toolbar-images/info04.png "salt metin araç çubuğu")

Araç çubuğunu sağ tıklatıp bir görüntü modu bağlamsal menüden seçerek Bu modlar arasında geçiş:

![Bir araç için bağlam menüsü](toolbar-images/info05.png "bir araç için bağlam menüsü")

Araç çubuğu küçük bir boyutta görüntülemek için aynı menüyü kullanın:

![Bir araç çubuğu küçük simgelerle](toolbar-images/info06.png "küçük simgelerle bir araç çubuğu")

Menü Ayrıca araç çubuğunu özelleştirme sağlar:

![Araç çubuğu özelleştirmek için kullanılan iletişim](toolbar-images/info07.png "bir araç çubuğunu özelleştirmek için kullanılan iletişim kutusu")

Bir araç çubuğu Xcode'nın arabirimi Oluşturucu kurarken bir geliştirici varsayılan yapılandırması parçası olmayan ek araç çubuğu öğeleri sağlar. Uygulama kullanıcılarının, ardından gerekli olarak önceden tanımlanmış bu öğeler ekleme ve kaldırma araç özelleştirebilirsiniz. Elbette, araç varsayılan yapılandırmasıyla sıfırlanabilir.

Araç çubuğu otomatik olarak bağlanır **Görünüm** gizleme, bunu göstermek ve özelleştirmek kullanıcılara menüsünde:

![Araç çubuğu ile ilişkili öğeleri Görünüm menüsünde](toolbar-images/info08.png "Görünüm menüsünde araç ile ilişkili öğeleri")

Bkz: [yerleşik menü işlevi](~/mac/user-interface/menu.md) daha fazla ayrıntı için belgeleri.

Araç çubuğu arabirimi Oluşturucusu'nda doğru yapılandırılmış olduğundan, ayrıca, uygulama otomatik olarak araç çubuğu özelleştirmeleri arasında uygulamanın birden çok başlatır korunur.

Bu kılavuzun sonraki bölümlerde nasıl oluşturulacağı ve araç çubuklarını Xcode'nın arabirimi Oluşturucu ile korumak ve bunlarla kodda çalışma konusunda açıklanmaktadır.

## <a name="setting-a-custom-main-window-controller"></a>Özel ana penceresi denetleyicisini ayarlama

C# kodu çıkışlar ve Eylemler, Xamarin.Mac uygulama için kullanıcı Arabirimi öğeleri göstermek için bir özel pencere denetleyicisi kullanmanız gerekir:

1. Uygulamanın film şeridi Xcode'nın arabirimi Oluşturucu'da açın.
2. Tasarım yüzeyine penceresi denetleyicisinde seçin.
3. Geçiş **kimlik denetçisi** ve "WindowController" olarak girin **sınıf adı**: 

    [![Pencere denetleyicisi için özel bir sınıf adı ayarlama](toolbar-images/windowcontroller01.png "penceresi denetleyicisi için özel bir sınıf adı ayarlama")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. Değişikliklerinizi kaydetmek ve Visual Studio eşitlemek için Mac için geri dönün.
5. A **WindowController.cs** dosya projenizde eklenecek **çözüm paneli** Mac için Visual Studio'da: 

    ![Çözüm defterinde WindowController.cs seçerek](toolbar-images/windowcontroller02.png "WindowController.cs çözüm defterinde seçme")

6. Xcode'nın arabirimi Oluşturucu film şeridi yeniden açın.
7. **WindowController.h** dosya kullanılabilir olacaktır: 

    [![WindowController.h dosya](toolbar-images/windowcontroller03.png "WindowController.h dosyası")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Oluşturma ve araç çubuklarını xcode'da koruma

Araç çubukları oluşturulur ve Xcode'nın arabirimi Oluşturucu ile korunur. Bir araç için bir uygulama eklemek için uygulamanın birincil film şeridi Düzenle (Bu durumda **Main.storyboard**) içinde çift tıklatarak **çözüm paneli**:

![Çözüm defterinde Main.Storyboard açma](toolbar-images/edit01.png "Main.storyboard çözüm defterinde açma")

İçinde **kitaplığı denetçisi**, "aracı" girin **arama kutusu** tüm kullanılabilir araç çubuğu öğelerinin görmeyi kolaylaştırmak için:

![Kitaplık denetçisi filtre araç çubuğu öğeleri göstermek için](toolbar-images/edit02.png "kitaplığı denetçisi, araç çubuğu öğeleri göstermek için filtrelenir")

Bir araç penceresinde sürükleyin **arabirimi Düzenleyicisi**. Seçili araç ile özellikleri ayarlayarak davranışını yapılandırma **öznitelikleri denetçisi**:

![Bir araç için öznitelikler denetçisine](toolbar-images/edit04.png "öznitelikleri denetçisi bir araç çubuğu")

Aşağıdaki özellikler mevcuttur:

1. **Görüntü** -araç simgeler, metin ya da her ikisini de görüntüler olup olmadığını denetler
2. **Başlatma sırasında görünür** -seçtiyseniz, araç varsayılan olarak görünür.
3. **Özelleştirilebilir** -seçtiyseniz, kullanıcıların düzenleyebilir ve araç çubuğunu özelleştirme.
4. **Ayırıcı** -ince yatay çizgi seçtiyseniz, pencerenin içeriği araç ayırır.
5. **Boyutu** -araç boyutunu ayarlar
6. **Otomatik kaydetme** -seçtiyseniz, uygulama kullanıcının araç yapılandırma değişiklikleri uygulama başlatır korunur.

Seçin **otomatik kaydetme** seçeneği ve diğer tüm özellikleri kendi varsayılan ayarlarında bırakın. 

Araç çubuğunda açma sonra **arabirimi hiyerarşi**, araç çubuğu öğesi seçerek özelleştirme iletişim kutusunu Getir:

![Araç çubuğunu özelleştirme](toolbar-images/edit05.png "araç çubuğunu özelleştirme")

Zaten tasarlarken uygulama için varsayılan araç çubuğu araç parçası olan öğeleri özelliklerini ayarlamak ve araç çubuğunu özelleştirme zamanı seçmek bir kullanıcı için ek araç çubuğu öğeleri sağlamak için bu iletişim kutusunu kullanın. Araç çubuğuna öğeler eklemek için bunları sürükleyin **kitaplığı denetçisi**:

![Kitaplık denetçisi](toolbar-images/edit06.png "kitaplığı denetçisi")

Aşağıdaki araç çubuğu öğeleri eklenebilir:

- **Görüntü araç çubuğu öğesi** -simge olarak özel bir görüntü ile araç çubuğu öğesi.
- **Esnek alanı araç çubuğu öğesi** -sonraki araç çubuğu öğeleri yaslamak için kullanılan esnek alanı. Örneğin, bir veya daha fazla araç çubuğu öğeleri esnek alanı araç çubuğu öğesi tarafından izlenen ve başka bir araç çubuğu öğesi araç çubuğunun sağ tarafında için son öğeyi sabitlemek.
- **Araç çubuğu öğesi boşluk** -sabit alan araç çubuğundaki öğeler arasında
- **Ayırıcı araç çubuğu öğesi** -iki veya daha fazla araç çubuğu öğeleri arasında gruplandırma için görünür bir ayırıcı
- **Araç çubuğu öğesi özelleştirme** -kullanıcıların araç çubuğunu özelleştirme olanak tanır
- **Araç çubuğu öğesi yazdırma** -açık belge yazdırma olanağı sağlar
- **Renkleri araç çubuğu öğesi Göster** -standart sistem renk seçici görüntüler: 

     ![Sistem Renk Seçici](toolbar-images/edit07.png "sistem renk seçici")

- **Yazı tipi araç çubuğu öğesi Göster** -standart sistem yazı tipi iletişim kutusu görüntüler: 

     ![Yazı tipi Seçici](toolbar-images/edit08.png "yazı tipi Seçici")

> [!IMPORTANT]
> Daha sonra görülür gibi arama alanlarını, bölümlenmiş denetimleri ve yatay kaydırıcılar gibi birçok standart Cocoa UI denetimleri de bir araç çubuğuna eklenebilir.

### <a name="adding-an-item-to-a-toolbar"></a>Bir araç çubuğuna öğe ekleme

Bir araç için bir öğe eklemek için araç çubuğunda seçin **arabirimi hiyerarşi** görünmesi özelleştirme iletişim neden öğelerinden birini tıklatın. Ardından, yeni bir öğeye sürükleyin **kitaplığı denetçisi** için **izin araç çubuğu öğeleri** alanı:

![Araç çubuğunu özelleştirme iletişim izin araç çubuğu öğeleri](toolbar-images/add01.png "araç çubuğunu özelleştirme iletişim izin araç çubuğu öğeleri")

Yeni bir öğe varsayılan araç çubuğu parçası olduğundan emin olmak için sürükleyin **varsayılan araç çubuğu öğeleri** alanı: 

![Araç çubuğu öğesi sürükleyerek yeniden sıralama](toolbar-images/add02.png "sürükleyerek araç çubuğu öğesi yeniden sıralama")

Varsayılan araç çubuğu öğeleri yeniden sıralamak için bunları sola veya sağa sürükleyin.

Ardından, kullanın **öznitelikleri denetçisi** öğesinin varsayılan özelliklerini ayarlamak için:

![Öznitelikleri Inspector'ı kullanarak bir araç çubuğu öğesi özelleştirme](toolbar-images/add03.png "öznitelikleri Inspector'ı kullanarak bir araç çubuğu öğesi özelleştirme")

Aşağıdaki özellikler mevcuttur:

- **Görüntü adı** -öğe için bir simgesi olarak kullanılacak resim
- **Etiket** -araç çubuğu öğesi için görüntülenecek metin
- **Palet etiketi** -öğesi için görüntülenecek metin **izin araç çubuğu öğeleri** alanı
- **Etiket** -kod öğesinde tanımlamanıza yardımcı olan isteğe bağlı, benzersiz tanımlayıcı.
- **Tanımlayıcı** -tanımlar araç çubuğu öğesi türü. Özel bir değer, kodda bir araç çubuğu öğesini seçmek için kullanılabilir.
- **Seçilebilir** -işaretlenmişse öğesi bir açık/kapalı düğmesine gibi hareket eder.

> [!IMPORTANT]
> Öğe ekleme **izin araç çubuğu öğeleri** alan var, ancak olmayan kullanıcılar için özelleştirme seçenekleri sağlamak için varsayılan araç. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>Bir araç çubuğuna diğer UI denetimleri ekleme

Alanları gibi birkaç Cocoa kullanıcı Arabirimi öğeleri aramak ve bölümlenmiş denetimleri de bir araç çubuğuna eklenebilir.

Bu denemek için araç çubuğunda açmak **arabirimi hiyerarşi** ve özelleştirme iletişim kutusunu açmak için bir araç çubuğu öğesini seçin. Sürükleme bir **arama alanı** gelen **kitaplığı denetçisi** için **izin araç çubuğu öğeleri** alanı:

![Araç çubuğunu özelleştirme iletişim kutusunu kullanarak](toolbar-images/add05.png "araç çubuğunu özelleştirme iletişim kutusunu kullanarak")

Buradan, arabirimi Oluşturucu arama alanı yapılandırmak ve bir eylem veya çıkış koduyla kullanıma sunmak için kullanın.

## <a name="built-in-toolbar-item-support"></a>Yerleşik araç çubuğu öğesi desteği

Birkaç Cocoa kullanıcı Arabirimi öğeleri varsayılan olarak standart araç çubuğu öğeleri ile etkileşim. Örneğin, sürükleyin bir **metin görünümü** uygulamanın penceresi üzerine ve içerik alanı dolduracak şekilde yerleştir:

[![Metin görünümü uygulamaya ekleme](toolbar-images/edit09.png "uygulamaya metin görünümü ekleme")](toolbar-images/edit09-large.png#lightbox)

Visual Studio için Xcode ile eşitleme, uygulamayı çalıştırın, bazı metinleri girin, seçin ve'Mac dönün belgeyi kaydetmek **renkleri** araç çubuğu öğesi. Metin görünümü ile Renk Seçici otomatik olarak çalışır dikkat edin:

![Metin görünümü ve Renk Seçici yerleşik araç işlevselliği](toolbar-images/edit10.png "metin görüntüleme ve Renk Seçici yerleşik araç işlevi")

## <a name="using-images-with-toolbar-items"></a>Araç çubuğu öğeleri ile görüntüleri kullanma

Kullanarak bir **görüntü araç çubuğu öğesi**, herhangi bir bit eşlem görüntü eklenen **kaynakları** klasörü (ve bir yapı eylemi belirtilen **paket kaynak**) araç çubuğunda simge olarak görüntülenir:

1. Mac için Visual Studio içinde **çözüm paneli**, sağ tıklatın **kaynakları** klasörü ve seçin **Ekle** > **dosyaları Ekle** .
2. Gelen **dosyaları Ekle** iletişim kutusunda, istenen görüntülere gidin, bunları seçin ve tıklatın **açık** düğmesi: 

    [![Eklemek için görüntüleri seçme](toolbar-images/edit11.png "eklemek için görüntüleri seçme")](toolbar-images/edit11-large.png#lightbox)

3. Seçin **kopyalama**, denetleme **aynı eylem seçilen tüm dosyaları için kullanan**, tıklatıp **Tamam**:

    ![Eklenen görüntüleri kopyalama eylemi seçme](toolbar-images/edit12.png "eklenen görüntüleri kopyalama eylemi seçme")

4. İçinde **çözüm paneli**, çift **MainWindow.xib** Xcode'da açın.

5. Araç çubuğunda seçin **arabirimi hiyerarşi** özelleştirme iletişim kutusunu açmak için öğelerinden birini tıklatın.

6. Sürükleme bir **görüntü araç çubuğu öğesi** gelen **kitaplığı denetçisi** araç çubuğunun için **izin araç çubuğu öğeleri** alanı: 

    ![Araç çubuğu öğeleri izin verilen alanı bir görüntü araç çubuğu öğesi eklenen](toolbar-images/edit14.png "bir görüntü araç çubuğu öğesi izin araç çubuğu öğeleri alana eklendi")

7. İçinde **öznitelikleri denetçisi**, Mac için Visual Studio'da yeni eklenenler görüntüyü seçin: 

    ![Araç çubuğu öğesi için özel bir görüntü ayarlama](toolbar-images/edit15.png "araç çubuğu öğesi için özel bir görüntü ayarlama")

8. Ayarlama **etiket** "Çöp" için ve **Palet etiketi** "Belge silmek için": 

    ![Araç çubuğu ayarı madde etiketi ve palet etiketi](toolbar-images/edit16.png "araç ayarı madde etiketi ve palet etiketi")

9. Sürükleme bir **ayırıcı araç çubuğu öğesi** gelen **kitaplığı denetçisi** araç çubuğunun için **izin araç çubuğu öğeleri** alanı: 

    [![Araç çubuğu öğeleri izin alan ayırıcı araç çubuğu öğesi eklenen](toolbar-images/edit17.png "bir ayırıcı araç çubuğu öğesi izin araç çubuğu öğeleri bölgesine eklenen")](toolbar-images/edit17-large.png#lightbox)

10. Ayırıcı öğenin ve "Çöp" öğesine sürükleyin **varsayılan araç çubuğu öğeleri** alan ve araç sırasını öğeleri kümesi soldan sağa aşağıdaki gibi (renkleri, yazı tipi, ayırıcı, çöp, esnek boşluk, yazdırma): 

    ![Varsayılan araç çubuğu öğeleri](toolbar-images/edit18.png "varsayılan araç çubuğu öğeleri")

11. Değişiklikleri kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Yeni araç çubuğu varsayılan olarak görüntülendiğini doğrulamak için uygulamayı çalıştırın:

![Özelleştirilmiş varsayılan öğelerine sahip bir araç çubuğu](toolbar-images/edit19.png "özelleştirilmiş varsayılan öğelerine sahip bir araç çubuğu")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Araç çubuğu öğeleri çıkışlar ve Eylemler ile gösterme

Bir araç veya araç çubuğu öğesi kodda erişmek için bu prizine veya bir eylem bağlı olması gerekir:

1. İçinde **çözüm paneli**, çift **Main.storyboard** Xcode'da açın.
2. "WindowController", ana pencereyi denetleyicisine atanmış özel bir sınıf emin **kimlik denetçisi**:

    [![Pencere denetleyici için özel bir sınıf ayarlamak için kimlik Inspector'ı kullanarak](toolbar-images/edit20a.png "penceresi denetleyici için özel bir sınıf ayarlamak için kimlik Inspector'ı kullanarak")](toolbar-images/edit20a-large.png#lightbox)

3. Ardından, araç çubuğu öğesi seçin **arabirimi hiyerarşi**: 

    ![Araç çubuğu öğesi arabirimi hiyerarşisinde seçme](toolbar-images/edit20.png "arabirimi hiyerarşisinde araç çubuğu öğesi seçme")  

4. Açık **Yardımcısı Görünüm**seçin **WindowController.h** dosya ve denetim sürükleme araç çubuğu öğesi için **WindowController.h** dosya.
5. Ayarlama **bağlantı** için yazın **eylem**, "trashDocument" girin **adı**, tıklatıp **Bağlan** düğmesi: 

    [![Araç çubuğu öğesi için bir eylem yapılandırma](toolbar-images/edit23.png "araç çubuğu öğesi için bir eylem yapılandırma")](toolbar-images/edit23-large.png#lightbox)

6. Kullanıma **metin görünümü** "documentEditor" adlı bir çıkış olarak **ViewController.h** dosyası: 

    [![Metin görünümü için bir çıkış yapılandırma](toolbar-images/edit24.png "prizine metin görünümü için yapılandırma")](toolbar-images/edit24-large.png#lightbox)

7. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Mac için Visual Studio'da Düzenle **ViewController.cs** dosya ve aşağıdaki kodu ekleyin:

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

Ardından, düzenleme **WindowController.cs** dosya ve altına aşağıdaki kodu ekleyin `WindowController` sınıfı:

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

Uygulama çalışırken **çöp** araç çubuğu öğesi etkin olacaktır:

![Etkin çöp öğeyi içeren bir araç çubuğu](toolbar-images/edit25.png "etkin çöp öğeyi içeren bir araç çubuğu")

Dikkat **çöp** araç çubuğu öğesi artık metni silmek için kullanılabilir.

## <a name="disabling-toolbar-items"></a>Araç çubuğu öğeleri devre dışı bırakma

Öğenin araç çubuğundaki devre dışı bırakmak için bir özel Oluştur `NSToolbarItem` sınıfı ve geçersiz kılma `Validate` yöntemi. Ardından, arabirimi Oluşturucusu'nda, özel tür bırakmak istediğiniz öğesine atayın.

Özel bir oluşturmak için `NSToolbarItem` sınıfı, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** . Seçin **genel** > **boş sınıfı**, "ActivatableItem" girin **adı**, tıklatıp **yeni** düğmesi: 

![Mac için Visual Studio'da boş bir sınıf ekleme](toolbar-images/custom01.png "Mac için Visual Studio'da boş bir sınıf ekleme")

Ardından, düzenleme **ActivatableItem.cs** gibi görünecek şekilde dosyası:

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

Çift **Main.storyboard** Xcode'da açın. Seçin **çöp** araç çubuğu öğesi yukarıda oluşturduğunuz ve "ActivatableItem" değiştirmek kendi sınıfı **kimlik denetçisi**:

![Araç çubuğu öğesi için özel bir sınıf ayarı](toolbar-images/custom02.png "araç çubuğu öğesi için özel bir sınıf ayarlama")

Adlı prizine oluşturma `trashItem` için **çöp** araç çubuğu öğesi. Değişiklikleri kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün. Son olarak, açık **MainWindow.cs** ve güncelleştirme `AwakeFromNib` gibi görünecek şekilde yöntemi:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

Uygulamayı çalıştırın ve unutmayın **çöp** öğesi artık araç çubuğunda devre dışı:

![Etkin olmayan çöp öğeyi içeren bir araç çubuğu](toolbar-images/custom03.png "etkin olmayan çöp öğeyi içeren bir araç çubuğu")

## <a name="summary"></a>Özet

Bu makalede, araç çubukları ve Xamarin.Mac uygulama araç çubuğu öğeleri ile çalışma ayrıntılı bir bakış sürdü. Oluşturmak ve araç çubuklarını Xcode'nın arabirimi Oluşturucu sürdürmek nasıl, nasıl bazı kullanıcı Arabirimi denetimlerini araç çubuğu öğeleri ile otomatik olarak çalışır, C# kodunda araç çubukları ile nasıl çalışılacağını ve etkinleştirme ve araç öğelerini devre dışı açıklanmaktadır.


## <a name="related-links"></a>İlgili bağlantılar

- [MacToolbar (örnek)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Araç çubukları için İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [Araç çubukları giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
