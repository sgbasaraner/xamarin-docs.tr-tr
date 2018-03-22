---
title: "Etkileşimli çalışma kitapları"
description: "Eğitme, eğitim veya keşfetme denemeler için C# kod ile canlı belgeleri oluşturmak için çalışma kitaplarını kullanın."
ms.topic: article
ms.prod: xamarin
ms.assetid: B79E5DE9-5389-4691-9AA3-FF4336CE294E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 69c4b25e17c31d57701f99e84f6f686c65dc7028
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="interactive-workbooks"></a>Etkileşimli çalışma kitapları

_Eğitme, eğitim veya keşfetme denemeler için C# kod ile canlı belgeleri oluşturmak için çalışma kitaplarını kullanın._

IDE içinden ayrı bir tek başına uygulama olarak, çalışma kitaplarını kullanabilirsiniz.

Yeni bir çalışma kitabı oluşturmaya başlamak için çalışma kitaplarını uygulamayı çalıştırın. Bu henüz yüklemediyseniz ziyaret [yükleme](~/tools/workbooks/install.md#install) sayfası. Bir çalışma kitabı platformunuz belgenizi gerçek zamanlı görselleştirmek olanak tanıyan bir aracı uygulama için otomatik olarak bağlanacak tercih oluşturmak için istenir.

Çalışma kitapları uygulama zaten çalışıyorsa, göz atarak yeni bir belge oluşturabilirsiniz **Dosya > Yeni**.

Çalışma kitapları kaydedilir ve daha sonra yeniden uygulamasında açılır. Sonra bunları fikirleri göstermek için yeni API'leri keşfedin veya yeni kavram öğretmek başkalarıyla paylaşabilirsiniz.

## <a name="code-editing"></a>Kod düzenleme

Penceresi düzenleme kod kod tamamlama, sözdizimi renklendirmesi, satır içi Canlı tanılama ve çok satırlı deyimi desteği sağlar.

[ ![](workbook-images/inspector-0.6.0-repl-small.png "Penceresi düzenleme kod kod tamamlama, sözdizimi renklendirmesi, satır içi Canlı tanılama ve çok satırlı deyimi desteği sağlar")](workbook-images/inspector-0.6.0-repl.png#lightbox)

Xamarin çalışma kitaplarını kaydedilir bir `.workbook` CommonMark dosyası üst bazı meta veri dosyası (bkz [çalışma kitaplarını dosya türlerini](#workbooks-files-types) çalışma kitaplarını kaydedilme hakkında daha fazla bilgi).

### <a name="nuget-package-support"></a>NuGet paketi desteği

Birçok popüler NuGet paketleri doğrudan Xamarin çalışma kitapları desteklenir. Göz atarak paketler için arama yapabilirsiniz **Dosya > Paketi Ekle**. Bir paket ekleme otomatik olarak getirecek `#r` deyimleri hemen kullanmanıza olanak sağlayan paket derlemelere başvurma.

Paket referanslarını çalışma kitabını kaydettiğinizde bu başvuruları da kaydedilir. Çalışma kitabı başka bir kişiyle paylaşıyorsanız, başvurulan paketlerde otomatik olarak indirir.

Çalışma kitaplarında NuGet paketi desteğiyle bilinen bazı sınırlamalar vardır:

  * Yerel kitaplıkları yalnızca iOS desteklenir ve yalnızca yönetilen kitaplık ile bağlantılı.
  * Bağımlı paketler `.targets` dosyaları veya PowerShell komut dosyaları büyük olasılıkla başarısız olacak beklendiği şekilde çalışması.
  * Kaldırmak veya paket bağımlılığı değiştirmek için bir metin düzenleyicisiyle çalışma kitabının bildirimi düzenleyin. Uygun paket yönetimi üzerinde yoludur.

### <a name="xamarinforms-support"></a>Xamarin.Forms desteği

Çalışma kitabınızı Xamarin.Forms NuGet paketi başvurursanız, çalışma kitabını uygulama Xamarin.Forms temelli olacak şekilde, ana görünüm değiştirin. Aracılığıyla erişebilmesi için `Xamarin.Forms.Application.Current.MainPage`.

Görünüm denetçisi sekmesi ayrıca, düzenleri anlamanıza yardımcı olması için Xamarin.Forms görünüm hiyerarşisini gösteren özel desteğe sahiptir.

## <a name="rich-text-editing"></a>Zengin metin düzenleme

Aşağıda gösterildiği gibi dahil, zengin metin düzenleyicisi kullanarak kodunuzu geçici metnini düzenleyebilirsiniz:

![](workbook-images/inspector-0.6.2-editing.gif "Yerleşik Zengin Metin Düzenleyicisi'ni kullanarak kod çevresine metin düzenleme")

### <a name="markdown-authoring"></a>Markdown yazma

Çalışma kitabı yazarlar bazen doğrudan kendi sık kullanılan düzenleyicisiyle çalışma kitabının "kaynak" CommonMark düzenlemek daha kolay.

Ardından düzenlemek ve çalışma kitaplarını istemcideki, çalışma kitabını kaydedin, CommonMark metninizi yeniden biçimlendirilen unutmayın.

Lütfen nedeniyle CommonMark uzantısı çalışma kitabı dosyalarını YAML meta verilerde etkinleştirmek için kullandığımızı Not `---` bu amaç için ayrılmıştır. Oluşturmak istiyorsanız, [temaya sonları](http://spec.commonmark.org/0.27/#thematic-break) metninizi, kullanmanız gereken `***` veya `___` yerine. Bu tür sonları çalışma kitaplarını 1.2 ve daha önce sırasında bir hata nedeniyle kaçınılmalıdır.

### <a name="improvements-in-workbooks-13"></a>Çalışma kitapları 1.3 yenilikleri

Biz de biraz sunu geliştirmek için Markdown blok teklif sözdizimi genişletilmiş. Blok teklifiniz ilk karakteri olarak bir emoji ekleyerek, teklif arka plan rengini etkileyebilir:

- `> [!NOTE]
>' Not mavi arka plana sahip olarak kabul eder
- `> [!IMPORTANT]
>' sarı bir arka plan içeren bir uyarı olarak kabul eder
- `> [!WARNING]
>' red arka plan ile ilgili bir sorun olarak kabul eder

Çalışma kitabı belgesini üstbilgilerinde bağlayabilirsiniz. Biz gibi işlenen üstbilgi metni olan bağlantı Kimliğine sahip her üstbilgisi bağlayıcılarını oluştur:

1. Alt ortası başlığıdır.
1. Alfasayısal ve tire dışında tüm karakterleri kaldırılır.
1. Boşluklar, tireler ile değiştirilir.

"Önemli üstbilgisi" bir kimliğini alır gibi bir üstbilgi buna `important-header` ve bir bağlantı ekleyerek bağlanabilir `#important-header` çalışma kitabı.

## <a name="document-structure"></a>Belge yapısı

### <a name="cell"></a>Hücre

Yürütülebilir kod veya markdown temsil eden bir ayrık birimi içeriği. Kod hücresini en fazla dört alt bileşenden oluşur:

- Düzenleyici
  - Arabellek
- Derleyici tanılama
- Konsol çıkışı
- Yürütme sonuçları

### <a name="editor"></a>Düzenleyici

Bir hücrenin etkileşimli metin bileşeni. Kod hücreler için sözdizimi vurgulama, vb. ile gerçek kod düzenleyicisinde budur. Markdown hücreler için zengin metin içeriği düzenleyicisiyle bağlama duyarlı biçimlendirme ve araç yazma budur.

### <a name="buffer"></a>Arabellek
Bir düzenleyici gerçek metin içeriği.

### <a name="compiler-diagnostics"></a>Derleyici tanılama

Tüm tanılama kodu, yalnızca açık yürütme istendiğinde çizilir derlerken üretti. Hemen hücre Düzenleyicisi görüntülenir.

### <a name="console-output"></a>Konsol çıkışı

Standart dışı ya da standart hata hücre yürütülmesi sırasında yazılmış herhangi bir çıktı. Standart dışı siyah metin olarak işlenir ve standart hata kırmızı metin olarak işlenir.

### <a name="execution-results"></a>Yürütme sonuçları

Bir sonuç gerçekte yürütme tarafından üretilen sağlanan bir hücre için sonuçları zengin ve potansiyel olarak etkileşimli gösterimlerini başarılı derleme sırasında işlenir. Özel durumlar derleme yürütmenin sonucu olarak üretilir olduğundan bu bağlamda sonuçları olarak kabul edilir.

## <a name="workbooks-files-types"></a>Çalışma kitapları dosya türleri

### <a name="plain-files"></a>Düz dosya

Varsayılan olarak, bir çalışma kitabı bir düz metin olarak kaydeder `.workbook` CommonMark biçimli metin içeren dosya.

### <a name="packages"></a>Paketler

Bir çalışma kitabı paketi ile adlı bir dizindir `.workbook` uzantısı.
Bir dosya değilmiş gibi Mac'ın Finder ve Xamarin çalışma kitaplarını Aç iletişim ve son dosyalar menüsü, bu dizin tanınır.

Dizin içermelidir bir `index.workbook` Xamarin çalışma kitaplarında yüklenecek gerçek düz metin çalışma kitabının dosya. Dizin de gerekli kaynakları içerebilir `index.workbook`resimler veya diğer dosyalar dahil olmak üzere.

Düz metin, `.workbook` aynı dizininden kaynaklara başvuran dosya 0.99.3 çalışma kitaplarında açıldığında veya kaydedildiğinde, daha sonra onu dönüştürülecek bir `.workbook` paketi. Bu, hem Mac hem de Windows geçerlidir.

> [!NOTE]
> Windows kullanıcıları açılır `package.workbook\index.workbook` dosyasını doğrudan ancak Aksi takdirde paket aynı Mac gibi davranacak

### <a name="archives"></a>Arşivler

Çalışma kitabı paketleri, dizinler, olan Internet üzerinden kolayca dağıtmak zor olabilir. Çalışma kitabı arşivler çözümüdür. Çalışma kitabı arşiv adında bir çalışma kitabı zip sıkıştırılmış paketi olduğu `.workbook` uzantısı.

' De çalışma kitaplarını 1.1, bir çalışma kitabı paket kaydedilirken başlayarak, Kaydet iletişim bunun yerine bir arşivi olarak kaydetme seçeneği sunar. Çalışma kitapları 1.0 oluşturulurken veya arşivleri kaydetme yerleşik hiçbir yolu yoktu.

Çalışma kitapları 1. 0'da, çalışma kitabı arşiv açıldığında bir çalışma kitabı pakete saydam dönüştürüldü ve zip dosyası kesildi. Çalışma kitapları 1.1 zip dosyası kalır. Kullanıcı arşiv kaydettiğinde, yeni bir posta dosya ile değiştirilir.

Çalışma kitabı arşiv el ile bir çalışma kitabı paket sağ tıklayıp seçerek oluşturabileceğiniz **Sıkıştır** Mac üzerinde veya **göndermek için > sıkıştırılmış (daraltılmış) klasör** Windows. İçin zip dosyasını yeniden adlandırma bir `.workbook` dosya uzantısı. Bu, yalnızca çalışma kitabı paketlerle değil düz çalışma kitabı dosyalarını çalışır.

## <a name="related-links"></a>İlgili bağlantılar

- [Çalışma kitapları Hoş Geldiniz](https://developer.xamarin.com/workbooks/workbooks/getting-started/welcome.workbook)
