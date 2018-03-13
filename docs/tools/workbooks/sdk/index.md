---
title: "Xamarin çalışma kitaplarını SDK ile çalışmaya başlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 3978b046a8ab4d42cbf86bf524452a033b5dbb4d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Xamarin çalışma kitaplarını SDK ile çalışmaya başlama

Bu belge tümleştirmeler Xamarin çalışma kitapları için geliştirme ile çalışmaya başlama için hızlı bir kılavuz sağlar. Bu çoğunu kararlı Xamarin kitaplarıyla çalışır ancak **NuGet paketlerini aracılığıyla tümleştirmeler yüklenirken yalnızca desteklenir çalışma kitaplarını 1.3**, alfa kanal yazma zaman.

# <a name="general-overview"></a>Genel bakış

Xamarin çalışma kitaplarını tümleştirmeler olan kullanmak küçük kitaplıkları [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] SDK Xamarin çalışma kitaplarını ve Inspector ile geliştirilmiş deneyimler sağlamak için aracıları tümleştirmek için.

Bir tümleştirme geliştirme ile çalışmaya başlama 3 ana adım vardır: biz bunları burada anahat.

## <a name="creating-the-integration-project"></a>Tümleştirme projesi oluşturma

Tümleştirme kitaplıkları en iyi birden çok platformlu kitaplıkları geliştirilir. Tüm kullanılabilir aracıları, geçmiş ve gelecekteki en iyi tümleştirme sağlamak istediğiniz çünkü geniş çapta desteklenen bir dizi seçmek istersiniz. Geniş desteği "Taşınabilir Kitaplığı" şablonu kullanmanızı öneririz:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Mac için taşınabilir kitaplığı şablonu Visual Studio](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Taşınabilir Kitaplığı şablonu Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

Visual Studio'da aşağıdaki hedef platformlar için taşınabilir kitaplığınızın seçtiğinizden emin olun isteyeceksiniz:

[![Taşınabilir Kitaplığı platformları Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

Kitaplığı projesi oluşturduktan sonra bir başvuru ekleyin bizim `Xamarin.Workbooks.Integration` NuGet kitaplığı aracılığıyla NuGet Paket Yöneticisi.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Mac için NuGet Visual Studio](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

Projenin bir parçası olarak oluşturulan boş sınıfını silmek istersiniz —, onu bu gereksinim duyuyor olması olmaz. Bu adımları yaptıktan sonra tümleştirme oluşturmaya başlamak hazırsınız.

## <a name="building-an-integration"></a>Bir tümleştirme oluşturma

Biz basit bir tümleştirme yapı. Yeşil renk her nesneye bir temsili olarak ekleyeceğiz böylece biz gerçekten rengi yeşil memnuniyet. İlk olarak adlı yeni bir sınıf oluşturun `SampleIntegration`ve uygulama hale bizim [ `IAgentIntegration` ] [ integration-type] arabirimi:

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

Yapmak istiyoruz eklemektir bir [gösterimi](~/tools/workbooks/sdk/representations.md) yeşil renkte her nesne için. Biz bu bir gösterimi sağlayıcısı kullanarak gerçekleştirirsiniz. Sağlayıcıları devral [ `RepresentationProvider` ] [ reppr] sınıfı — bizim için yalnızca geçersiz kılmak ihtiyacımız [ `ProvideRepresentations` ] [ prrep]:

```csharp
using Xamarin.Interactive.Representations;

class SampleRepresentationProvider : RepresentationProvider
{
    public override IEnumerable<object> ProvideRepresentations (object obj)
    {
        // This corresponds to Pantone 2250 XGC, our favorite color.
        yield return new Color (0.0, 0.702, 0.4314);
    }
}
```

Biz döndüren bir [ `Color` ] [ color], bir gösterim türü bizim SDK önceden oluşturulmuş.
Dönüş türü burada olduğunu fark edeceksiniz bir `IEnumerable<object>` &mdash;bir gösterimi sağlayıcısı, bir nesne için birçok Beyanları döndürebilir! Hangi nesneleri için geçirilen hakkında varsayımlar olmamasını önemlidir tüm gösterimi sağlayıcıları her nesne için denir.

Gerçekte aracı ile bizim sağlayıcıyı kaydettirin ve bizim tümleştirme türü nerede bulacağını çalışma kitaplarına bildirir için son adımdır bakın. Sağlayıcıyı kaydetmek için bu kodu ekleyin `IntegrateWith` yönteminde `SampleIntegration` daha önce oluşturduğumuz sınıfı:

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

Ayar tümleştirme türü bir derleme genelinde özniteliği gerçekleştirilir. Bu, AssemblyInfo.cs veya tümleştirme türünüz kolaylık sağlamak için aynı sınıfta koyabilirsiniz:

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

Geliştirme sırasında bunu kullanmak daha kolay bulabilirsiniz [ `AddProvider` aşırı] [ addprovider] üzerinde `RepresentationManager` temsili bir çalışma kitabı içindeki sağlamak için basit bir geri çağırma kaydetmek izin ver ve ardından bu kodu içine taşıyın, `RepresentationProvider` tamamlanmış sonra uygulama. İşleme için örnek bir [ `OxyPlot` ] [ oxyplot] `PlotModel` şuna benzeyebilir:

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> Bu API'leri başlamak ve çalıştırmak için hızlı bir yol sağlar, ancak bunları kullanarak yalnızca tüm tümleştirme sevkiyat öneririz değil&mdash;türlerinizi istemci tarafından nasıl işleneceğini üzerinde çok az denetim sağlarlar.

Kayıtlı gösterimi ile tümleştirmenize sevk hazırsınız!

## <a name="shipping-your-integration"></a>Tümleştirmenize aktarma

Tümleştirmenize sevk etmek için NuGet paketini eklemeniz gerekir.
Varolan kitaplığınızın NuGet ile birlikte veya yeni bir paket oluşturuyorsanız, bu şablon .nuspec dosyası başlangıç noktası olarak kullanabilirsiniz.
Tümleştirmesi için ilgili bölümleri doldurmak gerekir. Tüm dosyalar, tümleştirme içinde olmalıdır en önemli parçası olan bir `xamarin.interactive` paketin kökünde dizin. Bu, bize kolayca olup bir var olan paketi kullan veya yeni bir tane oluşturun bağımsız olarak, tümleştirme için tüm dosyaları bulmak sağlar.

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
      <id>$YourNuGetPackage$</id>
      <version>$YourVersion$</version>
      <authors>$YourNameHere$</authors>
      <projectUrl>$YourProjectPage$</projectUrl>
      <description>A short description of your library.</description>
    </metadata>
    <files>
      <file src="Path\To\Your\Integration.dll" target="xamarin.interactive" />
    </files>
</package>
```

.Nuspec dosyası oluşturduktan sonra NuGet paketi sözlüğüdür:

```csharp
nuget pack MyIntegration.nuspec
```

ve ardından yayımlanacağı [NuGet][nugetorg]. Var olduğunda, herhangi bir çalışma kitabından başvuru ve eylemde görmek mümkün olur. Aşağıdaki ekran görüntüsünde, biz Biz bu belgede yerleşik ve bir çalışma kitabında NuGet paketi yüklü örnek tümleştirme paketlenmiş:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Çalışma kitabı ile tümleştirme](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Çalışma kitabı ile tümleştirme](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

Bildirim herhangi görmüyorum `#r` yönergeleri veya tümleştirme başlatmak için herhangi bir şeyi — çalışma kitaplarını yapılan verdiğiniz tümünü sizin için arka planda!

## <a name="next-steps"></a>Sonraki Adımlar

Diğer Belgelerimizi SDK yapmak taşıma parçaları hakkında daha fazla bilgi için göz atın ve bizim [örnek tümleştirmeler](~/tools/workbooks/samples/index.md) ek işlemler çalıştırmak, özel bir JavaScript sağlamak gibi tümleştirme gelen yapmak için çalışma kitapları istemci.

[integration-type]: /api/type/Xamarin.Interactive.IAgentIntegration/
[repman-api]: /api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[color]: /api/type/Xamarin.Interactive.Representations.Color/
[xir]: /api/namespace/Xamarin.Interactive.Representations/
[reppr]: /api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[prrep]: /api/member/Xamarin.Interactive.Representations.RepresentationProvider.ProvideRepresentations/p/System.Object/
[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[addprovider]: /api/member/Xamarin.Interactive.Representations.IRepresentationManager.AddProvider/
[oxyplot]: http://www.oxyplot.org/
