---
title: İzlenecek yol - Xamarin.Android içinde yerel bildirimleri kullanma
description: Bu yönerge, Xamarin.Android uygulamalarında yerel bildirimleri kullanma işlemini gösterir. Bu, oluşturma ve yerel bir bildirim yayımlama temellerini gösterir. Kullanıcı bildirim alanında bildirim tıkladığında, ikinci bir etkinliği başlatır.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/16/2018
ms.openlocfilehash: a2ca3755e3201263584447ba47ec36d2096386da
ms.sourcegitcommit: f9fb316344fc4dce2fdce0dde3c5f4c2e4a42d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "30766588"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>İzlenecek yol - Xamarin.Android içinde yerel bildirimleri kullanma

_Bu yönerge, Xamarin.Android uygulamalarında yerel bildirimleri kullanma işlemini gösterir. Bu, oluşturma ve yerel bir bildirim yayımlama temellerini gösterir. Kullanıcı bildirim alanında bildirim tıkladığında, ikinci bir etkinliği başlatır._


## <a name="overview"></a>Genel Bakış

Bu kılavuzda, kullanıcı bir etkinlik içinde bir düğmeyi tıkladığında bir uyarı oluşturan bir Android uygulaması oluşturacağız. Kullanıcı bildirimi tıkladığında, kullanıcı ilk etkinliğini düğmeye tıklamış sayısını görüntüler, ikinci bir etkinliği başlatır.

Aşağıdaki ekran görüntüleri, bu uygulamanın bazı örnekler göstermektedir:

[![Örnek ekran görüntüleri ile bildirim](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)

> [!NOTE]
> Bu kılavuzda odaklanır [NotificationCompat API'leri](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) gelen [Android desteği Kitaplığı](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Bu API'ler maksimum geriye dönük uyumluluk için Android 4.0 (API düzeyi 14) sağlayacaktır.

## <a name="creating-the-project"></a>Projeyi oluşturma

Başlamak için şimdi kullanarak yeni bir Android projesi oluşturma **Android uygulaması** şablonu. Bu proje adlandıralım **LocalNotifications**. (Xamarin.Android projeleri oluşturma ile ilgili bilgi sahibi değilseniz bkz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)

Kaynak dosyayı düzenlemek **values/Strings.xml** bildirim kanalı oluşturmak için zaman olduğunda, kullanılacak olan iki ek dize kaynaklarını içeren:


```xml
<?xml version="1.0" encoding="utf-8"?>

<resources>
  <string name="Hello">Hello World, Click Me!</string>
  <string name="ApplicationName">Notifications</string>

  <string name="channel_name">Local Notifications</string>
  <string name="channel_description">The count from MainActivity.</string>
</resources>
```

### <a name="add-the-androidsupportv4-nuget-package"></a>Android.Support.V4 NuGet paketi ekleme

Bu kılavuzda, kullanıyoruz `NotificationCompat.Builder` bizim yerel bir bildirim oluşturmak için. İçinde anlatıldığı gibi [yerel bildirimleri](~/android/app-fundamentals/notifications/local-notifications.md), içermesi gerekir [Android desteği kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) kullanılacak Projemizin Nuget'te `NotificationCompat.Builder`.

Ardından, düzenleyelim **MainActivity.cs** ve aşağıdakileri ekleyin `using` deyimi böylece türlerinde `Android.Support.V4.App` kodumuz için kullanılabilir:

```csharp
using Android.Support.V4.App;
```

Ayrıca, biz bunu kullanıyoruz derleyici Temizle yapmalısınız `Android.Support.V4.App` sürümünü `TaskStackBuilder` yerine `Android.App` sürümü. Aşağıdaki `using` deyimi herhangi belirsizliği çözmek için:

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

### <a name="create-the-notification-channel"></a>Bildirim kanalı oluşturmak

Ardından, bir yöntem ekleyin `MainActivity` , oluşturur bir bildirim kanalı (gerekirse):

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var name = Resources.GetString(Resource.String.channel_name);
    var description = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, name, NotificationImportance.Default)
                  {
                      Description = description
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Güncelleştirme `OnCreate` bu yeni bir yöntem çağırmak için yöntem:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();
}
```


### <a name="define-the-notification-id"></a>Bildirim kimliği tanımlayın

Benzersiz bir kimliği bizim bildirim ve bildirim kanalı için gerekir. Düzenleyelim **MainActivity.cs** ve aşağıdaki statik bir örnek değişkeni ekleyin `MainActivity` sınıfı:

```csharp
static readonly int NOTIFICATION_ID = 1000;
static readonly string CHANNEL_ID = "location_notification";
internal static readonly string COUNT_KEY = "count";
```

### <a name="add-code-to-generate-the-notification"></a>Bildirim oluşturmak için kod ekleyin

Ardından, düğme için yeni bir olay işleyicisi oluşturmak ihtiyacımız `Click` olay. Aşağıdaki yöntemi ekleyin `MainActivity`:

```csharp
void ButtonOnClick(object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    var valuesForActivity = new Bundle();
    valuesForActivity.PutInt(COUNT_KEY, count);

    // When the user clicks the notification, SecondActivity will start up.
    var resultIntent = new Intent(this, typeof(SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras(valuesForActivity);

    // Construct a back stack for cross-task navigation:
    var stackBuilder = TaskStackBuilder.Create(this);
    stackBuilder.AddParentStack(Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent(resultIntent);

    // Create the PendingIntent with the back stack:
    var resultPendingIntent = stackBuilder.GetPendingIntent(0, (int) PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    var builder = new NotificationCompat.Builder(this, CHANNEL_ID)
                  .SetAutoCancel(true) // Dismiss the notification from the notification area when the user clicks on it
                  .SetContentIntent(resultPendingIntent) // Start up this activity when the user clicks the intent.
                  .SetContentTitle("Button Clicked") // Set the title
                  .SetNumber(count) // Display the count in the Content Info
                  .SetSmallIcon(Resource.Drawable.ic_stat_button_click) // This is the icon to display
                  .SetContentText($"The button has been clicked {count} times."); // the message to display.

    // Finally, publish the notification:
    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(NOTIFICATION_ID, builder.Build());

    // Increment the button press count:
    count++;
}
```

`OnCreate` MainActivity yöntemi bildirim kanalı oluşturmak ve atamak için bir çağrı yapmak gerekir `ButtonOnClick` yönteme `Click` olay düğmesi (şablon tarafından sağlanan temsilci olay işleyicisi değiştirin):

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();

    // Display the "Hello World, Click Me!" button and register its event handler:
    var button = FindViewById<Button>(Resource.Id.MyButton);
    button.Click += ButtonOnClick;
}
```


### <a name="create-a-second-activity"></a>İkinci bir etkinlik oluşturma

Artık kullanıcı bizim bildirim tıkladığında Android görüntüler başka bir etkinlik oluşturmak ihtiyacımız var. Adlı projenize başka bir Android etkinliği eklemek **SecondActivity**. Açık **SecondActivity.cs** ve içerikleri şu kodla değiştirin:

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
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            // Get the count value passed to us from MainActivity:
            var count = Intent.Extras.GetInt(MainActivity.COUNT_KEY, -1);

            // No count was passed? Then just return.
            if (count <= 0)
            {
                return;
            }

            // Display the count sent from the first activity:
            SetContentView(Resource.Layout.Second);
            var txtView = FindViewById<TextView>(Resource.Id.textView1);
            txtView.Text = $"You clicked the button {count} times in the previous activity.";
        }
    }
}
```

Biz de kaynak düzeni oluşturmalısınız **SecondActivity**. Yeni bir **Android düzeni** dosya adlı projenize **Second.axml**. Düzen **Second.axml** düzeni aşağıdaki kodu yapıştırın:

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
        android:id="@+id/textView1" />
</LinearLayout>
```


### <a name="add-a-notification-icon"></a>Bir bildirim simgesi Ekle

Son olarak, bildirim alanında bildirim başlatıldığında görünecek küçük bir simge ekleyin. Kopyalayabilirsiniz [bu simgeyi](local-notifications-walkthrough-images/ic-stat-button-click.png) projenize veya kendi özel bir simge oluşturun. Simge dosyası adı **IC\_stat\_düğmesi\_click.png** ve kopyalayıp **kaynakları/drawable** klasör. Kullanmayı unutmayın **Ekle > mevcut öğe...**  bu simge dosyası projenize eklenecek.


### <a name="run-the-application"></a>Uygulamayı çalıştırın

Derleme ve uygulamayı çalıştırın. Aşağıdaki ekran görüntüsüne benzer ilk etkinliğin erişemediklerinde sunulması gereken:

[![İlk etkinlik ekran görüntüsü](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

Düğmeye tıkladığınızda, küçük simge bildirimi için bildirim alanında görüntülendiğini fark:

[![Uyarı simgesi görüntülenir](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

Aşağı doğru çekin ve bildirim çekmecesini kullanıma sunma, şu bildirimi görmelisiniz:

[![Bildirim iletisi](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

Bildirime tıklayın, ortadan kalkması gerekir ve başka bir etkinliği bizim başlatılan &ndash; bakıma aşağıdaki ekran görüntüsünde aranıyor:

[![İkinci etkinlik ekran görüntüsü](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

Tebrikler! Bu noktada Android yerel bildirim izlenecek yolu tamamladınız ve başvurabilirsiniz bir çalışma örneği vardır. Bildirimleri için daha fazla biz burada gösterilen daha bu nedenle daha fazla bilgi edinmek istiyorsanız göz atın birçok [Google'nın belgeleri bildirimleri](http://developer.android.com/guide/topics/ui/notifiers/notifications.html).


## <a name="summary"></a>Özet

Bu kılavuzda kullanılan `NotificationCompat.Builder` oluşturma ve bildirimleri görüntüleme. Bildirim ile kullanıcı etkileşimine yanıt için bir yol olarak ikinci bir etkinlik başlatmak için basit bir örneği gösterilmiştir ve, veri aktarımını ilk etkinliğinden ikinci etkinliği gösterilmektedir.


## <a name="related-links"></a>İlgili bağlantılar

- [LocalNotifications (örnek)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Android Oreo bildirim kanalları](https://blog.xamarin.com/android-oreo-notification-channels/)
- [Bildirim](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
