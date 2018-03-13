---
title: Birim testi
ms.topic: article
ms.prod: xamarin
ms.assetid: BD959779-3239-79B6-5289-3A9ECDFBD973
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: f7c743bab2a6acb3dcd57ebca207957f983e0c0f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="unit-testing"></a>Birim testi

Bu belge, Xamarin.iOS projelerinizi için birim testleri oluşturmak açıklar.
Birim testi Xamarin.iOS ile yapılır hem iOS içeren Touch.Unit framework kullanarak test Çalıştırıcısı yanı sıra NUnit değiştirilmiş bir sürümünü adlı [Touch.Unit](https://github.com/xamarin/Touch.Unit) birim testleri yazma için tanıdık bir API kümesi sağlar.

## <a name="setting-up-a-test-project"></a>Bir Test projesi ayarlama

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bir birim testi çerçevesi projeniz için kurulumu için yapmanız gereken tek şey çözümünüze türündeki bir proje eklemek için **iOS birim testleri projesi**. Çözüm üzerinde sağ tıklayıp seçerek bunu **Ekle > Yeni Proje Ekle**. Listeden seçin **iOS > testleri > Unified API > iOS birim testleri projesi** (C# veya F # seçebileceğiniz).

![](touch.unit-images/00.png "C# veya F # seçin")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bir birim testi çerçevesi projeniz için kurulumu için yapmanız gereken tek şey çözümünüze türündeki bir proje eklemek için **iOS birim testleri projesi**. Çözüm üzerinde sağ tıklayıp seçerek bunu **Ekle > Yeni proje...** . Listeden seçin **Visual C# > iOS > birim testi uygulama (iOS)**.

![](touch.unit-images/00a.png "Birim testi uygulama iOS")

-----

Yukarıdaki temel Çalıştırıcısı programı içeren temel bir proje oluşturur ve yeni bir MonoTouch.NUnitLite derleme, projenizin başvuran şuna benzeyecektir:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](touch.unit-images/01.png "Çözüm Gezgini'nde projeye")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](touch.unit-images/01a.png "Çözüm Gezgini'nde projeye")

-----

`AppDelegate.cs` Sınıfı test Çalıştırıcısı içerir ve şöyle görünür:

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
        UIWindow window;
        TouchRunner runner;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
                // create a new window instance based on the screen size
                window = new UIWindow (UIScreen.MainScreen.Bounds);
                runner = new TouchRunner (window);

                // register every tests included in the main application/assembly
                runner.Add (System.Reflection.Assembly.GetExecutingAssembly ());

                window.RootViewController = new UINavigationController (runner.GetViewController ());

                // make the window visible
                window.MakeKeyAndVisible ();

                return true;
        }
}
```

## <a name="writing-some-tests"></a>Bazı testleri yazma

Temel Kabuk kullanıyor, ilk testleri kümesini yazmanız gerekir.

Testleri olan sınıfları oluşturarak yazılır `[TestFixture]` uygulanmış özniteliği. Her TestFixture sınıfında, uygulamalıdır `[Test]` özniteliği çağırmak için test Çalıştırıcısı istediğiniz her yöntemi. Gerçek test armatürleri testleri projenizdeki herhangi bir dosya dinamik.

Hızlı bir şekilde select başlamak için **yeni dosya Ekle/Ekle** Xamarin.iOS grubu seçin ve **UnitTests**. Bu bir geçirme test, başarısız olan bir test ve bir yoksayılan testleri içeren bir iskelet dosyası ekler, şöyle görünür:

```csharp
using System;
using NUnit.Framework;

namespace Fixtures {

        [TestFixture]
        public class Tests {

                [Test]
                public void Pass ()
                {
                        Assert.True (true);
                }

                [Test]
                public void Fail ()
                {
                        Assert.False (true);
                }

                [Test]
                [Ignore ("another time")]
                public void Ignore ()
                {
                        Assert.True (false);
                }
        }
}
```

## <a name="running-your-tests"></a>Testleri çalıştırma

Çalıştırmak için çözümünüz içinde bu projeye sağ tıklayın ve seçin **hata ayıklama öğesi** veya **çalıştırmak öğesi**.

Sınama Çalıştırıcısı, hangi testlerin kaydedilir ve hangi testlerin yürütülebilecek ayrı ayrı seçmeniz görmenize olanak sağlar.

[![](touch.unit-images/02.png "Kayıtlı testlerin listesi")](touch.unit-images/02.png#lightbox) 

[![](touch.unit-images/03.png "Tek bir metin")](touch.unit-images/03.png#lightbox) 

[![](touch.unit-images/04.png "Çalıştırma sonuçları")](touch.unit-images/04.png#lightbox)

İç içe geçmiş görünümleri metin donanımı seçerek ayrı ayrı test armatürleri çalıştırabilirsiniz veya tüm testleri "Çalıştırmak olan her şeyi" komutunu çalıştırabilirsiniz. Bir geçirme test, bir hata ve yok sayılacak test dahil olmak üzere beklenen varsayılan test çalıştırırsanız. Bu raporun nasıl göründüğünü ve doğrudan başarısız testleri detaya ve hata hakkında daha fazla bilgi bulabilirsiniz:

[![](touch.unit-images/05.png "Bir örnek raporu") ](touch.unit-images/05.png#lightbox) [ ![ ] (touch.unit-images/05.png "bir örnek raporu") ](touch.unit-images/05.png#lightbox) [ ![ ] (touch.unit-images/05.png "bir örnek raporu")](touch.unit-images/05.png#lightbox)

Uygulama çıkış penceresine Ayrıca, hangi testlerin yürütülen görmek için IDE ve bunların geçerli durumu da bakabilirsiniz.

## <a name="writing-new-tests"></a>Yeni testleri yazma

NUnitLite olduğu adlı NUnit değiştirilmiş bir sürümünü [Touch.Unit](https://github.com/xamarin/Touch.Unit) projesi. Fikirlerinizi göre .NET için basit bir sınama çerçevesi olan [NUnit](http://nunit.com/) ve özelliklerinin bir alt sağlama.
En az kaynak kullanır ve katıştırılmış ve mobil geliştirme Kullanılanlar gibi kaynak kısıtlanmış platformlarında çalışır. Yapabilecekleriniz [NUnitLite API Gözat](https://developer.xamarin.com/api/namespace/NUnitLite/) Xamarin.iOS içinde de kullanılabilir. Birim testi şablon tarafından sağlanan temel çatıyı ile ana giriş noktanızdır olan [sınıfı Assert](https://developer.xamarin.com/api/type/NUnit.Framework.Assert/) yöntemleri.

Assert sınıfı yöntemleri ek olarak, birim sınama işlevselliği NUnitLite parçası olan şu ad alanlarından üzerinde ayrılır:

-   [NUnit.Framework](https://developer.xamarin.com/api/namespace/NUnit.Framework/)
-   [NUnit.Constraints](https://developer.xamarin.com/api/namespace/NUnit.Framework.Constraints/)
-   [NUnitLite](https://developer.xamarin.com/api/namespace/NUnitLite/)
-   [NUniteLite.Runner](https://developer.xamarin.com/api/namespace/NUnitLite.Runner/)


Xamarin.iOS özel birim test Çalıştırıcısı burada belgelenmiştir:

-   [NUnit.UI.TouchRunner](https://developer.xamarin.com/api/type/NUnit.UI.TouchRunner/)
