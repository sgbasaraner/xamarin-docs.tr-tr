---
title: "Çıkış yolu IPA dosyasının değiştirebilir miyim?"
ms.topic: article
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 2cb5ef615bfd965ce3fbd4efbab7669fe12679a4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>Çıkış yolu IPA dosyasının değiştirebilir miyim?

## <a name="for-cycle-7-and-higher"></a>Döngü 7 ve üzeri
Evet, bunu başarmak için özelleştirilmiş MSBuild hedefleri kullanabilirsiniz. En basit seçenek büyük olasılıkla kopyalamaktır `.ipa` , oluşturulduktan sonra dosya.

Bu adımları, Mac veya Windows MSBuild yapı altyapısını kullanan herhangi bir iOS projesi için çalışır. (Not: tüm birleşik API projeleri MSBuild yapı altyapısını kullanır.)

1. Açık `.csproj` dosyasını bir metin düzenleyicisinde iOS uygulama projesi için ve ardından sonuna şu satırları ekleyin (kapatmadan önce hemen `</Project>` etiketi):
    
    ```
    <PropertyGroup>
           <CreateIpaDependsOn>
           $(CreateIpaDependsOn);
            CopyIpa
           </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. DestinationFolder istenen çıkış klasörünü ayarlayın. MSBuild özellikleri (ör. isterseniz bu bağımsız değişken içinde $(OutputPath)). her zamanki gibi kullanabilirsiniz

## <a name="notes"></a>Notlar
- `CreateIpaDependsOn` Özelliği tanımlanmış `Xamarin.iOS.Common.targets` dosya Xamarin.iOS parçası. Altında açıklandığı gibi davranır *geçersiz kılınıyor 'DependsOn' özellikleri* üzerinde [https://msdn.microsoft.com/en-us/library/ms366724.aspx](https://msdn.microsoft.com/en-us/library/ms366724.aspx).

- Kullanabileceğinizi bir **taşıma** görev yerine bir **kopyalama** tercih ettiğiniz durumunda görev. Windows üzerinde seçeneği ve oluşturmakta olduğunuz seçerseniz, görev tam adı kullanmanız gerekecektir `<Microsoft.Build.Tasks.Move>` XamarinVS ile bir belirsizlik önlemek için Görevler oluşturun.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Xamarin Studio 6.0.0.5174 önceki sürümler için | Visual Studio 4.1.0.530 için Xamarin

Evet, bunu başarmak için özelleştirilmiş MSBuild hedefleri kullanabilirsiniz. En basit seçenek büyük olasılıkla kopyalamaktır `.ipa` , oluşturulduktan sonra dosya.

Bu adımları, Mac veya Windows MSBuild yapı altyapısını kullanan herhangi bir iOS projesi için çalışır. (Not: tüm birleşik API projeleri MSBuild yapı altyapısını kullanır.)

1. Açık `.csproj` dosyasını bir metin düzenleyicisinde iOS uygulama projesi için ve ardından sonuna şu satırları ekleyin (kapatmadan önce hemen `</Project>` etiketi).

    ```csharp
    <PropertyGroup>
        <CreateIpaDependsOn>
            $(CreateIpaDependsOn);
            CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. Ayarlama `DestinationFolder` istenen çıkış klasörüne. MSBuild özellikleri her zamanki gibi kullanabilirsiniz (gibi `$(OutputPath)`) içinde isterseniz bu bağımsız değişken.

## <a name="notes"></a>Notlar
- `CreateIpaDependsOn` Özelliği tanımlanmış `Xamarin.iOS.Common.targets` dosya Xamarin.iOS parçası. Altında açıklandığı gibi davranır *geçersiz kılınıyor "DependsOn" özellikleri* üzerinde [https://msdn.microsoft.com/en-us/library/ms366724.aspx](https://msdn.microsoft.com/en-us/library/ms366724.aspx).

- Kullanabileceğinizi bir **taşıma** görev yerine bir **kopyalama** tercih ettiğiniz durumunda görev. Windows üzerinde seçeneği ve oluşturmakta olduğunuz seçerseniz, görev tam adı kullanmanız gerekecektir `<Microsoft.Build.Tasks.Move>` XamarinVS ile bir belirsizlik önlemek için Görevler oluşturun.
