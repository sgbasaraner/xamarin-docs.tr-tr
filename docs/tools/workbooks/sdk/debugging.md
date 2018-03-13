---
title: "Hata ayıklama tümleştirmeler"
ms.topic: article
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: a0873c6b902e29174da5e27a09e8f580d6d69eb7
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="debugging-integrations"></a>Hata ayıklama tümleştirmeler

## <a name="debugging-agent-side-integrations"></a>Aracı tarafındaki tümleştirmeler hata ayıklama

Aracı tarafındaki tümleştirmeler hata ayıklama en iyi şekilde yapılır tarafından sağlanan günlüğe kaydetme yöntemleri kullanarak `Log` sınıfını `Xamarin.Interactive.Logging`. Bkz: [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/) çağırmak yöntemleri için.

MacOS üzerinde günlük iletilerini her iki günlük Görüntüleyici menüsünde görüntülenir (**penceresi > Günlük Görüntüleyici**) ve istemci günlüğü. Günlük Görüntüleyici var olduğundan, Windows iletileri istemci günlüğünde yalnızca görüntülenir.

MacOS ve Windows üzerinde aşağıdaki konumlarda istemci günlüğü şöyledir:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

Dikkat edilmesi gereken tek şey olduğundan, bu, normal aracılığıyla tümleştirmeler yüklenirken `#r` mekanizması geliştirme sırasında tümleştirme derleme toplanmış olarak bir _bağımlılık_ çalışma kitabının ve mutlak bir yol ise ile paketlenen kullanılmıyor. Bu değişiklikleri yaymak değil için tümleştirme yeniden hiçbir şey gibi olmanızın görünmesi neden olabilir.

## <a name="debugging-client-side-integrations"></a>İstemci-tarafı tümleştirmeler hata ayıklama

İstemci-tarafı tümleştirmeler JavaScript'te yazılmış ve bizim web tarayıcısı yüzeye yüklü olarak (bkz [mimarisi](~/tools/workbooks/sdk/architecture.md) belge), bunları hata ayıklamak için en iyi yolu Mac WebKit geliştirme araçlarını kullanarak veya Windows F12 Seçici kullanarak .

İki araçları, JavaScript/TypeScript kaynağını görüntülemek, kesme noktalarını ayarlayın, konsol çıkışı görüntülemek ve denetlemek ve DOM değiştirmek izin ver

### <a name="mac"></a>Mac

Mac Xamarin çalışma kitapları için Geliştirici Araçları etkinleştirmek için terminal aşağıdaki komutu çalıştırın:

```shell
defaults write com.xamarin.Inspector WebKitDeveloperExtras -bool true
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