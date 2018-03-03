---
title: Xamarin.Forms Xamarin.UITest ve uygulama Center ile testi otomatik hale getirme
description: "Xamarin.Forms ile Xamarin UITest bileşen aygıtları yüzlerce üzerinde bulutta çalıştırmak için UI testleri yazmak için kullanılır."
ms.topic: article
ms.prod: xamarin
ms.assetid: b674db3d-c526-4e31-a9f4-b6d6528ce7a9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/31/2016
ms.openlocfilehash: 78788524c1afdda127762049018ca769926f729e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="automate-xamarinforms-testing-with-xamarinuitest-and-app-center"></a>Xamarin.Forms Xamarin.UITest ve uygulama Center ile testi otomatik hale getirme

_Xamarin.Forms ile Xamarin UITest bileşen aygıtları yüzlerce üzerinde bulutta çalıştırmak için UI testleri yazmak için kullanılır._

## <a name="overview"></a>Genel Bakış

**[Uygulama Merkezi Test](/appcenter/test-cloud/)**  iOS ve Android uygulamaları için otomatik kullanıcı arabirimi testleri yazmak geliştiricilerin sağlar. Bazı küçük tweaks ile Xamarin.Forms uygulamalar aynı test kod paylaşımını dahil olmak üzere Xamarin.UITest kullanılarak sınanabilir. Bu makalede Xamarin.Forms ile çalışma Xamarin.UITest almak için belirli ipuçları tanıtılır.

Bu kılavuz o aşina Xamarin.UITest varsayalım. Aşağıdaki kılavuzlar, Xamarin.UITest öğrenilmesi için önerilir:

- [Uygulama Merkezi Test giriş](/appcenter/test-cloud/)
- [UITest giriş](/appcenter/test-cloud/preparing-for-upload/uitest/)

UITest proje adımları yazma için bir Xamarin.Forms çözümüne eklendikten sonra ve ve bir Xamarin.Forms uygulaması testleri çalıştırılıyor bir Xamarin.Android veya Xamarin.iOS uygulaması ile aynıdır.

## <a name="requirements"></a>Gereksinimler

Başvurmak [Xamarin.UITest](/appcenter/test-cloud/uitest/) projenizi otomatikleştirilmiş UI testleri için hazır olduğunu doğrulamak için.

## <a name="adding-uitest-support-to-xamarinforms-apps"></a>Xamarin.Forms uygulamalarına UITest desteği ekleme

UITest kullanıcı arabirimi ekranında denetimleri etkinleştirme ve bir kullanıcı normal olarak uygulama ile etkileşime herhangi bir yere giriş gerçekleştirilerek otomatikleştirir. İçin testleri etkinleştirmek için *bir düğmesine basın* veya *metin kutusundaki kutuya girin* test kodu ekranında denetimleri tanıtmak için bir yol gerekir.

Denetimlere başvuru yapmak UITest kodu etkinleştirmek için benzersiz bir tanımlayıcı her denetim gerekir. Xamarin.Forms içinde bu tanımlayıcı ayarlamak için önerilen yöntem kullanmaktır `AutomationId` aşağıda gösterildiği gibi özelliği:

```csharp
var b = new Button {
    Text = "Click me",
    AutomationId = "MyButton"
};
var l = new Label {
    Text = "Hello, Xamarin.Forms!",
    AutomationId = "MyLabel"
};
```

`AutomationId` Özelliği XAML'de de ayarlanabilir:

```xaml
<Button x:Name="b" AutomationId="MyButton" Text="Click me"/>
<Label x:Name="l" AutomationId="MyLabel" Text="Hello, Xamarin.Forms!" />
```

Benzersiz bir `AutomationId` (düğmeleri, metin girişleri ve değeri sorgulanmasını gerekebilir etiketleri de dahil olmak üzere) test etmek için gerekli olan tüm denetimler eklenmelidir.

> [!NOTE]
> Unutmayın bir `InvalidOperationException` ayarlamak için bir girişimde varsa durum `AutomationId` özelliği bir `Element` birden çok kez.

### <a name="ios-application-project"></a>iOS uygulama projesi

İos'ta, testleri çalıştırmak için [Xamarin Test Cloud Aracısı NuGet paketi](https://www.nuget.org/packages/Xamarin.TestCloud.Agent/) projeye eklenmelidir. Eklendikten sonra aşağıdaki kodu kopyalayın `AppDelegate.FinishedLaunching` yöntemi:

```csharp
#if ENABLE_TEST_CLOUD
// requires Xamarin Test Cloud Agent
Xamarin.Calabash.Start();
#endif
```

Calabash derleme genel olmayan Apple App Store tarafından reddedilir apps neden olacak API'nin kullanımlarını yapar. Ancak, açıkça koddan başvurulan değil Xamarin.iOS bağlayıcı son IPA Calabash derleme kaldırır.

> [!NOTE]
>  Yayın derlemeleri yok `ENABLE_TEST_CLOUD` uygulama paketinden kaldırılacak Calabash derleme neden olacak derleyici değişkeni. Ancak, hata ayıklama yapıları bağlayıcı derleme kaldırmanızı önleme, derleyici yönergeyle sahip.

Aşağıdaki ekran görüntüsü gösterildiği `ENABLE_TEST_CLOUD` derleyici değişkenini ayarlamak için hata ayıklama yapıları:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](uitest-and-test-cloud-images/12-compiler-directive-vs.png "Derleme seçenekleri")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](uitest-and-test-cloud-images/11-compiler-directive-xs.png "Derleyici Seçenekleri")

-----

### <a name="android-application-project"></a>Android uygulama projesi

İOS, Android projeleri herhangi bir özel başlatma kod gerekmez.

## <a name="writing-uitests"></a>Yazma UITests

UITests yazma hakkında daha fazla bilgi için bkz: [UITest belgelerine](/appcenter/test-cloud/preparing-for-upload/uitest/). Özellikle açıklayan bir özeti, aşağıdaki adımları olan nasıl [Xamarin.Forms demo **UsingUITest** ](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/) yerleşik olarak bulunur.

### <a name="use-automationid-in-the-xamarinforms-ui"></a>Automationıd Xamarin.Forms Arabiriminde kullanın

Tüm UITests yazılabilir önce Xamarin.Forms uygulama kullanıcı arabirimi kodlanabilir olması gerekir. Kullanıcı arabiriminde tüm denetimler sahip olduğundan emin olun bir `AutomationId` böylece test kodda başvurulabilir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

### <a name="adding-a-uitest-project-to-a-new-solution"></a>Yeni bir çözüm UITest projesi ekleme

Mac için Visual Studio kullanarak yeni bir Xamarin.Forms projesi oluştururken, yeni bir UITest proje çözüme seçerek eklenebilir **Xamarin Test Cloud: otomatik bir UI testi projesi eklemek**:

![](uitest-and-test-cloud-images/01-new-solution-xs.png "Yeni projeniz yapılandırın")

Yeni bir çözüm Xamarin.UITests karşı Xamarin.Forms uygulamayı çalıştırmak için otomatik olarak yapılandırılır.

-----

#### <a name="referring-to-the-automationid-in-uitests"></a>Automationıd UITests içinde başvurma

UITests, yazarken `AutomationId` değeri farklı her platformda gösterilir:

- **iOS** kullanan `id` alan.
- **Android** kullanan `label` alan.

Bulacaksınız platformlar arası UITests yazmak için `AutomationId` hem iOS hem de Android `Marked` test sorgusu:

```csharp
app.Query(c=>c.Marked("MyButton"))
```

Kısa biçimi `app.Query("MyButton")` de çalışır.

### <a name="adding-a-uitest-project-to-an-existing-solution"></a>Varolan bir çözümü UITest projesi ekleme

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio varolan Xamarin.Forms çözümünü Xamarin.UITest projesi eklemek yardımcı olmak için bir şablon vardır:

1. Çözüm üzerinde sağ tıklatın ve seçin **Dosya > Yeni proje**.
1. Gelen **Visual C#** şablonları seçin **Test** kategorisi. Seçin **UI testi uygulama > platformlar arası** şablonu:

    ![](uitest-and-test-cloud-images/08-new-uitest-project-vs.png "Yeni Proje Ekle")

    Bu yeni bir proje ile ekler **NUnit**, **Xamarin.UITest**, ve **NUnitTestAdapter** çözüm için NuGet paketleri:

    ![](uitest-and-test-cloud-images/09-new-uitest-project-xs.png "NuGet Package Manager")

    **NUnitTestAdapter** Visual Studio'dan NUnit testleri çalıştırmak Visual Studio sağlayan bir üçüncü taraf test Çalıştırıcısı değil.

    Yeni Proje iki sınıf içinde da sahiptir. **AppInitializer** yardımcı olmak için kodu içeren başlatmak ve Kurulum testleri. Başka bir sınıf **testleri**, UITests başlamanıza yardımcı olması için Demirbaş kod içerir.

1. Proje başvurusu UITest projeden Xamarin.Android projeye ekleyin:

    ![](uitest-and-test-cloud-images/10-test-apps-vs.png "Proje başvuru Yöneticisi")

    Bu, olanak sağlayacaktır **NUnitTestAdapter** UITests Android için Visual Studio'dan çalıştırmak için.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Yeni bir Xamarin.UITest proje varolan bir çözümü el ile eklemek mümkündür:

1. Yeni bir proje çözümü seçerek ve tıklatarak eklemeye başlayın **Dosya > Yeni Proje Ekle**. İçinde **yeni proje** iletişim kutusunda **platformlar arası > testleri > Xamarin Test Cloud > UI testi uygulama**:

    ![](uitest-and-test-cloud-images/02-new-uitest-project-xs.png "Bir şablon seçin")

    Bu, zaten sahip yeni bir proje ekler **NUnit** ve **Xamarin.UITest** çözümde NuGet paketleri:

    ![](uitest-and-test-cloud-images/03-new-uitest-project-xs.png "Xamarin UITest NuGet paketleri")

    Yeni Proje iki sınıf içinde da sahiptir. **AppInitializer** yardımcı olmak için kodu içeren başlatmak ve Kurulum testleri. Başka bir sınıf **testleri**, UITests başlamanıza yardımcı olması için Demirbaş kod içerir.

1. Seçin **Görünüm > klavye takımı > birim testleri** birim testi paneli görüntülemek için. Genişletme **UsingUITest > UsingUITest.UITests > Test uygulamaları**:

    ![](uitest-and-test-cloud-images/04-unit-test-pad-xs.png "Birim testi paneli")

1. Sağ tıklayın **Test uygulamaları**, tıklayın **uygulaması projesi eklemek**, görüntülenen iletişim kutusunda iOS ve Android projeleri seçin:

    ![](uitest-and-test-cloud-images/05-add-test-apps-xs.png "Test uygulamaları iletişim")

    **Birim testi** paneli iOS ve Android projeler başvuru şimdi sahip olmalıdır. Bu, Visual Studio UITests iki Xamarin.Forms projelere göre yerel olarak çalıştırmak test Çalıştırıcısı Mac için izin verir.

#### <a name="adding-uitest-to-the-ios-app"></a>İOS uygulaması UITest ekleme

İOS uygulama Xamarin.UITest çalışmadan önce gerçekleştirilmesi gereken bazı ek değişiklikler vardır:

1. Ekleme **Xamarin Test Cloud Aracısı** NuGet paketi. Sağ tıklayın **paketleri**seçin **paketleri Ekle**, NuGet için arama **Xamarin Test Cloud Aracısı** ve Xamarin.iOS projeye ekleyin:

    ![](uitest-and-test-cloud-images/07-add-test-cloud-agent-xs.png "NuGet paketleri ekleme")

1. Düzen `FinishedLaunching` yöntemi **AppDelegate** sınıfı iOS uygulaması başlatıldığında Xamarin Test Cloud Aracısı başlatılamadı ve ayarlamak için `AutomationId` görünümleri özelliği. `FinishedLaunching` Yöntemi, aşağıdaki kod örneğinde benzer:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
        #if ENABLE_TEST_CLOUD
        Xamarin.Calabash.Start();
        #endif

        global::Xamarin.Forms.Forms.Init();

        LoadApplication(new App());

        return base.FinishedLaunching(app, options);
}
```

-----

Xamarin.Forms çözümünü Xamarin.UITest ekledikten sonra UITests oluşturmak, yerel olarak çalıştırın ve Xamarin Test Cloud göndermek mümkündür.

## <a name="summary"></a>Özet

Xamarin.Forms uygulamalar kolayca test ile **Xamarin.UITest** kullanıma sunmak için basit bir mekanizma kullanarak `AutomationId` test otomasyonu için bir benzersiz görünüm tanımlayıcı olarak. UITest proje adımları yazma için bir Xamarin.Forms çözümüne eklendikten sonra ve ve bir Xamarin.Forms uygulaması testleri çalıştırılıyor bir Xamarin.Android veya Xamarin.iOS uygulaması ile aynıdır.

Uygulama Merkezi Test testleri gönderme hakkında daha fazla bilgi için bkz: [gönderme UITests](/appcenter/test-cloud/preparing-for-upload/uitest/). UITest hakkında daha fazla bilgi için bkz: [uygulama Center Test belgelerine](/appcenter/test-cloud/).


## <a name="related-links"></a>İlgili bağlantılar

- [UITestSample](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/)
- [Xamarin.Forms örnekleri](https://github.com/xamarin/xamarin-forms-samples)
- [Uygulama Merkezi Test](/appcenter/test-cloud/)
- [Xamarin.UITest](/appcenter/test-cloud/uitest/)
- [NUnit](http://www.nunit.org)
