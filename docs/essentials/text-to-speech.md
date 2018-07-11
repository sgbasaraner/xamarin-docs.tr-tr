---
title: 'Xamarin.Essentials: metin okuma'
description: Bir uygulamayı kullanan yerleşik cihaz hem de altyapısını destekleyen bir sorgu kullanılabilir diller için geri metni konuşmaya, metin okuma altyapılarındaki Xamarin.Essentials etkinleştirir TextToSpeech sınıfı.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 9383411074bc43af1034138aadbb6ac5494c2c01
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815667"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: metin okuma

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**TextToSpeech** sınıfı CİHAZDAN ve altyapısını destekleyen bir sorgu kullanılabilir diller için geri metni konuşmaya, metin okuma altyapılarındaki yerleşik kullanmak bir uygulama sağlar.

## <a name="using-text-to-speech"></a>Metinden konuşmaya özelliğini kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Metinden konuşmaya işlevini çağırarak çalışır `SpeakAsync` yöntemi metin ve isteğe bağlı parametreler utterance tamamlandıktan sonra döndürür. 

```csharp
public async Task SpeakNowDefaultSettings()
{
    await TextToSpeech.SpeakAsync("Hello World");

    // This method will block until utterance finishes.
}

public void SpeakNowDefaultSettings2()
{
    TextToSpeech.SpeakAsync("Hello World").ContinueWith((t) => 
    {
        // Logic that will run after utterance finishes.

    }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

Bu yöntem, başladıktan sonra utterance durdurmak için isteğe bağlı bir CancellationToken alır. 
```csharp
CancellationTokenSource cts;
public async Task SpeakNowDefaultSettings()
{
    cts = new CancellationTokenSource();
    await TextToSpeech.SpeakAsync("Hello World", cancelToken: cts.Token);

    // This method will block until utterance finishes.
}

public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? false)
        return;

    cts.Cancel();
}
```

Metin okuma, aynı iş parçacığından sesli istekleri otomatik olarak kuyruğa ekler. 

```csharp
bool isBusy = false;
public void SpeakMultiple()
{
    isBusy = true;
    Task.Run(async () =>
    {
        await TextToSpeech.SpeakAsync("Hello World 1");
        await TextToSpeech.SpeakAsync("Hello World 2");
        await TextToSpeech.SpeakAsync("Hello World 3");
        isBusy = false;
    });

    // or you can query multiple without a Task:
    Task.WhenAll(
        TextToSpeech.SpeakAsync("Hello World 1"),
        TextToSpeech.SpeakAsync("Hello World 2"),
        TextToSpeech.SpeakAsync("Hello World 3"))
        .ContinueWith((t) => { isBusy = false; }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

### <a name="speech-settings"></a>Konuşma ayarları

Daha fazla denetime nasıl ses konuşmalar için yedekleme ile `SpeakSettings` sağlayan hacim, aralık ve yerel ayarı.

```csharp
public async Task SpeakNow()
{
    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

Bu parametreler için desteklenen değerler şunlardır:

| Parametre | Minimum | Maksimum |
| --- | :---: | :---: |
| Aralık | 0 | 2,0 |
| Birim | 0 | 1.0 |

### <a name="speech-locales"></a>Konuşma yerel ayarlar

Birden çok dil ve aksan geri metni konuşmaya yerel ayarlar her bir platform sunar. Her platform farklı kodlar ve olmasının nedeni, bu yoldan sahip Essentials, platformlar arası sağlar `Locale` sınıfı ve bunları sorgulamak için bir yol `GetLocalesAsync`.

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>Sınırlamalar

- Birden çok iş parçacığı üzerinde çağrılırsa utterance kuyruk garanti edilmez.
- Ses çalma arka plan resmi olarak desteklenmez.

## <a name="api"></a>API

- [TextToSpeech kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [TextToSpeech API belgeleri](xref:Xamarin.Essentials.TextToSpeech)
