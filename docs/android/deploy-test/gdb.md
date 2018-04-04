---
title: GDB
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 1292db3534570dace90639958a3d5be9f6466716
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="gdb"></a>GDB

## <a name="overview"></a>Genel Bakış

Xamarin.Android 4.10 sunulan kullanarak kısmi desteği `gdb` kullanarak `_Gdb` MSBuild hedef. 

> [!NOTE]
> `gdb` Destek Android NDK yüklü olmasını gerektirir.

Kullanmak için üç yolu vardır `gdb`:

1.  [Hata ayıklama derlemeleri hızlı etkin dağıtımı ile](#Debug_Builds_with_Fast_Deployment) .
1.  [Hata ayıklama derlemeleri hızlı devre dışı dağıtımı ile](#Debug_Builds_without_Fast_Deployment) .
1.  [Yayın derlemeleri](#Release_Builds) .


Şeyler ters gittiğinde, lütfen bkz. [sorun giderme](#Troubleshooting) bölümü.

<a name="Debug_Builds_with_Fast_Deployment" />

### <a name="debug-builds-with-fast-deployment"></a>Hata ayıklama derlemeleri hızlı dağıtım

Oluşturma ve bir hata ayıklama dağıtma hızlı etkinse, dağıtımı ile derlerken `gdb` kullanarak eklenebilecek `_Gdb` MSBuild hedef.

İlk olarak, yükleme uygulama. Bu, IDE aracılığıyla veya komut satırı aracılığıyla yapılabilir:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:Install *.csproj
```

İkincisi, çalıştırmak `_Gdb` hedef. Yürütme, sonunda bir `gdb` komut satırı yazdırılabilir:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
...
```

`_Gdb` Hedef etkinlik bildirilen içinde rastgele bir Başlatıcısı başlayacaktır, `AndroidManifest.xml` dosya. Çalıştırmak için hangi etkinlik açıkça belirtmek için kullanın `RunActivity` MSBuild özelliği. Hizmetleri ve diğer Android yapıları başlangıç şu anda desteklenmiyor.

`_Gdb` Hedef oluşturacak bir `gdb-symbols` dizin ve, hedefin içeriğini kopyalayın `/system/lib` ve `$APPDIR/lib` dizinleri vardır.


> [!NOTE]
> İçeriğini `gdb-symbols` dizin dağıttığınız Android hedef bağlıdır ve otomatik olarak olmaz yerine hedef değiştirmeniz gerekir. (Bu hatayı göz önünde bulundurun.) Android hedef cihazlar değiştirirseniz, bu dizin el ile silmeniz gerekir.

Son olarak, oluşturulan kopyalama `gdb` komut ve Kabuğu'nda yürütün:

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

## <a name="debug-builds-without-fast-deployment"></a>Hata ayıklama derlemeleri hızlı dağıtımı olmadan

Hata ayıklama derlemeleri *ile* hızlı dağıtım çalışması Android NDK's kopyalayarak `gdbserver` hızlı dağıtım programa `.__override__` dizin. Hızlı Dağıtım devre dışı bırakıldığında, bu dizin var olmayabilir.

İki geçici çözüm vardır:

-   Ayarlama `debug.mono.log` sistem özelliği böylece `.__override__` dizin oluşturulur.
-   Dahil `gdbserver` içinde `.apk`.

### <a name="setting-the-debugmonolog-system-property"></a>Ayarı `debug.mono.log` sistem özelliği

Ayarlamak için `debug.mono.log` sistem özelliği, kullanım `adb` komutu:

```bash
$ adb shell setprop debug.mono.log gc
```

Sistem özelliği ayarladıktan sonra yürütme `_Gdb` hedef ve yazdırılan `gdb` komutunu, olarak hata ayıklama yapıları ile hızlı dağıtım yapılandırması:

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

Eklenecek `gdbserver` , uygulamanızın içinde:

1. Bul `gdbserver` Android NDK içinde (olmalıdır **$ANDROID\_NDK\_yol/önceden oluşturulmuş/android-arm/gdbserver/gdbserver**) ve, proje dizinine kopyalayın.

2. Yeniden Adlandır `gdbserver` için **libs/armeabi-v7a/libgdbserver.so**.

3. Ekleme **libs/armeabi-v7a/libgdbserver.so** ile projenize bir **yapı eylemi** , `AndroidNativeLibrary`.

4. Yeniden oluşturun ve uygulamanızı yeniden yükleyin.

Uygulama yeniden sonra yürütme `_Gdb` hedef ve yazdırılan `gdb` komutunu, olarak hata ayıklama yapıları ile hızlı dağıtım yapılandırması:

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

`INTERNET` İzni hata ayıklama uygulamalarında varsayılan olarak etkinleştirilir. Uygulamanızda mevcut değilse, onu da düzenleyerek eklemeniz **Properties/AndroidManifest.xml** veya düzenleyerek [proje özelliklerini](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/).

Uygulama hata ayıklama etkin olabilir ya da ayarıyla [ApplicationAttribute.Debugging](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Debuggable/) özel öznitelik özelliğine `true`, ya da düzenleyerek **Properties/AndroidManifest.xml** ve ayarı `//application/@android:debuggable` özniteliğini `true`:

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

Erişilebilir bir `gdbserver` izleyerek sağlanabilir [hata ayıklama yapıları hızlı dağıtımı olmadan](#Debug_Builds_without_Fast_Deployment) bölümü.

Bir ipucu: `_Gdb` MSBuild hedef daha önce çalışan uygulama örnekleri KILL. Bu, önceden Android v4.0 hedeflerde çalışmaz.

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Sorun giderme

### <a name="monopmip-doesnt-work"></a>`mono_pmip` çalışmıyor

`mono_pmip` İşlevi (için yararlı [Yönetilen yığın çerçeveleri alma](http://www.mono-project.com/docs/debug+profile/debug/#debugging-with-gdb)) den dışarı aktarılan `libmonosgen-2.0.so`, hangi `_Gdb` hedef değil şu anda çekme. (Bu bir sonraki sürümde düzeltilecektir.)

Bulunan işlevleri çağırma etkinleştirmek için `libmonosgen-2.0.so`, hedef cihazda kopyalamak `gdb-symbols` dizini:

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

Ardından, hata ayıklama oturumu yeniden başlatın.

### <a name="bus-error-10-when-running-the-gdb-command"></a>Veri yolu, hata: çalıştırırken 10 `gdb` komutu

Zaman `gdb` komut çıkışı hatalarla `"Bus error: 10"`, Android cihazı yeniden başlatın.

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
Bus error: 10
$
```

### <a name="no-stack-trace-after-attach"></a>Sonra hiçbir yığın izleme ekleme

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
(gdb) bt
No stack.
```

Bu genellikle işaretidir, içeriğini `gdb-symbols` dizin Android hedefiniz ile eşitlenmemiş. (Android hedef değiştirdiniz?)

Lütfen silin `gdb-symbols` dizin ve yeniden deneyin.
