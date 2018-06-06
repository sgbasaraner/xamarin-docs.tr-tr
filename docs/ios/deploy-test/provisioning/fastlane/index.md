---
title: İOS için fastlane giriş
description: Bu kılavuz imza iOS uygulamalarını kodlamak için kullanılabilecek çeşitli fastlane araçları sunar. Bu güncelleştirme, yükleme ve fastlane araçlarını kullanma açıklar.
ms.prod: xamarin
ms.assetid: 8202C57D-22FF-4224-A5B1-AAEF12B7C106
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: ffb7e0a088bcd227f45b97229f089ef59d4d6608
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785495"
---
# <a name="introduction-to-fastlane-for-ios"></a>İOS için fastlane giriş

fastlane iOS ve Android uygulamalarını yayınlama karmaşık ve genellikle sıkıcı işlemini basitleştirmek için oluşturulan bir açık kaynak projesidir. Her uygulama sürümü, belirli bir durum gibi ele birkaç yardımcı oluşur:

- [teslim](https://github.com/fastlane/fastlane/tree/master/deliver#readme) – yönetir ve karşıya ekran görüntüleri, meta verileri ve uygulama paketleri iTunes Bağlan için.
- [üretmek](https://github.com/fastlane/fastlane/tree/master/produce#readme) – oluşturur ve iTunes Bağlan uygulamada ve Geliştirici Portalı'nın (genellikle uygulama kimliği da bilinir). Uygulama grupları ve uygulama hizmetleri için destek de içerir.
- [pem](https://github.com/fastlane/fastlane/tree/master/pem#readme) – oluşturur ve anında iletilen bildirim sağlama profilleri yönetir.
- [Spor salonu](https://github.com/fastlane/fastlane/tree/master/gym#readme) – Bu, yapı ve bir iOS uygulaması imzalamak için kullanılabilir. (Xamarin uygulamaları zaten MSBuild yapı, oturum ve Arşiv uygulamaları kullanın)
- [Sertifika](https://github.com/fastlane/fastlane/tree/master/cert#readme) – oluşturur ve kod imzalama sertifikalarının yönetir 
- [sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme) – oluşturur ve sağlama profilleri yönetir
- [eşleşen](https://github.com/fastlane/fastlane/tree/master/match#readme) – oluşturur ve sertifikaları ve profilleri korur ve geliştirme ekibi arasında eşitlenen şekilde git deposunda saklar.

fastlane farklı şekillerde kullanılabilir: dosya tabanlı araçlarla veya sürekli tümleştirme derlemeler için ortam değişkenleri kullanarak terminal komutları aracılığıyla. 

Bu kılavuz, özellikle iOS uygulamaları ile geliştirme için bir cihaz ayarlama ile ilgilenir ve odaklanır **cert**, **sigh** ve **eşleşen** yardımcı programları. 

Dayanak olarak sağlanan içerik tam olarak bir sürekli tümleştirme sunucuda işlemini otomatikleştirmenin dahil olmak üzere uygulama dağıtım ile yardımcı olmak için kullanılır. Ancak, bu fastlane Xcode projeleri ve bu nedenle bazı araçlar veya komutları gibi desteklemek için Araçlar yapabilen bir 3. taraf Not önemli `fastlane init` csproj dosyalarıyla beklendiği gibi çalışmayabilir. Fastlane kullanma hakkında daha fazla bilgi için ek araçlar veya fastlane, kullanılarak Android için serbest bakın [https://fastlane.tools/](https://fastlane.tools/)

<a name="Installation" />

## <a name="installation"></a>Yükleme

1. Xcode komut satırı araçları macOS makinede yüklü olduğundan emin olun. Araçlarını yüklemek için komut kullanmak `xcode-select --install` Terminal. Zaten yüklü değilse, aşağıdaki hata görüntülenir:

    ```bash
    error: command line tools are already installed, use "Software Update" to install updates
    ```

2. Fastlane Araçları'ndan indirin [ https://download.fastlane.tools ](https://download.fastlane.tools). 

    > [!NOTE]
    > Homebrew kullanarak fastlane araçları yüklemek mümkündür `brew cask install fastlane` veya Rubygems (2.0 veya üstü) kullanarak aracılığıyla `sudo gem install fastlane –NV`. Ancak Yükleyicisi'ni kullanarak doğru bağımlılıkları kullanılabilir olmasını sağlar. 

3. Dosya unzipping tarafından fastlane yükleyin ve çift `install` yürütülebilir. "Bilinmeyen bir geliştiriciden olduğundan dosya açılamıyor", bildiren bir hata alırsanız, Tamam'ı tıklatın ve aşağıdakileri yapın:
    - Control + tıklayın `install` yürütülebilir. Bu iletişim görüntülenir:

      ![](images/fastlane-image12.png "Yükle iletişim kutusu")
    
    - Fastlane araçlarını yükleme başlatmak için Tamam'ı tıklatın

4. Terminal aşağıda gösterilen iletişim kutusuyla ister. Tuşuna `y`:

  ![](images/fastlane-image13.png "Terminal istemi")
 
4. Çalıştırma `which fastlane` fastlane ilk kez kullanmadan önce. Yol aşağıdaki gibi görünmelidir: 

    ```bash
    /Users/[user]/.fastlane/bin
    ```

5. Yolun yukarıdaki eşleşirse, başlamak hazırsınız.

     Aşağıdaki yaparsanız, not: üzerinde macOS açmak `.bash_profile`, giriş dizini, aşağıdaki komutla gizli düz metin dosyasında olduğu:

    ```bash
    open ~/.bash_profile
    ```

6. Aşağıdaki yol ortam değişkenine ekleyin ve dosyayı kaydedin: 

    ```bash
    export PATH="$HOME/.fastlane/bin:$PATH"
    ```

7.  Çalıştırma `which fastlane` gibi yol onaylamak için yeniden arar `/Users/[user]/.fastlane/bin`


## <a name="updating-fastlane"></a>Fastlane güncelleştiriliyor

fastlane düzenli olarak yeni sürümler iter çok etkin açık kaynak projesidir. Fastlane yeni bir sürümü kullanılabilir olduğunda, herhangi bir fastlane komutunu çalıştırdığınızda, dikkat edin:

[![](images/fastlane-image0.png "Hızlı lane güncelleştirme istemi")](images/fastlane-image0.png#lightbox)


Fastlane yeni bir sürüme güncelleştirmek için en son paketinden karşıdan [burada](https://download.fastlane.tools) ve çalıştırmak için yükleme paketini çift:

[![](images/fastlane-image0a.png "Yükleme paketi çalıştırma")](images/fastlane-image0a.png#lightbox)


## <a name="contents"></a>İçindekiler

Bu kılavuzlar dizi fastlane kullanımları geliştirme veya dağıtım için hazırlık iOS uygulamanızı imzalama kod araçlardan bazıları gösterir. Şu anda kapsamdaki araçlar şunlardır:

- [cert](~/ios/deploy-test/provisioning/fastlane/cert.md)
- [sigh](~/ios/deploy-test/provisioning/fastlane/sigh.md)
- [match](~/ios/deploy-test/provisioning/fastlane/match.md)

Sertifika ve sigh İmzalama sertifikaları ve bir yerel makinedeki sağlama profilleri oluşturma ve yönetme ile ilgilidir. eşleşme bu işlem daha ayrıntılı bir adım alır. Oluşturur ve sertifika ve sağlama profilleri yönetir ve bunları geliştirme ekibinin tüm üyeleri tarafından erişilebilir olmasını sağlayan bir git deposu depolar. Nasıl çalıştıklarını ve bunları nasıl kullanabileceğinizi öğrenmek için her bölüm aracılığıyla okuyun.

## <a name="using-fastlane-tools-with-xamarin"></a>Xamarin ile fastlane araçlarını kullanma

Bir kimlik imzalama ve sağlama profilleri fastlane ile oluşturduktan sonra Mac için Visual Studio seçenekleri imzalama paket ayarlama sertifikaları ve özel anahtarları Anahtarlık macOS ve olduğunu sağlayan basit olmalıdır sağlama profilleri olan klasöründe `~/Library/MobileDevice/Provisioning Profiles`.

Kod imzalama bir Xamarin.iOS uygulaması seçeneklerini ayarlamak için proje adına sağ tıklayın **proje Seçenekleri > Yapı > iOS paket imzalama** imzalama kimlik ve sağlama profili ve açıkça olarak aşağıda gösterilmiştir:

[![](images/fastlane-image11.png "İmzalama kimlik ve sağlama profili açıkça ayarlayın")](images/fastlane-image11.png#lightbox)

## <a name="related-links"></a>İlgili bağlantılar

- [fastlane belgeleri](https://fastlane.tools/)
- [fastlane kod imzalama belgeleri](https://docs.fastlane.tools/codesigning/getting-started/)
- [Kod imzalama Kılavuzu](https://codesigning.guide/)
