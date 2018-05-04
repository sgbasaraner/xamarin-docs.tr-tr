---
title: .NET katıştırma yükleme
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
ms.technology: xamarin-cross-platform
author: chamons
ms.author: chhamo
ms.date: 4/18/2018
ms.openlocfilehash: d4ee3ef610611dd489d90a364d15f727ea49a79e
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-net-embedding"></a>.NET katıştırma yükleme

## <a name="installing-net-embedding-from-nuget"></a>.NET Nuget'ten katıştırma yükleme

Seçin **Ekle > NuGet paketleri Ekle...**  yükleyip **Embeddinator 4000** NuGet Paket Yöneticisi'nden:

![NuGet Paket Yöneticisi](images/visualstudionuget.png)

Bu yükleyecek **Embeddinator 4000.exe** ve **objcgen** içine **paketleri/Embeddinator-4000/tools** dizin.

Genel olarak, Embeddinator 4000 en son sürümü karşıdan yüklemek için seçili olmalıdır. Objective-C Destek gerektiren 0.4 veya sonraki bir sürümü.

## <a name="running-manually"></a>El ile çalıştırmayı

NuGet yüklü, araç el ile çalıştırabilirsiniz.

- Terminal (macOS) veya bir komut istemi (Windows) açın.
- Çözüm kök dizini değiştirin
- Araç yüklenir:
    - **. / Embeddinator/paketler-4000. [Sürüm] / Araçlar/objcgen** (Objective-C)
    - **. / Embeddinator/paketler-4000. [VERSION]/tools/Embeddinator-4000.exe** (Java/C) 
- MacOS üzerinde **objcgen** doğrudan çalıştırılabilir. 
- Windows, **Embeddinator 4000.exe** doğrudan çalıştırılabilir.
- MacOS üzerinde **Embeddinator 4000.exe** ile çalıştırılması gerektiğini **mono**: 
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

Her komut çağırma platforma özgü belgelerinde listelenen parametre sayısı gerekir.

## <a name="automatic-binding-generation"></a>Otomatik bağlama oluşturma

Yapı işleminizin parçası .NET katıştırma otomatik olarak çalıştırmak için iki yaklaşım vardır.

- Özel MSBuild hedefleri
- Derleme sonrası adımları

Bu belgede her ikisi de açıklanmıştır olsa da, bir özel MSBuild hedef kullanarak tercih edilir. Komut satırından derlerken, derleme sonrası adımları mutlaka çalışmaz.

### <a name="custom-msbuild-targets"></a>Özel MSBuild hedefleri

MSbuild hedefleri olan yapınızın özelleştirmek için önce oluşturun bir **Embeddinator 4000.targets** dosya yanında, csproj aşağıdakine benzer:

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

Burada, platforma özgü belgelerinde listelenen .NET katıştırma çağrılarını birini oturum komutunun metin doldurulmalıdır.

Bu hedef kullanmak için:

- Mac için Visual Studio 2017 veya Visual Studio projenizde kapatın
- Kitaplık csproj bir metin düzenleyicisinde açın
- En son yukarıda altındaki bu satırı ekleyin `</Project>` satır:

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- Projenizi yeniden

### <a name="post-build-steps"></a>Derleme sonrası adımları

.NET katıştırma çalıştırmak için bir oluşturma sonrası adımı eklemek için adımları IDE bağlı olarak değişir:

#### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

Mac için Visual Studio'da Git **proje Seçenekleri > Yapı > özel komutları** ve ekleme bir **sonra yapı** adım.

Platforma özgü belgelerinde listelenen komutu ayarlayın.

> [!NOTE]
> Nuget'ten yüklü sürüm numarası kullandığınızdan emin olun

C# proje üzerinde devam eden geliştirme yaparak kullanacaksanız, temizlemek için özel bir komut ayrıca ekleme olasılığınız **çıkış** dizin .NET katıştırma çalıştırılmadan önce:

```shell
rm -Rf '${SolutionDir}/output/'
```

![Özel derleme eylemi](images/visualstudiocustombuild.png)

> [!NOTE]
> Belirtilen dizin oluşturulmuş bağlama yerleştirilecek `--outdir` veya `--out` parametresi. Bazı platformlar, paket adları sınırlamaları vardır oluşturulan bağlama adı değişebilir.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Biz temelde aynı anlama ayarlanır, ancak Visual Studio 2017 menülerde biraz farklıdır. Kabuk komutları da biraz farklıdır.

Git **proje Seçenekleri > Yapı olayları** ve platforma özgü belgeleri listelenen komutu girin **oluşturma sonrası olay komut satırı** kutusu. Örneğin:

![.NET Windows katıştırma](images/visualstudiowindows.png)
