---
title: "Android konuşma"
description: "Bu makalede çok güçlü Android.Speech ad alanını kullanarak temelleri kapsar. En başından itibaren Android Konuşma tanıması ve metin olarak çıkış mümkün olmuştur. Bu oldukça basit bir işlemdir. Konuşma altyapısı yalnızca göz önünde bulundurulması mevcut metin okuma için ancak, daha karmaşık bir işlemdir, ancak ayrıca diller metin okuma (TTS) sisteminden yüklü ve kullanılabilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 387294f4cfe7c706a57fadf6ad8a015fc7df2dfc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="android-speech"></a>Android konuşma

_Bu makalede çok güçlü Android.Speech ad alanını kullanarak temelleri kapsar. En başından itibaren Android Konuşma tanıması ve metin olarak çıkış mümkün olmuştur. Bu oldukça basit bir işlemdir. Konuşma altyapısı yalnızca göz önünde bulundurulması mevcut metin okuma için ancak, daha karmaşık bir işlemdir, ancak ayrıca diller metin okuma (TTS) sisteminden yüklü ve kullanılabilir._

## <a name="speech-overview"></a>Konuşma genel bakış

"İnsan konuşma anlar" ve ne yazılan enunciates bir sisteme sahip olmak — konuşma metin ve metin okuma — bizim aygıtlarla doğal iletişimi için isteğe bağlı miktarı artar mobil geliştirme içindeki sürekli büyüyen bir alanı aynıdır. Çok sayıda örneği vardır konuşma metin dönüştürür veya tersi, android uygulamanıza eklemenizi çok yararlı bir araçtır bir özellik'burada sahip.

Örneğin, ile yürüten sırasında cep telefonu kullanma clamp kullanıcılar cihazlarını işletme ellerini ücretsiz şekilde istiyorsunuz. Android farklı form faktörlerine sayısız — Android takmak gibi — ve bunlardan herhangi bir zamanda genişletme ekleme kullanın (örneğin, tabletler ve Not klavye takımı), Android cihazlar için oluşturduğu daha büyük bir odak harika TTS uygulamaları.

Google bir cihaz "konuşma haberdar" (örneğin, for the blind tasarlanmış yazılım) yapma çoğu örnekleri kapsayacak şekilde Geliştirici API'leri zengin bir dizi Android.Speech ad alanında sağlar.  Ad alanı konuşma içine Çevrilecek metin izin olanağı içerir `Android.Speech.Tts`, bir dizi yanı sıra çeviri yapmak için kullanılan altyapısı üzerinde denetim `RecognizerIntent`metne dönüştürülecek konuşma sağlayan s.

Tesis anlaşılması konuşma olmasına karşın, kullanılan donanım tabanlı sınırlamalar vardır. Cihaz başarıyla her şey için her dil kullanılabilir konuşulan yorumlar olduğunu düşüktür.

## <a name="requirements"></a>Gereksinimler

Cihazınızı mikrofon ve Konuşmacı sahip olma dışında bu kılavuzun özel gereksinimi yoktur.

Konuşma yorumlama bir Android cihaz çekirdek kullanımıdır bir `Intent` ilgili `OnActivityResult`.
Ancak, konuşma değil anlaşılır tanımak için önemlidir — ancak yorumlanan metin. Fark önemlidir.

### <a name="the-difference-between-understanding-and-interpreting"></a>Anlama ve yorumlama arasındaki fark

Bir basit anlama ton ve bağlam tarafından ne denirse, gerçek anlamını belirlemek mümkün tanımıdır. Yalnızca yorumlamaya sözcükleri alabilir ve başka bir biçimde çıktı anlamına gelir.

Her gün konuşmada kullanılan aşağıdaki basit örneği göz önünde bulundurun: 

<kbd>Merhaba nasılsın?</kbd>

Ton (belirli bir sözcük veya sözcük bölümlerini yerleştirilen Vurgu) bu basit bir sorunun olur. Ancak, yavaş hızınıza satırına uyguladıysanız, dinleme kişi onaylanan çok memnun değil ve belki de cheering gerekiyor veya onaylanan unwell olduğunu algılar. Vurgu girdiyseniz üzerinde "olan" isteyen kişi genellikle yanıtta daha ilgi.

Oldukça güçlü ses yapmak için işleme olmadan ton ve yapay Intelligence (AI) derecesini bağlamı anlamak için kullanmak, yazılımın ne denirse anlamak bile başlayamaz — basit bir telefon yapabilir en iyi metne konuşma Dönüştür.

## <a name="setting-up"></a>Ayarlama

Konuşma sistemi kullanmadan önce her zaman bir mikrofon aygıtın emin olmak için kontrol akıllıca olur. Uygulamanızı bir Kindle veya Google not defterinde yüklü mikrofon olmadan çalıştırmaya az noktası olacaktır.

Aşağıdaki kod örneği, mikrofon olup olmadığını ve değilse sorgulama gösterir bir uyarı oluşturmak için. Hiçbir mikrofon varsa bu noktada etkinlik çıkın ya da konuşma kaydetmek için özelliğini devre dışı.

```csharp
string rec = Android.Content.PM.PackageManager.FeatureMicrophone;
if (rec != "android.hardware.microphone")
{
    var alert = new AlertDialog.Builder(recButton.Context);
    alert.SetTitle("You don't seem to have a microphone to record with");
    alert.SetPositiveButton("OK", (sender, e) =>
    {
        return;
    });
    alert.Show();
}
```

### <a name="creating-the-intent"></a>Amacı oluşturma

Konuşma sistemi hedefini adlı hedefinin belirli bir tür kullanıyor `RecognizerIntent`. Çok sayıda kayıt algılar ve çıkış, diğer tüm diller, üzerinden kabul ile sessizlik beklemek ne kadar süreyle dahil parametreleri ve herhangi bir metin eklemek için bu amacı denetimleri `Intent`'s aracı yönerge olarak modal iletişim kutusu. Bu parçacığında bulunan `VOICE` olan bir `readonly int` tanıma için kullanılan `OnActivityResult`.

```csharp
var voiceIntent = new Intent(RecognizerIntent.ActionRecognizeSpeech);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguageModel, RecognizerIntent.LanguageModelFreeForm);
voiceIntent.PutExtra(RecognizerIntent.ExtraPrompt, Application.Context.GetString(Resource.String.messageSpeakNow));
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputPossiblyCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputMinimumLengthMillis, 15000);
voiceIntent.PutExtra(RecognizerIntent.ExtraMaxResults, 1);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguage, Java.Util.Locale.Default);
StartActivityForResult(voiceIntent, VOICE);
```

### <a name="conversion-of-the-speech"></a>Konuşma dönüştürülmesi

Konuşma yorumlanan metin içinde teslim `Intent`, etkinlik tamamlanana ve aracılığıyla erişilen döndürülen `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`. Bu döndürülecek bir `IList<string>`, hangi dizin kullanılır ve arayan hedefi içinde istenen dil sayısına bağlı olarak, görüntülenen (ve belirtilen `RecognizerIntent.ExtraMaxResults`). Herhangi bir listeyle yine de görüntülenecek verileri olduğundan emin olmak için kontrol değerinde olduğu gibi.

Dönüş değeri için dinlerken, bir `StartActivityForResult`, `OnActivityResult` yöntemi sağlanması gerekiyor.

Aşağıdaki örnekte `textBox` olan bir `TextBox` ne dikte çıktısı için kullanılır. Metin biçimi yorumlayıcı ve buradan geçirmek için eşit oranda kullanılabilir, uygulama metin ve başka bir uygulamanın parçası dala karşılaştırabilirsiniz.

```csharp
protected override void OnActivityResult(int requestCode, Result resultVal, Intent data)
{
    if (requestCode == VOICE)
    {
        if (resultVal == Result.Ok)
        {
            var matches = data.GetStringArrayListExtra(RecognizerIntent.ExtraResults);
             if (matches.Count != 0)
             {
                  string textInput = textBox.Text + matches[0];
                  textBox.Text = textInput;
                  switch(matches[0].Substring(0,5).ToLower())
                  {
                     case "north":
                      MovePlayer(0);
                     break;
                   case "south":
                     MovePlayer(1);
                     break;
             }
             else
                  textBox.Text = "No speech was recognised";
        }
   }
    base.OnActivityResult(requestCode, resultVal, data);
}
```

## <a name="text-to-speech"></a>Metin okuma

Metin okuma oldukça konuşma ters metne değil ve üzerinde iki temel bileşeni kullanır; bir metin okuma altyapısı cihazda yüklü ve bir dil yükleniyor.

Büyük ölçüde, Android cihazlar varsayılan gelen Google TTS hizmetinin yüklü ve en az bir dil. Cihaz önce ayarlanır ve cihaz zamanında olduğu temel oluşturulur (örneğin, Almanya ayarlanmış bir telefon Amerika birinde Amerikan İngilizcesi sahip olur ancak Almanca dil yükler).

### <a name="step-1---instantiating-texttospeech"></a>1. adım - TextToSpeech örnek oluşturma

`TextToSpeech` en fazla 3 parametre alabilir, ilk iki üçüncü ile gerekli olan isteğe bağlı olan (`AppContext`, `IOnInitListener`, `engine`). Dinleyici hizmeti ve test hatası için en az kullanılabilir Android metin okuma altyapılarının herhangi bir sayı olan altyapı ile bağlamak için kullanılır, cihaz Google kendi altyapısı sahip olur.

### <a name="step-2---finding-the-languages-available"></a>Adım 2 - kullanılabilir diller bulma

`Java.Util.Locale` Ad alanında adlı yardımcı bir yöntem `GetAvailableLocales()`. Bu konuşma altyapısı tarafından desteklenen dillerin listesi sonra yüklü dilleri karşı test edilebilir.

"Anladım" dillerin listesini oluşturmak için basit bir konudur. Ayrıca bir varsayılan dil (Dil Kullanıcı Ayarla bunlar ilk cihazlarını kurduğunuzda), her zaman olacak kadar bu örnekteki `List<string>` ilk parametre olarak "Varsayılan" sahipse, listenin geri kalanı sonucu bağlı olarak doldurulacaktır `textToSpeech.IsLanguageAvailable(locale)`.

```csharp
var langAvailable = new List<string>{ "Default" };
var localesAvailable = Java.Util.Locale.GetAvailableLocales().ToList();
foreach (var locale in localesAvailable)
{
    var res = textToSpeech.IsLanguageAvailable(locale);
    switch (res)
    {
        case LanguageAvailableResult.Available:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryVarAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
    }
}
langAvailable = langAvailable.OrderBy(t => t).Distinct().ToList();
```

### <a name="step-3---setting-the-speed-and-pitch"></a>Adım 3 - hızı ve sıklığı ayarlama

Android sağlar kullanıcıya, konuşma sesi değiştirilerek alter `SpeechRate` ve `Pitch` (hızı ve konuşma dilini oranını). Bu 0'dan 1'e her ikisi için de 1 olan "normal" konuşma gider.

### <a name="step-4---testing-and-loading-new-languages"></a>Adım 4 - sınama ve yeni dilleri yükleme

Bu kullanılarak gerçekleştirilen bir `Intent` içinde yorumlanmasını sonucunda `OnActivityResult`. Kullanılan konuşma metin örnek aksine `RecognizerIntent` olarak bir `PutExtra` parametresi `Intent`, hedefi kullanan yükleme bir `Action`.

Aşağıdaki kodu kullanarak Google yeni bir dil yüklemek mümkündür. Sonucu `Activity` dil gerekliyse ve onu olup olmadığını denetler indirme gerçekleşmesi istemde sonra dil yükler.

```csharp
var checkTTSIntent = new Intent();
checkTTSIntent.SetAction(TextToSpeech.Engine.ActionCheckTtsData);
StartActivityForResult(checkTTSIntent, NeedLang);
//
protected override void OnActivityResult(int req, Result res, Intent data)
{
    if (req == NeedLang)
    {
        var installTTS = new Intent();
        installTTS.SetAction(TextToSpeech.Engine.ActionInstallTtsData);
        StartActivity(installTTS);
    }
}
```

### <a name="step-5---the-ioninitlistener"></a>Adım 5 - IOnInitListener

Metin okuma, arabirim yöntemini dönüştürmek bir etkinlik için `OnInit` oluşturulması gerekir (Bu örnek bu oluşturma için belirtilen ikinci parametredir `TextToSpeech` sınıfı). Bu dinleyiciyi başlatır ve sonucu sınar.

Dinleyici için her ikisini de sınamalısınız `OperationResult.Success` ve `OperationResult.Failure` en az.
Aşağıdaki örnek, tam olarak bunu gösterir:

```csharp
void TextToSpeech.IOnInitListener.OnInit(OperationResult status)
{
    // if we get an error, default to the default language
    if (status == OperationResult.Error)
        textToSpeech.SetLanguage(Java.Util.Locale.Default);
    // if the listener is ok, set the lang
    if (status == OperationResult.Success)
        textToSpeech.SetLanguage(lang);
}
```

## <a name="summary"></a>Özet

Bu kılavuzda metin okuma ve konuşma metin ve nasıl içinde kendi uygulamalarınızı dahil olası yöntemleri dönüştürme temelleri inceledik. Her özel durum oluşturmuyor olsa da, artık konuşma nasıl yorumlanacağını, yeni dilleri yükleme ve uygulamalarınızı inclusivity artırmak nasıl temel bir anlayış olmalıdır.



## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Metin okuma (örnek)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [Konuşma metin (örnek)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Android.Speech ad alanı](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Android.Speech.Tts ad alanı](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
