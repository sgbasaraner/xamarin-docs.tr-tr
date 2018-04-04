---
title: Yerel kitaplıklarını kullanma
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 0fa66f3a16047c18af19cb7257c778b498bc0c9b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="using-native-libraries"></a>Yerel kitaplıklarını kullanma

Xamarin.Android yerel kitaplıkları standart PInvoke mekanizması aracılığıyla kullanımını destekler. Ayrıca, .apk içine işletim Sisteminin parçası olmayan ek yerel kitaplıklar paketleyebilirsiniz.

Bir Xamarin.Android uygulaması sahip yerel bir kitaplık dağıtmak için projeye ikili kitaplığı ekleyin ve ayarlama kendi **yapı eylemi** için **AndroidNativeLibrary**.

Bir Xamarin.Android kitaplık projesine sahip yerel bir kitaplık dağıtmak için projeye ikili kitaplığı ekleyin ve ayarlama kendi **yapı eylemi** için **EmbeddedNativeLibrary**.

Xamarin.Android Android birden çok uygulama ikili arabirimi (ABIs) desteklediğinden, yerel kitaplığı hangi ABI bilmelisiniz not için yerleşik olarak bulunur.
Bu yapılabilir iki yolu vardır:

1.  Algılaması yolu""
1.  Kullanarak bir `AndroidNativeLibrary/Abi` proje dosyası içindeki öğesi


Yol algılaması ile yerel kitaplığı üst dizin adını ABI belirtmek için kullanılır, kitaplık hedefler. Bu nedenle, eklerseniz `lib/armeabi/libfoo.so` projeye sonra ABI "olarak sniffed" `armeabi`.

Alternatif olarak, kullanılacak ABI açıkça belirtmek için proje dosyanızı düzenleyebilirsiniz:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Yerel kitaplıklarını kullanma hakkında daha fazla bilgi için bkz: [yerel kitaplıkları ile birlikte çalışma](http://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio-2015"></a>Visual Studio 2015 ile yerel kodda hata ayıklama

Kullanıyorsanız, *Visual Studio 2015*, proje dosyalarınızı (yukarıda açıklandığı gibi) değiştirmek zorunda değilsiniz.
Derleme ve C++, C++ projesi başvuru ekleyerek Xamarin.Android çözümünüz içinde hata ayıklama **dinamik paylaşılan kitaplığı (Android)** projesi.

Visual Studio C++ geliştiriciler görebilir [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) C++; Xamarin ile Visual Studio 2015 içinden hata ayıklama denemek için örnek ve başvurulacak bizim [blog gönderisi](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) daha fazla bilgi için.



## <a name="related-links"></a>İlgili bağlantılar

- [SanAngeles_NativeDebug (örnek)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
