---
title: 'Ibtool hatası: İşlem tamamlanamadı.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 593706de23d370d6bff03c97bb50e064d77a52ef
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350738"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>Ibtool hatası: İşlem tamamlanamadı.

## <a name="fixed-in-xcode-611"></a>Xcode'da 6.1.1 düzeltildi

Apple [sabit](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) bu `ibtool` Xcode 6.1.1, bu nedenle Xcode 6.1.1 için yükseltme veya daha yüksek hata olduğunu kolay düzeltme.

* * *

## <a name="description-of-the-problem"></a>Sorun açıklaması

`ibtool` Xcode 6.0 komutta, OS X 10.10 Yosemite üzerinde bir hata vardı. Xamarin.iOS kullanan Xcode'un `ibtool` film şeritleri derlemeye ve `XIB` dosyaları.

Aşağıdaki Xcode ile ilgili hata hakkında daha fazla bilgi bulunabilir yığın taşması gönderi: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>hata iletisi

> "MainStoryboard.storyboard" Belge açılamadı. İşlem tamamlanamadı. (com.apple.InterfaceBuilder hata -1.)

## <a name="workarounds-for-xcode-60"></a>Geçici Çözümler (için Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>1. seçenek: tüm yönetmek `UIImageView.Image` kod özellikleri

Ayar yerine `Image` özelliği bir `UIImageView` film şeridinde veya `.xib` dosyası ayarlayabilirsiniz özelliği bir görünümün görünüm denetleyicisi yaşam döngüsü geçersiz kılma yöntemleri (örneğin, `ViewDidLoad()`). Ayrıca bkz: [görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) kullanma hakkında ipuçları için `UIImage.FromBundle()` karşılaştırması `UIImage.FromFile()`.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>2. seçenek: tüm görüntü kaynakları için üst düzey taşıma `Resources` klasörü

Görüntüleri için üst düzey taşıdıktan `Resources` klasörü, film şeridini güncelleştirme gerekir ve `.xib` dosyaların yeni görüntü yollarını kullanın.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Seçenek 3: Kümesi `LogicalName` için üst düzey kopyalanırlar. böylece tüm sorunlu görüntü varlıkları için`.app` paket

Örneğin, orijinal düşünelim `.csproj` dosyası şu girdiyi içerir:

`<BundleResource Include="Resources\Images\image.png" />`

Bu öğeyi değiştirmek ve ekleme bir `LogicalName` böylece görüntü bunun yerine en üst düzeye kopyalanacak `.app `paket:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

Mac için Visual Studio'da `LogicalName` kullanılarak da ayarlanabilir `Resource ID` alan için bir görüntü altında **Görüntüle > doldurmalar > Özellikleri**. (Ayrıca bkz: [ http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Bu değişiklikten sonra görsel taslak güncelleştirmeniz gerekir ve `.xib` dosyaların yeni üst düzey görüntü yollarını kullanın. Mac için Visual Studio için otomatik tamamlama listesinde otomatik olarak güncelleştirilir `Image` iOS Designer'daki özelliği. Visual Studio'da yolu el ile düzenlemeniz gerekir. İOS Designer sonra bu eksik bir görüntü olarak görüntülenir, ancak projeyi derleyin ve doğru bir şekilde çalıştırın.

### <a name="next-steps"></a>Sonraki Adımlar

Bizimle iletişim kurun ya da Yukarıdaki bilgilerin kullanan daha sonra bu sorun devam ederse lütfen görmek için daha fazla yardım için [Xamarin için hangi destek seçenekleri mevcuttur?](~/cross-platform/troubleshooting/support-options.md) önerileri olan iletişim seçenekleri hakkında bilgi için nasıl yanı sıra Gerekirse yeni hata bildirin. 

