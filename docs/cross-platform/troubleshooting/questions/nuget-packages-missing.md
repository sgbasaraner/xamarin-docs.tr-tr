---
title: Nuget paketlerini güncelleştirdikten sonra eksik paketleri hatası
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: asb3993
ms.author: amburns
ms.openlocfilehash: 4f1c4f51b690e35711efc0fc4fac56565b9cb3c7
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Nuget paketlerini güncelleştirdikten sonra eksik paketleri hatası

Bu sorunu çoğunlukla Xamarin.Forms örnek uygulama çözümlerini bildirildi, ancak bu sorunun olası NuGet paketlerini kullanan herhangi bir projede oluşabilir. 

Proje veya çözüm Nuget paketlerini güncelleştirme varsa sonra eski paket sürüm numaraları gibi başvuruda bulunan bir hata görürsünüz:

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)

```

Bu örnekte *Xamarin.Forms.1.3.1.6296* Nuget paketi güncelleştirme kaldırıldı eski sürüm numarasıdır.

.Csproj dosyasındaki referans eski paket sürüm numarası XML öğeleri el ile eklenmiş bu durum veya düzenlenemeyen Nuget kaldırmaz veya proje olan paketler için artık arıyor böylece bunlar el ile eklenen/düzenlenebilir olsaydı bunları güncelleştirin silindi. 

Bu sorunu gidermek için el ile .csproj dosyaları düzenleyin ve referans eski sürüm numarası tüm öğeleri silin. 

(Eski paket sürüm numarasına sahip değilse) kaldırmak için örnek öğeleri:

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />

```

