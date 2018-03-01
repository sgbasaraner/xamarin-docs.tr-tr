---
title: "IBTool hata: İşlemi tamamlanamadı."
ms.topic: article
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: dd668859428da1abfa3a8e46a0810b2de6645fe2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool hata: İşlemi tamamlanamadı.

## <a name="fixed-in-xcode-611"></a>Xcode 6.1.1 sabit

Apple [sabit](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) bu `ibtool` hatasıdır 6.1.1, bu nedenle Xcode 6.1.1 yükseltme veya daha yüksek xcode'da kolay düzeltme.

* * *

## <a name="description-of-the-problem"></a>Sorun açıklaması

`ibtool` Xcode 6.0 komutta, OS X 10.10 Yosemite üzerinde bir hata vardı. Xamarin.iOS Xcode'nın kullandığı `ibtool` film şeritleri derlemek için ve `XIB` dosyaları.

Xcode ile ilgili hata hakkında daha fazla bilgi için şu sunuculara bulunabilir yığın taşması post: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Hata iletisi

> "MainStoryboard.storyboard" Belge açılamadı. İşlem tamamlanamadı. (com.apple.InterfaceBuilder error -1.)

## <a name="workarounds-for-xcode-60"></a>Geçici Çözümler (için Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Seçenek 1: tüm yönetmek `UIImageView.Image` kodda özellikleri

Ayar yerine `Image` özelliği bir `UIImageView` içinde film şeridi veya `.xib` dosyası ayarlayabilirsiniz özelliği bir görünümün görünüm denetleyicisini yaşam döngüsü geçersiz kılma yöntemleri (örneğin, `ViewDidLoad()`). Ayrıca bkz. [görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) kullanımı hakkında ipuçları için `UIImage.FromBundle()` karşılaştırması `UIImage.FromFile()`.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Seçenek 2: tüm görüntü kaynakları için en üst düzey taşıma `Resources` klasörü

Görüntüleri için üst düzey taşıdıktan `Resources` klasörü, film şeridi güncellemeniz gerekir ve `.xib` yeni görüntü yollarını kullanmak üzere dosyaları.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Seçenek 3: Ayarlamak `LogicalName` için üst düzey kopyalanırlar şekilde sorunlu görüntü varlıklar için`.app` paket

Örneğin, orijinal deyin `.csproj` dosyası şu girdiyi içerir:

`<BundleResource Include="Resources\Images\image.png" />`

Bu öğe değiştirebilir ve ekleyebilir bir `LogicalName` böylece görüntü bunun yerine en üst düzeye kopyalanacak `.app `paket:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

Mac için Visual Studio'da `LogicalName` kullanılarak da ayarlanabilir `Resource ID` altındaki görüntüyü alanındaki **Görünüm > klavye takımı > Özellikler**. (Ayrıca bkz: [http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Bu değişiklikten sonra film şeridi güncelleştirmeniz gerekir ve `.xib` yeni üst düzey görüntü yollarını kullanmak üzere dosyaları. Mac için Visual Studio için Otomatik Tamamlama listesini otomatik olarak güncelleştirilecek `Image` Tasarımcısı iOS özelliği. Visual Studio'da yolu el ile düzenlemeniz gerekir. Tasarımcısı iOS sonra bu eksik bir görüntü olarak görüntülenir, ancak projeyi oluşturmak ve düzgün bir şekilde çalışmaz.

### <a name="next-steps"></a>Sonraki Adımlar

Bizimle iletişime geçin veya bile Yukarıdaki bilgilerin kullanılarak sonra bu sorun devam ederse lütfen görmek için daha fazla yardım için [için Xamarin hangi destek seçenekleri kullanılabilir?](~/cross-platform/troubleshooting/support-options.md) önerileri, iletişim seçenekleri hakkında bilgi için nasıl yanı sıra Yeni bir hatanın gerekirse dosya. 

