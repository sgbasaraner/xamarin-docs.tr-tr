---
title: Açılır pencerelere görüntüleme
description: Xamarin.Forms iki pop up benzeri kullanıcı arabirimi öğeleri – bir uyarı ve bir eylem sayfası sağlar. Bu makalede, kullanıcıların basit sorular sormak için ve görevleri kullanıcılara kılavuzluk için uyarı ve eylem sayfası API'leri kullanılarak gösterilmektedir.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 97f0917e4e8670ab379aae1b2707ae08cb29bb70
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
ms.locfileid: "32018885"
---
# <a name="displaying-pop-ups"></a>Açılır pencerelere görüntüleme

_Xamarin.Forms iki pop up benzeri kullanıcı arabirimi öğeleri – bir uyarı ve bir eylem sayfası sağlar. Bu makalede, kullanıcıların basit sorular sormak için ve görevleri kullanıcılara kılavuzluk için uyarı ve eylem sayfası API'leri kullanılarak gösterilmektedir._

Bir uyarı görüntüleme veya bir seçim yapmak için bir kullanıcı soran bir ortak UI görevdir. Xamarin.Forms sahip iki yöntem [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) bir açılır pencere aracılığıyla kullanıcıyla etkileşim için sınıf: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) ve [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/). Her platformda uygun yerel denetimleri ile işlenir.

## <a name="displaying-an-alert"></a>Bir uyarı görüntüleme

Xamarin.Forms desteklenen tüm platformlar kullanıcıyı uyarmak veya bunların basit sorular sormak için kalıcı bir açılır pencere vardır. Xamarin.Forms içinde bu uyarıları görüntülemek için kullanın [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) herhangi bir yöntem [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Aşağıdaki kod satırını kullanıcıya basit bir ileti gösterilir:

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Uyarı iletişim kutusu bir düğmesi")

Bu örnekte, kullanıcıdan bilgi toplamaz. Uyarı kalıcı olarak görüntüler ve kullanıcının kapatıldığında sonra uygulama ile etkileşim kurmaya devam eder.

[ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) Yöntemi de kullanılabilir iki düğme sunan ve döndüren bir kullanıcının yanıtını yakalamak için bir `boolean`. Bir uyarıdan yanıt almak için her iki düğmelerinin metin sağlayın ve `await` yöntemi. Kullanıcı seçeneklerden birini seçtikten sonra yanıt kodunuzu döndürülür. Not `async` ve `await` örnek kodda anahtar sözcükler:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "uyarı iletişim iki düğmelerle")](pop-ups-images/alert2.png#lightbox "uyarı iki düğmeleri ile iletişim")

## <a name="guiding-users-through-tasks"></a>Görevleri boyunca yol gösterici kullanıcılar

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) bir ortak UI iOS öğedir. Xamarin.Forms [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) yöntemi, Android ve UWP yerel alternatifleri işleme platformlar arası uygulamalar, bu denetim dahil etmenizi sağlar.

Bir eylem sayfasını görüntülemek için `await` [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) herhangi [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), ileti geçirme ve düğmesini etiketleri dizeleri olarak. Yöntemi, kullanıcı tarafından tıklandığını düğmesinin dize etiketi döndürür. Basit bir örnek aşağıda gösterilmiştir:

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet iletişim")

`destroy` Düğmesi diğerlerinden farklı şekilde işlenir ve devre dışı bırakılabilir `null` veya üçüncü dizesi parametresi olarak belirtilmiş. Aşağıdaki örnek kullanır `destroy` düğmesi:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "eylem sayfası iletişim Destroy düğmesiyle")](pop-ups-images/action2.png#lightbox "yok et düğmesi eylemi sayfası iletişim kutusu")

## <a name="summary"></a>Özet

Bu makalede, kullanıcıların basit sorular sormak için ve görevleri kullanıcılara kılavuzluk için uyarı ve eylem sayfası API'leri kullanılarak gösterilmektedir. Xamarin.Forms sahip iki yöntem [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) bir açılır pencere aracılığıyla kullanıcıyla etkileşim için sınıf: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) ve [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/), ve bunlar her ikisi de her platformda uygun yerel denetimleri ile çizilir.



## <a name="related-links"></a>İlgili bağlantılar

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
