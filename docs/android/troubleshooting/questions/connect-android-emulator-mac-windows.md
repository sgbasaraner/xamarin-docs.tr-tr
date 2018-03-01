---
title: "Android öykünücüsünü bağlanmak mümkün, Windows sanal makineden bir Mac üzerinde çalışıyor mu?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2f0ef027d8a2d40ccf85e35d5a85eba4cd7c7ccc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Android öykünücüsünü bağlanmak mümkün, Windows sanal makineden bir Mac üzerinde çalışıyor mu?

Google Android Mac üzerinde çalışan bir Windows sanal makineden öykünücüsü bağlanmak için aşağıdaki adımları kullanın:

1.  Öykünücü Mac üzerinde Başlat

2.  KILL `adb` Mac sunucuda:

    ```bash
    adb kill-server
    ```

3.  Geri döngü Ağ arabirimindeki 2 TCP bağlantı noktaları öykünücü dinlemede dikkat edin:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Bağlanmak için kullanılan tek tek sayılı bağlantı noktasıdır `adb`. Ayrıca bkz. [http://developer.android.com/tools/devices/emulator.html#emulatornetworking](http://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4.  _Seçenek 1_: kullanım [ `nc` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html) ileri gelen TCP paketleri alınan harici bağlantı noktası 5555 (veya başka bir bağlantı da istediğiniz gibi) geri döngü arabirimde tek sayılı bağlantı noktasına (**127.0.0.1 5555** Bu örnekte), ve giden paketler başka bir şekilde yeniden iletin:

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0 < backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    Sürece `nc` komutları kalmayın çalışan bir Terminal penceresi, paketleri beklenen şekilde iletilir. Çıkmak için Terminal penceresi denetim C yazabilirsiniz `nc` işiniz bittiğinde komutları öykünücüsü kullanarak.

    (1. seçenek seçeneği 2'den genellikle daha kolay özellikle **sistem tercihleri > Güvenlik ve gizlilik > Güvenlik Duvarı** açılana.) 

    _Seçenek 2_: kullanım [ `pfctl` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html) bağlantı noktasındaki TCP paketleri yönlendirmek için `5555` (veya istediğiniz herhangi bir bağlantı) üzerinde [paylaşılan ağ](http://kb.parallels.com/en/4948) tek numaralı bağlantı noktasına arabirimi döngü arabirimine (`127.0.0.1:5555` Bu örnekte):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    Bu komut, bağlantı noktası kullanarak iletme ayarlar `pf packet filter` sistem hizmeti. Satır sonları önemlidir. Bunları olduğu gibi kopyalama yapıştırma zaman sakladığınızdan emin olun. Arabirim adından Ayarla gerekecektir *vmnet8* Parallels kullanıyorsanız. `vmnet8` Özel adıdır *NAT cihazının* için *paylaşılan ağ* VMWare Fusion modunda. Parallels uygun ağ arabiriminde olasıdır [vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5.  Öykünücü Windows makineden Bağlan:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    "IP-adresi---mac" Mac IP adresiyle örneğin listelendiği gibi tarafından Değiştir `ifconfig vmnet8 | grep 'inet '`. Gerekirse, Değiştir `5555` 4. adımdan gibi diğer bağlantı noktası ile\. (Not: komut satırı erişmek için tek yönlü `adb` durumda [ **Araçlar > Android > Android Adb komut istemi** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Teknik kullanılarak alternatif `ssh`

Etkinleştirdiyseniz _uzaktan oturum açma_ Mac üzerinde sonra kullanabileceğiniz `ssh` ileten öykünücüsü bağlanmak için bağlantı noktası.

1.  Bir SSH istemcisi Windows yükleyin. Bir seçenektir yüklemek için [Windows için Git](https://git-for-windows.github.io/). `ssh` Komut içinde kullanılabilir olacak **Git Bash** komut istemi.

2.  Adımlarını 1-3 üstten öykünücü başlatmak için KILL `adb` Mac sunucuda ve öykünücüsü bağlantı noktalarını belirleyin.

3.  Çalıştırma `ssh` Windows yerel bir bağlantı noktası arasında çift yönlü bağlantı noktası iletme ayarlamak için Windows (`localhost:15555` Bu örnekte) ve Mac'ın geri döngü arabirimde tek sayılı öykünücüsü bağlantı noktası (`127.0.0.1:5555` Bu örnekte):

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Değiştir `mac-username` tarafından listelenen Mac kullanıcı adınızı ile `whoami`. Değiştir `ip-address-of-the-mac` Mac IP adresiyle

4.  Windows yerel bağlantı noktası kullanarak öykünücüsü Bağlan:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Not: şekilde komut satırı erişmek kolay bir `adb` durumda [ **Araçlar > Android > Android Adb komut istemi** Visual Studio'da](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Küçük bir uyarı: bağlantı noktası kullanıyorsanız `5555` yerel bağlantı noktası için `adb` öykünücüsü Windows üzerinde yerel olarak çalıştığı düşünün. Bu Visual Studio'da güçlükle neden olmaz ancak Mac için Visual Studio, uygulama hemen sonra Başlat'ndan çıkmak neden olur.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>Alternatif yöntem kullanılarak `adb -H` henüz desteklenmiyor

Teorik olarak kullanmak üzere başka bir yaklaşım olacaktır `adb`'s bağlanmak için yerleşik yetenek bir `adb` uzaktaki bir makinede çalışan sunucu (örneğin [http://stackoverflow.com/a/18551325](http://stackoverflow.com/a/18551325)).
Ancak Xamarin.Android IDE Uzantıları şu anda bu seçeneği yapılandırmak için bir yol sağlamaz.

## <a name="contact-information"></a>İletişim bilgileri

Bu belge Mart 2016 itibariyle şu anki davranışı açıklanır. Bu belgede açıklanan teknikleri gelecekteki uğratabilir şekilde xamarin, kararlı test paketinin bir parçası değil.

Teknik artık çalıştığını fark ya da diğer hataları belgede görürseniz, aşağıdaki Forumu iş parçacığı üzerinde tartışma eklemek çekinmeyin: [http://forums.xamarin.com/discussion/33702/ Android-emulator-from-Host-Device-inside-Windows-VM](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
teşekkürler!

