---
title: "Nasıl ı TFS bırakma klasörüne IPA çıktı dosyalarını kopyalayabilirsiniz?"
ms.topic: article
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c89d81434cac43505c4f0341a10aaf4fc99407fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Nasıl ı TFS bırakma klasörüne IPA çıktı dosyalarını kopyalayabilirsiniz?

Açık `.csproj` dosyasını bir metin düzenleyicisinde iOS uygulama projesi için ve ardından sonuna şu satırları ekleyin (kapatmadan önce hemen `</Project>` etiketi):

```xml
<PropertyGroup>
    <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
    </CreateIpaDependsOn>
</PropertyGroup>

<Target Name="CopyIpa"
    Condition="'$(OutputType)' == 'Exe'
        And '$(ComputedPlatform)' == 'iPhone'
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>Notlar

-   Bu üzerinde ele alınan genel tekniğin aynısıdır [IPA dosyasının çıkış yolu değiştirebilirim?](~/ios/troubleshooting/questions/ipa-output-path.md). Ayarlamak için iki önemli noktaları olan `$(TF_BUILD_BINARIESDIRECTORY)` hedef klasör olarak ve bu nedenle ek bir koşul eklemek için `CopyIpa` TFS derlemeler için yalnızca çalıştırılır.

-   Bir açıklaması için `TF_BUILD_BINARIESDIRECTORY` bkz [https://msdn.microsoft.com/en-us/library/hh850448.aspx](https://msdn.microsoft.com/en-us/library/hh850448.aspx).

## <a name="additional-references"></a>Ek başvurular

- [Xamarin ile kullanmak için TFS yükleme belgeleri](https://docs.microsoft.com/vsts/tfvc/overview)
- [TFS derlemesi görevi: Xamarin.Android](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xamarin-android)
- [TFS derlemesi görevi: Xamarin.iOS](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Sonraki Adımlar
Bu belge, Visual Studio için Xamarin 3.11.666 itibariyle şu anki davranışı açıklanır ve Xamarin.iOS 8.10.3 Mac üzerinde konak oluşturun. Bizimle iletişime geçin veya bile Yukarıdaki bilgilerin kullanılarak sonra bu sorun devam ederse lütfen görmek için daha fazla yardım için [için Xamarin hangi destek seçenekleri kullanılabilir?](~/cross-platform/troubleshooting/support-options.md) önerileri, iletişim seçenekleri hakkında bilgi için nasıl yanı sıra Yeni bir hatanın gerekirse dosya. 



