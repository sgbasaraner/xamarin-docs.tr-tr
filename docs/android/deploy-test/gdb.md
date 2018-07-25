---
title: GDB
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 886cc1de87bd8225bd0389d2e7b84b546ffb39d7
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241503"
---
# <a name="gdb"></a>GDB

## <a name="overview"></a>Genel Bakış

Xamarin.Android 4.10 kullanmak için kısmi destek sunulan `gdb` kullanarak `_Gdb` MSBuild hedefi. 

> [!NOTE]
> `gdb` desteği, Android NDK'in yüklü olmasını gerektirir.

Kullanmak için üç yol vardır `gdb`:

1.  [Hata ayıklama derlemeleri hızlı dağıtımı etkin](#Debug_Builds_with_Fast_Deployment) .
1.  [Hata ayıklama derlemeleri hızlı devre dışı dağıtımla](#Debug_Builds_without_Fast_Deployment) .
1.  [Yayın derlemeleri](#Release_Builds) .


İşler kötüye gittiğinde bkz [sorun giderme](#Troubleshooting) bölümü.

<a name="Debug_Builds_with_Fast_Deployment" />

### <a name="debug-builds-with-fast-deployment"></a>Hata ayıklama yapıları ile hızlı dağıtım

Oluşturma ve bir hata ayıklama dağıtma hızlı etkin dağıtım ile derlerken `gdb` kullanarak eklenebilecek `_Gdb` MSBuild hedefi.

İlk olarak, yükleme uygulaması. Bu, IDE veya komut satırı yoluyla yapılabilir:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:Install *.csproj
```

İkincisi, çalıştırma `_Gdb` hedef. Yürütme, sonunda bir `gdb` komut satırı yazdırılır:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
...
```

`_Gdb` Hedef etkinlik bildirilen içindeki rastgele bir Başlatıcısı başlatır, `AndroidManifest.xml` dosya. Çalıştırmak için hangi etkinlik açıkça belirtmek için kullanın `RunActivity` MSBuild özelliği. Hizmetleri ve diğer Android yapıları başlatma işlemi şu anda desteklenmiyor.

`_Gdb` Hedef oluşturacak bir `gdb-symbols` dizin ve, hedefin içeriğini kopyalayın `/system/lib` ve `$APPDIR/lib` dizinleri yok.


> [!NOTE]
> İçeriğini `gdb-symbols` directory dağıttığınız Android hedef bağlıdır ve otomatik olarak olmayacaktır yerine hedef değiştirmeniz gerekir. (Bir hatayı düşünün.) Android hedef cihazlar değiştirirseniz, bu dizin el ile silmeniz gerekir.

Son olarak, oluşturulan kopyalama `gdb` komut ve kabuğunuzda yürütün:

```bash
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) bt
#0  0x40082e84 in nanosleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#1  0x4008ffe6 in sleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#2  0x74e46240 in ?? ()
#3  0x74e46240 in ?? ()
(gdb) c
```

<a name="Debug_Builds_without_Fast_Deployment" />

## <a name="debug-builds-without-fast-deployment"></a>Hata ayıklama derlemeleri hızlı dağıtım olmadan

Hata ayıklama derlemeleri *ile* hızlı dağıtım çalışan Android NDK's kopyalayarak `gdbserver` hızlı dağıtım programa `.__override__` dizin. Fast Deployment devre dışı bırakıldığında, bu dizin mevcut olmayabilir.

İki geçici çözüm vardır:

-   Ayarlama `debug.mono.log` sistem özelliği böylece `.__override__` dizin oluşturulur.
-   Dahil `gdbserver` içinde `.apk`.

### <a name="setting-the-debugmonolog-system-property"></a>Ayar `debug.mono.log` sistem özelliği

Ayarlanacak `debug.mono.log` sistem özelliği, kullanım `adb` komutu:

```bash
$ adb shell setprop debug.mono.log gc
```

Sistem özelliği ayarlandıktan sonra yürütme `_Gdb` hedef ve yazdırılan `gdb` komutunu olarak hata ayıklama yapıları ile hızlı dağıtım yapılandırması:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```


### <a name="including-gdbserver-in-your-app"></a>Dahil olmak üzere `gdbserver` uygulamanızda

Eklenecek `gdbserver` uygulamanızın içinde:

1. Bulma `gdbserver` içinde Android NDK (olmalıdır **$ANDROID\_NDK\_yolu/önceden oluşturulmuş/android-arm/gdbserver/gdbserver**) ve, proje dizinine kopyalayın.

2. Yeniden adlandırma `gdbserver` için **libs/armeabi-v7a/libgdbserver.so**.

3. Ekleme **libs/armeabi-v7a/libgdbserver.so** projenize bir **derleme eylemi** , `AndroidNativeLibrary`.

4. Yeniden oluşturun ve uygulamanızı yeniden yükleyin.

Uygulamayı yeniden sonra yürütme `_Gdb` hedef ve yazdırılan `gdb` komutunu olarak hata ayıklama yapıları ile hızlı dağıtım yapılandırması:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

<a name="Release_Builds" />

## <a name="release-builds"></a>Yayın Derlemeleri

`gdb` Destek üç şey gerektirir:

1.  `INTERNET` İzni.
2.  Uygulama hata ayıklama etkin.
3.  Erişilebilir bir `gdbserver`.

`INTERNET` İzni, hata ayıklama uygulamalarda varsayılan olarak etkinleştirilmiştir. Uygulamanızda mevcut değilse, düzenleme ya da ekleyebilir **Properties/AndroidManifest.xml** veya düzenleyerek [proje özellikleri](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest).

Uygulama hata ayıklama ya da ayarı tarafından etkinleştirilebilir [ApplicationAttribute.Debugging](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Debuggable/) özel öznitelik özelliğini `true`, veya düzenleyerek **Properties/AndroidManifest.xml** ve ayarı `//application/@android:debuggable` özniteliğini `true`:

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

Erişilebilir bir `gdbserver` izleyerek sağlanabilir [hata ayıklama yapıları hızlı dağıtım olmadan](#Debug_Builds_without_Fast_Deployment) bölümü.

Bir ipucu: `_Gdb` MSBuild hedefi daha önce çalışan uygulama örnekleri sonlandır. Bu, önceden Android v4.0 hedefler üzerinde çalışmaz.

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Sorun giderme

### <a name="monopmip-doesnt-work"></a>`mono_pmip` çalışmıyor

`mono_pmip` İşlevi (yararlı [Yönetilen yığın çerçevelerini alma](http://www.mono-project.com/docs/debug+profile/debug/#debugging-with-gdb)) öğesinden dışarı aktarılan `libmonosgen-2.0.so`, hangi `_Gdb` hedef değil şu anda çekme. (Bu gelecekteki bir sürümde düzeltilecektir.)

Bulunan çağırma işlevlerini etkinleştirmek için `libmonosgen-2.0.so`, hedef cihazda oturum kopyalama `gdb-symbols` dizini:

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

Ardından, hata ayıklama oturumunuzu yeniden başlatın.

### <a name="bus-error-10-when-running-the-gdb-command"></a>Veri yolu, hata: çalıştırırken 10 `gdb` komutu

Zaman `gdb` komutu ile hata `"Bus error: 10"`, Android cihazı yeniden başlatın.

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
Bus error: 10
$
```

### <a name="no-stack-trace-after-attach"></a>Sonra hiçbir yığın izlemesi Ekle

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
(gdb) bt
No stack.
```

Bu genellikle işaretidir, içeriğini `gdb-symbols` dizin ile Android hedef eşitlenmez. (Android hedef değişti?)

Lütfen silin `gdb-symbols` dizin ve yeniden deneyin.
