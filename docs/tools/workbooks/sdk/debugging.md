---
title: Hata ayıklama tümleştirmeler
description: Bu belge, Xamarin çalışma kitaplarını tümleştirmeler, aracı tarafı ve istemci tarafı Windows ve Mac üzerinde hata ayıklamak açıklar
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: 6e37b1ac3d0fb78b5737ebe97b5a28ab40adb648
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36269063"
---
# <a name="debugging-integrations"></a>Hata ayıklama tümleştirmeler

## <a name="debugging-agent-side-integrations"></a>Aracı tarafındaki tümleştirmeler hata ayıklama

Aracı tarafındaki tümleştirmeler hata ayıklama en iyi şekilde yapılır tarafından sağlanan günlüğe kaydetme yöntemleri kullanarak `Log` sınıfını `Xamarin.Interactive.Logging`. Bkz: [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/) çağırmak yöntemleri için.

MacOS üzerinde günlük iletilerini her iki günlük Görüntüleyici menüsünde görüntülenir (**penceresi > Günlük Görüntüleyici**) ve istemci günlüğü. Günlük Görüntüleyici var olduğundan, Windows iletileri istemci günlüğünde yalnızca görüntülenir.

MacOS ve Windows üzerinde aşağıdaki konumlarda istemci günlüğü şöyledir:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

Dikkat edilmesi gereken tek şey olduğundan, bu, normal aracılığıyla tümleştirmeler yüklenirken `#r` mekanizması geliştirme sırasında tümleştirme derleme toplanmış olarak bir _bağımlılık_ çalışma kitabının ve mutlak bir yol ise ile paketlenen kullanılmıyor. Bu değişiklikleri yaymak değil için tümleştirme yeniden hiçbir şey gibi olmanızın görünmesi neden olabilir.

## <a name="debugging-client-side-integrations"></a>İstemci-tarafı tümleştirmeler hata ayıklama

İstemci-tarafı tümleştirmeler JavaScript'te yazılmış ve bizim web tarayıcısı yüzeye yüklü olarak (bkz [mimarisi](~/tools/workbooks/sdk/architecture.md) belge), bunları hata ayıklamak için en iyi yolu Mac WebKit geliştirme araçlarını kullanarak veya Windows F12 Seçici kullanarak .

İki araçları, JavaScript/TypeScript kaynağını görüntülemek, kesme noktalarını ayarlayın, konsol çıkışı görüntülemek ve denetlemek ve DOM değiştirmek izin ver

### <a name="mac"></a>Mac

Mac Xamarin çalışma kitapları için Geliştirici Araçları etkinleştirmek için terminal aşağıdaki komutu çalıştırın:

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

ve Xamarin çalışma kitaplarını yeniden başlatın. Bunu yaptıktan sonra görmeniz gerekir **incelemek öğesi** sağ tıklatma bağlam menüsünde hem de yeni bir görünür **Geliştirici** bölmesinde çalışma kitaplarını tercihlerinde kullanılabilir olacak. Bu seçenek, başlangıçta açılan geliştirici araçları isteyip istemediğinizi seçmenizi sağlar:

[![Geliştirici bölmesi](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

Bu tercih yalnızca yeniden başlatma da — çalışma kitapları için bu yeni çalışma kitapları üzerinde etkili olması yeniden başlatmanız gerekir. Geliştirici Araçları bağlam menüsü veya tercihleri aracılığıyla etkinleştirme hakkında bilgi sahibi Safari UI gösterir:

[![Safari geliştirme araçları](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Safari geliştirici araçlarını kullanma hakkında daha fazla bilgi için bkz: [WebKit denetçisi belgelerine][webkit-docs].

### <a name="windows"></a>Windows

Windows, IE takım "F12 Seçici" bilinen bir araç sağlar. diğer bir deyişle bir uzaktan hata ayıklayıcı katıştırılmış Internet Explorer örnekleri için. Aracı şu konumda bulabilirsiniz:

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

Çalışma kitapları istemci yüzeyini listesinde'nın temelini oluşturan katıştırılmış örneği çalıştırma F12 Seçici ve görmeniz gerekir. Ve hata ayıklama araçları Internet Explorer'dan görünür, bilinen F12 istemciye bağlı seçin:

[![F12 araçları](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
