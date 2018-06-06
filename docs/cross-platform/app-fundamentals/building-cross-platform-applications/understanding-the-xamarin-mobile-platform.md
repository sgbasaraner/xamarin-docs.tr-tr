---
title: Bölüm 1 – Xamarin mobil Platform anlama
description: Bu belgede Xamarin platform derleme işlemi, platform SDK erişim, kod paylaşımını, kullanıcı arabirimi oluşturma, görsel tasarımcılar ve daha fazla arayan bir yüksek düzeyde açıklanır.
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 40183e5f18ee589adaf2ea903f6948293c8680f7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781900"
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>Bölüm 1 – Xamarin mobil Platform anlama

İOS ve Android için uygulama geliştirmeye olanak tanıyan öğe sayısı, Xamarin platform oluşur:

-   **C# dili** – tanıdık bir söz dizimi ve genel türler ve LINQ paralel görev kitaplığı gibi gelişmiş özellikler kullanmanıza olanak sağlar.
-   **Mono .NET framework** – Microsoft .NET framework kapsamlı özelliklerinde bir platformlar arası uygulamasını sağlar.
-   **Derleyici** –, platforma bağlı olarak (ör. yerel bir uygulama oluşturur iOS) veya bir tümleşik .NET uygulaması ve çalışma zamanı (ör.) Android). Derleyici birçok iyileştirmeler koyma beklemediğiniz kullanılan kod bağlama gibi mobil dağıtımı için de gerçekleştirir.
-   **IDE Araçları** – Mac ve Windows üzerinde Visual Studio oluşturma, derleme ve Xamarin projeleri dağıtmanıza olanak tanır.


Temel dil C# .NET framework ile olduğundan, ayrıca, projeler de Windows Phone için dağıtılabilir kod paylaşmak için yapısal olarak oluşturulabilir.

 <a name="Under_the_Hood" />


## <a name="under-the-hood"></a>Başlık altında

Xamarin uygulamaları C# dilinde yazma ve birden çok platform genelinde aynı kodu paylaşır sağlar ancak gerçek her sistemde çok farklı uygulamasıdır.

 <a name="Compilation" />


## <a name="compilation"></a>Derleme

C# kaynak yolu yerel bir uygulamaya her platformda çok farklı şekillerde sağlar:

-   **iOS** – C#, tamamlanan-in-ARM derleme dili için derlenmiş süre (Uygulama Nesne AĞACI). .NET framework uygulama boyutunu azaltmak için bağlama sırasında çıkarılır kullanılmayan sınıflarla içindedir. Apple izin vermiyor çalışma zamanı kodu oluşturma İos'ta, bazı dil özellikleri kullanılabilir değildir (bkz [Xamarin.iOS sınırlamalar](~/ios/internals/limitations.md) ).
-   **Android** – C# IL için derlenmiş ve paketlenmiş MonoVM + JIT'ing ile. Framework'te kullanılmayan sınıfları bağlama sırasında çıkarılır. Uygulama çalıştırır yan yana Java/resim (Android çalışma zamanı) ile ve JNI aracılığıyla yerel türler ile etkileşime giren (bkz [Xamarin.Android sınırlamalar](~/android/internals/limitations.md) ).
-   **Windows** – C# IL için derlenmiş yerleşik çalışma zamanı tarafından yürütülen ve Xamarin araçları gerektirmez. Windows uygulamaları tasarlama aşağıdaki Xamarin'ın kılavuz, iOS ve Android kodu yeniden kullanmak daha basit kolaylaştırır.
  Evrensel Windows platformu de sahip Not bir **.NET yerel** Xamarin.iOS Uygulama Nesne AĞACI derleme benzer şekilde davranır seçeneği.


Bağlayıcı belgelerine [Xamarin.iOS](~/ios/deploy-test/linker.md) ve [Xamarin.Android](~/android/deploy-test/linker.md) derleme işleminin bu bölümü hakkında daha fazla bilgi sağlar.

Çalışma zamanı 'derleme' koduyla dinamik olarak oluşturma – `System.Reflection.Emit` – kaçınılmalıdır.

Apple'nın çekirdek dinamik kod oluşturma iOS cihazlarda engeller, bu nedenle kod üzerinde-çalışma sırasında yayma Xamarin.iOS içinde çalışmaz. Benzer şekilde, dinamik dil çalışma zamanı özellikleri Xamarin araçları ile kullanılamaz.

Bazı yansıma özellikleri (ör. çalışma MonoTouch.Dialog kullanır, yansıtma API'si), kod oluşturmayı değil.

 <a name="Platform_SDK_Access" />


## <a name="platform-sdk-access"></a>Platform SDK erişim

Xamarin tanıdık C# sözdizimi ile kolayca erişilebilir platforma özgü SDK tarafından sağlanan özellikleri sağlar:

-   **iOS** – Xamarin.iOS C# ' dan başvurabileceğiniz ad alanları olarak Apple'nın CocoaTouch SDK çerçeveleri gösterir. Örneğin, tüm kullanıcı arabirimi denetimlerini içeren Uıkit framework ile basit bir dahil edilebilir `using UIKit;` deyimi.
-   **Android** – Xamarin.Android kullanıma sunan Google Android SDK'sı ad alanları, herhangi bir kısmını kullanarak bir desteklenen SDK'sı ile başvurmak için deyimi, gibi `using Android.Views;` kullanıcı arabirimi denetimlerini erişmek için.
-   **Windows** – Windows uygulamalarını Windows Visual Studio kullanarak yerleşiktir. Proje türleri Windows Forms, WPF, WinRT ve evrensel Windows Platformu (UWP) içerir.


 <a name="Seamless_Integration_for_Developers" />


## <a name="seamless-integration-for-developers"></a>Geliştiriciler için sorunsuz tümleştirme

Xamarin güzelliğinin başlık altında farklar rağmen tüm üç platformlarda yeniden kullanılabilir C# kod yazma için sorunsuz bir deneyim Xamarin.iOS ve (Microsoft'un Windows SDK'ları ile birlikte) Xamarin.Android sunar ' dir.

İş mantığı, veritabanı kullanımı, ağ erişimi ve diğer ortak işlevleri kez yazılmış ve her platformda görünümlü ve yerel uygulamalar olarak gerçekleştirmek platforma özgü kullanıcı arabirimleri için bir temel sağlayacak tekrar kullanılabilir.

 <a name="Integrated_Development_Environment_(IDE)_Availability" />


## <a name="integrated-development-environment-ide-availability"></a>Tümleşik geliştirme ortamı (IDE) kullanılabilirliği

Xamarin geliştirme, Mac veya Windows Visual Studio'da yapılabilir. Hedeflemek istediğiniz IDE platformlar tarafından belirlenir seçin.

Windows uygulamaları yalnızca iOS için oluşturmak için Windows geliştirilebilir çünkü Android, _ve_ Windows, Windows için Visual Studio gerektirir. Ancak projeleri ve Windows ve Mac bilgisayarlar, iOS ve Android uygulamalarını Mac üzerinde oluşturulabilir ve paylaşılan kodu daha sonra Windows projeye eklenemedi arasında dosya paylaşmak mümkündür.

Her platform için geliştirme gereksinimleri daha ayrıntılı olarak ele alınmıştır [gereksinim](~/cross-platform/get-started/requirements.md) Kılavuzu.


<a name="iOS" />

### <a name="ios"></a>iOS

İOS uygulamaları geliştirme macOS çalıştıran bir Mac bilgisayara gerektirir. Visual Studio, yazma ve Visual Studio'da xamarin'le iOS uygulamalarını dağıtmak için de kullanabilirsiniz. Ancak, yapı ve lisans amacıyla Mac hala gereklidir.

Derleme ve test etmek için simulator sağlamak için Apple'nın Xcode IDE yeniden yüklenmesi gerekir. Kendi cihazlarda test [ücretsiz](~/ios/get-started/installation/device-provisioning/free-provisioning.md), ancak (ör. dağıtım için uygulamalar oluşturmak için Uygulama Mağazası'nda) Apple Developer Program ($99 başına ABD Doları yıl) katılması gerekir. Gönderme veya bir uygulamayı güncelleştirmek her zaman bu gözden ve müşterilerin karşıdan yüklemek kullanılabilir hale getirilir önce Apple tarafından onaylanan gerekir.

Kod ile Visual Studio IDE yazılır ve ekranı düzeni programlı olarak yerleşik veya Xamarin'ın iOS Tasarımcısı'nda IDE ya da sahip düzenlenemez.

Başvurmak [Xamarin.iOS Yükleme Kılavuzu'na](~/ios/get-started/installation/index.md) ayarlanan hakkında ayrıntılı yönergeler için.

<a name="Android" />

### <a name="android"></a>Android

Android uygulama geliştirme Java ve Android SDK'ları yüklü gerektirir. Bunlar, derleyici, öykünücüsü ve oluşturma, dağıtım ve test için gereken diğer araçları sağlar. Java, Google Android SDK ve Xamarin'in araçlarını tüm yüklenebilir ve aşağıdaki yapılandırmaların çalıştırın:

-  Mac OS X El Capitan ve üstünde (10.11 sürümünü +) ile Mac için Visual Studio
-  Windows 7 & yukarıdaki Visual Studio 2015 veya 2017


Xamarin sisteminizi önkoşul Java ile (ekranı düzeni için görsel tasarımcı dahil), Android ve Xamarin araçları yapılandırma birleşik bir yükleyici sağlar. Başvurmak [Xamarin.Android Yükleme Kılavuzu'na](~/android/get-started/installation/index.md) ayrıntılı yönergeler için.

Derleme ve test Google, gerçek bir cihazda herhangi bir lisans olmadan uygulamalardan ancak uygulamanızı mağaza kullanarak dağıtmak için (Google Play, Amazon veya Barnes gibi &amp; Noble) kayıt ücret işlecine borç olabilir. Diğer depoları Apple için benzer bir onay işlemi varken Google Play uygulamanızı hemen yayımlayın.

 <a name="Windows_Phone" />


### <a name="windows"></a>Windows

Windows uygulamaları (WinForms, WPF veya UWP) Visual Studio ile oluşturulur. Bunlar Xamarin doğrudan kullanmayın. Ancak, C# kodu, Windows, iOS ve Android arasında paylaşılabilir.
Microsoft'un ziyaret [Dev Center](https://developer.microsoft.com/) Windows geliştirme için gerekli araçları hakkında bilgi edinmek için.

 <a name="Creating_the_User_Interface_(UI)" />


## <a name="creating-the-user-interface-ui"></a>Kullanıcı Arabirimi (UI) oluşturma

Xamarin kullanmanın önemli bir avantajı uygulama kullanıcı arabirimi Objective-C veya Java ile yazılmış bir uygulamadan ayırt uygulamaları oluşturma, her platformda yerel denetimlere kullanmasıdır (iOS ve Android için sırasıyla).

Ekranlar uygulamanızı oluştururken, kod denetimleri yerleştirme veya tam ekranlar her platform için sağlanan Tasarım araçları kullanarak oluşturun.

 <a name="Programmatically_Create_Controls" />


### <a name="create-controls-programmatically"></a>Denetimleri program aracılığıyla oluşturma

Her platform kullanıcı arabirimi denetimlerini kod kullanarak bir ekrana eklenecek sağlar. Tamamlanmış tasarım görselleştirmek zor olabilir olarak bu çok zaman alıcı olabilir kodlamak piksel zaman koordinatları denetim konumlar ve boyutları.

Program aracılığıyla denetimleri oluşturma avantajları yine de yeniden boyutlandırın veya farklı arasında iPhone ve iPad ekran boyutlarına işlemek görünümleri oluşturmak için özellikle iOS sahip.

 <a name="Visual_Designer" />


### <a name="visual-designer"></a>Görsel Tasarımcı

Her platformun görsel olarak ekranlar yerleştirme için farklı bir yöntem vardır:

-   **iOS** – Xamarin'ın iOS Tasarımcısı kolaylaştıran sürükle ve bırak işlevsellik ve özellik alanları kullanarak görünümleri oluşturma. Topluca Bu görünümlere film şeridi yapmak ve erişilebilir **. Film şeridi** projenizde dahil dosyası.
-   **Android** – Xamarin Android bir Sürükle ve bırak UI Tasarımcısı için Visual Studio sağlar. Android ekranı düzeni olarak kaydedilir **. AXML** Xamarin araçlarını kullanırken dosyaları.
-   **Windows** – Microsoft Visual Studio ve harmanlama bir Sürükle ve bırak UI tasarımcı sağlar. Ekranı düzeni olarak depolanır. XAML dosyaları.


Bu ekran görüntüleri her platformda kullanılabilir visual ekran tasarımcıları göster:

 [ ![](understanding-the-xamarin-mobile-platform-images/designer-all1.png "Bu ekran görüntüleri her platformda kullanılabilir visual ekran tasarımcıları Göster")](understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

Tüm durumlarda görsel olarak oluşturduğunuz öğeleri kodunuzda başvurulabilir.

 <a name="User_Interface_Considerations" />


### <a name="user-interface-considerations"></a>Kullanıcı arabirimi konuları

Platformlar arası uygulamalar oluşturmak için Xamarin kullanmanın önemli bir avantajı, bilinen bir arayüz kullanıcıya sunmak üzere yerel UI araç takımları özelliklerden yararlanabilirsiniz ' dir. Ayrıca, kullanıcı Arabirimi yerel herhangi bir uygulama olarak hızlı gerçekleştirir.

Bazı kullanıcı Arabirimi metaphors birden fazla platformda çalışması (örneğin, tüm üç platformlar benzer kaydırma liste denetimi kullanın) ancak sırada eşitleyerek' sağ ' uygulamanız için kullanıcı arabirimini platforma özgü kullanıcı arabirimi öğeleri uygun olduğunda yararlanmak. Platforma özgü UI metaphors örnekleri şunlardır:

-   **iOS** – yumuşak geri düğmesini hiyerarşik gezinme sekmeler ekranın üzerinde.
-   **Android** – sistem/donanım-yazılım düğmesi, eylem menüsü, ekranın üst kısmındaki sekmeleri yedekleyin.
-   **Windows** – Windows uygulamaları, masaüstü bilgisayarları, (örneğin, Microsoft Surface) tabletler ve telefonlar çalıştırabilirsiniz. Windows 10 cihazları düğmesi ve canlı kutucuklar, örneğin yedekleme donanım olabilir.


Hedeflediğiniz platformları ilgili tasarım yönergeleri okumanız önerilir:

-   **iOS** – [Apple İnsan Arabirimi yönergelerine](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
-   **Android** – [Google kullanıcı arabirimi yönergeleri](http://developer.android.com/guide/practices/ui_guidelines/index.html)
-   **Windows** – [Windows için kullanıcı deneyimi tasarım yönergeleri](https://developer.microsoft.com/windows/design)


 <a name="Library_and_Code_Re-use" />


## <a name="library-and-code-re-use"></a>Kitaplık ve kodu yeniden kullanma

Xamarin platformu, yerel olarak her platform için yazılmış kitaplıkları tümleştirme yanı sıra tüm platformlar genelinde yeniden kullanımını varolan C# kodu sağlar.

 <a name="C#_Source_and_Libraries" />


### <a name="c-source-and-libraries"></a>C# kaynak ve kitaplıklar

Xamarin ürünleri C# ve .NET framework kullandığından, mevcut kaynak kodunda (hem kaynak hem de şirket içi projeleri Aç) çok sayıda Xamarin.iOS veya Xamarin.Android projelerinde yeniden kullanılabilir. Genellikle kaynak yalnızca bir Xamarin çözüme eklenebilir ve hemen çalışır. Desteklenmeyen bir .NET framework özelliğini kullandıysanız bazı tweaks gerekli olabilir.

Xamarin.iOS veya Xamarin.Android kullanılan C# kaynak örnekler: SQLite NET, NewtonSoft.JSON ve SharpZipLib.

 <a name="Objective-C_Bindings_+_Binding_Projects" />


### <a name="objective-c-bindings--binding-projects"></a>Objective-C bağlamaları + bağlama projeleri

Xamarin adlı bir araç sağlar *btouch* Xamarin.iOS projelerinde kullanılacak Objective-C kitaplıkları izin bağlamaları oluşturulmasına yardım eder. Başvurmak [Objective-C türleri bağlama belgelerine](~/cross-platform/macios/binding/binding-types-reference.md) bunun nasıl yapılacağı hakkında bilgi.

Xamarin.iOS içinde kullanılabilir Objective-C kitaplıkları örnekler: RedLaser barkod tarama, Google Analytics ve PayPal tümleştirme. Açık kaynak Xamarin.iOS bağlamaları bulunur [github](https://github.com/mono/monotouch-bindings).

 <a name=".jar_Bindings_+_Binding_Projects" />


### <a name="jar-bindings--binding-projects"></a>.jar bağlamaları + bağlama projeleri

Xamarin Xamarin.Android varolan Java kitaplıklarında kullanılmasını destekler. Başvurmak [Java kitaplığı belgeleri bağlama](~/android/platform/binding-java-library/index.md) nasıl kullanılacağı hakkında ayrıntılar için bir. Xamarin.Android JAR dosyasını.

Açık kaynak Xamarin.Android bağlamaları bulunur [github](https://github.com/mono/monodroid-bindings).

 <a name="C_via_PInvoke" />


### <a name="c-via-pinvoke"></a>PInvoke aracılığıyla C

"Platform Invoke" teknoloji (P/Invoke) geri yönetilen kod yönetilen kod (C yanı sıra yerel kitaplıklarında yöntemleri çağırmak için yerel kitaplıkları çağırmak destek #) sağlar.

Örneğin, [SQLite NET](https://github.com/praeclarum/sqlite-net) kitaplığını kullanan deyimleri şuna benzer:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

Bu, iOS ve Android yerel C dili SQLite uygulamasında bağlar.
Varolan bir C API'si ile hakkında bilgi sahibi geliştiriciler, yerel API eşleştirmek ve varolan platform kodu kullanmak için C# sınıfları kümesi oluşturabilirsiniz. Belgelerine yoktur [yerel kitaplıkları bağlama](~/ios/platform/native-interop.md) Xamarin.iOS içinde benzer ilkeler Xamarin.Android için geçerlidir.

 <a name="C++_via_Cxxi" />


### <a name="c-via-cppsharp"></a>C++ CppSharp aracılığıyla

Miguel CXXI açıklar (şimdi adlı [CppSharp](https://github.com/mono/CppSharp)) parolasını üzerinde [blog](http://tirania.org/blog/archive/2011/Dec-19.html). C++ Kitaplığı doğrudan bağlama için bir C sarmalayıcı oluşturur ve için P/Invoke bağlamak için alternatiftir.

