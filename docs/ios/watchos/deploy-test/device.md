---
title: Apple Watch cihazlarda test etme
description: Bu belgede üzerinde gerçek bir Apple Watch test etmek için Xamarin ile oluşturulan bir watchOS uygulamasının nasıl dağıtılacağını açıklar. Sağlama profilleri, test, aygıtlar, açıklar ve bazı sorun giderme ipuçları sağlar.
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a960d81d41ff127fa3316e6190dfbf4881305c02
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790867"
---
# <a name="testing-on-apple-watch-devices"></a>Apple Watch cihazlarda test etme

İzledikten sonra [dağıtım adımları](~/ios/watchos/deploy-test/index.md) (gerekliyse) uygulama kimlikleri ve uygulama grupları oluşturmak için bu sayfadaki yönergeleri kullanın:

- [Kurulum aygıtlarınızı](#devices) Apple Dev Center'da ve
- [Geliştirme sağlama profilleri oluşturmak](#profiles), ardından
- [Dağıtma ve test](#testing) bir Apple Watch üzerinde.

<a name="devices" />

## <a name="devices"></a>Cihazlar

Gerçek iPhone veya iPad iOS uygulamalarını test etme geliştirme Merkezi'nde kaydedilmesi için cihazın her zaman gereklidir. Aygıt listesi şöyle (artı işaretini tıklatın **+** yeni aygıt eklemek için):

![](device-images/devices-sml.png "Aygıt listesi şöyle")

Gözcü farklı - şimdi uygulamaları dağıtmadan önce Apple Watch Cihazınızı eklemeniz gerekir. İzleme'nin UDID kullanarak bulun **Xcode** (**Windows > aygıtları** listesi). Eşleştirilmiş telefon bağlı olduğunda izleme ait bilgileri de görüntülenir:

[![](device-images/xcode-devices-sml.png "Eşleştirilmiş Gözcü bilgileri")](device-images/xcode-devices.png#lightbox)

İzleme'nin bildiğinizde UDID, Geliştirme Merkezi aygıt listesinde ekleyin:

![](device-images/devices-watch-sml.png "İzleme'nin aygıt listesinde UDID")

İzleme Aygıt eklendikten sonra herhangi bir yeni veya var olan geliştirme veya geçici sağlama profilleri oluşturduğunuz seçili olduğundan emin olun:

![](device-images/devices-provisioning.png "Kullanılabilir cihaz listesi")

Karşıdan yükle ve yeniden yüklemek için varolan bir sağlama profili düzenlerseniz unutmayın!

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>Sağlama profilleri geliştirme

Oluşturmanız gerekir, Cihazınızda test etmek için oluşturmak için bir **geliştirme sağlama profili** çözümünüzdeki her uygulama kimliği.

Joker karakter uygulama kimliği varsa *yalnızca bir sağlama profili gerekli olacaktır*; ancak her proje için ayrı bir uygulama Kimliğine sahip sonra her uygulama kimliği için bir sağlama profili gerekir:

![](device-images/provisioningprofile-development.png "Geliştirme sağlama profili")

Üç profil oluşturduktan sonra bunlar listede görüntülenir. Her biri yükleyip unutmayın:

![](device-images/provisioningprofiles.png "Kullanılabilir geliştirme sağlama profilleri")

Sağlama profilinde doğrulayabilirsiniz **proje seçenekleri** seçerek **Yapı > iOS paket imzalama** ekran ve seçerek **sürüm** veya **Debug iPhone** yapılandırma.

**Sağlama profili** listesini tüm eşleşen Profilleri Göster - Bu aşağı açılan listesinde oluşturduğunuz eşleşen profili görmeniz gerekir:

![](device-images/options-selectprofile.png "Sağlama profili listesi")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>Gözcü aygıtta test etme

Cihaz, uygulama kimlikleri ve sağlama profilleri yapılandırdıktan sonra test etmek hazır olursunuz.

1. İPhone Cihazınızı takılı olduğundan ve izleme iPhone ile zaten eşleştirilmiş emin olun.

2. Yapılandırma ayarlandığından emin olun **sürüm** veya **hata ayıklama**.

3. Bağlı iPhone aygıt hedef listesinde seçili olduğundan emin olun.

4. İOS uygulama projede (izleme veya uzantısı değil) sağ tıklatın ve seçin **başlangıç projesi olarak ayarla**.

5. Tıklatın **çalıştırmak** düğmesi (veya seçin bir **Başlat** gelen seçeneği **çalıştırmak** menüsü).

6. Çözümü derlemek ve iOS uygulaması için iPhone dağıtılır.
  İOS uygulaması veya izleme uzantısı sağlama doğru ayarlanmamışsa iPhone dağıtım başarısız olacaktır.

7. Dağıtım başarıyla tamamlarsa, iPhone otomatik olarak izleme uygulama eşleştirilmiş saate göndermeye çalışır. Uygulama simgesi bir döngüsel izleme ekranla görünür *yükleme* İlerleme göstergesi.

8. İzleme uygulaması başarıyla yüklendiyse, simgeyi kalacak izleme ekranda - uygulamanızı test etme başlayacak şekilde touch!


## <a name="troubleshooting"></a>Sorun giderme

Dağıtım kullanım sırasında bir hata oluşursa, **Görünüm > klavye takımı > cihaz günlük** hata hakkında daha fazla bilgi için. Bazı hatalar ve bunların nedenleri aşağıda listelenmiştir:

### <a name="error-mt3001-could-not-aot-the-assembly"></a>Hata MT3001: AOT derlemesi yüklenemedi

Bir Apple Watch aygıta dağıtmak için hata ayıklama modunda oluştururken bu durum ortaya çıkabilir.

İçin *geçici olarak* çözüm bu sorunu çözmek için devre dışı **artımlı derlemeler** izleme uzantısı'nda **proje Seçenekleri > Yapı > watchOS yapı** penceresi:

[![](device-images/disable-incremental-sml.png "Artımlı derlemeler onay kutusu")](device-images/disable-incremental.png#lightbox)

Bu, daha sonra artımlı derlemeler daha hızlı derleme süreleri yararlanmak için yeniden etkin hale getirilebilir bir sonraki sürümde düzeltilecektir.


### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>İzleme uygulama cihaz üzerinde hata ayıklama sırasında başlatılamıyor

Fiziksel bir cihaza, yalnızca simgesi & yükleme değiştiricisi izleme uygulamanın hata ayıklama girişiminde görüntülenirken (ve sonunda zaman aşımı). Bu, sonraki sürümlerde ele alınacaktır; geçici bir çözüm (hangi hata ayıklama izin vermez) yayın derlemesi çalıştırmaktır.


### <a name="invalid-application-executable-or-application-verification-failed"></a>Geçersiz uygulama yürütülebilir dosya veya uygulama doğrulaması başarısız oldu

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "Geçersiz uygulama yürütülebilir dosyanın Uyarısı")

Bu iletiler görünüyorsa *izleme ekranında* uygulama yüklemeye çalıştı sonra birkaç sorunları olabilir:

- Apple Dev Center üzerinde bir cihaz olarak izleme cihaz eklenmedi. Yönergeleri izleyerek [aygıtları doğru şekilde yapılandırmanız](#devices).

- Test etmek için kullanılan geliştirme sağlama profilleri dahil izleme cihaz yok; veya izleme yeniden yüklü yeniden indirilir ve doğru sağlama profilleri eklendikten sonra. Yönergeleri izleyerek [sağlama profilleri doğru şekilde yapılandırmanız](#profiles).

- Varsa **iOS aygıtı günlük** içeren `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` sonra izleme uygulamanın **Info.plist** yanlış sahip **MinimumOSVersion** değeri.
  Bu olmalıdır **8.2** - Xcode el ile kaynak Düzenle gerekebilir 6.3 yüklediyseniz Ekle 8.2 için ayarlanabilir.

- Gözcü uygulamanın **Entitlements.plist** yanlış bir yetkilendirme etkinleştirilmiş (uygulama grupları gibi) sahip olmamalıdır.

- Gözcü uygulamanın **uygulama kimliği** sahip olmamalıdır geliştirme Merkezi'nde sahiptir (uygulama grupları gibi) etkin bir yetkilendirme yanlış.



### <a name="install-never-finished"></a>Asla tamamlanan yükleme

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

Bu hata gereksiz (ve geçersiz) anahtarları İzle uygulamasının gösterebilir **Info.plist** dosya. İOS uygulaması ya da izleme uzantısı izleme uygulama amacı anahtarları içermemesi gerekir.

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>"bağlanmak hata ayıklayıcı bekleniyor"

Varsa **uygulama çıktısı** penceresi takılmış gösterme

```csharp
waiting for debugger to connect
```

Projenizde dahil NuGets hiçbirini üzerinde bir bağımlılığa sahip olmadığını denetleme **Microsoft.Bcl.Build**. Bu, Microsoft yayımlanan kitaplıkları popüler dahil olmak üzere bazı otomatik olarak eklenir [Microsoft Http istemci kitaplıkları](http://www.nuget.org/packages/Microsoft.Net.Http/).

**Microsoft.Bcl.Build.targets** eklenen dosya **.csproj** iOS Uzantıları Paketleme dağıtımı sırasında ile engelleyebilir. İzleyebilirsiniz [hata](https://bugzilla.xamarin.com/show_bug.cgi?id=29912).
.Csproj dosyasını düzenleyin ve el ile taşımak için olası geçici bir çözüm değildir **Microsoft.Bcl.Build.targets** son öğe olmalıdır.

