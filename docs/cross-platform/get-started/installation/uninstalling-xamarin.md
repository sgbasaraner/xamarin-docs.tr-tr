---
title: Uninstalling Xamarin
description: "Xamarin ürünleri bir bilgisayardan kaldırma"
ms.topic: article
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 1b998628efc133590a543dd45730070a457d61d5
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2018
---
# <a name="uninstalling-xamarin"></a>Uninstalling Xamarin

> [!IMPORTANT]
> Bu makalede, Xamarin Studio veya diğer Xamarin ürünleri Mac veya Windows bilgisayardan kaldırmak açıklanmaktadır. Mac için Visual Studio kaldırma hakkında daha fazla bilgi için bkz [kaldırma](https://docs.microsoft.com/visualstudio/mac/uninstall) docs.microsoft.com Kılavuzu

Platformlar arası uygulama geliştirme, Visual Studio'da Xamarin Studio ve Xamarin destek gibi diğer uygulamalar için uzantılar gibi tek başına uygulamalar dahil olmak üzere etkinleştirme Xamarin ürünleri mevcuttur.

Bu kılavuz kaldırma macOS veya Windows Visual Studio'dan Xamarin işlevi açıklanmaktadır:

1.  [Uninstalling Xamarin Studio](#uninstallxamarinstudio)
  1.  [Mono kaldırma](#uninstallmono)
  1.  [Uninstalling Xamarin.Android](#uninstallandroid)
  1.  [Uninstalling Xamarin.iOS](#uninstallios)
  1.  [Uninstalling Xamarin.Mac](#uninstallmac)
  2.  [Kaldırma denetçisi ve çalışma kitapları](#uninstallworkbooks)
1.  [Xamarin Windows'dan kaldırma](#uninstallwindows)
  1.  [Xamarin Visual Studio 2015 ve önceki sürümleri kaldırma](#uninstallvs2015)
  1.  [Visual Studio 2017'den kaldırma Xamarin](#uninstallvs2017)
1.  [Mac için Visual Studio kaldırma](#uninstallvsmac)

Xamarin Evrensel Yükleyicisi'ni kullanarak yeniden yüklemeniz gerekirse, her zaman bilgisayar ilk kez yeniden önerilir.

## <a name="uninstalling-xamarin-on-mac"></a>Xamarin Mac üzerinde kaldırma

Bu kılavuz, her ürünün ayrı ayrı ilgili bölümüne giderek kaldırmak için kullanılabilir. Tüm Xamarin araç takımı, tüm aşamalardaki bu kılavuzu izleyerek kaldırılabilir.

Kullanma konusunda yardım için [betik kaldırma](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh), atlamak [kaldırma komut dosyası kullanarak](#uninstallscript) bu kılavuzun sonundaki.

<a name="uninstallxamarinstudio" />

### <a name="uninstall-xamarin-studio"></a>Uninstall Xamarin Studio

Xamarin Studio Mac kaldırma ilk adımı bulmaktır **Xamarin Studio.app** içinde **/Applications** directory sürükleyin **çöp kutusu**. Alternatif olarak, sağ tıklatın ve seçin **taşımak için çöp** aşağıda gösterildiği gibi:

 [![](uninstalling-xamarin-images/image1.png "Alternatif olarak, sağ tıklayın ve aşağıda gösterildiği gibi çöp Git seçin")](uninstalling-xamarin-images/image1.png#lightbox)

Bu uygulama paketi silmek Xamarin Studio kaldırır, ancak hala bir dosya sistemi için Xamarin ile ilgili diğer dosyaları vardır.

Xamarin Studio tüm izlerini kaldırmak için aşağıdaki komutları terminale çalıştırılmalıdır:

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Mono SDK'sı (MDK) kaldırma

Mono Microsoft .NET Framework'ün açık kaynak uygulamasıdır ve tüm Xamarin Products—Xamarin.iOS, Xamarin.Android ve Xamarin.Mac tarafından bu platformlar geliştirilmesini C# ' ta izin vermek için kullanılır.

> [!IMPORTANT]
> Not: De Unity gibi Mono kullanan diğer uygulamalar Xamarin dışında vardır. Başka bir bağımlılık üzerinde Mono kaldırmadan önce emin olun.

Mono Framework bir makineden kaldırmak için terminale aşağıdaki komutları çalıştırın:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Uninstall Xamarin.Android

Bir Android SDK ve Java SDK'sı gibi Xamarin.Android kullanımını ve yükleme için gerekli öğe sayısı vardır. Bu gerekli bileşenler hakkında daha fazla bilgi bulunur [el ile yükleme](https://docs.microsoft.com/visualstudio/mac/installation/) Kılavuzu.

Xamarin.Android kaldırmak için aşağıdaki komutları kullanın:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Android SDK ve Java SDK kaldırma

Android SDK'sı, Android uygulamaları geliştirme için gereklidir. Android SDK'ın tüm bölümleri tamamen kaldırmak için dosyasını bulun **~/Library/Developer/Xamarin/** ve taşımak **çöp**aşağıda gösterildiği gibi:

 [![](uninstalling-xamarin-images/image2.png "Android SDK'ın tüm bölümleri tamamen kaldırmak için dosyayı bulun ve taşımak için çöp kutusu, aşağıda gösterildiği gibi")](uninstalling-xamarin-images/image2.png#lightbox)

Mac OS X bir parçası olarak paketlenmiş öncesi zaten olduğundan Java SDK'sı (JDK) kaldırılması, gerek yoktur.

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Uninstall Xamarin.iOS

Xamarin.iOS iOS Xamarin Studio'yu bir Mac bilgisayar üzerinde C# veya F # kullanarak uygulama geliştirme sağlar.
Xamarin yapı konağı da Visual Studio iOS geliştirme için izin vermek için Xamarin.iOS önceki sürümleriyle otomatik olarak yüklendi. Hem bir makineden kaldırmak için aşağıdaki adımları izleyin:

Aşağıdaki komutları terminale tüm Xamarin.iOS dosyaları dosya sisteminden kaldırmak için kullanın:

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
```

#### <a name="uninstall-the-mac-build-host"></a>Mac yapı konak kaldırma

Not:, zaten Xamarin 4 çalıştırmak için yapı konağı uygulamayı kaldırmak için aşağıdaki komutu Terminal güncelleştirdiyseniz bu zaten kaldırılmış olabilir:

```bash
sudo rm -rf "/Applications/Xamarin.iOS Build Host.app"
```

Yapı konağı işlem veya `launchd` iş hala çalışıyor olabilir veya belirli bağlantı noktalarını dinler.
Çalıştırarak durumunu denetleyebilirsiniz `launchctl list | grep com.xamarin.mtvs.buildserver` Terminal.

```bash
sudo launchctl unload /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
sudo rm -f /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Uninstall Xamarin.Mac

Xamarin Studio başarıyla kaldırıldıktan sonra Xamarin.Mac makinenizden Mac lisansıyla ve ürün sırasıyla yok etmek aşağıdaki iki komutu kullanılarak kaldırılabilir:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks-and-inspector"></a>Çalışma kitapları ve Inspector kaldırma

Aşağıdaki Bash komutu Xamarin denetçisi ve çalışma kitaplarını sürümü 1.2.2 kaldırır ve yukarıdaki:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Önceki sürümler için bkz: [çalışma kitaplarını](~/tools/workbooks/install.md#uninstall-macos) Kılavuzu kaldırın.

### <a name="uninstall-the-xamarin-installer"></a>Xamarin yükleyici kaldırma

Xamarin Evrensel yükleyici tüm izlerini kaldırmak için aşağıdaki komutları kullanın:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Preferences/Xamarin/
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Kaldırma komut dosyası kullanma

[Betik kaldırma](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh) Xamarin bir bilgisayardan kaldırır. Kaldırma komut dosyası kullanmak için:

1.  Kaldırma komut dosyası karşıdan yükleme ve indirme konumu not edin. Varsayılan olarak **/indirmeleri** dizin.
1.  Açık **Terminal** ve komut dosyasını indirdiğiniz için çalışma dizini değiştirin:

        $ cd /location/of/file

1. Komut dosyası yürütülebilir ve Çalıştır ile **sudo**:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

1. Son olarak, kaldırma komut dosyası silin.

Bu noktada, Xamarin bilgisayarınızdan kaldırılması.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Windows'ta Xamarin kaldırma

<a name="uninstallvs2015" />

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 ve önceki

Xamarin ile bir Windows makineden kaldırılması **Denetim Masası**. Gidin **programlar ve Özellikler** veya **Programlar > Program Kaldır** aşağıda gösterildiği gibi:

 [![](uninstalling-xamarin-images/image3.png "Programlar ve özellikler veya programları kaldırma programı aşağıda gösterildiği gibi gidin") ](uninstalling-xamarin-images/image3.png#lightbox) [ ![ ] (uninstalling-xamarin-images/image3.png "programlar ve özellikler veya programları kaldırmak için bir Program olarak gidin Burada gösterilen")](uninstalling-xamarin-images/image3.png#lightbox)

Xamarin Studio kaldırmak için bulma **Xamarin Studio 5.x.x** Programlar'ı tıklatın ve listedeki **kaldırma** düğmesi. Visual Studio ve SDK'ları için Xamarin uzantıyı kaldırmak için bulma **Xamarin** Programlar'ı tıklatın ve listedeki **kaldırma**. Bunlar aşağıdaki ekran görüntüsünde gösterilmiştir:

 [![](uninstalling-xamarin-images/image4a.png "Bunlar bu ekran görüntüsünde gösterilen")](uninstalling-xamarin-images/image4a.png#lightbox)

Bu programlar da tamamen tüm Xamarin bileşenleri kaldırılabilir:

-  Android SDK


  [![](uninstalling-xamarin-images/image5.png "Bu programlar da tamamen tüm Xamarin bileşenleri Kaldırılabilir")](uninstalling-xamarin-images/image5.png#lightbox)
-  GTK#


  [![](uninstalling-xamarin-images/image6.png "Bu programlar da tamamen tüm Xamarin bileşenleri Kaldırılabilir")](uninstalling-xamarin-images/image6.png#lightbox)
-  Xamarin Evrensel yükleyici


 [![](uninstalling-xamarin-images/image7.png "Bu programlar da tamamen tüm Xamarin bileşenleri Kaldırılabilir")](uninstalling-xamarin-images/image7.png#lightbox)
-  Java SDK'sını (olabilir gibi başka bir bağımlılık üzerinde bunu kaldırırken dikkatli olun)


 [![](uninstalling-xamarin-images/image8.png "Olabilir gibi başka bir bağımlılık üzerinde Java SDK'sı kaldırırken dikkatli olun")](uninstalling-xamarin-images/image8.png#lightbox)

Visual Studio tamamen kaldırmak için izleyin [Microsoft'un yönergeleri](https://msdn.microsoft.com/library/mt720585.aspx).


<a name="uninstallvs2017" />

## <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin yükleyici uygulamasını kullanarak Visual Studio 2017 kaldırılabilir:

1. Kullanım **Başlat menüsünden** açmak için **Visual Studio yükleyicisi**.

  [![](uninstalling-xamarin-images/vs2017-01-sml.png "Visual Studio Yükleyicisi'ni başlatın")](uninstalling-xamarin-images/vs2017-01.png#lightbox)

1. Tuşuna **Değiştir** değiştirmek istediğiniz örneği düğmesi.

  [![](uninstalling-xamarin-images/vs2017-02-sml.png "Değiştir düğmesine basın")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

1. İçinde **iş yükleri** sekmesinde, seçimini **mobil geliştirme .NET ile** seçeneği (içinde **mobil ve oyun** bölümü).

  [![](uninstalling-xamarin-images/vs2017-03-sml.png "Mobil Geliştirme iş yükü seçeneğinin işaretini kaldırın")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

1. Tıklatın **Değiştir** penceresinin sağ alt düğmesini.
1. Yükleyici (Yükleyici herhangi bir değişiklik yapabilmeniz için önce Visual Studio 2017 kapatılmalıdır) XML'deki seçili bileşenleri kaldırır.

  [![](uninstalling-xamarin-images/vs2017-04-sml.png "Değiştir düğmesine basın")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Tek tek Xamarin bileşenleri (örneğin, profil oluşturucu veya çalışma kitaplarını) geçerek kaldırılması **bileşenleri tek tek** 3. adım sekmesi ve kaldırarak belirli bileşenleri:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Tek tek bileşenlerini Kaldır")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Visual Studio 2017 tamamen kaldırmak için seçin **kaldırma** üç çubuğundan menü öğesinin yanında **başlatma** düğmesi.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Visual Studio tamamen kaldırın")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> **Uyarı:** Visual Studio yüklüyse yan yana (SxS) – bir yayın ve önizleme sürümü – gibi iki (veya daha fazla) örneklerini varsa bir örnek Xamarin işlevselliğinin diğer Visual Studio Örnekleri kaldırma şunları içerir:
>
> - Xamarin Profiler
> - Xamarin çalışma kitaplarını/denetçisi
> - Xamarin uzaktan iOS simülatörü
> - Apple Bonjour SDK'sı
>
> Belirli koşullar altında SxS örneklerden birini kaldırma, bu özelliklerin yanlış kaldırılmasına neden olabilir. Bu sistemde SxS örneği kaldırılmasından sonra kalan Visual Studio Örnekleri Xamarin platformda performansını düşürebilir.
>
>Bu çalıştırarak çözülebilir **onarım** eksik bileşenlerini yeniden yüklemek Visual Studio yükleyicisinde seçeneği.

<a name="uninstallvsmac" />

## <a name="uninstalling-visual-studio-for-mac"></a>Mac için Visual Studio kaldırma

Mac için Visual Studio'yu kaldırmak ancak Xamarin Studio kullanmaya devam etmek için bulun **Visual Studio.app** içinde **/Applications** dizin için Can. çöp sürükleyin Alternatif olarak, sağ tıklatın ve seçin **taşımak için çöp** aşağıda gösterildiği gibi:

 [![](uninstalling-xamarin-images/image9.png "Visual Studio simgesine sağ tıklayın ve çöp Git seçin")](uninstalling-xamarin-images/image9.png#lightbox)

Xamarin makinenizden tamamen kaldırmak için önce Visual Studio Mac için silin ve sonra listelenen adımları izleyin [kaldırma Xamarin Studio](#uninstallxamarinstudio) bölümü.

## <a name="summary"></a>Özet

Makine aracılığıyla Aranan Xamarin tamamen Terminal komutları kullanımı ile bir Mac alanından kaldırma sırasında bu makalede, bir Windows Xamarin kaldırma yanı sıra **programlar ve Özellikler** seçeneği (Visual Studio 2015 için ve daha önce) ve kullanarak **Visual Studio yükleyicisi** Visual Studio 2017 için.


## <a name="related-links"></a>İlgili bağlantılar

- [Komut dosyası (örnek) kaldırma](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)
