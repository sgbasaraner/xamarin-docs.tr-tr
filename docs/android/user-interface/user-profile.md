---
title: "Kullanıcı profili"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2017
ms.openlocfilehash: 53ac30abea05095583fcac5ddc315f93ce7024f2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="user-profile"></a>Kullanıcı profili

Android numaralandırma kişilerle desteklenen `ContactsContract` API Düzey 5 itibaren sağlayıcısı. Örneğin, listeye kişiler kullanmak kadar basit `ContactContracts.Contacts` aşağıdaki kodda gösterildiği gibi sınıfı:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
           
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id,
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);
           
if (cursor.MoveToFirst ()) {
    do {
        Console.WriteLine ("Contact ID: {0}, Contact Name: {1}",
            cursor.GetString (cursor.GetColumnIndex (projection [0])),
            cursor.GetString (cursor.GetColumnIndex (projection [1])));
                   
    } while (cursor.MoveToNext());
}
```

Android 4 (API düzeyi 14) ile yeni bir `ContactsContact.Profile` sınıfı ContactsContract sağlayıcısı üzerinden kullanılabilir. `ContactsContact.Profile` Cihaz sahibinin adı ve telefon numarası gibi kişi verilerini içeren bir cihaz sahibinin kişisel profil erişim sağlar.

<a name="Required_Permissions" />

## <a name="required-permissions"></a>Gerekli İzinler

Kişi verilerini okuma ve yazma için uygulamaları istemelidir `Read_Contacts` ve `Write_Contacts` izinleri, sırasıyla. Ayrıca, okuma ve kullanıcı profili düzenlemek için uygulamaları istemelidir `Read_Profile` ve `Write_Profile` izinleri.

<a name="Updating_Profile_Data" />

## <a name="updating-profile-data"></a>Profil verileri güncelleştirme

Bu izinleri ayarladıktan sonra uygulamanın kullanıcı profilinin verileriyle etkileşim kurmak için normal Android teknikleri kullanabilirsiniz. Örneğin, diyoruz profilinin görünen adı güncelleştirmek için `ContentResolver.Update` ile bir `Uri` üzerinden alınan `ContactsContract.Profile.ContentRawContactsUri` özelliği, aşağıda gösterildiği gibi:

```csharp
var values = new ContentValues ();
          
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName,
    "John Doe");
           
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri,
    values, null, null);
```

<a name="Reading_Profile_Data" />

## <a name="reading-profile-data"></a>Profil verileri okuma

Bir sorgu verme `ContactsContact.Profile.ContentUri` okuma profil verileri yedekleyin. Örneğin, aşağıdaki kod, kullanıcı profilinin görünen adı şöyle olacaktır:

```csharp
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);

if (cursor.MoveToFirst ()) {
    Console.WriteLine(
        cursor.GetString (cursor.GetColumnIndex (projection [0])));
}
```

<a name="Navigating_to_the_People_App" />

## <a name="navigating-to-the-people-app"></a>Kişiler uygulamasına gidin

Son olarak, Android 4 ile gelen yeni kişiler uygulamasını kullanıcı profiline gitmek için yalnızca bir amacıyla oluşturun bir `ActionView` eylem ve `ContactsContract.Profile.ContentUri`ve ona geçirin `StartActivity` yöntemi şuna benzer:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Yukarıdaki kod çalıştırırken, kişi uygulama aşağıdaki ekran görüntüsünde gösterildiği gibi kullanıcı profiline yükleyecek:

[![Ekran görüntüsü, kişi uygulama John Doe kullanıcı profili görüntüleme](user-profile-images/15-people-app.png)](user-profile-images/15-people-app.png)

Kullanıcı profili çalışmak artık Android diğer verilerle etkileşim için benzer ve cihaz kişiselleştirme, ek bir düzeyi sunar.



## <a name="related-links"></a>İlgili bağlantılar

- [ContactsProviderDemo (örnek)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
