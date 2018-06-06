---
title: Xamarin sorun giderme Profil Oluşturucu
description: Bu belge için Xamarin Profil Oluşturucu ile ilgili sorun giderme bilgileri sağlar. Günlüğe kaydetme ve Tanılama, IDE ve diğer konular ilgili sorunlar açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: 247a18cf7f645ea90d100cb3f4900f30ac7754cc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793863"
---
# <a name="xamarin-profiler-troubleshooting"></a>Xamarin sorun giderme Profil Oluşturucu

## <a name="logging-and-diagnostics"></a>Günlüğe kaydetme ve tanılama

Xamarin takım bize bilgilerle sağlarsanız, sorunları izlemenize yardımcı olması de dahil olmak üzere:

- Sorun, kilitlenme, veya hata ve buna baştaki akışınızı ekran kaydı.
- Günlük çıkışları (aşağıya bakın).
- **.Mlpd** profil oluşturma oturumu (aşağıya bakın) oluşturuluyor.

### <a name="getting-log-outputs"></a>Günlük çıkışları alma

Mac için günlükleri kaydedilir `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`.

Windows bu kaydedilir `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` bir sorun gönderme her Lütfen en son günlük ekleyin.

Bu çıktı büyür ve zaman içinde daha kullanışlı hale böylece biz, Git gibi daha fazla günlük eklemiş.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>.Mlpd dosyalar oluşturma

Bir **.mlpd** mono çalışma zamanı profil oluşturucu çıktısı sıkıştırılmış bir dosyadır. Xamarin profil oluşturucu GUI veri okuyan bir **.mlpd** ve kullanıcı için görüntüler. **.mlpd** dosyaları çünkü Profil Oluşturucu ile verilerinizi sahip sorunları tanılamak bizim mühendisleri yardımcı hata ayıklama için Xamarin araçları kullanışlıdır.

**.Mlpd** geçerli oturumu otomatik olarak Mac içinde 's kaydedilir `/tmp` dizini ve zaman damgası tarafından belirlenebilir. Günlük kaydını etkinleştirirseniz, ilk çıkış yolu olacaktır **.mlpd** dosya. **.Mlpd** dosya ~/var/klasörlerin başlangıç dizininde normal şekilde kaydedilecek...

**.Mlpd** geçerli bir oturum seçerek de kaydedilebilir için **Dosya > Kaydet...** Profil Oluşturucu'nın menüsünden:

**Mac için Visual Studio**:

![](troubleshooting-images/image17.png "Mac için Visual Studio .mlpd dosyasını kaydetme")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Visual Studio'da .mlpd dosyası kaydediliyor")

Bu dikkate almak önemlidir **.mlpd** çok sayıda bilgi içerir ve dosya boyutu büyük olacaktır.

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki liste, ortak FRİKİKLERİNDEN, geçici çözümler, ipuçları ve profil oluşturucu kullanmak için püf noktaları gösterir.

> [!NOTE]
> **Not**: Visual Studio olmasına gerek **Kurumsal** abone Mac için bu özelliği Windows ya da Visual Studio Enterprise ya da Visual Studio kilidini açmak için

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>İOS profil oluşturucu seçeneği göremiyorum veya [Visual Studio ve Mac için Visual Studio] devre dışı

Bu sorunu çözmek için aşağıdaki ayarları kontrol edin:

- Hata ayıklama yapılandırmasını kullandığınızdan emin olun
- SGen atık toplayıcı kullandığınızdan emin olun.
- Platform olun [desteklenen](~/tools/profiler/index.md#Profiler_Support).
- Doğru lisans olduğundan emin olun.
- Oturum kimliği doğrulanmış olarak ve doğru emin olun.
- [Visual Studio] Kullanıyor olmanız gerekir [Visual Studio Enterprise](https://www.visualstudio.com/vs/enterprise/) ve geçerli bir Enterprise lisansınız.

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>Profil Oluşturucu başlatmaya çalışırken bir hata alıyorsunuz

Visual Studio profil oluşturucu kullanırken, bu hata kutusuna çalıştırırsanız:

![](troubleshooting-images/error.png "Profil Oluşturucu Visual Studio'da kullanırken hata kutusu")

Normalde Simulator başlatamıyor olması nedeniyle olur / öykünücüsü. Deneyin ve uygulama normal olarak çalışır, verir ve profil oluşturucu yeniden kullanmayı deneyin sorunları giderin.

#### <a name="to-watch-a-specific-thread"></a>Belirli bir iş parçacığı izlemek için

Özellikle izlemek istediğiniz bir iş parçacığı varsa, get almak bir çok kendi oluşturulmasını başlayarak, iş parçacığı adı ideal olacaktır `ThreadName` yerine `0x0`. Örneğin, kullanıcı Arabirimi iş parçacığı adı ayarlama için aşağıdaki kodu kullanabilirsiniz:

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>İlgili bağlantılar

- [İzlenecek yol - Xamarin profil oluşturucu kullanma](~/tools/profiler/index.md)
- [Bellek ve performans en iyi uygulamalar](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Sürüm Notları](https://developer.xamarin.com/releases/profiler/preview/)
