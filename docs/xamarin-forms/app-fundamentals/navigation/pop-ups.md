---
title: Açılır pencereleri görüntüleme
description: Xamarin.Forms iki pop kaydolma gibi kullanıcı arabirimi öğeleri – uyarıyı ve eylem sayfası sağlar. Bu makalede, kullanıcıların basit soru sormak ve görevleri aracılığıyla kullanıcılara kılavuzluk etmesi için uyarıyı ve eylem sayfası API'leri kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 156c2f9dca47a7755d4f810d7921a05662388ded
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996720"
---
# <a name="displaying-pop-ups"></a>Açılır pencereleri görüntüleme

_Xamarin.Forms iki pop kaydolma gibi kullanıcı arabirimi öğeleri – uyarıyı ve eylem sayfası sağlar. Bu makalede, kullanıcıların basit soru sormak ve görevleri aracılığıyla kullanıcılara kılavuzluk etmesi için uyarıyı ve eylem sayfası API'leri kullanmayı gösterir._

Uyarı görüntüleme veya bir seçim yapmasını anın genel kullanıcı Arabirimi bir görevdir. Xamarin.Forms sahip iki yöntem [ `Page` ](xref:Xamarin.Forms.Page) bir açılır pencere aracılığıyla kullanıcı ile etkileşim kurmak için sınıf: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) ve [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*). Her platformda uygun yerel denetimleri ile işlenir.

## <a name="displaying-an-alert"></a>Uyarı görüntüleme

Tüm Xamarin.Forms desteklenen platformlar, kullanıcıyı uyarmak veya bunların basit sorular sormak için kalıcı bir açılır pencere vardır. Xamarin.Forms içinde bu uyarıları görüntülemek için kullanın [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) herhangi yöntemi [ `Page` ](xref:Xamarin.Forms.Page). Aşağıdaki kod satırını kullanıcıya basit bir ileti gösterir:

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Bir düğme ile uyarı iletişim kutusu")

Bu örnek, kullanıcıdan bilgi toplamaz. Uyarı kalıcı olarak görüntüler ve kullanıcı kapatıldı sonra uygulama ile etkileşim kurmaya devam eder.

[ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) Yöntemi de kullanılabilir iki düğme sunmak ve döndüren bir kullanıcının yanıt yakalamak için bir `boolean`. Bir uyarıdan bir yanıt almak için her iki düğme metni sağlayın ve `await` yöntemi. Kullanıcı seçeneklerden birini seçtikten sonra kodunuzu yanıt döndürülür. Not `async` ve `await` örnek kodda aşağıdaki anahtar sözcükler:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![Await DisplayAlert](pop-ups-images/alert2-sml.png "iki düğme bir iletişim kutusunda uyarı")](pop-ups-images/alert2.png#lightbox "uyarı iki düğme ile iletişim")

## <a name="guiding-users-through-tasks"></a>Görevleri aracılığıyla temel kullanıcılar

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) iOS içinde ortak bir kullanıcı Arabirimi öğesidir. Xamarin.Forms [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) yöntemi platformlar arası uygulamalarda yerel Android ve UWP alternatiflerini oluşturma, bu denetim eklemenize olanak sağlar.

Bir eylem sayfasını görüntülemek için `await` [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) herhangi [ `Page` ](xref:Xamarin.Forms.Page), ileti geçirme ve düğme dizeler olarak etiketler. Bu yöntem, kullanıcı tarafından tıklandığını düğmenin dize etiketi döndürür. Basit bir örnek aşağıda gösterilmiştir:

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet iletişim")

`destroy` Düğmesi diğerlerinden farklı olarak işlenir ve devre dışı bırakılabilir `null` ya da belirtilen üçüncü dizesi parametresi olarak. Aşağıdaki örnekte `destroy` düğmesi:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "eylem sayfası iletişim yok et düğmesi")](pop-ups-images/action2.png#lightbox "eylem sayfası iletişim yok et düğmesi")

## <a name="summary"></a>Özet

Bu makale, kullanıcıların basit soru sormak ve görevleri aracılığıyla kullanıcılara kılavuzluk etmesi için uyarıyı ve eylem sayfası API'leri kullanarak gösterdik. Xamarin.Forms sahip iki yöntem [ `Page` ](xref:Xamarin.Forms.Page) bir açılır pencere aracılığıyla kullanıcı ile etkileşim kurmak için sınıf: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) ve [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*), ve bunlar hem de her platformda uygun yerel denetimleri ile çizilir.



## <a name="related-links"></a>İlgili bağlantılar

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
