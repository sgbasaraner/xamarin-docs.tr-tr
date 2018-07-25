---
title: İlerleme ve Xamarin.iOS etkinlik göstergeleri
description: Bu belge Xamarin.ios'ta ilerleme durumunu ve etkinlik göstergeleri kullanmayı açıklar. Bunu programlı ve görsel taslak ile nasıl kullanacağınızı açıklar.
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 24ad47bd37693eccf38033159acef72a9d4942be
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242185"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>İlerleme ve Xamarin.iOS etkinlik göstergeleri

Uygulamanızı uzun gerçekleştirmek sahip olacağını büyük olasılıkla yüklenirken veya veri ve kullanıcı Arabirimi güncelleştirilirken bu gecikme bir gecikmeye neden olabileceğini işleme gibi görevleri çalıştırma. Bu süre boyunca kullanıcı sistemin iş yapan meşgul olduğunu reassure için bir İlerleme göstergesi her zaman kullanmalısınız. Bu uygulama, giriş için beklenmiyor, istek üzerinde çalışmakta olduğu kullanıcı denetimi sağlar ve bunlar beklemek zorunda tam olarak ne kadar süreyle gerçekleşen bir yol sağlayabilir.

iOS uygulamanızda bu İlerleme göstergesi sağlamak için iki ana yolu sağlar: etkinlik göstergeleri (belirli bir dahil olmak üzere _ağ_ etkinliği göstergesini) ve ilerleme çubukları.

## <a name="activity-indicator"></a>Etkinlik göstergesi

Etkinlik göstergeleri uygulamanızın uzun bir işlem çalışıyor, ancak görev gerektirecek tam süreyi tanımadığınız gösterilmesi gerekir.

Apple etkinlik göstergeleri ile çalışmak için aşağıdaki önerileri vardır:

- **Mümkün olduğunda yerine ilerleme çubukları kullanın** - etkinliği göstergesini çalıştırılan işlem ne kadar sürer dair hiçbir geri kullanıcı sağladığından uzunluğu olduğunu biliyorsanız (örneğin, bir dosyayı indirmek için kaç bayt) her zaman bir ilerleme çubuğu kullanın.
- **Gösterge animasyonlu tutmak** -kullanıcıların her zaman, gösterilen sırada animasyonlu göstergesi olmalıdır böylece bu sabit bir etkinliği göstergesini durdurulmuş bir uygulamaya ilgilidir.
- **İşlenmekte olan görev açıklayan** -etkinliği göstergesini kendisi tarafından yalnızca görüntüleme, değilse yeterli, kullanıcı bunlar bekletme işlemi hakkında haberdar olması gerekiyor. Görev açıkça tanımlayan anlamlı bir etiket (genellikle tek ve eksiksiz cümle) içerir.

### <a name="implementing-an-activity-indicator"></a>Bir etkinliği göstergesini uygulama

Bir etkinliği göstergesini aracılığıyla uygulanır [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) göstermek için sınıf bir `UIActivity` yer alıyor.

### <a name="activity-indicators-and-storyboards"></a>Etkinlik göstergeleri ve görsel Taslaklar

İOS Tasarımcısı kullanıcı Arabirimi oluşturmak için kullanıyorsanız, etkinliği göstergesini araç kutusundan düzeninizde eklenebilir. Aşağıdaki özellikleri Özellikler panelinden ayarlanabilir:

![Özellikleri paneli](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Etkinlik göstergesi davranışı yönetme

Kullanım `StartAnimating()` ve `StopAnimating()` yöntemleri etkinlik göstergesi animasyon durdurmak ve başlatmak.

Ayarlama `HidesWhenStopped` özelliğini `true` sonra kaybolur etkinliği göstergesini yapmak `StopAnimating()` çağrıldı. Bu ayar `true` varsayılan olarak. Kontrol ederek etkinliği göstergesini dönen animasyonunun çalışıp çalışmadığını görebilirsiniz herhangi bir noktada `IsAnimating` özelliği. 


### <a name="managing-activity-indicator-appearances"></a>Etkinlik göstergesi görünümleri yönetme

`UIActivityIndicatorViewStyle` Numaralandırma etkinliği göstergesini örneği oluşturulurken parametre olarak geçirilebilir. Bu görsel stili ayarlamak için kullanabileceğiniz `Gray`, `White`, veya `WhiteLarge`, örneğin:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Tarafından sağlanan rengi kılabilirsiniz `UIActivityIndicatorViewStyle` ayarlayarak `Color` özelliği.

## <a name="progress-bar"></a>İlerleme Çubuğu

Durum ve zaman alıcı bir görev uzunluğunu belirtmek üzere rengi ile dolduran bir satır olarak bir ilerleme çubuğu gösterir. İlerleme çubukları, her zaman görevleri uzunluğunu olduğunu veya hesaplanan değer kullanılmalıdır.

Apple ilerleme çubukları ile çalışmak için aşağıdaki önerileri vardır:

- **Doğru ilerlemeyi rapor** -ilerleme çubukları her zaman bir görevi tamamlamak için gereken süreyi doğru bir gösterimi olmalıdır. Uygulama meşgul görünür kılmak için zaman hiçbir zaman ilgili yanlış beyanda bulunma.
- **Kullanım Well-Defined süreleri için** -ilerleme çubuğu değil yalnızca göstermelidir uzun bir iş sürüyor yerleştirin, ancak kullanıcı ve göstergesini görevin ne kadar tamamlanır ve kalan tahmini sağlar.

### <a name="implementing-an-progress-bar"></a>Uygulama bir ilerleme çubuğu

Bir ilerleme çubuğu örneği tarafından oluşturulan bir [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>İlerleme çubukları ve görsel Taslaklar

İOS Designer kullanırken kullanıcı Arabirimi için bir ilerleme çubuğu de ekleyebilirsiniz. Arama **İlerleyenler görünümünde** içinde **araç kutusu** görünümünüzü sürükleyin.

Aşağıdaki özellikler, Özellikler panelinde ayarlanabilir:

![Özellikleri paneli](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>İlerleme çubuğu davranışını yönetme

İlerleme çubuğu başlangıçta kullanılarak ayarlanabilir `Progress` özelliği:

```csharp
ProgressBar.Progress = 0f;
```

İlerleme kullanılarak ayarlanabilir `SetProgress` yöntemi ve bir Boole değeri veya animasyon değişiklik isteyip istemediğinizi bildirmek geçirme.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

İlerleme çubuğu kullanma hakkında daha fazla bilgi için [ilerleme raporlaması](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/networking/download_progress) tarif ve [UICatalog tvOS örnek](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/).

### <a name="managing-progress-bar-appearance"></a>İlerleme çubuğu görünümü yönetme

Benzer şekilde bir etkinliği göstergesini `UIProgressViewStyle` numaralandırma ilerleme çubuğu örneği oluşturulurken parametre olarak geçirilebilir.

İlerleme ve izleme görüntüsü ve renk tonu rengi aşağıdaki özellikler kullanılarak ayarlanabilir:

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



