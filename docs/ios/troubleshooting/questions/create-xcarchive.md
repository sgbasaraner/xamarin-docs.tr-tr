---
title: Visual Studio'dan .xcarchive arşivi oluşturma mümkündür?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 1c20534e1d4476ce7ff85d9ffd5ae8742c445983
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350216"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Visual Studio'dan .xcarchive arşivi oluşturma mümkündür?

## <a name="for-xamarin-4"></a>Xamarin 4 için

Xamarin itibarıyla 4.x mümkündür şimdi oluşturmak bir `.xcarchive` ayarlayarak Windows gelen `ArchiveOnBuild` özelliğini `true`. Örneğin, kullanarak `MSBuild` komut satırında:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive` Yerleştirileceği `$HOME/Library/Developer/Xcode/Archives` hem Xcode ve Xamarin Studio için daha önce oluşturulmuş görüntü arşivleri arama Mac derleme konağı üzerinde dizin.

Bkz. Bu [Xamarin Forum gönderisini](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) için bazı ek notlar hakkında kısa `ArchiveOnBuild` özelliği. Belgeler hakkında bilgi [Xamarin.iOS komut satırı derlemeleri üzerinde Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) hakkında ek ayrıntılar için `ServerAddress` ve `ServerUser` özellikleri.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Xamarin için 3 ve öncesi

Xamarin 3.x Visual Studio uzantısı üretmek için bir mekanizma sağlamaz `.xcarchive` arşivler. Bununla birlikte, oluşturmak için kullanılan mantıksal `.xcarchive` arşivleri Mac'te Xamarin Studio [burada açıklanan](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), büyük olasılıkla kendi oluşturduğunuz böylece `.xcarchive` , el ile ayarlarını şahsen yapmasını istediniz.

Ancak, gerekmez hatalarının ayıklanabileceğini belirtmekte yarar bir `.xcarchive` göndermek için App Store için. Bir App Store dağıtım profili (değil bir geçici dağıtım profili) ile imzalı olmadığı sürece bir IPA dosyasını gönderebilirsiniz.

Aslında, bile yalnızca zip `.app` (yani bir App Store dağıtım profiliyle imzalanan), paket ve göndermek, `.zip` app Store'a dosya.

Her iki durumda da, uygulama (yerine Xcode) göndermek için uygulama yükleyicisi uygulama kullanabilirsiniz.

