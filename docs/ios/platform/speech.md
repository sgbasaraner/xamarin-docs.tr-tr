---
title: Konuşma tanıma
description: Bu makalede yeni konuşma API sunar ve sürekli konuşma tanıma desteği ve konuşma (gelen Canlı veya kaydedilmiş ses akışları) metne transcribe bir Xamarin.iOS uygulaması uygulama gösterilmektedir.
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: fa010f57d163cabe544176608cff2eb6efe872ad
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="speech-recognition"></a>Konuşma tanıma

_Bu makalede yeni konuşma API sunar ve sürekli konuşma tanıma desteği ve konuşma (gelen Canlı veya kaydedilmiş ses akışları) metne transcribe bir Xamarin.iOS uygulaması uygulama gösterilmektedir._

Yeni iOS 10, Apple sahip serbest konuşma tanıma (gelen Canlı veya kaydedilmiş ses akışları) konuşma transcribe ve sürekli konuşma tanıma desteklemek bir iOS uygulaması metne veren bir API.

Apple göre konuşma tanıma API'si aşağıdaki özellikler ve avantajları vardır:

- Yüksek oranda doğru
- Harikası
- Kullanımı kolaydır
- Hızlı
- Birden çok dili destekler
- Gizliliğinize kullanıcı gizliliği

## <a name="how-speech-recognition-works"></a>Konuşma tanıma nasıl çalışır?

Konuşma tanıma bir iOS uygulaması Canlı veya önceden kaydedilmiş ses (herhangi bir API'yi destekleyip konuşulan dile) alınırken ve onu bir düz metin transcription konuşulan sözcüklerin döndüren bir konuşma tanıyıcısı tarafından uygulanır.

[![](speech-images/speech01.png "Konuşma tanıma nasıl çalışır?")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>Klavye yazdırma

Çoğu kullanıcı bir iOS cihazında konuşma tanıma düşünürken, bunların yanı sıra klavye dikte iOS 5 iPhone 4S ile yayımlanan yerleşik Siri ses yardımcısının düşünün.

Klavye dikte TextKit destekleyen arabirimi öğesi tarafından desteklenen (gibi `UITextField` veya `UITextArea`) ve (doğrudan için Ara çubuğuna solunda) iOS sanal klavye dikte düğmesini tıklatarak kullanıcı tarafından etkinleştirilir.

Apple (bu yana 2011 toplanan) aşağıdaki klavye dikte istatistikleri yayımladı.

- İOS 5 yayımlandıktan sonra klavye dikte yaygın olarak kullanılmaktadır.
- Yaklaşık 65.000 uygulamaları günde kullanır.
- Tüm iOS üçüncü bir hakkında dikte 3 taraf uygulamada yapılır.

Klavye dikte kullanımı son derece kolay aynıdır Geliştirici Bölümü, uygulamanın kullanıcı Arabirimi tasarımında TextKit arabirimi öğesi kullanma dışındaki hiçbir çaba gerektirir. Klavye dikte de kullanılabilmesi için önce tüm özel ayrıcalık istekleri uygulamasından gerektirmeyen avantajı vardır.

Yeni konuşma tanıma API'leri kullanan uygulamalar konuşma tanıma iletim ve Apple'nın sunucularındaki verileri geçici olarak depolanmasını gerektirdiğinden kullanıcı tarafından verilmesi için özel izinleri gerektirir. Lütfen bakın bizim [güvenlik ve gizlilik geliştirmeler](~/ios/app-fundamentals/security-privacy.md) Ayrıntılar için belgelerine bakın.

Klavye dikte uygulamak kolay olsa da, bazı sınırlamalar ve dezavantajları getirir:

- Bir metin giriş alanı kullanımını ve klavye görüntüsünü gerektirir.
- Canlı ses yalnızca giriş çalışır ve uygulama ses kaydetme işlemi üzerinde denetimi yoktur.
- Kullanıcının konuşma bilgilerini yorumlamak için kullanılacak dil üzerinde hiçbir denetimi sağlar.
- Dikte düğmesi kullanıcılar için bile uygun olup olmadığını bilmek uygulama için bir yolu yoktur.
- Uygulama ses kaydetme işlemi özelleştiremezsiniz.
- Zamanlama ve güvenilirlik gibi bilgileri eksik çok basit sonuçları kümesi sağlar.

### <a name="speech-recognition-api"></a>Konuşma tanıma API'si

Yeni iOS 10, konuşma tanıma uygulamak bir iOS uygulaması için daha güçlü bir yol sağlayan konuşma tanıma API'si Apple yayımladı. Bu API aynı güç Siri ve klavye dikte için Apple kullanan ve hızlı transcription harikası doğruluk ile sağlama yeteneğine olur.

Konuşma tanıma API'si tarafından sağlanan sonuçlarla toplayabilir veya özel kullanıcı verilerini erişmek zorunda kalmadan uygulama şeffaf bir şekilde bireysel kullanıcılara özelleştirilir.

Konuşma tanıma API'si sonuçları tekrar arama uygulamasında yakın gerçek zamanlı yalnızca metinden çeviri sonuçlarıyla ilgili daha fazla bilgi sağlar ve kullanıcı konuşarak olarak sağlar. Bu güncelleştirmeler şunlardır:

- Hangi kullanıcının birden çok yorumlar belirtti.
- Tek tek çevirileri için güven düzeyleri.
- Zamanlama bilgileri.

Yukarıda belirtildiği gibi bir canlı akış tarafından veya önceden kaydedilmiş kaynağından ve 50'den diller ve 10 iOS tarafından desteklenen Portekizce hiçbirinde çeviri ses sağlanabilir.

Konuşma tanıma API'si iOS 10 çalıştıran herhangi bir iOS cihazında kullanılabilir ve çevirileri toplu Apple'nın sunucularda gerçekleştikten sonra çoğu durumda, canlı bir Internet bağlantısı gerektirir. Bununla bazı yeni iOS cihazları her zaman desteği, belirli diller aygıt çevrilmesi.

Apple belirli bir dile çevirme o anda için kullanılabilir olup olmadığını belirlemek için bir kullanılabilirlik API eklemiştir. Uygulama, internet bağlantısı için kendisini doğrudan test yerine bu API kullanmanız gerekir.

Yukarıdaki klavye dikte bölümünde belirtildiği gibi konuşma tanıma iletim ve Apple'nın sunucularındaki verileri geçici olarak depolanmasını internet üzerinden ve bu şekilde, uygulamayı gerektirir _gerekir_ kullanıcının gerçekleştirme izni iste ekleyerek tanıma `NSSpeechRecognitionUsageDescription` anahtarını kendi `Info.plist` dosyasını ve arama `SFSpeechRecognizer.RequestAuthorization` yöntemi. 

Konuşma tanıma için kullanılan ses kaynak bağlı olarak, diğer değişiklikler uygulamanın `Info.plist` dosya gerekli olabilir. Lütfen bakın bizim [güvenlik ve gizlilik geliştirmeler](~/ios/app-fundamentals/security-privacy.md) Ayrıntılar için belgelerine bakın.

## <a name="adopting-speech-recognition-in-an-app"></a>Konuşma tanıma bir uygulamada uygulamasını kullanma

Geliştirici bir iOS uygulaması konuşma tanıma benimsemek için gerçekleştirmeniz gereken dört ana adım vardır:

- Uygulamasının kullanım bir açıklama sağlayın `Info.plist` kullanarak dosya `NSSpeechRecognitionUsageDescription` anahtarı. Örneğin, kamerayı uygulama aşağıdaki açıklama içerebilir _"Bu fotoğrafı word söyleyerek 'Peynir' yararlanmanıza olanak sağlar."_
- Çağırarak yetkilendirme isteği `SFSpeechRecognizer.RequestAuthorization` bir açıklama sunmak için yöntemi (sağlanan `NSSpeechRecognitionUsageDescription` anahtar yukarıdaki) neden uygulama konuşma istediği tanıma kullanıcı bir iletişim kutusu için erişim ve izin vermeniz kabul edin veya reddedin.
- Bir konuşma tanıma isteği oluşturun:
    * Disk üzerinde önceden kaydedilen sesi için kullanmak `SFSpeechURLRecognitionRequest` sınıfı.
    * Canlı ses (veya ses bellekten) için kullanmak `SFSPeechAudioBufferRecognitionRequest` sınıfı.
- Konuşma tanıma isteği bir konuşma tanıyıcıya geçirin (`SFSpeechRecognizer`) tanıma başlamak için. Uygulama isteğe bağlı olarak döndürülen tutabilir `SFSpeechRecognitionTask` görüntülemek ve tanıma sonuçlarını izlemek için.

Bu adımlar aşağıda ayrıntılı olarak ele alınacaktır.

### <a name="providing-a-usage-description"></a>Kullanım açıklamasını sağlama

Gerekli sağlamak için `NSSpeechRecognitionUsageDescription` anahtarını `Info.plist` dosya, aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Çift `Info.plist` dosyayı düzenlemek için açın.
2. Geçiş **kaynak** görünümü: 

    [![](speech-images/speech02.png "Kaynak Görünümü")](speech-images/speech02.png#lightbox)
3. Tıklayın **yeni giriş Ekle**, girin `NSSpeechRecognitionUsageDescription` için **özelliği**, `String` için **türü** ve **kullanım açıklama** olarak **değeri**. Örneğin: 

    [![](speech-images/speech03.png "NSSpeechRecognitionUsageDescription ekleme")](speech-images/speech03.png#lightbox)
4. Uygulama Canlı ses transcription işleme, bu da mikrofon kullanım açıklamasını gerektirir. Tıklayın **yeni giriş Ekle**, girin `NSMicrophoneUsageDescription` için **özelliği**, `String` için **türü** ve **kullanım açıklama** olarak **değeri**. Örneğin: 

    [![](speech-images/speech04.png "NSMicrophoneUsageDescription ekleme")](speech-images/speech04.png#lightbox)
4. Değişiklikleri dosyaya kaydedin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çift `Info.plist` dosyayı düzenlemek için açın.
3. Tıklayın **yeni giriş Ekle**, girin `NSSpeechRecognitionUsageDescription` için **özelliği**, `String` için **türü** ve **kullanım açıklama** olarak **değeri**. Örneğin: 

    [![](speech-images/speech03w.png "NSSpeechRecognitionUsageDescription ekleme")](speech-images/speech03w.png#lightbox)
4. Uygulama Canlı ses transcription işleme, bu da mikrofon kullanım açıklamasını gerektirir. Tıklayın **yeni giriş Ekle**, girin `NSMicrophoneUsageDescription` için **özelliği**, `String` için **türü** ve **kullanım açıklama** olarak **değeri**. Örneğin: 

    [![](speech-images/speech04w.png "NSMicrophoneUsageDescription ekleme")](speech-images/speech04w.png#lightbox)
4. Değişiklikleri dosyaya kaydedin.

-----

> [!IMPORTANT]
> Yukarıdaki birini sağlamak başarısız olan `Info.plist` anahtarları (`NSSpeechRecognitionUsageDescription` veya `NSMicrophoneUsageDescription`) uyarmadan konuşma tanıma ya da Canlı ses mikrofon erişmeye çalışırken başarısız uygulama neden olabilir.




### <a name="requesting-authorization"></a>İstekte bulunan Yetkilendirme

Konuşma tanıma erişim yazmasına izin verir. gerekli kullanıcı yetkilendirme isteği için ana görünüm denetleyicisi sınıfı düzenleyin ve aşağıdaki kodu ekleyin:

```csharp
using System;
using UIKit;
using Speech;

namespace MonkeyTalk
{
    public partial class ViewController : UIViewController
    {
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Request user authorization
            SFSpeechRecognizer.RequestAuthorization ((SFSpeechRecognizerAuthorizationStatus status) => {
                // Take action based on status
                switch (status) {
                case SFSpeechRecognizerAuthorizationStatus.Authorized:
                    // User has approved speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Denied:
                    // User has declined speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.NotDetermined:
                    // Waiting on approval
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Restricted:
                    // The device is not permitted
                    ...
                    break;
                }
            });
        }
    }
}
```

`RequestAuthorization` Yöntemi `SFSpeechRecognizer` sınıfı isteğine izin kullanıcıdan Geliştirici sağlanan neden kullanarak erişim konuşma tanıma `NSSpeechRecognitionUsageDescription` , anahtar `Info.plist` dosya.

A `SFSpeechRecognizerAuthorizationStatus` sonuç döndürülür `RequestAuthorization` eyleme geçmek için kullanılan yöntemin geri arama yordamı dayalı kullanıcının izni. 

> [!IMPORTANT]
> Kullanıcının bu izne istemeden önce Konuşma tanıma gerektiren uygulamadaki bir eylem başlatıldı kadar bekleyen Apple önerir.

### <a name="recognizing-pre-recorded-speech"></a>Önceden kaydedilmiş konuşma tanıma

Önceden kaydedilmiş WAV veya MP3 dosyasından konuşma tanımak uygulama istiyorsa, aşağıdaki kodu kullanabilirsiniz:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
...

public void RecognizeFile (NSUrl url)
{
    // Access new recognizer
    var recognizer = new SFSpeechRecognizer ();

    // Is the default language supported?
    if (recognizer == null) {
        // No, return to caller
        return;
    }

    // Is recognition available?
    if (!recognizer.Available) {
        // No, return to caller
        return;
    }

    // Create recognition task and start recognition
    var request = new SFSpeechUrlRecognitionRequest (url);
    recognizer.GetRecognitionTask (request, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said, \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}
```

Bu kod ayrıntılı bakarak, ilk olarak, bu Konuşma tanıyıcı oluşturmaya çalışır (`SFSpeechRecognizer`). Konuşma tanıma için varsayılan dili desteklenmiyorsa `null` döndürülür ve işlevleri çıkar.

Konuşma tanıyıcı için varsayılan dil kullanılabilir ise, uygulama tanıma kullanmak için şu anda mevcut olup olmadığını görmek için denetler `Available` özelliği. Örneğin, tanıma cihazda etkin bir Internet bağlantısı yoksa kullanılamayabilir.

A `SFSpeechUrlRecognitionRequest` oluşturulur `NSUrl` iOS cihazını ve önceden kaydedilmiş dosyasının konumunun karmalayan geri arama yordamı ile işlemek için konuşma Ekle.

Ne zaman geri çağırma çağrıldığında, `NSError` değil `null` ele alınması gereken bir hata oluştu. Konuşma tanıma artımlı olarak yapıldığından, geri arama yordamı daha sonra bu nedenle daha fazla çağrılabilir `SFSpeechRecognitionResult.Final` çeviri tamamlandıktan ve en iyi sürümü çeviri yazılır görmek için özellik test (`BestTranscription`).

### <a name="recognizing-live-speech"></a>Canlı konuşma tanıma

Canlı konuşma tanımak uygulama istiyorsa, önceden kaydedilmiş konuşma tanıma için çok benzer bir işlemdir. Örneğin:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
using AVFoundation;
...

#region Private Variables
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
#endregion
...

public void StartRecording ()
{
    // Setup audio session
    var node = AudioEngine.InputNode;
    var recordingFormat = node.GetBusOutputFormat (0);
    node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
        // Append buffer to recognition request
        LiveSpeechRequest.Append (buffer);
    });

    // Start recording
    AudioEngine.Prepare ();
    NSError error;
    AudioEngine.StartAndReturnError (out error);

    // Did recording start?
    if (error != null) {
        // Handle error and return
        ...
        return;
    }

    // Start recognition
    RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}

public void StopRecording ()
{
    AudioEngine.Stop ();
    LiveSpeechRequest.EndAudio ();
}

public void CancelRecording ()
{
    AudioEngine.Stop ();
    RecognitionTask.Cancel ();
}
```

Bu kod ayrıntılı bakarak, tanıma işlemi işlemek için çeşitli özel değişkenler oluşturur:

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

AV Foundation geçirilecektir ses kaydetmek için kullandığı bir `SFSpeechAudioBufferRecognitionRequest` tanıma isteği işlemek için:

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

Uygulama kaydı başlatmak çalışır ve kayıt başlattıysanız hatalarını ele alınır:

```csharp
AudioEngine.Prepare ();
NSError error;
AudioEngine.StartAndReturnError (out error);

// Did recording start?
if (error != null) {
    // Handle error and return
    ...
    return;
}
```

Tanıma görev başlatılır ve bir tanıtıcı tanıma görevi tutulur (`SFSpeechRecognitionTask`):

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

Geri arama, yukarıda üzerinde önceden kaydedilmiş konuşma kullanılan bir benzer bir şekilde kullanılır.

Kaydı kullanıcı tarafından durdu, Ses altyapısı ve konuşma tanıma isteği bildirilmeden:

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

Kullanıcı tanıma iptal ederse, Ses altyapısı ve tanıma görev bildirilir:

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

Çağrı önemlidir `RecognitionTask.Cancel` hem belleği hem de cihazın işlemci boşaltmak için çeviri kullanıcı iptal ederse.

> [!IMPORTANT]
> Sağlamak başarısız olan `NSSpeechRecognitionUsageDescription` veya `NSMicrophoneUsageDescription` `Info.plist` anahtarları uyarmadan konuşma tanıma ya da Canlı ses mikrofon erişmeye çalışırken başarısız uygulama neden olur (`var node = AudioEngine.InputNode;`). Lütfen bakın **kullanım tanımı sağlayarak** daha fazla bilgi için bölüm üstünde.

## <a name="speech-recognition-limits"></a>Konuşma tanıma sınırları

Apple konuşma tanıma ile bir iOS uygulamasına çalışırken aşağıdaki kısıtlamaları getirir:

- Konuşma tanıma tüm uygulamalar için ücretsiz ancak kullanım sınırsız değil:
    - Tek tek iOS cihazları günde gerçekleştirilebilir teşekkürler sınırlı sayıda var.
    - Uygulamaları genel bir istek başına günlük temelinde kısıtlanacak.
- Uygulama konuşma tanıma ağ bağlantısı ve kullanım oranı sınırı hataları işlemek için hazırlanması gerekir.
- Konuşma tanıma pil boşaltma ve yüksek ağ trafiği yüksek maliyet kullanıcının iOS cihazında, bu nedenle olabilir, Apple yaklaşık bir dakika konuşma max katı ses süre sınırı uygular.

Bir uygulamayı düzenli olarak hızı azaltma sınırlarına basarsa, Apple Geliştirici bunları başvurun sorar.

## <a name="privacy-and-usability-considerations"></a>Gizlilik ve kullanılabilirlik konuları

Apple saydam ve bir iOS uygulaması konuşma tanıma birlikte kullanıldığında kullanıcı gizlilik saygı göstermek için aşağıdaki önerisi vardır:

- Kullanıcının konuşma kaydederken kaydı uygulamanın kullanıcı arabiriminde yer aldığı açıkça belirtmek emin olun. Örneğin, uygulama bir bir "kaydetme" ses çalma ve kayıt göstergesi görüntüler.
- Konuşma tanıma parolalar, sistem durumu verileri ya da mali bilgileri gibi hassas kullanıcı bilgileri için kullanmayın.
- Tanıma sonucu göster _önce_ bunlar üzerinde çalışan. Bu, yalnızca ne uygulama yapıyor, ancak bunlar gerçekleştirilmediğinden tanıma hataları işlemek kullanıcı verir için geri bildirim sağlar.

## <a name="summary"></a>Özet

Bu makalede, yeni konuşma API sunulan ve sürekli konuşma tanıma desteği ve konuşma (gelen Canlı veya kaydedilmiş ses akışları) metne transcribe bir Xamarin.iOS uygulaması olarak uygulamak nasıl oluşturulacağını gösterir. 



## <a name="related-links"></a>İlgili bağlantılar

- [SpeakToMe (örnek)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
