---
title: Android Tasarımcı Java bellek parametrelerini ayarlama
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 691be280b80e379863cc09d0f1bba0ff5882cf21
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732927"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Android Tasarımcı Java bellek parametrelerini ayarlama

Başlatma sırasında kullanılan varsayılan bellek parametreleri `java` işlemek için Android Tasarımcısı bazı sistem yapılandırmasıyla uyumsuz olabilir.

Xamarin Studio 5.7.2.7 (ve daha sonra Visual Studio Mac için) ile başlayan ve Visual Studio Araçları için Xamarin 3.9.344, bu ayarlar bir proje başına temelinde özelleştirilebilir.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Yeni Android Tasarımcısı özellikleri ve karşılık gelen Java seçenekleri

Aşağıdaki özellik adlarını belirtilen java karşılık gelen [komut satırı seçeneği](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** - Xms

- **AndroidDesignerJavaRendererMaxMemory** - Xmx

- **AndroidDesignerJavaRendererPermSize** - XX: MaxPermSize


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Çözümünüzü Visual Studio'da açın.

2.  Çözüm Gezgini'nde her Android projesi birer birer seçin ve'ı tıklatın [tüm dosyaları göster](https://msdn.microsoft.com/en-us/library/4afxey9h.aspx) her projede iki kez. Herhangi içermediğinden projeleri atlayabilirsiniz `.axml` düzeni dosyaları. Bu adımı her proje dizini içerdiğinden emin olun bir `.csproj.user` dosyası.

3.  Visual Studio çıkın.

4.  Bulun `.csproj.user` 2. adımda projelerin her biri için dosya.

5.  Her Düzenle `.csproj.user` dosyasını bir metin düzenleyicisinde.

6.  İçindeki yeni bir Android Tasarımcı bellek özellik bir bölümünü veya tamamını eklemek bir `<PropertyGroup>` öğesi. Var olan kullanabilirsiniz `<PropertyGroup>` veya yeni bir tane oluşturun. Tam bir örnek şudur `.csproj.user` tüm 3 öznitelikleri içeren bir dosya varsayılan değerlerine ayarlayın:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7.  Kaydet ve Kapat güncelleştirilmiş tümünün `.csproj.user` dosyaları.

8.  Visual Studio'yu yeniden başlatın ve çözümünüzü yeniden açın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Açık çözüm dizini emin olmak Mac için Visual Studio çözümünüzde içeren bir `.userprefs` dosyası.

2.  Mac için Visual Studio çıkın

3.  Bulun `.userprefs` çözüm dizini dosyasında.

4.  Düzen `.userprefs` dosyasını bir metin düzenleyicisinde.

5.  Varolan XML öğesi aşağıdaki biçimde bulun. Bu öğe adı son parçası projenizin adını eşleşir: Bu örnekte "AndroidApplication1":

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  Varsa `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` öğesi yok, herhangi bir yere kapsayan içinde oluştur `<Properties>` öğesi. "AndroidApplication1" projenizin adı ile değiştirdiğinizden emin olun.

7.  Öğe özniteliklerinde olarak ya da yeni Android Tasarımcı bellek özelliklerin tümünü ekleyin. Tam bir örnek şudur `.userprefs` tüm 3 öznitelikleri içeren bir dosya varsayılan değerlerine ayarlayın:

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8.  Her bir Android projesi herhangi içeren çözümü ile için 5-7 arasındaki adımları yineleyin `.axml` düzeni dosyaları. (Diğer bir deyişle, bir tane ekleyin `<MonoDevelop.Ide.ItemProperties.ProjectName>` öğesi her proje için.)

9.  Kaydet ve Kapat `.userprefs` dosya.

10. Mac için Visual Studio'yu yeniden başlatın ve çözümünüzü yeniden açın.

-----

