---
title: "Kullanıcı profili"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2017
ms.openlocfilehash: cf8230c5832104fd17b14532f1d32822a1fc0097
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
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


## <a name="required-permissions"></a>Gerekli İzinler

Kişi verilerini okuma ve yazma için uygulamaları istemelidir `Read_Contacts` ve `Write_Contacts` izinleri, sırasıyla. Ayrıca, okuma ve kullanıcı profili düzenlemek için uygulamaları istemelidir `Read_Profile` ve `Write_Profile` izinleri.


## <a name="updating-profile-data"></a>Profil verileri güncelleştirme

Bu izinleri ayarladıktan sonra uygulamanın kullanıcı profilinin verileriyle etkileşim kurmak için normal Android teknikleri kullanabilirsiniz. Örneğin, diyoruz profilinin görünen adı güncelleştirmek için `ContentResolver.Update` ile bir `Uri` üzerinden alınan `ContactsContract.Profile.ContentRawContactsUri` özelliği, aşağıda gösterildiği gibi:

```csharp
var values = new ContentValues ();
          
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName,
    "John Doe");
           
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri,
    values, null, null);
```


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


## <a name="navigating-to-the-people-app"></a>Kişiler uygulamasına gidin

Son olarak, Android 4 ile gelen yeni kişiler uygulamasını kullanıcı profiline gitmek için yalnızca bir amacıyla oluşturun bir `ActionView` eylem ve `ContactsContract.Profile.ContentUri`ve ona geçirin `StartActivity` yöntemi şuna benzer:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Yukarıdaki kod çalıştırırken, kişi uygulama aşağıdaki ekran görüntüsünde gösterildiği gibi kullanıcı profiline yükleyecek:

[![Ekran görüntüsü, kişi uygulama John Doe kullanıcı profili görüntüleme](user-profile-images/15-people-app.png)](user-profile-images/15-people-app.png#lightbox)

Kullanıcı profili çalışmak artık Android diğer verilerle etkileşim için benzer ve cihaz kişiselleştirme, ek bir düzeyi sunar.



## <a name="related-links"></a>İlgili bağlantılar

- [ContactsProviderDemo (örnek)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
