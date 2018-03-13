---
title: Sorun giderme
ms.topic: article
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/0201
ms.openlocfilehash: c5f6e6ef61e3705920770d317e4d5f680d4c8fbe
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="troubleshooting"></a>Sorun giderme

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin.iOS cannot resolve System.ValueTuple

Visual Studio ile bir uyumsuzluk nedeniyle bu hata oluşur.

- **Visual Studio 2017 güncelleştirme 1** (sürüm 15.1 veya daha eski) ile uyumlu yalnızca **System.ValueTuple NuGet 4.3.0** (veya daha eski).

- **Visual Studio 2017 güncelleştirme 2** (15.2 ya da daha yeni sürümü) uyumlu yalnızca **System.ValueTuple NuGet 4.3.1** ya da daha yeni.

Lütfen Visual Studio 2017 yüklemenizle birlikte karşılık gelen doğru System.ValueTuple NuGet seçin.


## <a name="receiving-error-retrieving-update-information-error-message"></a>'Güncelleştirme bilgilerini alma hatası' hata iletisini almaya

Lütfen yazılımı ve bu hata iletisini güncelleştirilmeye çalışılırken göründüğünde, IDE ve günlüğe kaydetme ve sonra tekrar içinde hesabınızı IDE içinde yeniden başlatmayı deneyin.

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>Çıkışlar veya Eylemler arabirimi Oluşturucu ile nasıl oluşturabilirim?

İle Mac ve Visual studio için giriş iOS Visual Studio için Xamarin Designer Xamarin.iOS geliştiriciler artık film şeritleri ve .xibs aracılığıyla bir kullanıcı Arabirimi oluşturma yararlanabilir. Başvurmak [Hello, iOS](~/ios/get-started/hello-ios/index.md) kılavuzları Tasarımcısı'nı kullanma hakkında daha fazla bilgi için.

Apple için de başvurabilir [çıkışı](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) ve [Eylemler](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html) kılavuzları çıkışlar ve eylemleri IB içinde kullanma hakkında daha fazla bilgi için.

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>NotSupportedException System.Text.Encoding.GetEncoding oluşturur

Varsayılan olarak eklenmez bir kodlama kullanılarak. Denetleme [uluslararası](~/ios/app-fundamentals/localization/index.md) sayfasında daha fazla kodlaması için destek eklemeyi öğrenin.

## <a name="systemmissingmethodexception-anything-else"></a>System.MissingMethodException (başka bir şey)

Üye bağlayıcı tarafından olasılıkla kaldırıldı ve bu nedenle çalışma zamanında derlemesindeki yok.  Bu çeşitli çözümler vardır:

-  Ekleme [[Koru]](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute) özniteliği için üye.  Bu bağlayıcı, kaldırmasını engeller.
-  Çağrılırken, [mtouch](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29) , kullanın **- nolink** veya **- linksdkonly** seçenekleri. -    **-nolink** tüm bağlama devre dışı bırakır.
-    **-linksdkonly** yalnızca sağlanan Xamarin.iOS derlemeler gibi bağlayacaksınız *monotouch.dll* veya xamarin.ios.dll.

Elde edilen yürütülebilir daha küçük olmasını sağlamak derlemeleri bağlı olduğunu unutmayın; Bu nedenle, bağlamayı devre dışı bırakma arzu olandan daha büyük bir yürütülebilir neden olabilir.

## <a name="you-are-getting-a-modelnotimplementedexception"></a>Bir ModelNotImplementedException alma

Bu özel durumun alıyorsanız bu temel aradığınız anlamına gelir. Bir Model geçersiz kılan bir sınıf yöntemi (). ([Model] özniteliğiyle işaretlenmiş sınıfları bunlar) modelleri için bir sınıf temel yöntemini çağırmak gerekmez.

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>Bu sınıf anahtarı değeri kodlama için XXXX anahtar uyumlu değil

NIB dosya yüklenirken bu hata alırsanız, XXXX yönetilen sınıfınız bulunamadı değeri anlamına gelir. Başka bir deyişle, böyle bir bildirimi eksik:

```csharp
[Connect]
TypeName XXXX {
   get {
       return (TypeName) GetNativeField ("XXXX");
   }
   set {
       SetNativeField ("XXXX", value);
   }
}
```

Yukarıdaki tanımı otomatik olarak Visual Studio tarafından Mac için Visual Studio içinde Mac için eklediğiniz XIB dosyalar için oluşturulan `NAME_OF_YOUR_XIB_FILE.designer.xib.cs` dosyası.

Ayrıca, yukarıdaki kod içeren türleri öğesinin bir alt kümesi olmalıdır [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).  İçeren türü bir ad alanı içinde ise, ayrıca olmalıdır bir [[Kaydet]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) ad alanı olmayan bir tür adı (arabirimi Oluşturucu ad alanlarını türlerinde desteklemez gibi) sağlayan öznitelik:

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>Bilinmeyen bir sınıf arabirimi Oluşturucu dosyasında XXXX

Bu hata, arabirimi Oluşturucu dosyalarınızda bir sınıf tanımlama, ancak gerçek uygulaması, C# kodunda sağlamaz oluşturulur.

Bu gibi bazı kodlar eklemeniz gerekir:

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>System.MissingMethodException: Foo.Bar::ctor(System.IntPtr) için bulunan bir oluşturucu yok

Kod bir örneğini arabirimi Oluşturucu dosyanızdan başvurulan sınıflardan çalıştığında bu hata çalışma zamanında oluşturulur. Bu, bir parametre olarak tek bir IntPtr alan bir oluşturucu eklemek mı unuttunuz anlamına gelir.

Oluşturucu bir IntPtr tanıtıcısı ile yönetilen nesneler kendi yönetilmeyen Beyanları ile bağlamak için kullanılır.

Bu sorunu gidermek için Foo.Bar sınıfına aşağıdaki kod satırını ekleyin:

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>{Foo} türü için tanım içermiyor `GetNativeField' and no extension method `GetNativeField' türündeki {Foo} bulunamadı

Tasarımcı oluşturulan dosyaları bu hata iletisini alırsanız (*. xib.designer.cs), ikisinden birini anlamına gelir:

 **1) eksik parçalı sınıf veya temel sınıf**

Bazı alt devralan karşılık gelen kısmi kullanıcı kodu sınıflarda tasarımcısı tarafından oluşturulan kısmi sınıflar olmalıdır `NSObject`, genellikle `UIViewController`. Böyle bir sınıfın hata vermiş türü için olduğundan emin olun.

 **2) değiştirilen varsayılan ad alanları**

Tasarımcı dosyaları projenizin varsayılan ad alanı ayarları kullanılarak oluşturulur. Bu ayarları değiştirilmiş veya projeyi yeniden adlandırılmış değilse, oluşturulan kısmi sınıflar artık kullanıcı kodu dekiler gibi aynı ad olabilir.

Namespace ayarlar proje Seçenekleri iletişim kutusunda bulunabilir. Varsayılan ad alanı bulunan **genel -> Main ayarları** bölümü. Boş ise, projenizin adını varsayılan olarak kullanılır. Daha gelişmiş ad alanı ayarlarını bulunabilir **kaynak kodu .NET adlandırma ilkeler ->** bölümü.

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>Eylemler için Uyarı: 'Foo' hiç kullanılmamış özel yöntem. (CS0169)

Bu uyarı beklendiği şekilde arabirimi Oluşturucu dosya eylemleri pencere öğeleri için çalışma zamanında yansıma tarafından bağlı.

"#Pragma uyarısı devre dışı bırak 0169" kullanabilirsiniz "#pragma uyarısı etkinleştirmek 0169" eylemlerinizi geçici bu yöntemleri için yalnızca bu uyarıyı gizlemek veya tüm projeniz (not için devre dışı bırakmak istiyorsanız 0169 derleyici seçenekleri "Yoksay Uyarılar" alanına eklemek istiyorsanız Önerilen).

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>mtouch aşağıdaki iletiyle başarısız oldu: derleme açılamıyor ' / path/to/yourproject.exe'

Bu hata iletisini görürseniz, genellikle projenizi mutlak yolu boşluk içeren sorunudur. Bu Xamarin.iOS gelecek bir sürümünde düzeltilecektir ancak sorunun geçici bir klasöre boşluksuz proje taşıyarak çalışabilir.

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Sqlite3 sürümünüzü eski - Lütfen en az yükseltme v3.5.0!

Aşağıdakilerin tümü yaptığınızda bu gerçekleşir:

1.  Mono.Data.Sqlite kullanın
1.  Mac OS X Leopard (10.5) kullanın
1.  Simulator uygulamanızda çalıştırın.


Mono OS X çekme olduğunu sorunudur `libsqlite3.dylib`, değil iPhoneSimulator's `libsqlite3.dylib` dosya. Uygulamanızı *olacak* cihaz ancak değil, simulator iş.

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>Aygıt başarısız System.Exception ile dağıtın: AMDeviceInstallApplication 3892346901 döndürdü

Bu hata, sertifika/paket kimliği için kod imzalama yapılandırması aygıtınızda yüklü sağlama profili eşleşmiyor anlamına gelir.  İPhone paket imzalama -> Proje seçeneklerinde seçilen uygun sertifika varsa ve proje seçeneklerinde belirtilen doğru paket kimliği iPhone uygulama -> Onayla

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>Kod tamamlama Visual Studio'da Mac için çalışmıyor

Mac ve Xamarin.iOS için Visual Studio en son sürümünü kullandığınızdan emin olun

Lütfen sorunu hala mevcut olup olmadığını [dosyalama](http://monodevelop.com/Developers#Reporting_Bugs), bağlanmasını **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{zaman damgası} .log**, ve **bileşenleri-{zaman damgası} .log** günlük dosyaları.

Tüm yöntemler başarısız olursa, böylelikle onu yeniden kod tamamlama önbelleği kaldırma deneyebilirsiniz:

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

Bu komut doğru yazın veya önemli dosyaların yanlışlıkla kaldıramadı dikkatli olun.

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>Metin kopyaladığınızda, Mac için visual Studio kilitleniyor

Popüler Mac yardımcı programları QuickSilver, Google araç çubuğu ve Başlatma çubuğu, Visual Studio için Mac'ın bellek bozuk Pano özellikleri vardır. Kendi Seçenekler'de, bunlar engellenmemelidir bir işlem olarak Mac için Visual Studio listeleyebilirsiniz.

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Mac için Visual Studio Mono gerekli 2.4 hakkında complains

Visual Studio, Mac için yeni bir güncelleştirme nedeniyle güncelleştirildi ve yeniden onu Mono mevcut olmaması 2.4 hakkında complains başlatmak çalıştığınızda, yapmanız gereken tek şey [Mono 2.4 yüklemenizi yükseltme](http://www.go-mono.com/mono-downloads/download.html).  

Mono 2.4.2.3_6, Visual Studio Mac için güvenilir bir şekilde, başlangıçta Mac için bazen askıdaki Visual Studio çalışmasını engelledi veya kod tamamlama veritabanı oluşturulmasını önler engelledi bazı önemli sorunları giderir.

Yeni Mono yükledikten sonra Mac için Visual Studio beklendiği gibi başlar.

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>Onaylama işlemi sırasında... /.. /.. /.. /Mono/metadata/Generic-Sharing.c:704, koşulu karşılanmadı'OTI '

Aşağıdaki yığın izleme alıyorsanız:

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

Bu, Flash koduyla projenize derlenmiş bir statik kitaplık bağlama anlamına gelir. İPhone SDK sürüm 3.1 (veya üstü bu yazma zamanında) itibariyle Apple sunulan kendi bağlayıcı hatada Flash olmayan kodunu (Xamarin.iOS) (statik kitaplık) Flash koduyla bağlarken. Bu sorunu azaltmak için statik kitaplığınızın Flash olmayan sürümüyle bağlamanız gerekir.

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1getcount-"></a>System.ExecutionEngineException: Çalışırken JIT derleme yöntemi (sarmalayıcı yönetilen) Foo[]:System.Collections.Generic.ICollection'1.get_Count (için)

[] Soneki, siz veya sınıf kitaplığı çağırdığınız, yöntemi bir dizi IEnumerable <>, ICollection <> veya IList <> gibi bir genel koleksiyon üzerinden gösterir. Geçici bir çözüm olarak kendiniz yöntemini çağırarak bu tür yöntem eklemek için uygulama nesne AĞACI derleyici explicitely zorla olabilir ve dikkat ederek tarafından bu kod, özel durum tetikledi çağrısından önce yürütülür. Bu durumda, yazabilirsiniz:

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

Hangi get_Count yöntemi eklemek için uygulama nesne AĞACI derleyici zorlar.

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Visual Studio Mac Kaynak Düzenleyici için son derece yavaş

Bazen Mac Kaynak Düzenleyici için Visual Studio karakterleri yazmaya arasında birkaç saniye boyunca askıda görünmesine son derece düşük olur.

Bu sorunu çok nadir ve yeniden oluşturmak son derece sabit - bu genellikle aynı makinede Mac için Visual Studio yeniden başlatıldıktan sonra yeniden oluşturulamaz Mac için Visual Studio yeniden başlatmadan önce birkaç hata ayıklama adımları gerçekleştirmek ve sonuçları bize gönderirseniz bu nedenle bu değer veriyoruz.

1.  Düzenleyici sekmesini kapatın ve yeniden açmak. Düzenleme veya yavaşlama yeniden işlem yapılana kadar şapka dolaşma biraz sürer?
1.  "Işını eşitleme" "Quartz Debug" Geliştirici aracı kullanılarak devre dışı bırak (bulabileceğiniz Spotlight kullanarak) ve Kaynak Düzenleyicisi performans normal olarak geri olup olmadığını denetleyin.
1.  Adım (1) yinelenen ışını hala devre dışı eşitleme ile deneyin.
1.  Birden fazla birkaç saniye boyunca Düzenleyicisi asılı kalırsa çalıştırmayı denediğinizde "killall-[Mac için Visual Studio] çıkın" askıda sırada bir terminalde. Düzenleyici askıda, ancak komut Yığın izlemeleri tüm iş parçacıklarının XS askıda ederken iş parçacığı bulunan hangi durumunun bulmak için kullanabileceğiniz MD günlüğüne yazılması için Mono zorladığından Bunu yapmak için gerekli olmasını KILL komutu zaman zor olabilir.



Lütfen XS günlükleri, attach **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{zaman damgası} .log**, ve ** bileşenleri-{zaman damgası} .log ** (XS eski sürümlerinde / MonoDevelop, yalnızca gönderme **~/Library/Logs/MonoDevelop-(3.0|2.8|2.6)/MonoDevelop.log**).

 **Not: Yukarıdaki sorunu XS 2.2 kesin olarak düzeltildi**

## <a name="compiled-application-is-very-large"></a>Derlenmiş uygulama çok büyük

Hata ayıklamayı desteklemek için ek kod hata ayıklama derlemeleri içerir. Yayın modunda oluşturulan projeler boyutunun kesir ' dir.

Hata ayıklama Xamarin.iOS 1.3 itibariyle Mono (çerçeveleri, her bir sınıftaki her yöntemi) tek her bileşeninin desteği hata ayıklama derlemeleri dahil.  

Xamarin.iOS 1.4 ile biz hata ayıklama için bir daha hassas yöntemi getirir, varsayılan yalnızca kodunuzu ve Kitaplıklarınızı için hata ayıklama araçları'nı sağlayın ve tüm için bunu olacak [Mono derlemeleri](~/cross-platform/internals/available-assemblies.md) (Bu devam eder mümkündür, ancak bu derlemeler hata ayıklama için katılımı gerekir).

## <a name="installation-hangs"></a>Yükleme askıda kalır

İPhone benzeticisi çalışan varsa Mono ve Xamarin.iOS yükleyicileri askıda. Bu sorun Mono veya Xamarin.iOS ile sınırlı değil, iPhone benzeticisi yükleme zamanında çalışıyorsa, MacOS kar Leopard üzerinde yazılım yüklemeye çalıştığında herhangi bir yazılım genelinde tutarlı bir sorun olduğunu.

İPhone benzeticisi çıkın ve yüklemeyi yeniden deneyin emin olun.

<a name="trampolines"/>

## <a name="ran-out-of-trampolines-of-type-0"></a>Tür 0 trampolines dışında çalışan

Cihaz çalıştırırken bu iletisi alırsanız, proje Seçenekleri "iPhone derleme" bölümünü değiştirerek 0 trampolines (özel türü için) daha fazla türü oluşturabilirsiniz.  Cihaz için ek bağımsız değişkenler hedefleri derleme eklemek istediğiniz:

 `-aot "ntrampolines=2048"`

Varsayılan trampolines 1024 sayısıdır.  Uygulamanız için yeterli kadar bu sayının artırılması deneyin.

## <a name="ran-out-of-trampolines-of-type-1"></a>1 türü trampolines dışında çalışan

Özyinelemeli genel türler kullanımına ağırlık yaparsanız, bu ileti cihazda alabilirsiniz.  Proje Seçenekleri "iPhone derleme" bölümünü değiştirerek 1 trampolines (türü RGCTX) daha fazla türü oluşturabilirsiniz.  Cihaz için ek bağımsız değişkenler hedefleri derleme eklemek istediğiniz:

 `-aot "nrgctx-trampolines=2048"`

Varsayılan trampolines 1024 sayısıdır.  Genel türler kullanımınızı için yeterli kadar bu sayının artırılması deneyin.

## <a name="ran-out-of-trampolines-of-type-2"></a>Tür 2 trampolines dışında çalışan

Koyu arabirimlerini kullanan yaparsanız, bu ileti cihazda alabilirsiniz.
Proje Seçenekleri "iPhone derleme" bölümünü değiştirerek 2 trampolines (tür IMT dönüştürücüler) daha fazla türü oluşturabilirsiniz.  Cihaz için ek bağımsız değişkenler hedefleri derleme eklemek istediğiniz:

 `-aot "nimt-trampolines=512"`

IMT dönüştürücü trampolines varsayılan sayısı 128'dir.  Yeterli arabirimleri kullanımınızı için kadar bu sayının artırılması deneyin.

## <a name="debugger-is-unable-to-connect-with-the-device"></a>Hata ayıklayıcı aygıtla bağlanamıyor

Bir aygıt yapılandırması hata ayıklama başlattığınızda, uygulamaya bağlanmak çalışıyor olduğunu belirten bir iletişim Göster hata ayıklayıcı görürsünüz. Hata ayıklayıcı uygulamaya bağlanmak mümkün olmayabilir birkaç nedeni vardır, moda bağlı olarak (USB veya WiFi) bağlanmak için kullandığınız.

 **Cihaz ve hata ayıklayıcısı ana farklı ağlarda ise**, güvenlik duvarı veya özel ağ uygulaması WiFi modunda hata ayıklayıcı ana bağlanmasını engelliyor olabilir.

 **Mac için Visual Studio konağının doğru IP sorgu mümkün olmayabilir**. WiFi içinde modu Mac için Visual Studio uygulama bulabileceğiniz tüm IP'ler konağının sunar ve tümünü, bunlardan herhangi birinin Mac için Visual Studio bağlanmak için kullanıp kullanamayacağını görmek için uygulama çalışır

 **Başka bir cihazı konaktaki USB bağlantı noktasına bağlı.** Bazı durumlarda diğer aygıtlar için USB bağlı ana bilgisayar bağlantı noktalarına USB modda hata ayıklama ile şekilde müdahale bilinmektedir.

WiFi veya USB modu çalışmazsa kolayca diğer deneyebilirsiniz: tercihler Mac için Visual Studio'da açın, hata ayıklayıcı/Tercihler/iPhone hata ayıklayıcı sayfasına gidin ve "iOS cihazları yerine WiFi üzerinden USB üzerinden Debug" onay kutusunun Değiştir.   Hiçbiri çalışırsa, ayrıntılı mod aygıt konsolunda hata hakkında daha fazla bilgi görebilirsiniz (hangi etkin ekleyerek "-v - v - v" projenin seçeneklerinde ek mtouch değişkenlerini için).

## <a name="error-134-mtouch-failed-with-the-following-message"></a>Hata 134: mtouch aşağıdaki iletiyle başarısız oldu:

-Nolink ile sürümleri Xamarin.iOS 1.4 stilini oluşturmaya çalışıyorsanız, bu hata gerçekleşti. Bu hataya monodevelop proje yapılandırmanızda ek bağımsız değişkenler belirterek çalışabilir.

Bağımsız değişken Ekle

 `-nosymbolstrip`

ve sorunun çözülmesi gerekir.

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>Dağıtım kimlik imzalama seçenekleri Mac projesi için Visual Studio'da gösterilmez

Mac 2.2 for Visual Studio virgül içeren dağıtım sertifikalar değil algılamak üzere neden olan bir hata var. Lütfen Visual Studio'ya 2.2.1 Mac için güncelleştirin.

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>Hata "döndürülen AFCFileRefWrite: 1" karşıya yükleme sırasında

Cihazınız için bir uygulama karşıya yüklenirken bir hata iletisi alabilirsiniz "döndürülen AFCFileRefWrite: 1". Sıfır uzunluklu dosyanız varsa bu durum oluşabilir.

## <a name="error-mtouch-failed-with-no-output"></a>Hata "mtouch hiçbir çıktısı olmadan başarısız oldu"

Geçerli sürümü Xamarin.iOS ve Visual Studio Mac için başarısız proje adı ya da çözüm ya da proje depolandığı dizine boşluk içeremez.
Bu sorunu gidermek için:


-  Projenizi ya da nerede depolandığı dizine bir alan içerdiğinden emin olun.
-  Proje "ana ayarlarınızda" proje adı boşluk karakterleri içermediğinden emin olun.

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>Hata "karşıya yüklediğiniz ikili geçersizdi. SDK yayın öncesi beta sürümü, uygulama oluşturmak için kullanılan"

Bu hata genellikle Xamarin.iOS 2.0.0 yayınlanmadan önce iPad geliştirme başlatılmasından bir projeyle nedeni, büyük olasılıkla, Info.plist gibi bazı anahtarlara sahip:

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Mac için Visual Studio, sizin için otomatik olarak işleme gibi bu anahtar çiftinin süresi kaldırılması gerekir.

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>Hata "SDK yayın öncesi beta sürümü uygulamanızı oluşturmak için kullanılan"

(Ed Anuff tarafından katkıda)

Aşağıdaki adımları uygulayın:

-  İPhone yapı 3.2 veya iTunes SDK sürümünde bağlanmak değişiklik 3.2'den küçük bir SDK sürümü kullanılarak oluşturulmuş bir iPad uyumlu uygulama görmesini olduğundan, karşıya yükleme sırasında reddeder
-  Proje için özel bir Info.plist oluşturun ve açıkça 3.0 da MinimumOSVersion ayarlayın.   Bu Xamarin.iOS tarafından ayarlanan MinimumOSVersion 3.2 değerini geçersiz kılar.   Bunu yaparsanız, uygulama bir iPhone üzerinde çalıştırmak mümkün olmaz.
-  Bağlantı yeniden oluşturma, ZIP ve iTunes yüklenecek.

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>İşlenmeyen özel durum: System.Exception: Seçici someSelector bulmak için başarısız oldu: {} türündeki

Bu özel durumun işlemlerden biri tarafından kaynaklanır:


1.  Objective-C çalışma zamanı için bir seçici karşılık gelen [dışarı aktarma] özniteliği için bir yöntem uygulamadan sağladığınız
1.  Tam bağlantılandırma etkinleştirdiniz ve [Koru] özniteliği [verme] ed yöntem uygulanmadı.
1.  Devralınan bir tür özel bir yönteme [dışarı aktarma] özniteliği uyguladınız.


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>MainWindow.xib.designer.cs dosyası güncelleştirilmiyor

Yeni projeler MainWindow.xib.designer dosyasında MainWindow.xib dosyasıyla Grup neden Xamarin Studio 2.4 bir hata vardı. Bu, belirli dosya için tasarımcı kodu güncelleştirmeyi anlamına gelir.

Kendi yerleşik güncelleştirici içinde kullanılabilir Mac için Visual Studio sürümünde bu sorun düzeltilene için lütfen daha yeni sürümü kullandığınız olduğundan emin olun.

(Değil silme) kaldırarak mevcut projeleri düzeltebilirsiniz xib ve tasarımcı dosyası, ardından ekleyerek yedekleyin. Bu dosyaları doğru şekilde yeniden gruplayabilirsiniz.

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>UIAlertView veya UIActionSheet kaybolur oluşturulan sonra

Bu gibi bazı bir kod varsa:

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

işlevinde geçici bir değişken olarak "actionSheet" nesne yaşar ve işlevi sonlandırır hemen toplanacak olan yukarı bitip şekilde atık toplama için uygun nesnesidir.

Bu sorunu gidermek için bir başvuru ", yönteminizi Canlı actionSheet" yönteminize yere dışında tutmanız gerekir.

## <a name="project-always-runs-in-the-ipad-simulator"></a>Proje her zaman çalışır Simulator iPad

İPhone SDK 4.0 yükleyici 2 SDK - yalnızca iPad uygulamaları oluşturmaya yönelik 3.2 SDK ve yapı iPhone ve evrensel uygulamaları için kullanılan 4.0 SDK yükler. Yalnızca iPad benzetimini yapar, 3.2 bir simulator ve iPhone veya iPhone 4 benzetim 4.0 bir simulator de yükler. Tüm eski SDK'ları ve benzeticileri kaldırılır.

Visual Studio Mac iPhone proje derleme seçenekleri için uygulamanızı oluşturmada kullanılacak SDK sürümü için bir ayar içerir. Bu ayar bulunabilir **proje Seçenekleri -> Yapı iPhone Yapı ->**.

Mac için Visual Studio'da yeni proje kendi SDK varsayılan olarak en eski yüklü SDK'yı kullanın ve belirtilen SDK'sı yoksa, Mac için Visual Studio ve uygulamanızı oluşturmak için bulabileceğiniz en yakın kullanır. Projeleri her zaman en yeni SDK requre gerekir böylece bu gerçekleştirilir. Ancak, bu şu anda 3.2 olması SDK içinde sonuçları kullanılan - hangi sonuçları kullanılan iPad benzeticisinde.

4.0 SDK'sını kullanarak bu sorunu gidermek için şu adrese gidin **proje Seçenekleri -> Yapı iPhone Yapı ->**> ve "açılır kutusunu kullanarak 4.0" SDK değerini değiştirin. Her yapılandırma ve platform birleşimi, bölmenin en üstünde bırakmalar kullanılarak erişilir için bunu yapmalısınız.

SDK sürümü "En düşük işletim sistemi sürümü" ayarlarla karıştırılmamalıdır.
Bu değer SDK sürüm değeri eşleşmesi gerekmez - en düşük sürüm, daha eski işletim sisteminde mevcut veya yeni özellikleri çalışma zamanı işletim sistemi sürümünü denetle kullanarak kullanımını koruma API'leri kullandığınız sürece, SDK eski olabilecek uygulamanız yüklenecek işletim sistemi etkiler görevleri. Uygulamanızı test en eski işletim sistemi sürümü için ayarlamanız.

Ayrıca **Proje -> iPhone benzeticisi hedef**> menü, varsayılan olarak bir proje çalışmasını/hata ayıklama sırasında kullanılan simulator seçmek için kullanılabilir. Ayrıca, **Çalıştır -> Çalıştır ile**> menü, belirli bir simulator çalıştırılacağı seçmek için kullanılabilir

## <a name="ibtool-returns-error-133"></a>hata 133 ibtool döndürür

Bu, XCode 4 yüklü olduğu anlamına gelir.   XCode 4 aracı ibtool kaldırıldı, artık XIB dosyalarınızı ayrı bir araç ile düzenlemek mümkün değildir.

Arabirim Oluşturucu kullanmak istiyorsanız, yükleme [XCode serisi 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792), Apple web sitesinden kullanılabilir.

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-wbrinterface-builder"></a>"Görünen bağlama MIME türü için oluşturulamıyor: application/vnd.apple -<wbr/>arabirimi Oluşturucu"

Bu hata, bir iPhone olmayan projeden UI iPhone oluşturmayı denerseniz oluşur. Bir iPhone/iPad çözümüyle'ı başlatın, yalnızca bir iPhone/iPad olmayan projeye iPhone kullanıcı Arabirimi öğeleri eklemek mümkün değildir emin olun.

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>İOS simülatörü içinde yürütülürken başlangıç kilitlenme

Şuna benzer bir yığın izleme ile birlikte simulator içinde çalışma zamanı çökmeyle (SIGSEGV) alırsanız:

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
.. büyük olasılıkla elinizde bir (veya daha fazla) eski derleme simulator uygulama dizininizde .then. Bu tür derlemeler Apple iOS simülatörü ekler ve güncelleştirme dosyaları, ancak hiçbir zaman onları siler beri bulunmaktadır. Sonra en kolay çözüm aşması durumunda "Sıfırlamak ve içerik ve ayarlar..." simulator menüsünden seçmektir.   

**Uyarı:** bu tüm dosyaları, uygulamaları ve verileri simulator kaldırır.   Sonraki açışınızda, uygulamanızın yürütme Mac için Visual Studio simulator dağıtır ve kilitlenme neden eski, eski derleme olacaktır.

## <a name="simulator-hangs-during-application-installation"></a>Uygulama yüklemesi sırasında Simulator kilitleniyor

Uygulama adları dahil, bu durum oluşabilir bir '.' (adında nokta).
Mümkün olsa bile çalışır (aygıtlar gibi) diğer birçok durumda bu yürütülebilir dosya adı CFBundleExecutable - olarak izin verilmiyor.

 * "Değeri uzantıyı adına içermemelidir." - [http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>Hata: "özel öznitelik türü 0x43 desteklenmiyor" çift zaman .xib dosyaları tıklatıldığında

Bu ortam değişkenleri yanlış ayarlandığında .xib dosyaları açmaya tarafından kaynaklanır. Bu ile Visual Studio'nun normal kullanım için Mac/Xamarin.iOS oluşmamalıdır ve Visual Studio /Applications Mac için yeniden açmak sorunu çözer.

Güncellerken yazılımı ve bu hata iletisi görüntülenirse, lütfen e-posta *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>Uygulama simulator'da çalışır ancak aygıtta başarısız

Bu sorun birkaç formlarında bildirilebilir ve tutarlı bir hata her zaman oluşturmuyor. Uygulama bir .xib içeriyorsa, emin olmak için kontrol **yapı eylemi** üzerinde .xib kümesine **InterfaceDefinition**. Bu .xibs için varsayılan derleme işlemdir.

Yapı eyleminin denetlemek için .xib dosyasını sağ tıklatın ve seçin **yapı eylemi**.


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: 437 kodlama için kullanılabilir veri yok.

Xamarin.iOS uygulamanızı 3 taraf kitaplıklar dahil olmak üzere, formda bir hata alabilirsiniz "System.NotSupportedException: 437 kodlama için kullanılabilir veri yok." derlemek ve uygulamayı çalıştırmak çalışırken. Örneğin, kitaplıklar, gibi `Ionic.Zip.ZipFile`, bu işlem sırasında özel durum.

Bu seçenekler gidip Xamarin.iOS projesi için açarak çözülebilir **iOS yapı** > **uluslararası** ve denetimi **Batı** uluslararası duruma getirme.



<a name="Can't_upgrade_to_Indie/Business_from_Trial_Account" />


## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>Indie/iş deneme hesabından yükseltemiyor.

Yakın zamanda Xamarin.iOS satın alınan ve Xamarin.iOS deneme sürümünü daha önce başlatıldı, Mac veya Visual Studio için Visual Studio tarafından toplanma bu lisans değişiklik almak için aşağıdaki adımları tamamlayın gerekebilir.

-  Mac/Visual Studio için Visual Studio'yu kapatın
-  Windows için tüm dosyaları Mac üzerinde ~/Library/MonoTouch veya %PROGRAMDATA%\MonoTouch\License\ Kaldır
-  Mac/Visual Studio için Visual Studio'yu yeniden açın ve bir Xamarin.iOS projesi oluşturma


## <a name="receiving-activation-incomplete-error-message"></a>Alıcı 'Etkinleştirme tamamlanmamış' hata iletisi

Xamarin.iOS Visual Studio için kullanırken bu sorun ortaya çıkabilir. Bu sorunu çözmek için lütfen günlükleri şu konuma gönderin [ contact@xamarin.com ](mailto:contact@xamarin.com).

-  Günlük konumu: %LocalAppData%/Xamarin/Logs
