---
title: Visual Studio'dan .xcarchive arşivini oluşturmak mümkün mü?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 23af3bf277ab68ffe93df2e1ee8ee64afc01828d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776813"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Visual Studio'dan .xcarchive arşivini oluşturmak mümkün mü?

## <a name="for-xamarin-4"></a>Xamarin 4

Xamarin itibariyle 4.x, bunu şimdi oluşturmak olası bir `.xcarchive` ayarlayarak Windows `ArchiveOnBuild` özelliğine `true`. Örneğin, kullanarak `MSBuild` komut satırında:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive` Yerleştirilecek `$HOME/Library/Developer/Xcode/Archives` Xcode ve Xamarin Studio daha önce oluşturulmuş görüntü arşivler arama Mac yapı ana bilgisayarda dizin.

Bu bkz [Xamarin forumları sonrası](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) bazı hakkında ek notlar kısa için `ArchiveOnBuild` özelliği. Belgelerine bakın [Xamarin.iOS komut satırı derlemeleri Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) hakkında ek ayrıntılar için `ServerAddress` ve `ServerUser` özellikleri.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Xamarin 3 ve önceki sürümleri

Xamarin 3.x Visual Studio uzantısı üretmek için bir mekanizma sağlamaz `.xcarchive` arşivler. Bununla oluşturmak için kullanılan mantığı `.xcarchive` arşivler Mac üzerinde Xamarin Studio'da [burada açıklanan](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), büyük olasılıkla kendi oluşturabilirsiniz `.xcarchive` el ile onlardan durumunda.

Ancak, gerekmeyen belirtmeye değerinde bir `.xcarchive` uygulama Mağazası'na göndermek için. Bir uygulama deposu dağıtım profili (olmayan bir Ad Hoc dağıtım profili) ile imzalanmış olduğu sürece, bir IPA dosyası gönderebilirsiniz.

Aslında, hatta yalnızca zip `.app` (yani bir uygulamanın depolamak dağıtım profili ile imzalanmış) paket ve, gönderme `.zip` uygulama mağazası dosya.

Her iki durumda da uygulama (yerine Xcode) göndermek için uygulama yükleyicisi uygulama kullanabilirsiniz.

