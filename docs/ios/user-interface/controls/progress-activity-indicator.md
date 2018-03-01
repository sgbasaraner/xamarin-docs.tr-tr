---
title: "İlerleme durumu ve etkinliği göstergeleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: b91c0d7504b391630beded7a52e240a0d043152f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="progress-and-activity-indicators"></a>İlerleme durumu ve etkinliği göstergeleri

Uygulamanızı uzun taşımak gerekecek olasıdır yüklenirken veya veri ve UI güncelleştirme bu gecikme bir gecikmeye neden olabileceğini işleme gibi görevleri çalıştırma. Bu süre boyunca sistem çalışarak meşgul olduğunu kullanıcı reassure için bir İlerleme göstergesi her zaman kullanmalısınız. Bu uygulama için kendi giriş, beklenmiyor kendi istek üzerinde çalışan kullanıcı denetimi sağlar ve bunlar beklemek zorunda tam olarak ne kadar ayrıntılı bir araç sağlar.

iOS bu İlerleme göstergesi, uygulamanızda sağlamak için iki ana yol sağlar: etkinlik göstergeleri (belirli bir de dahil olmak üzere _ağ_ etkinliği göstergesini) ve ilerleme çubukları.

## <a name="activity-indicator"></a>Etkinlik göstergesi

Etkinlik göstergeleri uygulamanızın uzun bir işlem çalışıyor, ancak görev gerektirir tam süreyi tanımadığınız gösterilmesi gerekir.

Apple etkinlik göstergeleri ile çalışmak için aşağıdaki önerileri vardır:

- **Mümkün olduğunda, ilerleme çubukları kullanmanız** - bir etkinliği göstergesini çalıştırılan işlem ne kadar sürer, konusunda hiçbir geri bildirim kullanıcı verdiğinden uzunluğu olduğunu biliyorsanız (örneğin, bir dosyayı indirmek için kaç tane bayt) her zaman bir ilerleme çubuğu kullanın.
- **Gösterge animasyonlu tutmak** -kullanıcılar, gösterilen sırada animasyonlu göstergesi her zaman gerekir böylece bu sabit bir etkinliği göstergesini sızması bir uygulama için ilgili.
- **İşlenmekte olan görev açıklamak** -yalnızca etkinliği göstergesini tek başına görüntüleme, değilse yeterli, bunlar bekletme işlemi hakkında bilgi sahibi olmak kullanıcı gerekir. Görev açıkça tanımlayan anlamlı bir etiket (genellikle tek, tam bir cümle) içerir.

### <a name="implementing-an-activity-indicator"></a>Bir etkinliği göstergesini uygulama

Bir etkinliği göstergesini aracılığıyla uygulanır [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) belirtmek için sınıf bir `UIActivity` yer alıyor.

### <a name="activity-indicators-and-storyboards"></a>Etkinlik göstergeleri ve film şeritleri

UI oluşturmak için iOS Tasarımcısı kullanıyorsanız, etkinliği göstergesini Araç Kutusu'ndan düzeninizde eklenebilir. Aşağıdaki özellikler özellikler panelinden ayarlanabilir:

![Özellikler paneli](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Etkinlik göstergesi davranışı yönetme

Kullanım `StartAnimating()` ve `StopAnimating()` etkinliği göstergesini animasyon durdurmak ve başlatmak için yöntemleri.

Ayarlama `HidesWhenStopped` özelliğine `true` sonra kayboluyor etkinliği göstergesini yapmak için `StopAnimating()` çağrıldı. Bu ayar `true` varsayılan olarak. Denetleyerek etkinliği göstergesini dönen animasyonunun çalışıp çalışmadığını görebilirsiniz herhangi bir noktada `IsAnimating` özelliği. 


### <a name="managing-activity-indicator-appearances"></a>Etkinlik göstergesi görünümleri yönetme

`UIActivityIndicatorViewStyle` Numaralandırma etkinliği göstergesini başlatılırken bir parametre olarak geçirilebilir. Bu görsel stil ayarlamak için kullanabileceğiniz `Gray`, `White`, veya `WhiteLarge`, örneğin:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Tarafından sağlanan renk kılabilirsiniz `UIActivityIndicatorViewStyle` ayarlayarak `Color` özelliği.

## <a name="progress-bar"></a>İlerleme Çubuğu

Durum ve zaman alıcı bir görev uzunluğunu belirtmek rengiyle doldurur bir satır olarak bir ilerleme çubuğu gösterir. İlerleme çubukları, görevleri uzunluğu olduğunu veya hesaplanan değer her zaman kullanılmalıdır.

Apple ilerleme çubukları ile çalışmak için aşağıdaki önerileri vardır:

- **Doğru şekilde rapor ilerleme** -ilerleme çubukları her zaman bir görevi tamamlamak için gereken süreyi doğru bir gösterimi olmalıdır. Meşgul görünen uygulama yapmak için gereken süre hiçbir zaman çalıştırmasını sağlamak yanlış tanıtılır.
- **Kullanım Well-Defined süreleri için** -ilerleme çubuğu yalnızca gösterme uzun bir iş sürüyor yerleştirin, ancak kullanıcı ve görevin ne kadarının tamamlandıktan göstergesi ve zaman kalan tahmini verin.

### <a name="implementing-an-progress-bar"></a>Bir ilerleme çubuğu uygulama

Bir ilerleme çubuğu örneği tarafından oluşturulan bir [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>İlerleme çubukları ve film şeritleri

UI Tasarımcısı iOS kullanırken bir ilerleme çubuğu ekleyebilirsiniz. Arama **ilerleme durumunu görüntüle** içinde **araç** ve görünümünüze sürükleyin.

Aşağıdaki özellikler özellikleri defterinde ayarlanabilir:

![Özellikler paneli](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>İlerleme çubuğu davranışı yönetme

İlerleme çubuğunun başlangıçta kullanılarak ayarlanabilir `Progress` özelliği:

```csharp
ProgressBar.Progress = 0f;
```

İlerleme kullanılarak ayarlanabilir `SetProgress` yöntemi ile bir Boole değeri veya animasyonlu değişiklik isteyip istemediğinizi bildirme geçirme.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

İlerleme çubuğu kullanma hakkında daha fazla bilgi için bkz [ilerleme raporlama](https://developer.xamarin.com/recipes/cross-platform/networking/download_progress/#Reporting_Progress_in_iOS) tarif ve [UICatalog tvOS örnek](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/).

### <a name="managing-progress-bar-appearance"></a>İlerleme çubuğu görünümü yönetme

Bir etkinliği göstergesini benzer `UIProgressViewStyle` numaralandırma ilerleme çubuğu başlatılırken bir parametre olarak geçirilebilir.

İlerleme durumu ve izleme görüntü ve renk aşağıdaki özellikler kullanılarak ayarlanabilir:

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



