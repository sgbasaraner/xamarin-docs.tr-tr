---
title: "Kişiler ve ContactsUI"
description: "Bu makalede, bir Xamarin.iOS uygulaması içinde çerçeve yeni kişiler ve kişi UI ile çalışma kapsar. Bu çerçeveleri adres defteri iOS önceki sürümlerinde kullanılan UI ve var olan adres defteri değiştirin."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 996723db83a1f972cce26090d1253f97b6c818d3
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="contacts-and-contactsui"></a>Kişiler ve ContactsUI

_Bu makalede, bir Xamarin.iOS uygulaması içinde çerçeve yeni kişiler ve kişi UI ile çalışma kapsar. Bu çerçeveleri adres defteri iOS önceki sürümlerinde kullanılan UI ve var olan adres defteri değiştirin._

İOS 9 başlanmasıyla, iki yeni çerçeveler Apple yayımladı `Contacts` ve `ContactsUI`, o Değiştir var olan adres defteri ve adres defteri UI çerçeveleri 8 ve daha önceki iOS tarafından kullanılır.

İki yeni çerçeveleri aşağıdaki işlevselliği içerir:

- [**Kişiler** ](#contacts) -kullanıcının kişi listesi verilere erişim sağlar.
    Çoğu uygulamalar yalnızca salt okunur erişim gerektirdiğinden bu framework iş parçacığı güvenli, salt okunur erişim için optimize edilmiştir.

- [**ContactsUI** ](#contactsui) -sağlar Xamarin.iOS UI öğelerini görüntülemek için düzenleme, seçin ve iOS cihazlarda kişileri oluşturun.

[![](contacts-images/add01.png "Bir iOS cihazında bir örnek kişi listesi")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> **Not:** varolan `AddressBook` ve `AddressBookUI` çerçeveler iOS 8 kullandığı (ve önceki) iOS 9'u kullanım dışı bırakıldı ve yeni değiştirilmelidir `Contacts` ve `ContactsUI` varolan tüm Xamarin.iOS için mümkün olan en kısa sürede çerçeveler uygulama. Yeni uygulamalar karşı yeni çerçeveleri yazılması gerekir.




Aşağıdaki bölümlerde, biz bu yeni çerçeveler ve bunların bir Xamarin.iOS uygulaması nasıl uygulanacağını göz atın.

<a name="contacts" />

## <a name="the-contacts-framework"></a>Kişiler Framework

Kişiler Framework kullanıcının kişi bilgilerini Xamarin.iOS erişim sağlar. Çoğu uygulamalar yalnızca salt okunur erişim gerektirdiğinden bu framework iş parçacığı güvenli, salt okunur erişim için optimize edilmiştir.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Kişi nesneleri

`CNContact` Sınıf adını, adresini veya telefon numaraları gibi bir kişinin özellikleri iş parçacığı güvenli, salt okunur erişim sağlar. `CNContact` gibi işlev bir `NSDictionary` ve içeren birden çok, salt okunur özelliklerinin koleksiyonlarından oluşan (örneğin, adresler veya telefon numarası):

[![](contacts-images/contactobjects.png "Kişi nesne genel bakış")](contacts-images/contactobjects.png#lightbox)

(Örneğin, e-posta adresi veya telefon numarası) birden çok değer sağlayabilirsiniz herhangi bir özellik için bunlar bir dizi temsil edilir `NSLabeledValue` nesneleri. `NSLabeledValue` etiketleri salt okunur kümesinden oluşan bir iş parçacığı güvenli tanımlama grubu olan ve burada etiket değeri (örneğin ev veya iş e-posta) kullanıcıya tanımlar değerleri. Önceden tanımlanmış etiketleri seçimi kişiler çerçevesi sağlar (aracılığıyla `CNLabelKey` ve `CNLabelPhoneNumberKey` statik sınıflar) uygulamanızda kullanabilirsiniz veya gereksinimleriniz için özel etiketler tanımlama seçeneğiniz vardır.

Varolan bir kişinin değerlerini ayarlayın (veya yenilerini oluşturmak için) gereken tüm Xamarin.iOS uygulaması için kullanmak `NSMutableContact` sınıfı ve onun alt sürümü (gibi `CNMutablePostalAddress`).

Örneğin, izleme kodu yeni bir kişi oluşturun ve kullanıcının kişiler koleksiyonuna ekleyin:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Add email addresses
var homeEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@mac.com"));
var workEmail = new CNLabeledValue<NSString>(CNLabelKey.Work, new NSString("john.appleseed@apple.com"));
contact.EmailAddresses = new CNLabeledValue<NSString>[] { homeEmail, workEmail };

// Add phone numbers
var cellPhone = new CNLabeledValue<CNPhoneNumber>(CNLabelPhoneNumberKey.iPhone, new CNPhoneNumber("713-555-1212"));
var workPhone = new CNLabeledValue<CNPhoneNumber>("Work", new CNPhoneNumber("408-555-1212"));
contact.PhoneNumbers = new CNLabeledValue<CNPhoneNumber>[] { cellPhone, workPhone };

// Add work address
var workAddress = new CNMutablePostalAddress()
{
    Street = "1 Infinite Loop",
    City = "Cupertino",
    State = "CA",
    PostalCode = "95014"
};
contact.PostalAddresses = new CNLabeledValue<CNPostalAddress>[] { new CNLabeledValue<CNPostalAddress>(CNLabelKey.Work, workAddress) };

// Add birthday
var birthday = new NSDateComponents()
{
    Day = 1,
    Month = 4,
    Year = 1984
};
contact.Birthday = birthday;

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

// Attempt to save changes
NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error))
{
    Console.WriteLine("New contact saved");
}
else
{
    Console.WriteLine("Save error: {0}", error);
}
```

Bu kodu iOS 9 cihazında çalıştırırsanız, yeni bir kişi kullanıcının koleksiyonuna eklenir. Örneğin:

[![](contacts-images/add01.png "Kullanıcının koleksiyona eklenen yeni bir ilgili kişi")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>Kişi biçimlendirme ve yerelleştirme

Bazı nesneler kişiler framework içerir ve yardımcı olacak yöntemler biçimlendirmek ve içerik kullanıcıya görünen için yerelleştirme. Örneğin, aşağıdaki kodu doğru kişiler adını ve posta adresi görüntülenmesi için biçim:

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

Uygulamanızın kullanıcı Arabiriminde görüntüleyerek özelliği etiketleri için kişi framework de bu dizeleri yerelleştirme için yöntemlerine sahiptir. Yeniden, bu uygulama çalıştırıldığı iOS cihazı geçerli yerel temel alır. Örneğin:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOption.Nickname));
Console.WriteLine(CNLabeledValue.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>Var olan kişileri getirme

Örneğini kullanarak `CNContactStore` sınıfı, kişi bilgilerini kullanıcının kişiler veritabanından getirebilirsiniz. `CNContactStore` Tüm getirme veya kişiler ve gruplar veritabanından güncelleştirmek için gereken yöntemleri içerir. Bu yöntemlerin zaman uyumlu olduğundan, kullanıcı arabirimini Engellemesi tutmak için bir arka plan iş parçacığı üzerinde çalıştırmanızı önerilir.

Koşulları kullanarak (oluşturulmuş `CNContact` sınıfı), kişiler veritabanından getirilirken döndürülen sonuçları filtreleyebilirsiniz. Dizeyi içeren kişiler getirmek `Appleseed`, aşağıdaki kodu kullanın:

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> **Not:** genel ve bileşik koşulları kişiler çerçevesi tarafından desteklenmiyor.

Örneğin, yalnızca fetch sınırlamak için **GivenName** ve **FamilyName** kişinin özelliklerini şu kodu kullanın:

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

Son olarak, veritabanı aramak ve sonuçları döndürmek için aşağıdaki kodu kullanın:

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

Bu kod, oluşturduğumuz örnek sonra çalıştırılmışsa **kişiler nesne** yukarıdaki bölümde, yeni oluşturduğumuz "John Appleseed" kişiyi döndürür.

### <a name="contact-access-privacy"></a>Kişi erişim gizlilik

Son kullanıcılara vermek veya uygulama başına temelinde kişi bilgileri erişimini engellemek için ilk kez yaptığınız yapılan bir çağrı `CNContactStore`, bunları uygulamanız için erişime izin verecek şekilde soran bir iletişim kutusu sunulur.

İzin isteği yalnızca bir kez sunulur, uygulamayı çalıştırma ve sonraki ilk kez çalışan veya çağrılar `CNContactStore` kullanıcı o anda seçili izni kullanır.

Böylece düzgün biçimde kişi kendi veritabanı erişimi reddetme kullanıcıyı işleyen uygulamanızı tasarlamanız gerekir.

#### <a name="fetching-partial-contacts"></a>Kısmi kişiler getirme

A _kısmi başvurun_ kullanılabilir özellikler yalnızca bazıları için kişi Mağazası'ndan getirildi bir ile iletişime geçin. Değil önceden getirildi bir özellik erişmeye çalışırsa, bir özel durum neden olur.

Belirli bir kişi ya da kullanarak istenen özelliği olup olmadığını görmek için kolayca kontrol edebilirsiniz `IsKeyAvailable` veya `AreKeysAvailable` yöntemlerinin `CNContact` örneği. Örneğin:

```csharp
// Does the contact contain the requested key?
if (!contact.IsKeyAvailable(CNContactOption.PostalAddresses)) {
    // No, re-request to pull required info
    var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName, CNContactKey.PostalAddresses};
    var store = new CNContactStore();
    NSError error;
    contact = store.GetUnifiedContact(contact.Identifier, fetchKeys, out error);
}
```

> [!IMPORTANT]
> **Not:** `GetUnifiedContact` ve `GetUnifiedContacts` yöntemlerinin `CNContactStore` sınıfı _yalnızca_ kısmi sağlanan fetch anahtarları istenen özellikler sınırlı kişi döndürür.

### <a name="unified-contacts"></a>Birleşik kişiler

Bir kullanıcı, kişi kendi veritabanında (gibi iCloud, Facebook veya Google Mail) tek bir kişinin iletişim bilgilerini farklı kaynaklarının sahip olabilir. İOS ve OS X uygulamaları, bu kişi bilgilerini otomatik olarak birbirine bağlı ve olması olarak tek bir kullanıcıya görüntülenen _birleşik başvurun_:

[![](contacts-images/unified01.png "Birleşik kişiler genel bakış")](contacts-images/unified01.png#lightbox)

Bu birleşik başvurun, (gerekirse kişi yeniden getirmesi için kullanılması gereken) kendi benzersiz tanımlayıcısı verilecek bağlantı bilgilerini, geçici, bellek içi bir görünümüdür. Varsayılan olarak, mümkün olduğunda birleşik kişi kişiler framework döndürür.

### <a name="creating-and-updating-contacts"></a>Oluşturma ve kişiler güncelleştirme

İçinde gördüğümüz gibi [başvurun nesneleri](#Contact_Objects) yukarıdaki bölümde, kullandığınız bir `CNContactStore` bir örneği ile bir `CNMutableContact` oluşturmak için kullanıcının sonra yazılan yeni kişiler veritabanı kullanarak bağlantı kurun bir `CNSaveRequest`:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("New contact saved");
} else {
    Console.WriteLine("Save error: {0}", error);
}
```

A `CNSaveRequest` tek bir işlemde birden çok kişi ve Grup değişiklikleri önbelleğe alınması ve bu değişiklikleri toplu için de kullanılabilir `CNContactStore`.

Getirme işlemi elde edilen değişebilir olmayan bir kişi güncelleştirmek için önce ardından değiştirin ve kişi deposuna kaydedin değişebilir bir kopya istemeniz gerekir. Örneğin:

```csharp
// Get mutable copy of contact
var mutable = contact.MutableCopy() as CNMutableContact;
var newEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@xamarin.com"));

// Append new email
var emails = new NSObject[mutable.EmailAddresses.Length+1];
mutable.EmailAddresses.CopyTo(emails,0);
emails[mutable.EmailAddresses.Length+1] = newEmail;
mutable.EmailAddresses = emails;

// Update contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.UpdateContact(mutable);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("Contact updated.");
} else {
    Console.WriteLine("Update error: {0}", error);
}
```

### <a name="contact-change-notifications"></a>Kişi değişiklik bildirimleri

Bir kişi değiştirildiğinde, kişi deposu yazılarını bir `CNContactStoreDidChangeNotification` varsayılan bildirim Merkezi. Önbelleğe alınmış veya şu anda tüm kişileri görüntüleme, bu nesneler kişi deposundan yenileme gerekir (`CNContactStore`).

### <a name="containers-and-groups"></a>Kapsayıcılar ve grupları

Kullanıcının kişilerinin veya yerel olarak kullanıcının aygıtındaki (Facebook veya Google gibi) bir veya daha fazla sunucu hesaplarından aygıta eşitlenen kişiler olarak bulunabilir. Her kişilerin kendi sahip havuzu _kapsayıcı_ ve belirli bir kişi bir kapsayıcıda yalnızca bulunabilir.

[![](contacts-images/containers01.png "Kapsayıcılar ve gruplara genel bakış")](contacts-images/containers01.png#lightbox)

Bir veya daha fazla düzenlenmesini kişiler için bazı kapsayıcıları izin _grupları_ veya _alt gruplar_. Bu davranış, verilen bir kapsayıcı yedekleme deposu bağımlıdır. Örneğin, iCloud yalnızca bir kapsayıcıya sahip ancak birçok grupları (ancak hiçbir alt grubu) sahip olabilir. Diğer taraftan, Microsoft Exchange grupları desteklemez ancak birden çok kapsayıcı (her Exchange klasörü için bir tane) sahip olabilir.

[![](contacts-images/containers02.png "Kapsayıcılar ve grupları içinde çakışıyor")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>ContactsUI Framework

Burada, uygulamanızın özel Arabirim sunmak gerekmez durumlarda, ContactsUI framework görüntüleme, düzenleme, seçin ve ilgili kişi Xamarin.iOS uygulamanızı oluşturmak için kullanıcı Arabirimi öğeleri sunmak için kullanabilirsiniz.

Apple'nın yerleşik denetimlerini kullanarak, yalnızca Kişiler Xamarin.iOS uygulamanızı desteklemek üzere oluşturmak zorunda kod miktarını azaltır, ancak uygulamanın kullanıcılara tutarlı bir arabirim sunar.

### <a name="the-contact-picker-view-controller"></a>Kişi Seçici görünümü denetleyicisi

Kişi Seçici View Controller (`CNContactPickerViewController`) standart kişi seçici bir kişi ya da bir kişi özelliği kullanıcının kişi veritabanından seçmek kullanıcının sağlayan görünüm yönetir. Kişi Seçici View Controller Seçici görüntüleme önce izin istenmez ve kullanıcı (kendi kullanıma dayalı) bir veya daha fazla kişi seçebilirsiniz.

Çağırmadan önce `CNContactPickerViewController` sınıfı, kullanıcı seçin ve ilgili kişi özelliklerinin seçilmesi ve görüntü denetlemek için koşulları tanımlayın hangi özellikleri tanımlar.

Öğesinden devralınan sınıfının bir örneğini kullanmak `CNContactPickerDelegate` kullanıcı etkileşimi Seçici yanıtlamak için. Örneğin:

```csharp
using System;
using System.Linq;
using UIKit;
using Foundation;
using Contacts;
using ContactsUI;

namespace iOS9Contacts
{
    public class ContactPickerDelegate: CNContactPickerDelegate
    {
        #region Constructors
        public ContactPickerDelegate ()
        {
        }

        public ContactPickerDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ContactPickerDidCancel (CNContactPickerViewController picker)
        {
            Console.WriteLine ("User canceled picker");

        }

        public override void DidSelectContact (CNContactPickerViewController picker, CNContact contact)
        {
            Console.WriteLine ("Selected: {0}", contact);
        }

        public override void DidSelectContactProperty (CNContactPickerViewController picker, CNContactProperty contactProperty)
        {
            Console.WriteLine ("Selected Property: {0}", contactProperty);
        }
        #endregion
    }
}
```

Kullanıcının kendi veritabanında kişilerden bir e-posta adresi seçmesine izin vermek için aşağıdaki kodu kullanabilirsiniz:

```csharp
// Create a new picker
var picker = new CNContactPickerViewController();

// Select property to pick
picker.DisplayedPropertyKeys = new NSString[] {CNContactKey.EmailAddresses};
picker.PredicateForEnablingContact = NSPredicate.FromFormat("emailAddresses.@count > 0");
picker.PredicateForSelectionOfContact = NSPredicate.FromFormat("emailAddresses.@count == 1");

// Respond to selection
picker.Delegate = new ContactPickerDelegate();

// Display picker
PresentViewController(picker,true,null);
```

### <a name="the-contact-view-controller"></a>Kişi görünümünü denetleyicisi

Kişi View Controller (`CNContactViewController`) sınıfı, son kullanıcı için standart bir kişi görünümünü sunmak için bir denetleyici sağlar. Kişi görünümü yeni yeni, bilinmeyen veya varolan kişiler görüntüleyebilir ve görünümü doğru statik Oluşturucu çağırarak görüntülenmeden önce türü belirtilmelidir (`FromNewContact`, `FromUnknownContact`, `FromContact`). Örneğin:

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.iOS uygulaması başvurun ve kişi UI çerçeveleri ile çalışma ayrıntılı bir bakış duruma getirdi. İlk olarak, farklı nesne türleri ile ilgili kişi framework sağlayan nasıl bunları yeni oluşturmak veya var olan kişileri erişmek için kullandığınız ele. Ayrıca, var olan kişileri seçin ve bilgilerini görüntülemek için kullanıcı Arabirimi başvurun framework incelendi.


## <a name="related-links"></a>İlgili bağlantılar

- [QuickContacts örnek](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [İOS 9 yenilikler](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-DontLinkElementID_14))
- [Kişiler Framework başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/Contacts/Reference/Contacts_Framework/index.html#//apple_ref/doc/uid/TP40015328)
- [ContactsUI Framework başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/ContactsUI/Reference/ContactsUI_Framework/index.html#//apple_ref/doc/uid/TP40016207)
