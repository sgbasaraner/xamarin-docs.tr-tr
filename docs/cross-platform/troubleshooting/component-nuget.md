---
title: NuGet bileşen başvurularını güncelleştirme
description: Bileşen başvuruları uygulamalarınızı gelecekteki sağlaması için NuGet paketleri ile değiştirin.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: asb3993
ms.author: amburns
ms.date: 04/18/2018
ms.openlocfilehash: 31660ff1255878dbae15bda601da8e628aabd459
ms.sourcegitcommit: c5bb1045b2f4607dafe3101ad1ea6ade23e44342
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="updating-component-references-to-nuget"></a>NuGet bileşen başvurularını güncelleştirme

> [!IMPORTANT]
> Bileşen deposu 15 Mayıs 2018 itibariyle yarıda kesildi (Bu Kapatılmak üzere ilk yüklendiği [duyurdu](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) Kasım 2017 içinde).
>
> Xamarin bileşenleri Visual Studio artık desteklenmemektedir ve NuGet paketleri tarafından değiştirilmelidir. Bileşen başvuruları, projelerden el ile kaldırmak için aşağıdaki yönergeleri izleyin.

NuGet paketlerini eklemek için bu yönergeleri başvurmak [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) veya [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Popüler Xamarin listesini [eklentileri ve kitaplıklarını](https://github.com/xamarin/XamarinComponents/blob/master/README.md) NuGet pacakges kullanılabilir olan bileşenleri alternatifleri bulmak kullanılabilir.

## <a name="manually-removing-component-references"></a>Bileşen başvuruları el ile kaldırma

Artık 15.6 Visual Studio sürümü ve Visual Studio 7.4 sürümünü Mac için bileşen projenizde destekler. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio uygulamasına bir proje yüklerseniz, tüm bileşenleri projenizden el ile kaldırmanız gerekir, açıklayan aşağıdaki iletişim kutusu görüntülenir:

![Bir bileşeni projenize bulundu ve kaldırılmalıdır iletişim açıklayan uyar](component-nuget-images/component-alert-vs.png)

Projenizden bir bileşeni kaldırmak için:

1. Açık **.csproj** dosya. Bunu yapmak için proje adına sağ tıklayın ve seçin **projeyi**. 

2. Yeniden yüklenmeyen projeye sağ tıklayıp **{-proje-adınız} .csproj Düzenle**.

3. Dosyadaki tüm başvuruları Bul `XamarinComponentReference`. Aşağıdaki örneğe benzer görünmelidir:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

4. Başvurularının `XamarinComponentReference` ve dosyayı kaydedin. Yukarıdaki örnekte, tüm kaldırmak güvenli `ItemGroup`.

5. Dosyayı kaydettikten sonra proje adına sağ tıklayın ve seçin **projeyi yeniden yükle**.

6. Çözümünüzdeki her proje için yukarıdaki adımları yineleyin.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio uygulamasına bir proje yüklerseniz, tüm bileşenleri projenizden el ile kaldırmanız gerekir, açıklayan aşağıdaki iletişim kutusu görüntülenir:

![Bir bileşeni projenize bulundu ve kaldırılmalıdır iletişim açıklayan uyar](component-nuget-images/component-alert.png)

Projenizden bir bileşeni kaldırmak için:

1. .Csproj dosyasını açın. Bunu yapmak için proje adına sağ tıklayın ve seçin **Araçlar > Düzenle dosya**.

2. Dosyadaki tüm başvuruları Bul `XamarinComponentReference`. Aşağıdaki örneğe benzer görünmelidir:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

3. Başvurularının `XamarinComponentReference` ve dosyayı kaydedin. Yukarıdaki örnekte, tüm kaldırmak güvenli `ItemGroup`

4. Çözümünüzdeki her proje için yukarıdaki adımları yineleyin.

-----

> [!WARNING]
> Aşağıdaki yönergeler, yalnızca Visual Studio'nun eski sürümlerinde çalışır.
> **Bileşenleri** düğümdür artık Visual Studio 2017 veya Mac için Visual Studio geçerli sürümlerinde kullanılabilir

Aşağıdaki bölümlerde, NuGet paketlerini bileşen başvuruları değiştirmek için varolan Xamarin çözümleri güncelleştirmek açıklanmaktadır.

- [NuGet paketlerini içeren bileşenleri](#contain)
- [NuGet değişikliklerini bileşenleriyle](#replace)

Çoğu bileşenleri yukarıdaki kategoriden ayrılır.
Görünmeyen bir eşdeğer NuGet paketine sahip, okumak için bir bileşen kullanıyorsanız [NuGet geçiş yolu olmayan bileşenler](#require-update) bölümüne bakın.

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>NuGet paketlerini içeren bileşenleri

Birçok bileşen zaten NuGet paketlerini içerir ve geçiş yolu yalnızca component başvurusu silmektir.

Bileşen zaten bir NuGet paketi çözüm bileşeni çift tıklayarak içerip içermeyeceğini belirleyebilirsiniz:

![Genişletilmiş bileşenleri düğümü](component-nuget-images/solution-sml.png)

**Paketleri** sekmesini bileşeni dahil herhangi bir NuGet paketinin listeler:

![NuGet Paketleri sekmesi içerir](component-nuget-images/packages-tab-sml.png)

Unutmayın **derlemeleri** sekmesi boş olacaktır:

![Derlemeleri sekmesini boştur](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Çözüm güncelleştiriliyor

Çözümünüzü güncelleştirmek için silme **bileşen** çözümden girişi:

![Bileşen Sil](component-nuget-images/delete-component-sml.png)

NuGet paketi olarak listelenen kalacak **paketleri** düğümü ve uygulamanızı derlenir ve her zamanki gibi çalıştırın. Gelecekte, bu paket için güncelleştirmeleri aracılığıyla gerçekleştirilir **Nuget** güncelleştirme özelliği:

![NuGet paketi güncelleştirme](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>NuGet değişikliklerini bileşenleriyle

Varsa bileşen bilgileri sayfası **derlemeleri** sekmesi, aşağıda gösterildiği gibi girişleri içerir, eşdeğer NuGet paketi el ile bulmak gerekir.

![Derlemeleri içerir](component-nuget-images/assemblies-tab-sml.png)

Unutmayın **paketleri** sekmesini büyük olasılıkla boş olacaktır:

![](component-nuget-images/packages-tab-empty-sml.png)

_NuGet bağımlılıkları içerebilir, ancak bunlar göz ardı._

NuGet paket var. değiştirme onaylamak için arama [NuGet.org](https://www.nuget.org/packages), bileşen adı kullanarak veya alternatif olarak yazar tarafından.

Örnek olarak, popüler bulabilirsiniz **sqlite net pcl** için arayarak paketi:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) – Ürün adı.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) – Yazar profili.

### <a name="updating-the-solution"></a>Çözüm güncelleştiriliyor

Bileşen NuGet içinde kullanılabilir doğruladıktan sonra aşağıdaki adımları izleyin:

#### <a name="delete-the-component"></a>Bileşen Sil

Çözüm bileşeni sağ tıklayın ve seçin **kaldırmak**:

![Bileşen kaldırma](component-nuget-images/remove-component-sml.png)

Bu bileşen ve tüm başvuruları siler. Değiştirmek için eşdeğer NuGet paketi ekleyene kadar bu yapılandırma, çalışmamasına neden olur.

#### <a name="add-the-nuget-package"></a>NuGet paketi ekleme

1. Sağ **paketleri** düğümü seçin **paketleri Ekle...** .
2. Adı veya yazar tarafından NuGet değiştirme arayın:

  ![](component-nuget-images/nuget-search-sml.png)

3. Tuşuna **paket ekleme**.

NuGet paketini projenize tüm bağımlılıklarla birlikte eklenir.
Bu yapı sorununu çözer. Yapı başarısız olmaya devam ederse, API farklarını bileşeni ve NuGet paketi olup olmadığını görmek için her bir hata araştırın.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>Bir NuGet geçiş yolu olmayan bileşenler

Uygulamanızda kullanılan bileşenler için yenileme hemen bulamazsanız endişelenmeyin. Var olan bileşenlerin Visual Studio 15,5 içinde çalışmaya devam edecek ve **bileşenleri** düğümü görünür çözümünüzde her zamanki gibi.

Ancak, sonraki Visual Studio sürüm olacak _değil_ geri yükleyin veya güncelleştirin bileşenleri.
Bu çözüm yeni bir bilgisayarda açarsanız, bileşen karşıdan yüklenen ve bırakılır anlamına gelir; ve yazar güncelleştirmeleriyle sağlamak mümkün olmaz. İçin planlamanız gerekir:

* Derlemeleri bileşeninden ayıklayın ve bunları doğrudan projenizde başvuru.
* Bileşen yazarına başvurun ve Nuget'e geçirmek için planları hakkında isteyin.
* Alternatif NuGet paketleri araştırın veya bileşen açık kaynaklı ise kaynak kodunu arama.

Birçok bileşen satıcıları için NuGet geçirme hala çalışmaktadır ve diğerleri (dahil olmak üzere ticari koşulların elverdiği oranda kullanılabilir ürünler) alternatif teslim seçeneklerini İnceleme.


## <a name="related-links"></a>İlgili bağlantılar
- [Popüler Xamarin eklenti ve kitaplıkları listesi](https://github.com/xamarin/XamarinComponents/blob/master/README.md)
- [Yükleme ve bir NuGet paketi (Windows) kullanma](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [Bir NuGet paketi (Mac) dahil olmak üzere](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
