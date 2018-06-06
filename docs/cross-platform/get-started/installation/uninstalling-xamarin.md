---
title: Xamarin kaldırma
description: Bu belge, Xamarin Mac ve Windows üzerinde kaldırmak açıklar. Mono, Xamarin.Android, Xamarin.iOS ve diğer araçları kaldırma hakkında belirli yönergeler sağlar.
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: d5cf15b8ecd225fb75a3cfa0017cb84bc13cce1b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782027"
---
# <a name="uninstalling-xamarin"></a>Xamarin kaldırma

Bu kılavuz, Xamarin macOS veya Windows Visual Studio'dan çıkartın açıklanmaktadır.

Xamarin Evrensel Yükleyicisi'ni kullanarak yeniden yüklemeniz gerekirse, her zaman bilgisayar ilk kez yeniden önerilir.

## <a name="uninstalling-xamarin-on-macos"></a>Xamarin üzerinde macOS kaldırma

Bu kılavuz, her ürünün ayrı ayrı ilgili bölümüne giderek kaldırmak için kullanılabilir. Listelenen ürünler içerir, tüm Xamarin araç takımı, tüm aşamalardaki bu kılavuzu izleyerek kaldırılabilir:

- [Mono](#uninstallmono)
- [Xamarin.Android](#uninstallandroid)
- [Xamarin.iOS](#uninstallios)
- [Xamarin.Mac](#uninstallmac)
- [Denetleyici ve çalışma kitapları](#uninstallworkbooks)
- [Xamarin Profiler](#uninstallprofiler)
- [Yükleyici](#uninstallinstaller)

> [!TIP]
> Sağladık bir [betik kaldırma](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh) Xamarin macOS makinenizden kaldırırken kullanabilirsiniz. Komut dosyası kullanma hakkında daha fazla bilgi için bkz: [kaldırma komut dosyası kullanarak](#uninstallscript) bu kılavuzdaki bölüm.

### <a name="uninstalling-visual-studio-for-mac"></a>Mac için Visual Studio kaldırma

Xamarin Mac kaldırma ilk adımı bulmaktır **Visual Studio.app** içinde **/Applications** directory sürükleyin **çöp kutusu**. Alternatif olarak, sağ tıklatın ve seçin **taşımak için çöp** aşağıdaki görüntüde gösterildiği gibi:

![Çöp Kutusu Visual Studio uygulamaya taşı](uninstalling-xamarin-images/uninstall-image1.png)

Bu uygulama paketi silme olabilir ancak hala bir dosya sistemi için Xamarin ile ilgili diğer dosyaları Visual Studio, Mac için kaldırır.

Mac için Visual Studio tüm izlerini kaldırmak için terminale aşağıdaki komutları çalıştırın:

```bash
sudo rm -rf "/Applications/Visual Studio.app"
rm -rf ~/Library/Caches/VisualStudio
rm -rf ~/Library/Preferences/VisualStudio
rm -rf ~/Library/Preferences/Visual\ Studio
rm -rf ~/Library/Logs/VisualStudio
rm -rf ~/Library/VisualStudio
rm -rf ~/Library/Preferences/Xamarin/
rm -rf ~/Library/Developer/Xamarin
rm -rf ~/Library/Application\ Support/VisualStudio
rm -rf ~/Library/Application\ Support/VisualStudio/7.0/LocalInstall/Addins/
```

> [!NOTE]
> Mac için Visual Studio kaldırma hakkında daha fazla bilgi için bkz [kaldırma](https://docs.microsoft.com/visualstudio/mac/uninstall) docs.microsoft.com Kılavuzu

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Mono SDK'sı (MDK) kaldırma

Mono .NET Framework'ün açık kaynak uygulamasıdır ve tüm Xamarin Products—Xamarin.iOS, Xamarin.Android ve Xamarin.Mac tarafından bu platformlar geliştirilmesini C# ' ta izin vermek için kullanılır.

> [!WARNING]
> Ayrıca Unity gibi Mono kullanan diğer uygulamalar Xamarin dışında vardır. Başka bir bağımlılık üzerinde Mono kaldırmadan önce emin olun.

Mono Framework kaldırmak için terminale aşağıdaki komutları çalıştırın:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Xamarin.Android kaldırma

Bir Android SDK ve Xamarin.Android kaldırırken kaldırılmaları gerekir Java SDK gibi Xamarin.Android kullanırken gerekli olan öğe sayısı vardır. Bu bölümde, gerekli tüm bölümleri kaldırma aracılığıyla kılavuzluk eder.

Xamarin.Android kaldırmak için terminale aşağıdaki komutları çalıştırın:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Android SDK ve Java SDK kaldırma

Android SDK'sı, Android uygulamaları geliştirme için gereklidir. Android SDK'ın tüm bölümleri tamamen kaldırmak için dosyasını bulun **~/Library/Developer/Xamarin/** ve taşımak **çöp**.

Mac OS X bir parçası olarak paketlenmiş öncesi zaten olduğundan Java SDK'sı (JDK) kaldırılması, gerek yoktur.

#### <a name="uninstall-android-avd"></a>Android AVD kaldırma

> [!WARNING]
> Ayrıca Android AVD ve Android Studio gibi bu ek android bileşenleri kullanan diğer uygulamalar Visual Studio dışında Mac için vardır.
> Bu dizin kaldırma Android Studio'da bölüneceği projeleri neden olabilir. 

Tüm Android AVDs ve ek Android bileşenlerini kaldırmak için aşağıdaki komutu kullanın:

```bash
rm -rf ~/.android
```

Yalnızca Android AVDs kaldırmak için aşağıdaki komutu kullanın:

```bash
rm -rf ~/.android/avd
```

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Xamarin.iOS kaldırma

Xamarin.iOS iOS C# veya F # kullanarak uygulama geliştirme sağlar. Xamarin.iOS bir makineden kaldırmak için aşağıdaki adımları izleyin:

Tüm Xamarin.iOS dosyaları kaldırmak için terminale aşağıdaki komutları kullanın:

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
sudo pkgutil --forget com.xamarin.xamarin.ios.pkg
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Xamarin.Mac kaldırma

Xamarin.Mac ürün ve Mac lisanstan sırasıyla yok etmek aşağıdaki iki komutu kullanarak makinenizden kaldırılabilir:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks-and-inspector"></a>Çalışma kitapları ve Inspector kaldırma

Değiştirmek için Xamarin denetçisi ve çalışma kitaplarını sürüm 1.2.2 kaldırın ve yukarıdaki terminale aşağıdaki komutları kullanın:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Önceki sürümler için bkz: [çalışma kitaplarını](~/tools/workbooks/install.md#uninstall-macos) Kılavuzu kaldırın.

<a name="uninstallprofiler" />

### <a name="uninstall-the-xamarin-profiler"></a>Xamarin profil oluşturucu kaldırma

Xamarin profil oluşturucu kaldırmak için terminale aşağıdaki komutları kullanın:

```bash
sudo rm -rf "/Applications/Xamarin Profiler.app"
```

<a name="uninstallinstaller" />

### <a name="uninstall-the-xamarin-installer"></a>Xamarin yükleyici kaldırma

Xamarin Evrensel yükleyici tüm izlerini kaldırmak için aşağıdaki komutları kullanın:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Caches/VisualStudioInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Logs/VisualStudioInstaller/
rm -rf ~/Library/Preferences/Xamarin/
rm -rf "~/Library/Preferences/Visual Studio/"
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Kaldırma komut dosyası kullanma

Bu kaldırma komut dosyası, Mac ve onun ilişkili bir Git Xamarin bileşenleri için Visual Studio'yu kaldırmak sağlar.

Betik makalede bulunan komutlarının çoğunu içerir. Komut dosyası iki ana atlamalar vardır ve olası dış bağımlılıklar nedeniyle bulunmamaktadır:

- Mono kaldırma
- Android AVD kaldırma

Komut dosyasını çalıştırmak için aşağıdaki adımları uygulayın:

1. Komut dosyasını sağ tıklatın ve Farklı Kaydet'i seçin... Mac dosyayı kaydetmek için

2.  Açık **Terminal** ve komut dosyasını indirdiğiniz için çalışma dizini değiştirin:

        $ cd /location/of/file

3. Komut dosyası yürütülebilir ve Çalıştır ile **sudo**:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

4. Son olarak, kaldırma komut dosyası silin.

Bu noktada, Xamarin bilgisayarınızdan kaldırılması.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Windows'ta Xamarin kaldırma

Xamarin aşağıdakilere desteklenen:

- [Visual Studio 2017](#uninstallvs2017)
- [Visual Studio 2015](#uninstallvs2015)
- [Visual Studio 2013'ün](#uninstallvs2015) [**desteklenmeyen**]
- [Xamarin Studio](#uninstallxamarinstudio) [**desteklenmeyen**]

<a name="uninstallvs2017" />

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin yükleyici uygulamasını kullanarak Visual Studio 2017 kaldırılır:

1. Kullanım **Başlat menüsünden** açmak için **Visual Studio yükleyicisi**.

2. Tuşuna **Değiştir** değiştirmek istediğiniz örneği düğmesi.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Değiştir düğmesine basın")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. İçinde **iş yükleri** sekmesinde, seçimini **mobil geliştirme .NET ile** seçeneği (içinde **mobil ve oyun** bölümü).

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Mobil Geliştirme iş yükü seçeneğinin işaretini kaldırın")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. Tıklatın **Değiştir** penceresinin sağ alt düğmesini.

5. Yükleyici (Yükleyici herhangi bir değişiklik yapabilmeniz için önce Visual Studio 2017 kapatılmalıdır) XML'deki seçili bileşenleri kaldırır.

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Değiştir düğmesine basın")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Tek tek Xamarin bileşenleri (örneğin, profil oluşturucu veya çalışma kitaplarını) geçerek kaldırılması **bileşenleri tek tek** 3. adım sekmesi ve in işaretini kaldırarak belirli bileşenleri:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Tek tek bileşenlerini Kaldır")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Visual Studio 2017 tamamen kaldırmak için seçin **kaldırma** üç çubuğundan menü öğesinin yanında **başlatma** düğmesi.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Visual Studio tamamen kaldırın")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> Varsa yan yana (SxS) – bir yayın ve önizleme sürümü gibi Visual Studio iki (veya daha fazla) örneğinin yüklü – kaldırma bir örnek de dahil olmak üzere diğer Visual Studio örnekleri, bazı Xamarin işlevini kaldırabilir:
>
> - Xamarin Profiler
> - Xamarin çalışma kitaplarını/denetçisi
> - Xamarin uzaktan iOS simülatörü
> - Apple Bonjour SDK'sı
>
> Belirli koşullar altında SxS örneklerden birini kaldırma, bu özelliklerin yanlış kaldırılmasına neden olabilir. Bu sistemde SxS örneği kaldırılmasından sonra kalan Visual Studio Örnekleri Xamarin platformda performansını düşürebilir.
>
>Çalıştırarak bu isresolved **onarım** eksik bileşenlerini yeniden yüklemek Visual Studio yükleyicisinde seçeneği.


## <a name="uninstalling-older-and-unsupported-products"></a>Eski ve desteklenmeyen ürünleri kaldırılıyor

<a name="uninstallvs2015"></a>

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 ve önceki

Visual Studio 2015 tamamen kaldırmak için kullanın [VisualStudio.com destek yanıt](https://www.visualstudio.com/vs/support/vs2015/uninstall-visual-studio-2015/).

Xamarin ile bir Windows makineden kaldırılması **Denetim Masası**. Gidin **programlar ve Özellikler** veya **Programlar > Program Kaldır** aşağıda gösterildiği gibi:

 [![](uninstalling-xamarin-images/image3.png "Programlar ve özellikler veya programları kaldırma programı aşağıda gösterildiği gibi gidin")](uninstalling-xamarin-images/image3.png#lightbox) 

Denetim Masası'ndan mevcut aşağıdakilerden herhangi birini kaldırın:

- Xamarin
- Windows için Xamarin
- Xamarin.Android
- Xamarin.iOS
- Visual Studio için Xamarin

Gezgini'nde, Xamarin Visual Studio uzantısı klasörlerinden (tüm sürümler Program dosyaları ve Program dosyaları (x86) dahil olmak üzere) kalan tüm dosyaları silin:

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Şu konumda bulunan Visual Studio'nun MEF Bileşeni önbellek dizini, silin:

``` 
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

İade **VirtualStore** Windows herhangi depoladığınız, görmek için directory kaplama dosyaları **Extensions\Xamarin** veya **ComponentModelCache** dizinleri vardır:

``` 
%LOCALAPPDATA%\VirtualStore
```

Kayıt Defteri Düzenleyicisi'ni (regedit) açın ve aşağıdaki anahtarı için bakın:

``` 
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

Bulma ve bu bir desenle eşleşen herhangi bir giriş silme:

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Bu anahtar arayın:

``` 
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Xamarin için ilgili gibi ara girişleri silin. Örneğin, herhangi bir şey koşulları içeren `mono` veya `xamarin`.

Yönetici cmd.exe komut istemi açın ve ardından çalıştırın `devenv /setup` ve `devenv /updateconfiguration` komutları her yüklü Visual Studio sürümü. Örneğin, Visual Studio 2015 için:

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```

<a name="uninstallxamarinstudio"></a>

### <a name="uninstall-xamarin-studio-on-windows"></a>Xamarin Studio Windows kaldırma

Xamarin Studio bir Windows makineden kaldırılır **Denetim Masası**. Gidin **programlar ve Özellikler** veya **Programlar > Program Kaldır** 

Xamarin Studio kaldırmak için bulma **Xamarin Studio 5.x.x** Programlar'ı tıklatın ve listedeki **kaldırma** düğmesi. 

### <a name="uninstall-xamarin-studio-on-mac"></a>Xamarin Studio Mac üzerinde kaldırma

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

## <a name="summary"></a>Özet

Bu makalede, Xamarin tamamen Terminal komutları kullanımı ile bir Mac alanından kaldırma yönerge sağlanır. Xamarin ile bir Windows makineden kaldırma da yönerge sağlanan **programlar ve Özellikler** seçeneği (Visual Studio 2015 ve önceki sürümler için) ve kullanarak **Visual Studio yükleyicisi** için Visual Studio 2017.


## <a name="related-links"></a>İlgili bağlantılar

- [Komut dosyası (örnek) kaldırma](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)
