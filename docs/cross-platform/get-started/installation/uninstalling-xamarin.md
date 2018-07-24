---
title: Xamarin'i kaldırma
description: Bu belge, Mac ve Windows Xamarin kaldırmayı açıklar. Bu, Mono, Xamarin.Android, Xamarin.iOS ve diğer araçları kaldırma hakkında yönergeler sağlar.
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 87f59e9f0c2150291a43cdfee4fe6c5dfc2058f8
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203130"
---
# <a name="uninstalling-xamarin"></a>Xamarin'i kaldırma

Bu kılavuz, macOS veya Windows üzerinde Visual Studio'da Xamarin kaldırılacağı açıklanmaktadır.

Xamarin, Evrensel Yükleyicisi'ni kullanarak yeniden yüklemeniz gerekirse, her zaman bilgisayar ilk kez yeniden önerilir.

## <a name="uninstalling-xamarin-on-macos"></a>Macos'ta Xamarin'i kaldırma

Bu kılavuz, her ürün ilgili bölüme gidilerek ayrı ayrı kaldırmak için kullanılabilir. Tüm aşamalardaki bu kılavuzu izleyerek listelenen ürünleri içeren tüm Xamarin araç takımı kaldırılabilir:

- [Mono](#uninstallmono)
- [Xamarin.Android](#uninstallandroid)
- [Xamarin.iOS](#uninstallios)
- [Xamarin.Mac](#uninstallmac)
- [Çalışma Kitapları](#uninstallworkbooks)
- [Xamarin Profiler](#uninstallprofiler)
- [Yükleyici](#uninstallinstaller)

> [!TIP]
> Sağladık bir [betik kaldırma](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh) Xamarin macOS makinenizde kaldırırken kullanabilirsiniz. Betik kullanma hakkında daha fazla bilgi için bkz. [kaldırma betiği kullanarak](#uninstallscript) bu kılavuzdaki bölümü.

### <a name="uninstalling-visual-studio-for-mac"></a>Mac için Visual Studio'yu kaldırma

Bir Mac bilgisayardan Xamarin'i kaldırma ilk adımı bulmaktır **Visual Studio.app** içinde **/Applications** dizin sürükleyin **çöp kutusu**. Alternatif olarak, sağ tıklayın ve **çöp kutusuna Taşı** aşağıdaki görüntüde gösterildiği gibi:

![Visual Studio uygulamaya çöp Taşı](uninstalling-xamarin-images/uninstall-image1.png)

Olabilir ancak yine de bir dosya sisteminde Xamarin için ilgili diğer dosyalar bu uygulama paketi grubu silme, Mac için Visual Studio kaldırır.

Mac için Visual Studio'nun tüm izlemeleri kaldırmak için terminalde aşağıdaki komutları çalıştırın:

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
> Mac için Visual Studio kaldırma hakkında daha fazla bilgi için bkz [kaldırma](https://docs.microsoft.com/visualstudio/mac/uninstall) docs.microsoft.com'da Kılavuzu

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Mono SDK'sı (MDK) kaldırma

Mono, .NET Framework'ün açık kaynak uygulamasıdır ve tüm Xamarin Products—Xamarin.iOS, Xamarin.Android ve Xamarin.Mac bu platformlarda geliştirme C# ' ta izin vermek için kullanılır.

> [!WARNING]
> Ayrıca Unity gibi bir Mono kullanan diğer uygulamalar Xamarin dışında vardır. Başka bir bağımlılık üzerinde Mono kaldırmadan önce emin olun.

Mono Framework kaldırmak için terminalde aşağıdaki komutları çalıştırın:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Xamarin.Android kaldırma

Xamarin.Android, Android SDK ve Xamarin.Android kaldırırken kaldırılması gerekir Java SDK gibi kullanılırken gerekli olan öğeleri vardır. Bu bölümde gerekli tüm parçaları kaldırma aracılığıyla size yol gösterir.

Xamarin.Android kaldırmak için terminalde aşağıdaki komutları çalıştırın:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Android SDK ve Java SDK kaldırma

Android SDK'sı, Android uygulamalarının geliştirilmesini için gereklidir. Android SDK'sı tüm parçalarını tamamen kaldırmak için dosyayı bulun **~/Library/Developer/Xamarin/** ve taşımak **çöp**.

Mac OS X bir parçası olarak paketlenmiş öncesi olduğundan kaldırılması Java SDK (JDK) gerekmez.

#### <a name="uninstall-android-avd"></a>Android AVD kaldırma

> [!WARNING]
> Ayrıca Android AVD ve Android Studio gibi ek bu android bileşenlerini kullanan diğer uygulamalar Visual Studio dışında Mac için vardır.
> Bu dizin kaldırılırken projeleri Android Studio'da kesmesine neden olabilir. 

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

Xamarin.iOS uygulama geliştirme C# veya F # kullanarak iOS sağlar. Xamarin.iOS bir makineden kaldırmak için aşağıdaki adımları izleyin:

Tüm Xamarin.iOS dosyaları kaldırmak için terminalde aşağıdaki komutları kullanın:

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

Xamarin.Mac Mac Cihazınızda lisans ve ürün sırasıyla yok etmek aşağıdaki iki komutu kullanarak makinenizden kaldırabilirsiniz:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks"></a>Çalışma kitapları kaldırma

Yukarıdaki terminalde aşağıdaki komutları kullanın ve Xamarin Workbooks sürüm 1.2.2 kaldırmak için:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Önceki sürümler için bkz [çalışma kitapları](~/tools/workbooks/install.md#uninstall-macos) Kılavuzu kaldırın.

<a name="uninstallprofiler" />

### <a name="uninstall-the-xamarin-profiler"></a>Xamarin Profiler'ı kaldırma

Xamarin Profiler'ı kaldırmak için terminalde aşağıdaki komutları kullanın:

```bash
sudo rm -rf "/Applications/Xamarin Profiler.app"
```

<a name="uninstallinstaller" />

### <a name="uninstall-the-xamarin-installer"></a>Xamarin yükleyici kaldırın

Xamarin için evrensel Yükleyicisi'nin tüm izlemeleri kaldırmak için aşağıdaki komutları kullanın:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Caches/VisualStudioInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Logs/VisualStudioInstaller/
rm -rf ~/Library/Preferences/Xamarin/
rm -rf "~/Library/Preferences/Visual Studio/"
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Kaldırma betiğini kullanarak

Bu kaldırma komut dosyası, Mac ve onun ilişkili tek bir seferde Xamarin bileşenleri için Visual Studio'yu kaldırmak sağlar.

Betik makalesinde bulunan komutların çoğu içerir. Komut dosyası iki ana atlamalar vardır ve olası dış bağımlılıklar nedeniyle dahil edilmez:

- Mono kaldırma
- Android AVD kaldırma

Betiği çalıştırmak için aşağıdaki adımları uygulayın:

1. Komut dosyasını sağ tıklayın ve Farklı Kaydet'i seçin... mac'inizde dosyayı kaydetmek için

2.  Açık **Terminal** ve komut dosyasını indirdiğiniz için çalışma dizinini değiştirin:

        $ cd /location/of/file

3. Yürütülebilir kod ve çalışma ile **sudo**:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

4. Son olarak, kaldırma betiğini silin.

Bu noktada, Xamarin bilgisayarınızdan kaldırılması.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Windows üzerinde Xamarin'i kaldırma

Xamarin aşağıdakilere desteklenen:

- [Visual Studio 2017](#uninstallvs2017)
- [Visual Studio 2015](#uninstallvs2015)
- [Visual Studio 2013'ün](#uninstallvs2015) [**desteklenmeyen**]
- [Xamarin Studio](#uninstallxamarinstudio) [**desteklenmeyen**]

<a name="uninstallvs2017" />

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin, Visual Studio yükleyici uygulamasını kullanarak 2017'den kaldırılır:

1. Kullanım **Başlat menüsü** açmak için **Visual Studio yükleyicisi**.

2. Tuşuna **Değiştir** değiştirmek istediğiniz örneği için düğme.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Değiştir düğmesine basın")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. İçinde **iş yükleri** sekmesinde, seçimini **.NET ile Mobil Geliştirme** seçeneği (içinde **mobil ve oyun** bölümü).

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Mobil Geliştirme iş yükü'seçeneğinin işaretini kaldırın")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. Tıklayın **Değiştir** pencerenin sağ düğmesi.

5. Yükleyici (Yükleyici herhangi bir değişiklik yapabilmeniz için önce Visual Studio 2017 kapatılmalıdır) XML'deki seçili bileşenleri kaldırır.

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Değiştir düğmesine basın")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Tek tek Xamarin bileşenleri (örneğin, çalışma kitapları ve Profiler) geçerek kaldırılması **tek tek bileşenler** 3. adım sekmesi ve seçimini kaldırarak belirli bileşenler:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Tek tek bileşenleri kaldırma")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Visual Studio 2017 tamamen kaldırmak için seçin **kaldırma** üç çubuğundan yanındaki menü **başlatma** düğmesi.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Visual Studio'yu tamamen kaldırın")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> Varsa Visual Studio'nun iki (veya daha fazla) örneklerini yan yana (SxS) – gibi bir sürüm ve önizleme sürümü yüklü – kaldırma tek bir örnek de dahil olmak üzere diğer Visual Studio örnek, bazı Xamarin işlevini kaldırabilir:
>
> - Xamarin Profiler
> - Xamarin çalışma kitapları/denetçisi
> - Xamarin uzak iOS simülatörü
> - Apple Bonjour SDK'sı
>
> Belirli koşullar altında SxS örneklerinden birinde kaldırma, bu özelliklerin yanlış kaldırılmasına neden olabilir. Bu sistemde SxS örneği'nin kaldırılmasından sonra kalan Visual Studio Örnekleri Xamarin platformunda performansını düşürebilir.
>
>Bu çalıştırarak çözülene **onarım** seçeneğini Visual Studio yükleyicisi, eksik bileşenleri yeniden yükler.


## <a name="uninstalling-older-and-unsupported-products"></a>Eski ve desteklenmeyen ürünleri kaldırılıyor

<a name="uninstallvs2015"></a>

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 veya önceki

Visual Studio 2015 tamamen kaldırmak için kullanın [VisualStudio.com adresindeki destek yanıt](https://visualstudio.microsoft.com/vs/support/vs2015/uninstall-visual-studio-2015/).

Xamarin ile bir Windows makineden kaldırılması **Denetim Masası**. Gidin **programlar ve Özellikler** veya **Programlar > Program Kaldır** aşağıda gösterildiği gibi:

 [![](uninstalling-xamarin-images/image3.png "Programlar ve özellikler program veya kaldırmak için aşağıda gösterildiği gibi bir Program gidin")](uninstalling-xamarin-images/image3.png#lightbox) 

Denetim Masası'ndan mevcut olan aşağıdakilerden herhangi birini kaldırın:

- Xamarin
- Windows için Xamarin
- Xamarin.Android
- Xamarin.iOS
- Visual Studio için Xamarin

Gezgini'nde Xamarin Visual Studio uzantı klasörleri (tüm sürümler, Program dosyaları ve Program dosyaları (x86) hem de dahil olmak üzere) kalan dosyaları silin:

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Şu konumda bulunan Visual Studio'nun MEF Bileşeni önbellek dizinini silin:

``` 
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

İade **VirtualStore** Windows herhangi depoladığınız, görmek için directory kaplama dosyaları **Extensions\Xamarin** veya **ComponentModelCache** dizinleri vardır:

``` 
%LOCALAPPDATA%\VirtualStore
```

Kayıt Defteri Düzenleyicisi'ni (regedit) açın ve aşağıdaki anahtarı için'ı arayın:

``` 
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

Bulmak ve bu desenle eşleşen tüm girişleri silin:

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Bu anahtar için bakın:

``` 
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Xamarin için ilgili gibi ara tüm girişleri silin. Örneğin, koşulları içeren hiçbir şey `mono` veya `xamarin`.

Yönetici cmd.exe komut istemi açın ve ardından çalıştırın `devenv /setup` ve `devenv /updateconfiguration` Visual Studio'nun yüklü her sürüm için komutları. Örneğin, Visual Studio 2015 için:

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```

<a name="uninstallxamarinstudio"></a>

### <a name="uninstall-xamarin-studio-on-windows"></a>Windows üzerinde Xamarin Studio'yu kaldırın

Xamarin Studio bir Windows makineden kaldırılır **Denetim Masası**. Gidin **programlar ve Özellikler** veya **Programlar > Program Kaldır** 

Xamarin Studio'yu kaldırmak için bulma **Xamarin Studio 5.x.x** Programlar'ı tıklatın ve listedeki **kaldırma** düğmesi. 

### <a name="uninstall-xamarin-studio-on-mac"></a>Mac'te Xamarin Studio kaldırma

Xamarin Studio'yu bir Mac bilgisayardan kaldırma ilk adımı bulmaktır **Xamarin Studio.app** içinde **/Applications** dizin sürükleyin **çöp kutusu**. Alternatif olarak, sağ tıklayın ve **çöp kutusuna Taşı** aşağıda gösterildiği gibi:

 [![](uninstalling-xamarin-images/image1.png "Alternatif olarak, sağ tıklayın ve aşağıda gösterildiği gibi çöp kutusuna taşınacak seçin")](uninstalling-xamarin-images/image1.png#lightbox)

Bu uygulama paket silindiğinde, Xamarin Studio kaldırılır, ancak yine de bir dosya sisteminde Xamarin için ilgili diğer dosyaları vardır.

Xamarin Studio tüm izlemeleri kaldırmak için terminalde aşağıdaki komutları çalıştırılmalıdır:

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

## <a name="summary"></a>Özet

Bu makalede, Xamarin tamamen Mac Terminal komutları kullanımı'ndan kaldırma yönerge sağlanmadı. Xamarin ile bir Windows makineden kaldırılması da yönerge sağlanan **programlar ve Özellikler** seçeneği (Visual Studio 2015 ve önceki sürümleri için) ve kullanarak **Visual Studio yükleyicisi** için Visual Studio 2017.


## <a name="related-links"></a>İlgili bağlantılar

- [Betik (örnek) kaldırma](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)
