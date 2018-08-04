---
title: Xamarin.iOS uygulamaları Xcode ile hata ayıklama
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5FDDEDB3-AEB9-4D9C-9F7B-FEFAA9AF0031
ms.technology: xamarin-ios
author: migueldeicaza
ms.author: miguel
ms.date: 02/22/2018
ms.openlocfilehash: e0127d4b24236d350e5fa967110316544c320d0f
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514676"
---
# <a name="debugging-xamarinios-apps-with-xcode"></a>Xamarin.iOS uygulamaları Xcode ile hata ayıklama

Xcode Xamarin.iOS uygulamanızı bazı bölümlerinde hata ayıklamak için kullanmak istediğiniz senaryolar olabilir. .NET kodu hata ayıklamanız mümkün olmayacak, ancak yine de yerel kodda hata ayıklama ve Xcode'da yerel görselleştiriciler bazılarını kullanmak mümkün olacaktır.

## <a name="walkthrough"></a>İzlenecek yol

Xcode, Mac için Visual Studio'da hata ayıklama için yerleşik destek yok olsa da, bunun için aşağıdaki adımları kullanabilirsiniz:

1. Xamarin uygulamanızda olarak aynı paket kimliği ile bir Xcode iOS uygulaması oluşturun.
   
    - Xamarin.iOS projenizin paket grubu tanımlayıcısı açarak bulabilirsiniz **Info.plist** dosyası:

        ![Info.plist düzenleme](debugging-with-xcode-images/vsmac-infoplist.png "Info.list düzenleme")

    - Xcode'da, paket grubu tanımlayıcısı, projenizi oluştururken ya da hedef projede seçerek ayarlayın:

        ![Paket tanımlayıcısı Xcode'da ayarlama](debugging-with-xcode-images/xcode-bundle.png "paket grubu tanımlayıcısı Xcode'da ayarlama")

2. Uygulamayı otomatik olarak başlatma yerine başlatma beklemek için Xcode projesi değiştirin:

    - Açık **düzenle düzen paneli** seçerek **ürün > düzeni > Düzenle düzeni** veya bu adı kullanıyor **cmd⌘ + <** klavye kısayol.

    - Seçin **çalıştırma** şeması ve sağa, panel görmelisiniz **başlatma** seçenekleri. Seçin **başlatılacak çalıştırılabilir dosyanın bekleyin** tıklatıp **Kapat**.

        ![Başlatılacak yürütülebilir dosya için bekleyin](debugging-with-xcode-images/xcode-schemes.png "başlatılacak çalıştırılabilir dosyanın bekleyin")

3. Xcode projesi çalıştırın.

    İşlevsiz Xcode uygulamasını cihazınıza yüklenir, ancak bunu başlatmaz.

4. Xamarin uygulamasını çalıştırın.

    Başlattığında, Xcode Xamarin uygulamaya iliştirin.

### <a name="caveats"></a>Uyarılar

Başlatma, her zaman bir Xamarin.iOS uygulaması için küçük bir değişiklik yapın gerekebilir. Aksi takdirde, Mac için Visual Studio app oluşturulması gerekmez algılar *ve* zaten yüklüyse ve Xcode işlevsiz uygulamanın üzerine yeniden olmaz.

## <a name="alternative---using-lldb"></a>Alternatif - lldb kullanma

Kullanarak kullanabiliyorsanız **lldb** komut satırından bir çok daha basit bir çözüm yoktur.

Kabuğu'na aşağıdaki komutu yazın:

```bash
touch ~/.mtouch-launch-with-lldb
```

Yönergeler alırsınız **uygulama çıktısı** penceresinde, uygulamanızı çalıştırdığınızda, ancak temel olarak, yapmanız gerekenler oluşturabileceksiniz kullanılacak **lldb** uygulamanızda hata ayıklamak için komut satırından.
