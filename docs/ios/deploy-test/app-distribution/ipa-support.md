---
title: IPA desteği
description: Bu makalede geçici dağıtım, test ya da şirket içi dağıtım dahili uygulama kullanarak bir uygulamayı dağıtmak için kullanılan bir IPA dosyasının nasıl oluşturulacağı yer almaktadır.
ms.prod: xamarin
ms.assetid: D253C2DB-852E-6FC6-C9FD-574730B8DB19
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 3d63624ed486079f44e9756ee84612863e6176d7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="ipa-support"></a>IPA desteği

_Bu makalede geçici dağıtım, test ya da şirket içi dağıtım dahili uygulama kullanarak bir uygulamayı dağıtmak için kullanılan bir IPA dosyasının nasıl oluşturulacağı yer almaktadır._

Uygulamanın satışı iTunes App Store'da aracılığıyla serbest ek olarak, aşağıdaki amaçlarla dağıtılabilir:

- **Geçici sınama** — bir iOS uygulaması (UUID belirli iOS cihaz tarafından tanımlanan) en fazla 100 kullanıcılara alfa ve test amacıyla Beta için dağıtılabilir. Bkz: bizim [iOS geliştirme için cihazı sağlama](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning) test iOS cihazlarını Apple developer hesabınıza ekleme hakkında ayrıntılı bilgi için belgelere ve [geçici](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) hakkında daha fazla bilgi için Kılavuzu Bu şekilde dağıtmak için.
- **İçeride / kurumsal dağıtım** — bir iOS uygulaması dahili olarak, Apple Geliştirici Kurumsal programı üyeliğini gerektiren bir şirket içinde dağıtılabilir. In House dağıtım hakkında daha fazla bilgi içinde ayrıntılı [In House dağıtım](~/ios/deploy-test/app-distribution/in-house-distribution.md) Kılavuzu.

Her iki durumda da IPA paket (özel bir türü zip dosyası) oluşturulur ve doğru dağıtım sağlama profiliyle dijital olarak imzalanmış. Bu makalede IPA paketi oluşturmak ve bir Mac veya Windows PC iTunes kullanarak iOS cihazında paketi yüklemek için gerekli adımlar kapsanmaktadır.

<a name="iTunesMetadata" />

## <a name="the-itunesmetadataplist-file"></a>İTunesMetadata.plist dosyası

Bir iOS uygulaması iTunes Bağlan (ya da satış veya iTunes uygulama mağazasından ücretsiz sürüm için) oluşturulduğunda, geliştirici uygulamanın Tarz, alt Tarz, telif hakkı uyarısı, desteklenen iOS cihazları ve gerekli cihaz gibi bilgileri belirtebilirsiniz yetenekleri.

Geçici veya şirket içi dağıtım aracılığıyla ya da teslim iOS uygulamaları iTunes ve kullanıcının cihazının görünür olması için bu bilgileri desteklemek için bazı yol olması gerekir. Projenizi derleme ve proje dizininde depolanır her zaman varsayılan olarak, bir küçük iTunesMetadata.plist dosyası oluşturulur.

Özel bir **iTunesMetadata.plist** bir dağıtım için ek bilgi sağlamak için de oluşturulabilir. Bu dosya ve oluşturmak nasıl içeriğini hakkında daha fazla bilgi için lütfen bkz bizim [iTunesMetadata.plist içeriği](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_contents) ve [iTunesMetadata.plist dosya oluşturma](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_creating) belgeleri.

<a name="iTunesArtwork" />

## <a name="itunes-artwork"></a>iTunes resmi

Uygulamanızı App Store araçlarla teslim edilirken, ayrıca 512 x 512 ve iTunes uygulamanızda temsil etmek için kullanılan bir 1024 x 1024 görüntüsü eklemeniz gerekir.

Resmin iTunes belirtmek için aşağıdakileri yapın:

1. Çift **Info.plist** dosyasını **Çözüm Gezgini** düzenlemek için açın.
2. Kaydırma **iTunes resmi** Düzenleyicisi bölümü.
3. Küçük Resim Düzenleyicisi'nde eksik herhangi bir görüntü için tıklayın, istenen iTunes resmi görüntü dosyası **açık dosya** iletişim kutusu ve tıklatın **Tamam** veya **açık** düğme.
4. Bu kadar tüm görüntüleri, uygulamanız için belirtilmiş adımları yineleyin.

Lütfen bakın [iTunes resmi](~/ios/app-fundamentals/images-icons/app-icons.md) daha fazla ayrıntı için belgeleri.

<a name="createipa" />

## <a name="creating-an-ipa"></a>Bir IPA oluşturma

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bir IPA oluşturma şimdi yeni yayımlama akışına yerleşik olarak bulunur. Bunu yapmak için uygulamanızı ARŞİVLEMEYİN, imzalar ve, IPA kaydetmek için aşağıdaki yönergeleri izleyin.

Platformlar arası çözüm bir IPA oluşturmaya başlamadan önce başlangıç projesi olarak iOS projesi seçtiğinizden emin olun:

![](ipa-support-images/setasstartup.png "Başlangıç projesi olarak iOS projesinin seçili")

### <a name="build-your-archive"></a>Arşiviniz derleme

Bir IPA oluşturmak için bir _arşiv_ sürümü uygulamamız'ın oluşturulması gerekir. Bu Arşiv bizim uygulama ve tanımlama bilgilerini içerir.


1. Seçin **yayın | Aygıt** Mac için Visual Studio yapılandırması:!

    ![](ipa-support-images/buildxs01new.png "Yayın Seç | Aygıt Yapılandırması")

1. Gelen **yapı** menüsünde, select **yayımlama arşiv**:

    ![](ipa-support-images/buildxs02new.png "Yayımlama Arşivi'ni seçin")

1. Arşiv oluşturulduktan sonra **arşivler** görünüm görüntülenecek:

    ![](ipa-support-images/buildxs03new.png "Arşivler görünümü görüntülenir")


### <a name="sign-and-distribute-your-app"></a>Oturum ve uygulamanızı dağıtın

Arşiv, uygulamanızın derleme her seferinde onu otomatik olarak açılır **arşivler Görünüm**, tüm görüntüleme projeleri arşivlenmiş; çözümü tarafından gruplandırılır. Varsayılan olarak, bu görünüm yalnızca geçerli, açık çözüm gösterir. Arşivler sahip tüm çözümleri görmek için tıklayın **tüm arşivler Göster** seçeneği.

Böylece oluşturulan tüm hata ayıklama bilgileri daha sonraki bir tarihte görüntülenir (geçici veya şirket içi dağıtımlar) müşterilere dağıtılan arşivler kalmasını önerilir.

Uygulama dışı depolama için derlemeler unutmayın **iTunesMetadata.plist** dosya ve iTunes resmi kümesi otomatik olarak dahil edilir, IPA arşive bulunursa.

Uygulamanızı imzalamak ve dağıtım için hazırlamak için:


1. Seçin **oturum ve Dağıt...**  düğmesi, aşağıda gösterilmiştir:

    ![](ipa-support-images/buildxs04new.png "Oturum seçin ve Dağıt...")

1. Bu, Yayımlama Sihirbazı'nı açar. Seçin **geçici** veya **Kurumsal**(şirket içi) dağıtım kanal bir paket oluşturmak için:

    ![](ipa-support-images/distribute01.png "Geçici veya Kurumsal şirket içi dağıtım seçin")

1. Sağlama profili ekranında, kimlik imzalama ve sağlama profili karşılık gelen seçin veya başka bir kimlikle yeniden oturum açın:

    ![](ipa-support-images/distribute02.png "Kimlik imzalama ve sağlama profili karşılık gelen seçin")

1. Paketinizi ayrıntılarını doğrulayın ve tıklatın **Yayımla**:

    ![](ipa-support-images/distribute03.png "Paket ayrıntılarını doğrulayın")

1. Son olarak, IPA makinenize kaydedin:

    ![](ipa-support-images/distribute04.png "IPA bilgisayara kaydedin")


### <a name="building-via-the-command-line-on-mac"></a>Komut satırı (Mac) oluşturma

Bazı durumlarda, gibi bir CI ortamında, bu, IPA komut satırı oluşturmak gerekli olabilir. Bunu başarmak için aşağıdaki adımları izleyin:


1. Olun **proje Seçenekleri > iOS IPA Seçenekleri > iTunesArtwork görüntüleri dahil** denetlenir ve **yapı ad-geçici/kuruluş paket (IPA)** denetlenir:

    ![](ipa-support-images/imagexs04.png "IPA denetlenir ad-geçici/kuruluş paketi oluşturmak ve iTunesArtwork görüntüleri içerir")

    Tercih ederseniz, bunun yerine düzenleyebilirsiniz **.csproj** dosyasını bir metin düzenleyicisinde ve iki karşılık gelen özellik için el ile eklemeniz `PropertyGroup` uygulama oluşturmak için kullanılan yapılandırma için:

    ```xml
    <BuildIpa>true</BuildIpa>
    <IpaIncludeArtwork>false</IpaIncludeArtwork>
    ```

1. İsteğe bağlı bir ekliyorsanız **iTunesMetadata.plist** dosya, tıklatın **...**  düğmesini tıklatın listeden seçin ve tıklatın **Tamam** düğmesi:

     ![](ipa-support-images/imagexs03.png "İTunesMetadata.plist listeden seçin")

1. Çağrı **xbuild** (veya **mdtool** Klasik API'si) doğrudan ve bu özellik komut satırında geçirin:

    ```bash
    /Library/Frameworks/Mono.framework/Commands/xbuild YourSolution.sln /p:Configuration=Ad-Hoc /p:Platform=iPhone /p:BuildIpa=true
    ```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sağlama profiliyle oluşturulur ve seçili, isteğe bağlı bir kez **iTunesMetadata.plist** dosya oluşturulur ve resimleri ayarlama Visual Studio'da iTunes, dağıtım için bir IPA oluşturabilirsiniz. Ardından, projenizi yapılandırmanız gerekir. Aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, Xamarin.iOS proje adına sağ tıklayın ve seçin **özellikleri** düzenlemek üzere açmak için:

    ![](ipa-support-images/imagevs01.png "Özellikler'i seçin")

2. Seçin **iOS IPA seçenekleri** seçip **geçici** gelen **yapılandırma** açılır listesi:

    ![](ipa-support-images/imagevs02.png "Geçici yapılandırma açılır listeden seçin")

    > [!NOTE]
    > Bir geçici yapılandırma yeni Xamarin.iOS projeleri için kullanılamayabilir. Kullanılabilir durumda değilse, seçin **sürüm** yapılandırma.

3. Bir seçenek ekliyorsanız **iTunesMetadata.plist** dosya, tıklatın **...**  düğmesini tıklatın listeden seçin ve tıklatın **açık** düğmesi:

    ![](ipa-support-images/imagevs03.png "İTunesMetadata.plist listeden seçin")

4. İsteğe bağlı olarak belirleyebileceğiniz bir **paket adı** Xamarin.iOS projesi aynı ada sahip belirtilmezse IPA için.
5. Proje Özellikleri yaptığınız değişiklikleri kaydedin.
6. Seçin **geçici** gelen **yapı yapılandırması** varsa açılır. Aksi takdirde seçin **sürüm**:

    ![](ipa-support-images/imagevs05.png "Geçici yapı yapılandırması aşağı açılır listeden seçin")

7. IPA paketi oluşturmak için projeyi oluşturun.
8. IPA olması oluşturacaksınız **depo > iOS aygıtı > geçici (veya yayın)** klasörü:

    ![](ipa-support-images/imagevs06.png "Dosya Gezgini'nde IPA'sı")

-----

<a name="Customizing-the-IPA-Location" />

## <a name="customizing-the-ipa-location"></a>IPA konumu özelleştirme

Yeni bir **MSBuild** özelliği `IpaPackageDir` özelleştirmek kolaylaştırmak için eklenen **.ipa** çıkış dosyası konumu. Varsa `IpaPackageDir` özel bir konuma ayarlı **.ipa** dosya, varsayılan zaman damgalı alt yerine o konumda yerleştirilecek. Otomatik oluşturma sürekli tümleştirme (CI) derlemeler için kullanılanlar gibi doğru çalışması için bir özel dizin yolu Bel oluşturduğunda bu yararlı olabilir.

Yeni özellik kullanmak için olası birkaç yolu vardır:

Örneğin, çıktı için **.ipa** dosya eski varsayılan dizinine (Xamarin.iOS 9.6 olduğu gibi ve daha düşük), ayarladığınız `IpaPackageDir` özelliğine `$(OutputPath)` aşağıdaki yaklaşımlardan birini kullanarak. Her iki yaklaşımın yanı sıra kullanan komut satırı derlemeleri IDE yapılar dahil olmak üzere tüm birleşik API Xamarin.iOS derlemeleri ile uyumlu **xbuild**, **msbuild**, veya **mdtool**:

- İlk seçenek ayarlamaktır `IpaPackageDir` özelliği içinde bir `<PropertyGroup>` öğesinde bir **MSBuild** dosya. Örneğin, aşağıdaki ekleyebilirsiniz `<PropertyGroup>` iOS uygulaması projesi altına **.csproj** dosyası (yalnızca kapatmadan önce `</Project>` etiketi):

    ```xml
    <PropertyGroup>
        <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Daha iyi bir yaklaşım eklemektir bir `<IpaPackageDir>` var olan alt öğeye `<PropertyGroup>` oluşturmak için kullanılan yapılandırma karşılık gelen **.ipa** dosya. Bu proje iOS IPA seçeneklerini proje özellikleri sayfasında planlı bir ayar ile gelecekte uyumluluk hazırlayacağı iyidir. Şu anda kullanırsanız `Release|iPhone` oluşturmak için yapılandırma **.ipa** dosyası tam güncelleştirilmiş özellik grubu görünebilir aşağıdakine benzer:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' ">
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
    </PropertyGroup>
    ```

İçin alternatif bir tekniği **msbuild** veya **xbuild** komut satırı derlemeleri olduğu eklemek için bir `/p:` ayarlamak için komut satırı bağımsız değişkeni `IpaPackageDir` özelliği. Bu durumda unutmayın **msbuild** genişletme `$()` ifadeleri geçirilen komut satırında kullanmak mümkün değildir `$(OutputPath)` sözdizimi. Bunun yerine bir tam yol adı sağlamanız gerekir. Mono'nın **xbuild** Genişlet komutu `$()` ifadeler, ancak bir tam yol adı olduğundan kullanmak için hala tercih **xbuild** sonunda şunun için kullanım dışı kalacaktır [ platformlar arası sürümü **msbuild** ](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x) gelecekte serbest bırakır.

Bu yaklaşımı kullanır tam bir örnek Windows'da şuna benzer şekilde görünebilir:


```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Veya aşağıdaki Mac üzerinde:

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

<a name="installipa" />

## <a name="installing-an-ipa-using-itunes"></a>İTunes kullanarak IPA yükleme

Sonuçta elde edilen IPA paket iOS cihazlarına yüklemek için test kullanıcılarınıza teslim veya kurumsal dağıtım için gönderilen. Hangi yöntemin tercih olsun, son kullanıcı paketi Mac veya Windows PC kendi iTunes uygulamalarında IPA çift tıklatarak yükleyecek dosya (veya açık iTunes penceresinden sürükleme).

Yeni iOS uygulaması gösterildiği **My uygulamaları** bölümünde, burada üzerinde sağ tıklayın ve uygulama hakkında bilgi alın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 ![](ipa-support-images/installxs01.png "My uygulamalar bölümünde yeni iOS uygulaması")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 ![](ipa-support-images/installvs01.png "My uygulamalar bölümünde yeni iOS uygulaması")

-----

Kullanıcının cihazını yeni iOS uygulamasını yüklemek için şimdi iTunes eşitleyebilirsiniz.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.iOS uygulaması App Store derleme için hazırlamak üzere gerekli Kurulum ele. IPA paket nasıl oluşturulacağını gösterir ve sonuçta elde edilen iOS uygulama test etmek için son kullanıcının iOS cihazında veya şirket içi dağıtım yükleme sahiptir.


## <a name="related-links"></a>İlgili bağlantılar

- [App Store Dağıtımı](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Uygulama iTunes Connect yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store’da Yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Şirket içi dağıtım](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Geçici Dağıtım](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist Dosyası](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Sorun giderme](~/ios/deploy-test/troubleshooting.md)
- [iTunes resmi](~/ios/app-fundamentals/images-icons/app-icons.md#itunes)
- [İOS cihazlar için Kurumsal uygulamaları dağıtma](http://developer.apple.com/library/ios/#featuredarticles/FA_Wireless_Enterprise_App_Distribution/Introduction/Introduction.html)
