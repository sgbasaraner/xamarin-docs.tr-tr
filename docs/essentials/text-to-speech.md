---
title: 'Xamarin.Essentials: metin okuma'
description: Bir uygulamayı kullanmasına yerleşik geri metin aygıttan ve ayrıca altyapısı destekleyebilir sorgu kullanılabilir diller konuşma metin okuma altyapılarındaki Xamarin.Essentials etkinleştirir TextToSpeech sınıfta.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 9383411074bc43af1034138aadbb6ac5494c2c01
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782807"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: metin okuma

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**TextToSpeech** sınıfı geri metin aygıttan ve ayrıca altyapısı destekleyebilir sorgu kullanılabilir diller konuşma metin okuma altyapılarındaki yerleşik kullanmak bir uygulama sağlar.

## <a name="using-text-to-speech"></a>Metin okuma kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Metin okuma işlevselliği çalışır çağırarak `SpeakAsync` yöntemiyle metin ve isteğe bağlı parametreler ve utterance tamamlandıktan sonra döndürür. 

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

Bu yöntem isteğe bağlı bir CancellationToken başladıktan sonra utterance durdurmak için alır. 
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

Metin okuma, iş parçacığı konuşma isteklerinden otomatik olarak sıraya koyar. 

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

Ses nasıl konuşulan üzerinde daha fazla denetim için yedekleme ile `SpeakSettings` izin veren birim, aralık ve yerel ayar ayarlama.

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
| Sıklık | 0 | 2,0 |
| Birim | 0 | 1.0 |

### <a name="speech-locales"></a>Konuşma yerel ayarlar

Her platform geri metinde birden çok dil ve Vurgu ile iletişim kurması için yerel ayarlar sağlar. Her platform farklı kodları ve olmasının nedeni, belirtmenin yolları sahip bir platformlar Essentials sağlar `Locale` sınıfı ve bunlarla sorgulamak için bir yol `GetLocalesAsync`.

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

- Çoklu iş parçacıkları arasında çağrıldıklarında utterance sıra garanti edilmez.
- Ses çalma arka plan resmi olarak desteklenmez.

## <a name="api"></a>API

- [TextToSpeech kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [TextToSpeech API belgeleri](xref:Xamarin.Essentials.TextToSpeech)
