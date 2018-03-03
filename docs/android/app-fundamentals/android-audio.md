---
title: Android ses
description: "Android işletim sistemi, ses ve video multimedya için kapsamlı destek sağlar. Bu kılavuz, Android ses odaklanır ve çalma ve yerleşik ses player ve Kaydedici sınıfları yanı sıra, düşük düzey ses API kullanarak ses kaydını kapsar. Böylece geliştiriciler iyi çalışan uygulamalar oluşturabilir diğer uygulamalar tarafından yayınlanan ses olaylarla çalışma da kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: ea3fd7d73f104f7b9650431a5531fe4399a2630c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="android-audio"></a>Android ses

_Android işletim sistemi, ses ve video multimedya için kapsamlı destek sağlar. Bu kılavuz, Android ses odaklanır ve çalma ve yerleşik ses player ve Kaydedici sınıfları yanı sıra, düşük düzey ses API kullanarak ses kaydını kapsar. Böylece geliştiriciler iyi çalışan uygulamalar oluşturabilir diğer uygulamalar tarafından yayınlanan ses olaylarla çalışma da kapsar._

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

Modern mobil cihazlar, önceden ayrılmış donanım parçaları için şunlar gerekir işlevselliği benimsenen &ndash; kameralar, müzik çalar ve video kaydediciler. Bu nedenle, bir mobil API'leri birinci sınıf bir özellik multimedya çerçeveleri hale geldi.

Android multimedya için kapsamlı destek sağlar. Bu makalede, Android ses çalışmak inceler ve aşağıdaki konuları kapsar

1.  **MediaPlayer sesi çalma** &ndash; yerleşik kullanarak `MediaPlayer` yerel ses dosyaları ve akış ses dosyaları ile de dahil olmak üzere, ses çalınmaya sınıfı `AudioTrack` sınıfı.

2.  **Ses kaydını** &ndash; yerleşik kullanarak `MediaRecorder` ses kaydetmek için sınıf.

3.  **Ses bildirimleri ile çalışma** &ndash; askıya alma veya kendi ses çıkış iptal etme (örneğin, gelen telefon aramasını) olayları için doğru bir şekilde yanıt iyi çalışan uygulamaları oluşturmak için ses bildirimleri kullanarak.

4.  **Alt düzey ses ile çalışma** &ndash; ses kullanarak çalma `AudioTrack` doğrudan bellek arabelleği yazarak sınıf. Ses kullanarak kaydetme `AudioRecord` sınıfı ve doğrudan bellek önbelleklerden okuma.


## <a name="requirements"></a>Gereksinimler

Bu kılavuz Android 2.0 (API düzeyi 5) gerektirir ya da daha yüksek. Android ses hata ayıklama bir aygıtta yapılmalıdır unutmayın.

İstek için gerekli olan `RECORD_AUDIO` izinleri **AndroidManifest.XML**:

![Gerekli Android bildirim kaydıyla izinleri kısmına\_etkin ses](android-audio-images/image01.png)


<a name="Playing_Audio_with_the_MediaPlayer_Class" />

## <a name="playing-audio-with-the-mediaplayer-class"></a>MediaPlayer sınıfı ile ses çalma

Android ses çalınmaya en basit yolu ile yerleşik olan [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) sınıfı.
`MediaPlayer` Yerel veya uzak dosya, dosya yolunu geçirerek yürütebilirsiniz. Ancak, `MediaPlayer` çok durumu duyarlı ve yöntemlerinden birini durumu yanlış çağırma bir özel durum oluşturulmasına neden olur. Etkileşim önemlidir `MediaPlayer` hatalarını önlemek için aşağıda açıklanan sırayla.


<a name="Initializing_and_Playing" />

### <a name="initializing-and-playing"></a>Başlatma ve çalma

İle ses çalma `MediaPlayer` aşağıdaki sırayı gerektirir:

1. Yeni bir örneğini [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) nesnesi.

1. Aracılığıyla yürütmek için dosyayı yapılandırma [SetDataSource](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.SetDataSource/p/Java.IO.FileDescriptor/) yöntemi.

1. Çağrı [Prepare](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Prepare/) yöntemi player başlatılamadı.

1. Çağrı [Başlat](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start/) ses çalma başlatmak için yöntem.


Aşağıdaki kod örneği, bu kullanım gösterilmektedir:

```csharp
protected MediaPlayer player;
public void StartPlayer(String  filePath)
{
  if (player == null) {
    player = new MediaPlayer();
  } else {
    player.Reset();
    player.SetDataSource(filePath);
    player.Prepare();
    player.Start();
  }
}
```

<a name="Suspending_and_Resuming_Playback" />

### <a name="suspending-and-resuming-playback"></a>Askıya alma ve sürdürme kayıttan yürütme

Kayıttan yürütme çağırarak askıya alınabilir [duraklatma](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Pause%28%29/) yöntemi:

```csharp
player.Pause();
```

Duraklatılmış oynatmayı çağrısı [Başlat](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start%28%29/) yöntemi.
Bu kayıttan duraklatılmış konumdan devam eder:

```csharp
player.Start();
```

Çağırma [durdurmak](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Stop%28%29/) yöntemi player üzerinde devam eden bir Kayıttan Yürütme sona erer:

```csharp
player.Stop();
```

Player artık gerekli olmadığında kaynakları çağırarak serbest bırakılması gereken [sürüm](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Release%28%29/) yöntemi:

```csharp
player.Release();
```

<a name="Using_the_MediaRecorder_Class_to_Record_Audio" />


## <a name="using-the-mediarecorder-class-to-record-audio"></a>Kayıt ses MediaRecorder sınıfını kullanma

Corollary `MediaPlayer` Android kayıt ses için [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) sınıfı. Gibi `MediaPlayer`, durumu duyarlı olduğundan ve burada bu başlayabileceğini kayıt noktasına almak için birkaç durumları arasında geçiş. Ses, kaydı için `RECORD_AUDIO` izni ayarlanmış olması gerekir. Uygulama ayarlama hakkında yönergeler için bkz: izinleri [AndroidManifest.xml ile çalışma](~/android/platform/android-manifest.md).

<a name="Initializing_and_Recording" />

### <a name="initializing-and-recording"></a>Başlatma ve kaydetme

Ses kaydetme `MediaRecorder` aşağıdaki adımları gerektirir:

1. Yeni bir örneğini [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) nesnesi.

2. Ses giriş aracılığıyla yakalamak için kullanmak üzere hangi donanım aygıtı belirtin [SetAudioSource](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioSource/p/Android.Media.AudioSource/) yöntemi.

3. Çıkış dosyası ses biçimi kullanılarak ayarlanan [SetOutputFormat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFormat/p/Android.Media.OutputFormat/) yöntemi. Desteklenen ses türlerinin bir listesi için bkz: [Android desteklenen medya biçimleri](http://developer.android.com/guide/appendix/media-formats.html).

4. Çağrı [SetAudioEncoder](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioEncoder/p/Android.Media.AudioEncoder/) kodlama türü ses ayarlamak için yöntem.

5. Çağrı [SetOutputFile](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFile/p/System.String/) yöntemi ses verilerini yazılır çıkış dosyasının adını belirtin.

6. Çağrı [Prepare](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Prepare%28%29/) yöntemi Kaydedici başlatılamadı.

7. Çağrı [Başlat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Start%28%29/) kaydı başlatmak için yöntem.


Aşağıdaki kod örneği, bu sırası gösterilmektedir:

```csharp
protected MediaRecorder recorder;
void RecordAudio (String filePath)
{
  try {
    if (File.Exists (filePath)) {
      File.Delete (filePath);
    }
    if (recorder == null) {
      recorder = new MediaRecorder (); // Initial state.
    } else {
      recorder.Reset ();
      recorder.SetAudioSource (AudioSource.Mic);
      recorder.SetOutputFormat (OutputFormat.ThreeGpp);
      recorder.SetAudioEncoder (AudioEncoder.AmrNb);
      // Initialized state.
      recorder.SetOutputFile (filePath);
      // DataSourceConfigured state.
      recorder.Prepare (); // Prepared state
      recorder.Start (); // Recording state.
    }
  } catch (Exception ex) {
    Console.Out.WriteLine( ex.StackTrace);
  }
}
```

<a name="Stopping_recording" />

### <a name="stopping-recording"></a>Kaydı durdurma

Kaydı durdurmak için arama `Stop` yöntemi `MediaRecorder`:

```csharp
recorder.Stop();
```

<a name="Cleaning_up" />


### <a name="cleaning-up"></a>Temizleme

Bir kez `MediaRecorder` bırakıldı durduruldu, çağrı [sıfırlama](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Reset%28%29/) back boşta durumuna koymak istediğiniz yöntemi:

```csharp
recorder.Reset();
```

Zaman `MediaRecorder` olan artık gerekli kaynaklarını çağırarak serbest bırakılması gereken [sürüm](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Release%28%29/) yöntemi:

```csharp
recorder.Release();
```

<a name="Managing_Audio_Notifications" />

## <a name="managing-audio-notifications"></a>Sesli bildirimler yönetme

<a name="The_AudioManager_Class" />


### <a name="the-audiomanager-class"></a>AudioManager sınıfı

[AudioManager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/) sınıfı ses olayları olduğunda bilmeniz uygulamaların izin sesli bildirimler erişim sağlar. Bu hizmet ayrıca birim ve ses açma/kapama modu denetimi gibi diğer ses özelliklere erişim sağlar. `AudioManager` Ses çalma denetlemek için ses bildirimleri işlemek bir uygulama sağlar.

<a name="Managing_Audio_Focus" />


### <a name="managing-audio-focus"></a>Ses odak yönetme

(Yerleşik player ve Kaydedici) cihazın ses kaynakları tüm çalışan uygulamalar tarafından paylaşılır.

Kavramsal olarak, bu yalnızca bir uygulama klavye odağını bulunduğu bir masaüstü bilgisayar uygulamaları benzer: fare-tıklayarak çalışan uygulamalardan birini seçtikten sonra klavye girişi yalnızca bu uygulamaya gider.

Ses odak benzer bir yöntemdir ve çalma veya ses kaydını aynı anda birden fazla uygulama önler. Gönüllü olduğundan klavye odağını daha karmaşık &ndash; uygulama şu anda ses odağa sahip ve bakılmaksızın yürütmek, o olgu yoksayabilirsiniz &ndash; ve farklı türde olabilir ses odak olduğundan istedi. Örneğin, istek yalnızca çok kısa bir süre ses çalınmaya bekleniyorsa, geçici odak isteyebilir.

Ses odak hemen verilen veya başlangıçta reddedildi ve daha sonra verilen. Örneğin, bir uygulama isteklerini ses odak, bir telefon araması sırasında kabul edilmez, ancak telefon araması tamamlandıktan sonra odak iyi verilebilir. Bu durumda, bir dinleyici ses odak hemen alınmışsa uygun şekilde yanıt için kayıtlı değil. Ses odak isteyen olsun veya olmasın, Tamam yürütmek için ya da ses belirlemek için kullanılır.

Ses odak hakkında daha fazla bilgi için bkz: [yönetme ses odak](http://developer.android.com/training/managing-audio/audio-focus.html).


<a name="Registering_the_Callback_for_Audio_Focus" />

#### <a name="registering-the-callback-for-audio-focus"></a>Geri arama için ses odak kaydetme

Kaydetme `FocusChangeListener` geri aramasından `IOnAudioChangeListener` alma ve ses odak serbest önemli bir parçasıdır. Bu durum, ses odağını verme daha sonraki bir zamana kadar ertelenmesi çünkü. Örneğin, bir uygulama, devam eden bir telefon araması sırasında müzik isteyebilir. Telefon araması tamamlanana kadar ses odak izni yok.

Bu nedenle, geri çağırma nesnesi bir parametre olarak geçirilen `GetAudioFocus` yöntemi `AudioManager`, ve geri çağırma kaydeder bu çağrıdır. Ses odak başlangıçta reddedildi ancak daha sonra verilen, uygulama çağırarak bilgilendirilir `OnAudioFocusChange` üzerinde geri arama. Aynı yöntem uygulamaya ses odak yerine geçen olduğunu bildirmek için kullanılır.

Ses kaynakları kullanarak uygulama sona erdiğinde, çağıran `AbandonFocus` yöntemi `AudioManager`ve yeniden geri geçirir. Bu geri çağırma deregisters ve böylece diğer uygulamaları ses odak edinebilirsiniz ses kaynakları serbest bırakır.


<a name="Requesting_Audio_Focus" />

#### <a name="requesting-audio-focus"></a>Ses odak isteme

İstek cihazın ses kaynakları için gerekli adımlar aşağıdaki gibidir:

1.  Bir tanıtıcı elde etmek `AudioManager` sistem hizmeti.

2.  Geri çağırma sınıfının bir örneğini oluşturun.

3.  İstek cihazın ses kaynakları çağırarak `RequestAudioFocus` yöntemi `AudioManager` . Geri çağırma nesnesi, akış türü (müzik, sesli arama, halka vb.) ve sağa istenen erişim türünü parametreleridir (ses kaynakları kısa bir süre içinde ya da belirsiz bir süre için örneğin istenmesi).

4.  İstek kabul edilirse `playMusic` yöntemi hemen çağrılır ve kayıttan yürütmek ses başlatır.

5.  İstek reddedilirse, başka hiçbir işlem yapılmadı. Bu durumda, ses, yalnızca daha sonraki bir zamanda istek verilirse yürütülür.


Aşağıdaki kod örneği, bu adımlar gösterilmektedir:

```csharp
Boolean RequestAudioResources(INotificationReceiver parent)
{
  AudioManager audioMan = (AudioManager) GetSystemService(Context.AudioService);
  AudioManager.IOnAudioFocusChangeListener listener  = new MyAudioListener(this);
  var ret = audioMan.RequestAudioFocus (listener, Stream.Music, AudioFocus.Gain );
  if (ret == AudioFocusRequest.Granted) {
    playMusic();
    return (true);
  } else if (ret == AudioFocusRequest.Failed) {
    return (false);
  }
  return (false);
}
```

<a name="Releasing_Audio_Focus" />

#### <a name="releasing-audio-focus"></a>Ses odak serbest bırakma

Parça kayıttan yürütme tamamlandığında, `AbandonFocus` yöntemi `AudioManager` çağrılır. Bu cihazın ses kaynaklara erişmek üzere başka bir uygulama sağlar. Kendi dinleyicileri kaydolduysanız diğer uygulamalar bu ses odak değişiklik gösteren bir bildirim alırsınız.

<a name="Low_Level_Audio_API" />

## <a name="low-level-audio-api"></a>Düşük düzey ses API'si

Alt düzey ses API'leri ses çalma ve dosya URI'ler kullanmak yerine doğrudan bellek arabelleği etkileşim olduğundan kaydı üzerinde daha fazla denetim sağlar. Bu yaklaşım tercih olduğu bazı senaryolar vardır. Bu tür senaryoları şunları içerir:

1.  Şifrelenmiş ses dosyalardan yürütürken.

2.  Art arda kısa kliplerinin yürütürken.

3.  Ses akış.


 <a name="AudioTrack_Class" />


### <a name="audiotrack-class"></a>AudioTrack sınıfı

[AudioTrack](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/) sınıf kayıt için alt düzey ses API'lerini kullanır ve alt düzey eşdeğerdir `MediaPlayer` sınıfı.

<a name="Initializing_and_Playing" />

#### <a name="initializing-and-playing"></a>Başlatma ve çalma

Ses, yeni bir örneğini yürütmek için `AudioTrack` örneğinin oluşturulması gerekir. Bağımsız değişken listesi içine geçirilen [Oluşturucusu](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/#memberlist) arabellekte bulunan ses örnek çalmak nasıl belirtir. Bağımsız değişkenleri şunlardır:

1.  Akış türü &ndash; Sesli Zil, müzik sistem veya uyarı.

2.  Sıklık &ndash; Hz ifade edilen örnekleme hızı.

3.  Kanal yapılandırmasını &ndash; Mono veya stereo.

4.  Ses biçimi &ndash; 8 bit veya 16 bit kodlama.

5.  Arabellek boyutu &ndash; bayt.

6.  Arabellek modu &ndash; akış veya statik.


Yapı sonra [Yürüt](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Play%28%29/) yöntemi `AudioTrack` başlangıç çalma kadar ayarlamak için çağrılır. Ses arabellek yazma `AudioTrack` kayıttan başlar:

```csharp
void PlayAudioTrack(byte[] audioBuffer)
{
  AudioTrack audioTrack = new AudioTrack(
    // Stream type
    Stream.Music,
    // Frequency
    11025,
    // Mono or stereo
    ChannelOut.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length,
    // Mode. Stream or static.
    AudioTrackMode.Stream);

    audioTrack.Play();
    audioTrack.Write(audioBuffer, 0, audioBuffer.Length);
}
```

<a name="Pausing_and_Stopping_the_Playback" />

#### <a name="pausing-and-stopping-the-playback"></a>Duraklatma ve oynatmayı durdurma

Çağrı [duraklatma](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Pause%28%29/) kayıttan duraklatmak için yöntemi:

```csharp
audioTrack.Pause();
```

Çağırma [durdurmak](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Stop%28%29/) yöntemi sonlandırılacak kayıttan kalıcı olarak:

```csharp
audioTrack.Stop();
```

<a name="Cleaning_up" />

#### <a name="cleanup"></a>Temizleme

Zaman `AudioTrack` olan artık gerekli kaynaklarını çağırarak serbest bırakılması gereken [sürüm](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Release%28%29/):

```csharp
audioTrack.Release();
```

<a name="The_AudioRecord_Class" />

### <a name="the-audiorecord-class"></a>AudioRecord sınıfı

[AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) sınıftır denk `AudioTrack` kaydı tarafında. Gibi `AudioTrack`, dosyaları ve URI'ler yerine bellek arabelleği doğrudan kullanır. Bunu gerektiren `RECORD_AUDIO` izni bildiriminde ayarlanabilir.

<a name="Initializing_and_Recording" />

#### <a name="initializing-and-recording"></a>Başlatma ve kaydetme

Yeni bir oluşturmak için ilk adımdır [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) nesnesi. Bağımsız değişken listesi içine geçirilen [Oluşturucusu](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/#memberlist) kayıt için gerekli tüm bilgileri sağlar. Aksine, `AudioTrack`, büyük ölçüde numaralandırmalar, eşdeğer değişkenlerinde bağımsız değişkenler burada `AudioRecord` tamsayı olduğunu. Bu güncelleştirmeler şunlardır:

1.  Mikrofon gibi ses giriş kaynağı donanım.

2.  Akış türü &ndash; Sesli Zil, müzik sistem veya uyarı.

3.  Sıklık &ndash; Hz ifade edilen örnekleme hızı.

4.  Kanal yapılandırmasını &ndash; Mono veya stereo.

5.  Ses biçimi &ndash; 8 bit veya 16 bit kodlama.

6.  Arabellek boyutu, bayt


Bir kez `AudioRecord` oluşturulur, kendi [Startrecordıng](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.StartRecording%28%29/) yöntemi çağrılır. Artık, kaydetmeye başlamak hazırdır. `AudioRecord` Sürekli girişi için ses arabelleğini okur ve bu giriş çıkışı bir ses dosyasına yazar.

```csharp
void RecordAudio()
{
  byte[] audioBuffer = new byte[100000];
  var audRecorder = new AudioRecord(
    // Hardware source of recording.
    AudioSource.Mic,
    // Frequency
    11025,
    // Mono or stereo
    ChannelIn.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length
  );
  audRecorder.StartRecording();
  while (true) {
    try
    {
      // Keep reading the buffer while there is audio input.
      audRecorder.Read(audioBuffer, 0, audioBuffer.Length);
      // Write out the audio file.
    } catch (Exception ex) {
      Console.Out.WriteLine(ex.Message);
      break;
    }
  }
}
```

<a name="Stopping_the_Recording" />

#### <a name="stopping-the-recording"></a>Kaydını durdurma

Çağırma [durdurmak](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Stop%28%29/) yöntemi sonlandırır kaydı:

```csharp
audRecorder.Stop();
```

<a name="Clean_Up" />

#### <a name="cleanup"></a>Temizleme

Zaman `AudioRecord` nesne artık gerekli, çağırma kendi [sürüm](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Release%28%29/) yöntemi ile ilişkili tüm kaynakları serbest bırakır:

```csharp
audRecorder.Release();
```

<a name="Summary" />

## <a name="summary"></a>Özet

Android işletim sistemi çalma, kaydetme ve ses yönetmek için güçlü bir çerçeve sağlar. Bu makalede nasıl yürütüleceğini ve üst düzey kullanarak kayıt ses kapsamdaki `MediaPlayer` ve `MediaRecorder` sınıfları. Ardından, sesli bildirimler farklı uygulamalar arasında cihazın ses kaynakları paylaşmak için nasıl kullanılacağını incelediniz. Son olarak, nasıl kayıttan yürütme ve alt düzey API'lerini kullanarak ses kaydetmek için doğrudan bellek arabelleği ile arabirim ile ele.


## <a name="related-links"></a>İlgili bağlantılar

- [İle çalışma ses (örnek)](https://developer.xamarin.com/samples/Example_WorkingWithAudio/)
- [Media Player](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)
- [Medya Kaydedicisi](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)
- [Audio Manager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)
- [Ses İzle](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)
- [Ses Kaydedicisi](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)
