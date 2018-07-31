---
title: Yerel kitaplıkları kullanma
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 9175996f516a980d915d1501b4b18ea23ec86cef
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353586"
---
# <a name="using-native-libraries"></a>Yerel kitaplıkları kullanma

Xamarin.Android standart PInvoke mekanizması aracılığıyla yerel kitaplıkları kullanımını destekler. Ayrıca, .apk içine işletim Sisteminin bir parçası olmayan ek yerel kitaplıklar gruplandırabilirsiniz.

Bir Xamarin.Android uygulaması ile yerel bir kitaplığı dağıtmak için ikili kitaplığı projeye ekleyin ve ayarlamak kendi **derleme eylemi** için **AndroidNativeLibrary**.

Bir Xamarin.Android kitaplık projesi ile yerel bir kitaplığı dağıtmak için ikili kitaplığı projeye ekleyin ve ayarlamak kendi **derleme eylemi** için **EmbeddedNativeLibrary**.

Xamarin.Android Android birden çok uygulama ikili arabirimi (Abı'ler) desteklediğinden, yerel kitaplık hangi ABI bilmelisiniz Not oluşturulmuştur.
Bu yapılabilir iki yolu vardır:

1.  Yoklama yolu""
1.  Kullanarak bir `AndroidNativeLibrary/Abi` öğesi içinde proje dosyası


Yol algılaması ile yerel kitaplığı üst dizin adını ABI belirtmek için kullanılan, kitaplık hedefleri. Bu nedenle, eklerseniz `lib/armeabi/libfoo.so` projeye, ardından ABI "olarak sızılmasını" `armeabi`.

Alternatif olarak, kullanılacak ABI açıkça belirtmek için proje dosyanızı düzenleyebilirsiniz:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Yerel kitaplıkları kullanma hakkında daha fazla bilgi için bkz. [yerel kitaplıkları ile birlikte](http://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio-2017"></a>Visual Studio 2017 ile yerel kodda hata ayıklama

Kullanıyorsanız *Visual Studio 2017* ya da daha yukarıda açıklandığı gibi proje dosyalarında değişiklik gerekmez.
Derleme ve C++ proje başvuru ekleyerek Xamarin.Android çözümünüz içinde C++ hata ayıklama **dinamik paylaşılan kitaplık (Android)** proje. 

Projenizdeki yerel C++ kod hatalarını ayıklamak için şu adımları izleyin:

1. Proje çift **özellikleri** seçip **Android seçenekleri** sayfası.
2. Ekranı aşağı kaydırarak **hata ayıklama seçenekleri**.
3. İçinde **hata ayıklayıcı** açılır menüsünde, select **C++** (varsayılan yerine **.Net (Xamarin)**).

Visual Studio C++ geliştiricileri görebilirsiniz [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) bakın ve Visual Studio 2017; Xamarin ile C++ hata ayıklama denemek için örnek bizim [blog gönderisi](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) daha fazla bilgi için.



## <a name="related-links"></a>İlgili bağlantılar

- [SanAngeles_NativeDebug (örnek)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
- [Xamarin Android yerel uygulamaları geliştirme](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
