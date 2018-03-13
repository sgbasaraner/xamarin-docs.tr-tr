---
title: "Android ışını"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: e9936bb523db8ba8777df94a03bf12f9fa718fca
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="android-beam"></a>Android ışını

Android ışını uygulamaların yakınında olduğunda NFC üzerinden bilgi paylaşmanızı sağlayan Android 4'teki yeni bir yakın alan iletişimi (NFC) teknolojisidir.

[![İki bilgi paylaşımı yakın cihazlar gösteren diyagram](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android ışını iki cihazlar aralık içinde olduğunda NFC iletiler göndererek çalışır. Aygıtları yaklaşık 4cm birbirinden Android ışını kullanarak veri paylaşabilir. Bir etkinlik bir cihaz üzerinde bir ileti oluşturur ve bir etkinlik (veya etkinlikler) belirtir, itme işleyebilir. Belirtilen etkinlik ön planda ve cihazların kapsama alanında olduğunda Android ışını ikinci cihaza ileti gönderme. Alıcı aygıtta ileti verileri içeren bir hedefi çağrılır.

Android ayarı iletilerinin Android ışını ile iki şekilde destekler:

-   `SetNdefPushMessage` -Android ışını başlatılmadan önce bir uygulama NFC ve onu Ftp'den etkinlik üzerinden göndermek için bir NdefMessage belirtmek için SetNdefPushMessage çağırabilirsiniz. Bu düzenek en iyi uygulama kullanımdayken bir ileti değiştirmez kullanılır.

-   `SetNdefPushMessageCallback` -Android ışını başlatıldığında, bir uygulama bir NdefMessage oluşturmak için bir geri çağırma işleyebilir. İleti oluşturma işleminin aygıtları aralığındadır kaldıktan sonra Bu mekanizma sağlar. İletinin nerede değişebilir senaryoları uygulamada neler olduğunu üzerine dayalı destekler.


Her iki durumda da, Android ışını ile veri göndermek için uygulamanın gönderdiği bir `NdefMessage`, çeşitli veri paketleme `NdefRecords`. Biz Android ışını tetiklemeden önce ele alınması gereken önemli noktaları bir göz atalım. İlk olarak, biz oluşturma geri çağırma stiliyle karşılaşmayacağınızı bir `NdefMessage`.


## <a name="creating-a-message"></a>Bir ileti oluşturma

Geri aramalar ile kayıt yaptırabilirsiniz bir `NfcAdapter` etkinliğin içinde `OnCreate` yöntemi. Örneğin, varsayarak bir `NfcAdapter` adlı `mNfcAdapter` bildirilen bir sınıf değişken etkinlik olarak şu iletiyi oluşturmak geri çağırma oluşturmak için aşağıdaki kodu yazabilirsiniz:

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

Etkinlik uygulayan `NfcAdapter.ICreateNdefMessageCallback`, geçirilir `SetNdefPushMessageCallback` yukarıdaki yöntemi. Android ışını başlattığında, sistem çağıracak `CreateNdefMessage`, hangi etkinlik oluşturabileceğiniz gelen bir `NdefMessage` aşağıda gösterildiği gibi:

```csharp
public NdefMessage CreateNdefMessage (NfcEvent evt)
{
    DateTime time = DateTime.Now;
    var text = ("Beam me up!\n\n" + "Beam Time: " +
        time.ToString ("HH:mm:ss"));
    NdefMessage msg = new NdefMessage (
        new NdefRecord[]{ CreateMimeRecord (
            "application/com.example.android.beam",
            Encoding.UTF8.GetBytes (text)) });
        } };
    return msg;
}

public NdefRecord CreateMimeRecord (String mimeType, byte [] payload)
{
    byte [] mimeBytes = Encoding.UTF8.GetBytes (mimeType);
    NdefRecord mimeRecord = new NdefRecord (
        NdefRecord.TnfMimeMedia, mimeBytes, new byte [0], payload);
    return mimeRecord;
}
```


## <a name="receiving-a-message"></a>Bir ileti alma

Alıcı tarafında amacına sahip sistem çağırır `ActionNdefDiscovered` içinden biz ayıklayabileceği NdefMessage gibi eylem:

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Aşağıdaki ekran görüntüsünde, çalışan gösterilen Android ışını kullanan tam kod örneği için bkz: [Android ışını demo](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/) örnek galerisinde.

[![Android ışını demo gelen örnek ekran görüntüleri](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>İlgili bağlantılar

- [Android ışını Demo (örnek)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
