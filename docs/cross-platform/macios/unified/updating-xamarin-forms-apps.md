---
title: "Var olan Xamarin.Forms uygulamaları güncelleştirme"
description: "Birleşik API kullanın ve 1.3.1 sürüme güncelleştirmek için varolan bir Xamarin.Forms uygulaması güncelleştirmek için aşağıdaki adımları izleyin"
ms.topic: article
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 52b53618e23a47884bee6cb821d85b15d759968c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="updating-existing-xamarinforms-apps"></a>Var olan Xamarin.Forms uygulamaları güncelleştirme

_Birleşik API kullanın ve 1.3.1 sürüme güncelleştirmek için varolan bir Xamarin.Forms uygulaması güncelleştirmek için aşağıdaki adımları izleyin_


> [!IMPORTANT]
> Xamarin.Forms 1.3.1 Unified API destekler ilk sürüm olduğundan, çözümün tamamı için birleşik iOS uygulaması geçirme olarak aynı anda en son sürümünü kullanacak şekilde güncelleştirilmesi gerekir. Unified desteği için iOS projesi güncelleştirmeye ek olarak, ayrıca kodda düzenlemek gerekir, yani _tüm_ Çözümdeki projeler.



Güncelleştirme iki adımda gerçekleştirilir:

1. İOS uygulaması birleşik geçiş aracı Mac'ın derleme için Visual Studio kullanarak API'sine geçirin.

    - Projeyi otomatik olarak güncelleştirmek için geçiş aracını kullanın.

    - Güncelleştirme iOS yerel yönergeleri özetlendiği gibi API'leri [iOS uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-ios-apps.md) (özellikle kodda özel oluşturucu veya bağımlılık hizmeti).

2. Çözümün tamamında Xamarin.Forms sürüm 1.3 güncelleştirin.

    1. Xamarin.Forms 1.3.1 NuGet paketini yükleyin.

    2. Güncelleştirme `App` paylaşılan kod sınıfı.

    3. Güncelleştirme `AppDelegate` iOS projesinde.

    4. Güncelleştirme `MainActivity` Android projesinde.

    5. Güncelleştirme `MainPage` Windows Phone projesinde.


# <a name="1-ios-app-unified-migration"></a>1. iOS uygulaması (Birleşik geçiş)

Geçiş işleminin parçası Xamarin.Forms Unified API destekleyen sürümüne 1.3, yükseltmeyi gerektirir. Oluşturulacak doğru derleme başvuruları için sırayla biz öncelikle Unified API kullanmak için iOS projesi güncelleştirmeniz gerekir.

## <a name="migration-tool"></a>Geçiş Aracı

Seçili böylece iOS projesi tıklatın ardından seçin **Proje > Xamarin.iOS Unified API geçiş...**  ve görüntülenen uyarı iletisini kabul ediyorum.

![](updating-xamarin-forms-apps-images/beta-tool1.png "Proje seçin > Xamarin.iOS birleşik API'sine geçirin... ve görüntülenen uyarı iletisini kabul ediyorum")

Bu otomatik olarak ayarlanır:

 - Birleşik 64-bit API desteklemek için proje türünü değiştirin.
 - Çerçeve referansını değiştirme **Xamarin.iOS** (eski değiştirme **monotouch** başvuru).
 - Ad alanı başvurularını kaldırmak için kodda değişiklik `MonoTouch` öneki.
 - Güncelleştirme **csproj** doğru yapı hedefleri için birleşik API kullanmak üzere bir dosya.

**Temiz** ve **yapı** projenin düzeltmek için diğer hatalar olmadığından emin olun. Başka bir eylem gerekli olması gerekir. Bu adımları daha ayrıntılı olarak açıklanmıştır [Unified API belgeleri](~/cross-platform/macios/unified/updating-ios-apps.md).

## <a name="update-native-ios-apis-if-required"></a>Yerel iOS API (gerekliyse) güncelleştir

Ek iOS yerel kod (örneğin, özel oluşturucu veya bağımlılık Hizmetleri) eklediyseniz ek el ile kod düzeltmeleri gerçekleştirmeniz gerekebilir. Uygulamanızı yeniden derleyin ve başvurulacak [güncelleştirme var olan iOS uygulamaları yönergeleri](~/cross-platform/macios/unified/updating-ios-apps.md) gerekli olabilecek değişiklikleri hakkında daha fazla bilgi için. [Bu ipuçları](~/cross-platform/macios/unified/updating-tips.md) gerekli değişiklikleri belirlemenize yardımcı olur.


# <a name="2-xamarinforms-131-update"></a>2. Xamarin.Forms 1.3.1 güncelleştirme

İOS uygulaması için birleşik API güncelleştirildi sonra çözüm kalan Xamarin.Forms sürüm 1.3.1 güncelleştirilmesi gerekiyor. Şunları içerir:

 - Her proje Xamarin.Forms NuGet paketi güncelleştirme.
 - Yeni Xamarin.Forms kullanmak için kodu değiştirme `Application`, `FormsApplicationDelegate` (iOS) `FormsApplicationActivity` (Android), ve `FormsApplicationPage` (Windows Phone) sınıfları.

Bu adımları aşağıda açıklanmıştır:


## <a name="21-update-nuget-in-all-projects"></a>2.1 tüm projelerdeki güncelleştirme NuGet

Xamarin.Forms 1.3.1 için güncelleştirme sürüm öncesi Çözümdeki tüm projeleri için NuGet Paket Yöneticisi'ni kullanarak: PCL (varsa), iOS, Android ve Windows Phone. Önerilir Bu, **silin ve yeniden ekleyin** 1.3 sürüme güncelleştirmek için Xamarin.Forms NuGet paketi.

**Not:** Xamarin.Forms sürüm 1.3.1 halen *yayın öncesi*. Bu seçmelisiniz anlamına gelir **yayın öncesi** seçeneği NuGet içinde (bir onay kutusu Mac için Visual Studio) veya bir açılan aşağı-liste Visual Studio aracılığıyla en son sürüm öncesi sürümünü görmek için.


> [!IMPORTANT]
> Visual Studio kullanıyorsanız, uygulamanın son sürümü NuGet Paket Yöneticisi'nin yüklü olduğundan emin olun. Visual Studio'da NuGet eski sürümleri Xamarin.Forms 1.3.1 Unified sürümü doğru yüklenmez. Git **Araçlar > Uzantılar ve güncelleştirmeler...**  ve tıklayın **yüklü** denetlemek için liste **Visual Studio için NuGet Paket Yöneticisi** en az sürüm 2.8.5. Eski ise tıklayın **güncelleştirmeleri** en son sürümü karşıdan yüklemek için liste.



Xamarin.Forms 1.3.1 NuGet paketi güncelleştirdikten sonra her proje yeni yükseltmek için aşağıdaki değişiklikleri yapın `Xamarin.Forms.Application` sınıfı.

## <a name="22-portable-class-library-or-shared-project"></a>2.2 taşınabilir sınıf kitaplığı (veya paylaşılan bir proje)

Değişiklik **App.cs** dosya böylece:

 - `App` Şimdi sınıfının devraldığı `Application`.
 - `MainPage` Özelliği, görüntülemek istediğiniz ilk içerik sayfasına ayarlanır.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

Biz tamamen kaldırmış `GetMainPage` yöntemi ve bunun yerine `MainPage` *özelliği* üzerinde `Application` bir alt kümesi.

Bu yeni `Application` temel sınıfı da destekler `OnStart`, `OnSleep`, ve `OnResume` uygulamanızın yaşam döngüsünü yönetmenize yardımcı olması için geçersiz kılar.

`App` Sınıfının yeni bir geçirilir `LoadApplication` aşağıda açıklandığı gibi her uygulama projesi yöntemi:


## <a name="23-ios-app"></a>2.3 iOS uygulaması


Değişiklik **AppDelegate.cs** dosya böylece:

 - Sınıfının devraldığı `FormsApplicationDelegate` (yerine `UIApplicationDelegate` önceden).
 - `LoadApplication` Yeni bir örneğini ile adlı `App`.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```


## <a name="23-android-app"></a>2.3 android uygulaması

Değişiklik **MainActivity.cs** dosya böylece:

 - Sınıfının devraldığı `FormsApplicationActivity` (yerine `FormsActivity` önceden).
 - `LoadApplication` Yeni bir örneğini ile adlandırılır `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```


## <a name="24-windows-phone-app"></a>2.4 Windows Phone Uygulama

Güncelleştirmek ihtiyacımız **MainPage** -XAML ve arkasındaki koda.

Değişiklik **MainPage.xaml** dosya böylece:

 - Kök XAML öğe olmalıdır `winPhone:FormsApplicationPage`.
 - `xmlns:phone` Özniteliği olmalıdır *değiştirilen* için `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

Güncelleştirilmiş bir örnek aşağıda gösterilen - (öznitelikleri kalan aynı kalmalıdır) bunlardan düzenlemek yalnızca olması gerekir:

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```


Değişiklik **MainPage.xaml.cs** dosya böylece:

 - Sınıfının devraldığı `FormsApplicationPage` (yerine `PhoneApplicationPage` önceden).
 - `LoadApplication` Xamarin.Forms yeni bir örneğini adlı `App` sınıfı. Windows Phone olduğundan bu başvuru tam olarak nitelemek gerekebilir kendi `App` sınıf zaten tanımlı.

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

## <a name="troubleshooting"></a>Sorun giderme

Bazen Xamarin.Forms NuGet paketi güncelleştirdikten sonra aşağıdakine benzer bir hata görürsünüz. NuGet güncelleştirici eski sürümlerinden başvurular tamamen kaldırmaz oluşur, **csproj** dosyaları.

>\_PROJECT.csproj: hata: Bu proje bu bilgisayarda eksik olan NuGet paketlerine başvuruyor. Bunları indirmek NuGet paketi geri yüklemeyi etkinleştirin.  Daha fazla bilgi için http://go.microsoft.com/fwlink/?LinkID=322105 bakın. Dosyası eksik olduğundan... /.. /Packages/Xamarin.Forms.1.2.3.6257/Build/Portable-Win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets. (SİZİN\_PROJE)

Bu hataları gidermek için açın **csproj** dosyasını bir metin düzenleyicisinde açıp Ara `<Target` Xamarin.Forms, eski sürümleri öğesi aşağıda gösterildiği gibi başvurmak öğeleri. Tüm bu öğesinden el ile silmelisiniz **csproj** dosya ve değişiklikleri kaydedin.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

Bu eski başvuruları kaldırıldıktan sonra projeyi başarıyla oluşturması gerekir.

# <a name="considerations"></a>Dikkat Edilecekler

Aşağıdaki noktaları dikkate bu uygulama bir veya daha fazla bileşen veya NuGet paketi dayalıysa varolan Xamarin.Forms projesini yeni birleşik API için Klasik API'SİNDEN dönüştürülürken dikkat edilmelidir.

## <a name="components"></a>Bileşenler

Uygulamanıza dahil herhangi bir bileşeni de birleşik API için güncelleştirilmesi gerekir veya derlemeye çalıştığınızda bir çakışma alırsınız. Herhangi bir dahil bileşeni için geçerli sürümü deposundan Xamarin bileşeni Unified API destekleyen yeni bir sürümle değiştirin ve temiz bir yapı yapın. Yazar tarafından dönüştürülmemiş herhangi bir bileşeni bileşen deposunda yalnızca uyarı 32 bit görüntüler.


## <a name="nuget-support"></a>NuGet desteği

Biz Unified API desteği ile çalışmak için NuGet değişiklikleri katkıda bulunan, ancak değil yapılmış olan NuGet, yeni bir sürüm Biz yeni API'leri tanımak için NuGet alma değerlendirmek için.

Bileşenleri gibi o zamana kadar birleşik API'lerini destekleyen bir sürüm projenize dahil herhangi bir NuGet paket geçin ve daha sonra temiz bir yapı yapmak gerekir.

> [!IMPORTANT]
> **Not:** biçiminde bir hata varsa _"hatası 3 'monotouch.dll' ve 'Xamarin.iOS.dll' aynı Xamarin.iOS projede içeremez - 'Xamarin.iOS.dll' 'monotouch.dll' tarafından başvurulan sırada açıkça başvurulmaktadır ' xxx Sürüm 0.0.000, Culture = neutral, PublicKeyToken = null ='"_ Unified API uygulamanıza dönüştürdükten sonra genellikle bir bileşen veya NuGet paketi birleşik API'sine güncelleştirilmemiş projesinde sahip nedeniyle istenir. Varolan bileşeni/NuGet kaldırmak, birleşik API'lerini destekleyen bir sürüme güncelleştirmek ve temiz bir yapı yapmanız gerekir.




# <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS uygulamaları 64 Bit etkinleştirme derlemeler

Birleşik API'sine dönüştürülen bir Xamarin.iOS mobil uygulama için geliştirici 64-bit makine için uygulama uygulamanın seçeneklerinden oluşturulmasını etkinleştirmek için hala gerekir. Lütfen bakın **etkinleştirme 64 Bit oluşturur, Xamarin.iOS uygulamaları** , [32/64 bit Platform konuları](~/cross-platform/macios/32-and-64.md#enable-64) belge 64 bit etkinleştirme hakkında ayrıntılı yönergeler için oluşturur.

# <a name="summary"></a>Özet

İOS uygulaması (destekleyen 64-bit mimariyi iOS platformunda) birleşik API geçirilen ve Xamarin.Forms uygulaması artık 1.3.1 sürüme güncelleştirilmesi.

Xamarin.Forms uygulaması özel Oluşturucu gibi yerel kod içeriyorsa veya bağımlılık Hizmetleri bu yeni türleri kullanacak şekilde güncelleştirilmesi gerekebilir sonra yukarıda belirtildiği gibi [Unified API içinde sunulan](~/cross-platform/macios/index.md).


## <a name="related-links"></a>İlgili bağlantılar

- [İOS uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-apps.md)
- [İOS uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Platformlar arası uygulamalar içindeki yerel türler ile çalışma](~/cross-platform/macios/native-types-cross-platform.md)
- [İpuçları güncelleştiriliyor](~/cross-platform/macios/unified/updating-tips.md)
- [Klasik vs Unified API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
