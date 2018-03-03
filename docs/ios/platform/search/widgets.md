---
title: "Arama ve giriş ekranı pencere öğesi geliştirmeleri"
description: "Bu makalede, Apple iOS 10 pencere öğesi sistemde yapılan geliştirmeler yer almaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a6749ca9d8a793372ec088433780d622f2f05b41
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="search-and-home-screen-widget-enhancements"></a>Arama ve giriş ekranı pencere öğesi geliştirmeleri

_Bu makalede, Apple iOS 10 pencere öğesi sistemde yapılan geliştirmeler yer almaktadır._


Apple pencere öğeleri 10 kilit ekranı üzerinde yeni iOS mevcut herhangi bir arka plan üzerine harika göründüğünden emin olmak için pencere sistemin bazı geliştirmeler anlatılmıştır. Ayrıca, pencere öğeleri artık içeren bir [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) ne kadar içerik kullanılabilir açıklamak Geliştirici sağlar ve içeriği genişletme ve daraltma kullanıcıya izin veren özellik.

Pencere öğeleri (olarak da bilinen bugün uzantıları), az miktarda yararlı bilgileri görüntülemek veya zamanında uygulamaya özgü işlevselliği kullanıma iOS uzantısı özel türüdür. Örneğin, haber uygulama üst başlıkları gösteren bir pencere öğesi sahiptir ve Takvim uygulama iki farklı pencere öğeleri sağlar: bugünün olayları ve yaklaşan olayları göstermek için görüntülemek için.

Pencere öğeleri tümüyle özelleştirilebilir ve metin, görüntüler, düğmeleri, vb. gibi kullanıcı Arabirimi öğeleri içerebilir. Ayrıca, geliştirici, kendi pencere düzenini daha fazla özelleştirebilirsiniz.

[ ![](widgets-images/widgets01.png "Örnek pencere öğeleri")](widgets-images/widgets01.png)

Bir kullanıcının görüntüleyebileceği ve bir uygulamanın pencere öğeleri ile etkileşim, iki ana basamak vardır:

- **Arama ekran** -kullanıcılar, kendi arama ekranına en yararlı buldukları pencere öğeleri ekleyebilirsiniz. Arama ekran hem giriş hem de kilit ekranında sağ geçirilerek erişilir.
- **Giriş ekranı** -gelen giriş ekranında, kullanıcı 3B dokunma uygulamanın simgesine baskısı uygulayarak hızlı Eylemler listesini açın için kullanabilirsiniz. Bir uygulamanın pencere öğeleri yukarıdaki hızlı eylem listede görüntülenir. Lütfen bakın bizim [3B dokunma giriş](~/ios/platform/3d-touch.md) daha fazla bilgi için.

## <a name="widgets-developer-suggestions"></a>Pencere öğeleri Geliştirici önerileri

İdeal olarak, geliştirici her zaman deneyin ve kullanıcı için arama ekranlarını eklemek istersiniz pencere öğeleri tasarlayın. Bu amaçla, Apple aşağıdaki önerileri vardır:

- **Mükemmel, gözatılabilir deneyimi oluşturmak** -kullanıcının durum güncelleştirmeleri kısa, gözatılabilir bilgileri sağlayan veya basit görevleri hızla gerçekleştirmek imkan pencere öğeleri istiyor. Bu doğru miktarda bilgi ve etkileşim önemli bir sağlayan kılar. Mümkün olduğunda, tek bir dokunuşla belirli bir görevi gerçekleştirmek kullanıcı izin verin. Kaydırma veya kayan pencere öğeleri desteklemeyen olduğundan, ayrıca, bu pencere öğesi'nin tasarımında dikkate alınması gerekir.
- **İçeriği hızla Göster** -pencere öğeleri, kullanıcı hiçbir zaman bir pencere öğesi görüntülendikten sonra yüklemek, içerik için beklemek zorunda şekilde gözatılabilir, olacak şekilde tasarlanmıştır. Yerel olarak yeni içerik arka planda yüklenirken bunların son içerik gösterebilir şekilde pencere öğeleri içeriklerini önbelleğe.
- **Uygun doldurma ve kenar boşlukları tedarik** -pencere öğeleri hiçbir zaman kalabalık görünür, bu nedenle bir pencere öğesi'nin Görünüm kenarlarına içeriği genişletme kaçının. Her zaman bir birkaç piksel genişliğinde kenar boşluğu kenarları ve içeriği arasındaki olmalıdır. Apple hizalama kılavuz olarak pencere öğesinin üstünde görüntülenen uygulamanın simgesini kullanarak da önerir. Pencere öğesi bir kılavuz düzeni sunarsa olduğundan kılavuz içinde öğeleri arasında doldurma uygun emin ve dört en fazla öğe sayısını sınırlamak deneyin.
- **Uyarlanabilir düzenleri kullanmak** -A pencere'nın genişliği üzerinde çalıştığından cihaz ve cihazın yönüne göre değişir. Ayrıca bir daraltılmış (varsayılan) veya genişletilmiş (tüm pencere öğeleri tarafından desteklenmez) durumda görüntülenmektedir bir pencere öğesi'nin yüksekliği göre değişebilir. Daraltılmış bir pencere öğesi kabaca iki ve bir yarı standart iOS tablo satırları yüksekliğini sahiptir. Geliştirici için genişletilmiş pencere öğesi boyutu isteyebilir, ancak, ideal olarak ekran yüksekliğini daha az olmalıdır. Daraltılmış durumda pencere öğesi, yalnızca temel, tek başına bilgileri göstermesi gerekir. Ne zaman genişletilmiş, pencere öğesi daraltılmış durumda gösterilen birincil içerik geliştirir ek bilgileri göstermesi gerekir. Hızlı eylem listesinde gösterilen pencere, yalnızca daraltılmış durumda olacaktır.
- **Pencere öğesi'nin arka plan Özelleştir yok** -pencere öğeleri, sistem tarafından sağlanan hafif ve bulanık arka plan üzerinde görüntülenir. Bu pencere öğeleri arasında tutarlılığı yükseltmek ve içeriklerini okunabilirliğini artırmak için yapılır. Kullanıcının kilit ve giriş ekranı duvar kağıtları ile artar çünkü görüntüyü bir pencere öğesi arka planı olarak kullanmaktan kaçının.
- **Siyah veya koyu gri sistem kullanılacak** - bir pencere öğesinde, en iyi sistem yazı tipi works metin görüntüleme. Yazı tipi hafif ve bulanık pencere öğesi arka plan karşı göze için bir siyah veya koyu gri renkte olmalıdır.
- **Uygulama erişimi olduğunda uygun sağlamak** -pencere öğesi'nin her zaman çalışması ayrı olarak kendi uygulamasından, daha kapsamlı işlevsellik gerekliyse, ancak pencere öğesi bakın veya belirli bir bilgiyi düzenlemek için uygulamayı başlatmak olması. Hiçbir zaman bir "uygulamasını açın" düğmesi dahil yalnızca içeriğin kendisini dokunun kullanıcıya izin ve hiçbir zaman açık bir 3. taraf uygulama.
- **Açık, kısa pencere öğesi adı seçin** -uygulamanın simgesi ve pencere öğesi'nin adı üzerinde pencere öğesi'nin içerik her zaman görüntülenir. Uygulamanın adını, birincil pencere öğesi için ve herhangi diğer sağladığı için NET, kısa bir ad kullanarak Apple önerir. Özel pencere öğesi başlık sağlarken, uygulamanın adı (örneğin, Maps yakındaki, Maps Restoran, vb.) ile önek.
- **Kimlik doğrulama değeri eklediğinde bildirmek** - ek işlevler veya bilgileri yalnızca zaman kullanıcı kimlik doğrulaması varsa ve, imzalı bu kullanıcıya sunmak. Paylaşım uygulamasını my say "Kitap kıl için oturum açma" Örneğin, bir kıl.
- **Hızlı bir eylem listesi pencere öğesi seçin** -uygulama birden fazla pencere öğesi sağlıyorsa, geliştirici 3B Touch kullanarak uygulamanın simgesine baskısı uygulayarak kullanıcının hızlı eylem listesini getirir sunmak için birini seçmeniz gerekir.

Pencere öğeleri ile çalışma hakkında daha fazla ayrıntı için lütfen bkz. bizim [uzantıları giriş](~/ios/platform/extensions.md), [3B dokunma giriş](~/ios/platform/3d-touch.md) belgeleri ve Apple'nın [uygulama uzantısı Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>Canlılık verir ile çalışma

Canlılık verir bir pencere öğesi'nin metin pencere öğesi'nin hafif, (sistem tarafından sağlanan) bulanık arka plan üzerinde sunulduğunda okunaklı kalmasını sağlar. İOS 10 önce Geliştirici kullanacağınız bir [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) için pencere öğesi'nin canlılık verir. Örneğin:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

Bu iOS 10 sahip kullanım olması ve biriyle değiştirilmesi gereken bir [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) veya [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect). Örneğin:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>Daraltılmış ve genişletilmiş pencere öğeleri ile çalışma

Yeni iOS 10, şimdi pencere öğeleri içeren bir [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) ne kadar içerik kullanılabilir açıklamak Geliştirici sağlar ve içeriği genişletme ve daraltma kullanıcıya izin veren özellik.

Bir pencere öğesi başlangıçta gösterildiğinde bir daraltılmış durumda değil. Daraltılmış bir pencere öğesi kabaca iki ve bir yarı standart iOS tablo satırları yüksekliğini sahiptir. Geliştirici için genişletilmiş pencere öğesi boyutu isteyebilir, ancak, ideal olarak ekran yüksekliğini daha az olmalıdır. 

Daraltılmış durumda pencere öğesi, yalnızca temel, tek başına bilgileri göstermesi gerekir. Ne zaman genişletilmiş, pencere öğesi daraltılmış durumda gösterilen birincil içerik geliştirir ek bilgileri göstermesi gerekir. Örneğin, hava durumu uygulama daraltılmış olduğunda geçerli hava koşulları gösterir ve genişletildiğinde tahmin saatlik ekler.

Hızlı eylem listesinde gösterilen pencere, yalnızca daraltılmış durumda olacaktır. Uygulama birden fazla pencere öğesi sağlıyorsa, geliştirici 3B Touch kullanarak uygulamanın simgesine baskısı uygulayarak kullanıcının hızlı eylem listesini getirir sunmak için birini seçmeniz gerekir.

Aşağıdaki örnekte bir basit bugün daraltılmış ve genişletilmiş durumları işleyen (Pencere) uzantısıdır:

```csharp
using System;
using NotificationCenter;
using Foundation;
using UIKit;
using CoreGraphics;

namespace MonkeyAbout
{
    public partial class TodayViewController : UIViewController, INCWidgetProviding
    {
        protected TodayViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Tell widget it can be expanded
            ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);

            // Get the maximum size
            var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
        }

        [Export ("widgetPerformUpdateWithCompletionHandler:")]
        public void WidgetPerformUpdate (Action<NCUpdateResult> completionHandler)
        {
            // Take action based on the display mode
            switch (ExtensionContext.GetWidgetActiveDisplayMode()) {
            case NCWidgetDisplayMode.Compact:
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                Content.Text = "Gorilla!!!!";
                break;
            }

            // Report results
            // If an error is encoutered, use NCUpdateResultFailed
            // If there's no update required, use NCUpdateResultNoData
            // If there's an update, use NCUpdateResultNewData
            completionHandler (NCUpdateResult.NewData);
        }

        [Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
        public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
        {
            // Take action based on the display mode
            switch (activeDisplayMode) {
            case NCWidgetDisplayMode.Compact:
                PreferredContentSize = maxSize;
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                PreferredContentSize = new CGSize (0, 200);
                Content.Text = "Gorilla!!!!";
                break;
            }
        }

    }
}
```

Pencere öğesi görüntü modu belirli kod ayrıntılı olarak bakalım. Bu pencere öğesi genişletilmiş durum desteklediğini sistem bildirmek için onu kullanır:

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

Pencere öğesi'nin geçerli görüntü modu almak için bunu kullanır:

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

En büyük boyutu daraltılmış veya genişletilmiş durumunu almak için bunu kullanır:

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

Ve değiştirme (görüntü modu) durumu işlemek için onu kullanır:

```csharp
[Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
{
    // Take action based on the display mode
    switch (activeDisplayMode) {
    case NCWidgetDisplayMode.Compact:
        PreferredContentSize = maxSize;
        Content.Text = "Let's Monkey About!";
        break;
    case NCWidgetDisplayMode.Expanded:
        PreferredContentSize = new CGSize (0, 200);
        Content.Text = "Gorilla!!!!";
        break;
    }
}
```

(Daraltılmış veya genişletilmiş) her durum için istenen boyut ayarlamaya ek olarak, yeni boyutuna uyacak şekilde görüntülenmekte olan içeriğin de güncelleştirir.

## <a name="summary"></a>Özet

Bu makalede, Apple iOS 10 pencere öğesi sistemde yapılan geliştirmeler kapsamdaki ve bunların içinde Xamarin.iOS nasıl uygulanacağını gösterilen.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
- [Uzantıları giriş](~/ios/platform/extensions.md)
- [3B dokunmatik giriş](~/ios/platform/3d-touch.md)
- [Uygulama Uzantısı Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
