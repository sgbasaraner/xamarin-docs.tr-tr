---
title: "İzlenecek yol - yerel bildirimler Xamarin.Android içinde kullanma"
description: "Bu kılavuzda yerel bildirimlerinin Xamarin.Android uygulamaları nasıl kullanılacağını gösterir. Oluşturma ve yerel bir bildirim yayımlama temellerini gösterir. Kullanıcı bildirim alanında bildirim tıkladığında, ikinci bir faaliyeti başlatır."
ms.topic: article
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/30/2018
ms.openlocfilehash: 4728b50446033c02d33ccf8273f1dc2e50d66906
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>İzlenecek yol - yerel bildirimler Xamarin.Android içinde kullanma

_Bu kılavuzda yerel bildirimlerinin Xamarin.Android uygulamaları nasıl kullanılacağını gösterir. Oluşturma ve yerel bir bildirim yayımlama temellerini gösterir. Kullanıcı bildirim alanında bildirim tıkladığında, ikinci bir faaliyeti başlatır._

<a name="overview" />

## <a name="overview"></a>Genel Bakış

Bu kılavuzda, kullanıcı bir etkinlikte düğmesine tıkladığında, bir bildirim oluşturan bir Android uygulaması oluşturacağız. Kullanıcı bildirimi tıkladığında, kullanıcı ilk etkinliğini düğmesini tıklattıktan sayısını görüntüler ikinci bir etkinlik başlatır.

Aşağıdaki ekran görüntüleri, bu uygulamanın bazı örnekler göstermektedir:

[![Bildirim örnek ekran görüntüleri](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png)


<a name="walkthrough" />

## <a name="walkthrough"></a>İzlenecek yol

Başlamak için kullanarak yeni bir Android projesi oluşturalım **Android uygulaması** şablonu. Şimdi bu projenin adı **LocalNotifications**. (Xamarin.Android projeleri oluşturma ile bilmiyorsanız bkz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)

<a name="add-v4-support" />

### <a name="add-the-androidsupportv4app-component"></a>Android.Support.V4.App Bileşen Ekle

Bu kılavuzda kullanıyoruz `NotificationCompat.Builder` bizim yerel bildirim oluşturmak için. Açıklandığı gibi [yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications.md), biz içermelidir [Android destek kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) kullanmak için NuGet Projemizin içinde `NotificationCompat.Builder`.

Ardından, düzenleyelim **MainActivity.cs** ve aşağıdakileri ekleyin `using` deyimi böylece türler `Android.Support.V4.App` bizim kod için kullanılabilir:

```csharp
using Android.Support.V4.App;
```

Ayrıca, bu kullanıyoruz derleyici temizleyin vermiyoruz gerekir `Android.Support.V4.App` sürümü `TaskStackBuilder` yerine `Android.App` sürümü. Aşağıdakileri ekleyin `using` deyimi herhangi karışıklığı çözmek için:

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

<a name="define-id" />

### <a name="define-the-notification-id"></a>Bildirim kimliği tanımlayın

Biz bizim bildirim için benzersiz bir kimliği gerekir. Düzenleyelim **MainActivity.cs** ve aşağıdaki statik örnek değişkeni ekleyin `MainActivity` sınıfı:

```csharp
private static readonly int ButtonClickNotificationId = 1000;
```

<a name="add-code" />

### <a name="add-code-to-generate-the-notification"></a>Bildirim oluşturmak için kodu ekleyin

Ardından, düğme için yeni bir olay işleyicisi oluşturmak ihtiyacımız `Click` olay. Aşağıdaki yöntemi ekleyin `MainActivity`:

```csharp
private void ButtonOnClick (object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    Bundle valuesForActivity = new Bundle();
    valuesForActivity.PutInt ("count", count);

    // When the user clicks the notification, SecondActivity will start up.
    Intent resultIntent = new Intent(this, typeof (SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras (valuesForActivity);

    // Construct a back stack for cross-task navigation:
    TaskStackBuilder stackBuilder = TaskStackBuilder.Create (this);
    stackBuilder.AddParentStack (Java.Lang.Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent (resultIntent);

    // Create the PendingIntent with the back stack:            
    PendingIntent resultPendingIntent =
        stackBuilder.GetPendingIntent (0, (int)PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
        .SetAutoCancel (true)                    // Dismiss from the notif. area when clicked
        .SetContentIntent (resultPendingIntent)  // Start 2nd activity when the intent is clicked.
        .SetContentTitle ("Button Clicked")      // Set its title
        .SetNumber (count)                       // Display the count in the Content Info
        .SetSmallIcon(Resource.Drawable.ic_stat_button_click)  // Display this icon
        .SetContentText (String.Format(
            "The button has been clicked {0} times.", count)); // The message to display.

    // Finally, publish the notification:
    NotificationManager notificationManager =
        (NotificationManager)GetSystemService(Context.NotificationService);
    notificationManager.Notify(ButtonClickNotificationId, builder.Build());

    // Increment the button press count:
    count++;
}
```

İçinde `OnCreate` yöntemi, bu Ata `ButtonOnClick` yönteme `Click` düğmesi (şablon tarafından sağlanan temsilci olay işleyicisi değiştirin) olay:

```csharp
button.Click += ButtonOnClick;
```

<a name="second-activity" />

### <a name="create-a-second-activity"></a>İkinci bir etkinlik oluşturmak

Şimdi biz kullanıcı bizim bildirim tıkladığında, Android görüntüler başka bir etkinliğin oluşturmanız gerekir. Adlı projenize başka bir Android etkinlik eklemek **SecondActivity**. Açık **SecondActivity.cs** ve içeriğini bu kod ile değiştirin:

```csharp
using System;
using Android.App;
using Android.OS;
using Android.Widget;

namespace LocalNotifications
{
    [Activity(Label = "Second Activity")]
    public class SecondActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Get the count value passed to us from MainActivity:
            int count = Intent.Extras.GetInt ("count", -1);

            // No count was passed? Then just return.
            if (count <= 0)
                return;

            // Display the count sent from the first activity:
            SetContentView (Resource.Layout.Second);
            TextView textView = FindViewById<TextView>(Resource.Id.textView);
            textView.Text = String.Format (
                "You clicked the button {0} times in the previous activity.", count);
        }
    }
}
```

Biz de kaynak düzeni oluşturmalısınız **SecondActivity**. Yeni bir ekleme **Android düzeni** dosya adlı projenize **Second.axml**. Düzen **Second.axml** ve aşağıdaki düzen kodu yapıştırın:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text=""
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textView" />
</LinearLayout>
```

<a name="add-icon" />

### <a name="add-a-notification-icon"></a>Bir bildirim simgesi ekleme

Son olarak, bildirim alanında bizim bildirim başlatıldığında görünecek küçük bir simge ekleyelim. Kopyalayabilirsiniz [bu simgeyi](local-notifications-walkthrough-images/ic-stat-button-click.png) projenize veya kendi özel simge oluşturun. Simge dosyası adı **ŞA\_stat\_düğmesini\_click.png** ve kopyalayın **kaynakları/drawable** klasör. Kullanmayı unutmayın **Ekle > varolan öğeyi...**  bu simge dosyası projenize eklemek için.

<a name="run-app" />

### <a name="run-the-application"></a>Uygulamayı çalıştırın

Şimdi oluşturun ve uygulamayı çalıştırın. Aşağıdaki ekran görüntüsüne benzer ilk etkinliği ile sunulan:

[ ![İlk etkinlik ekran görüntüsü](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png)

Düğmesini gibi bildirim alanında görüntülenen bildirim küçük simgesi dikkat edin:

[ ![Bildirim simgesi görüntülenir](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png)

Aşağıya doğru çekin ve bildirim çekmecesini kullanıma, bildirim görmeniz gerekir:

[ ![Bildirim iletisi](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png)

Bildirime tıklayın, onu kaybolur ve bizim diğer etkinliklerin başlatılması gerekir &ndash; aşağıdaki ekran görüntüsüne benzer bir şey aranıyor:

[ ![İkinci etkinlik ekran görüntüsü](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png)

Tebrikler! Bu noktada, Android yerel bildirim Kılavuzu tamamladıktan ve başvurabilirsiniz bir çalışma örneği vardır. Bildirimleri daha fazla biz burada gösterilenden bu nedenle daha fazla bilgi istiyorsanız ele göz atın çok [Google belgelerine bildirimleri](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) ve Android [bildirimleri](http://developer.android.com/design/patterns/notifications.html) Tasarım Kılavuzu.


<a name="summary" />

## <a name="summary"></a>Özet

Bu kılavuzda kullandık `NotificationCompat.Builder` oluşturma ve bildirimleri görüntüleme. İkinci bir etkinlik bildirimi ile kullanıcı etkileşimi yanıt için bir yol olarak başlatmak nasıl basit bir örneği gördüğümüz ve biz verilerin aktarımını ilk etkinliğinden ikinci etkinlik gösterilmektedir.


## <a name="related-links"></a>İlgili bağlantılar

- [LocalNotifications (örnek)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Tasarım Kılavuzu modelleri bildirimleri](http://developer.android.com/design/patterns/notifications.html)
- [bildirim](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
