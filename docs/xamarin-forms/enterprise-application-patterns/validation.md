---
title: Doğrulama
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 58254fd3c7a3949b0ed6bb448223e34cf76f7103
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="validation"></a>Doğrulama

Kullanıcı girişi kabul eden herhangi bir uygulama girişi geçerli olduğundan emin olmanız gerekir. Örneğin, bir uygulama için belirli bir aralıkta yalnızca karakterler içeriyor, belirli bir süre veya belirli bir biçimde eşleşen giriş denetleyebilirsiniz. Doğrulama olmadan bir kullanıcı uygulama başarısız olmasına neden olan veri sağlayabilir. Doğrulama iş kurallarını uygular ve bir saldırgan kötü amaçlı veriler injecting önler.

Bağlamında, Model ViewModel modeli (MVVM) deseni, bir görünüm modeli veya model genellikle veri doğrulaması yapmak ve böylece kullanıcı düzeltmenize görüntülemek için herhangi bir doğrulama hatası sinyal gerekecektir. EShopOnContainers mobil uygulama görünüm modeli özelliklerinin eşzamanlı istemci tarafı doğrulama gerçekleştirir ve tüm doğrulama hatalarını kullanıcı geçersiz veri içeren denetimi vurgulama ve kullanıcıyı bilgilendirmek hata iletilerini göstererek bildirir neden verileri geçersiz. Şekil 6-1 sınıfları eShopOnContainers mobil uygulamaya doğrulama gerçekleştirme katılan gösterir.

[![](validation-images/validation.png "EShopOnContainers mobil uygulama doğrulama sınıflarda")](validation-images/validation-large.png#lightbox "eShopOnContainers mobil uygulamadaki doğrulama sınıfları")

**Şekil 6-1**: eShopOnContainers mobil uygulamadaki doğrulama sınıfları

Doğrulama gerektiren görünüm modeli özelliklerdir türü `ValidatableObject<T>`ve her `ValidatableObject<T>` örneği sahip eklenen doğrulama kuralları, `Validations` özelliği. Doğrulama çağrılır görünüm modelden çağırarak `Validate` yöntemi `ValidatableObject<T>` doğrulama alır örneği kuralları ve bunlara karşı yürütür `ValidatableObject<T>` `Value` özelliği. Tüm doğrulama hatalarını içine yerleştirilen `Errors` özelliği `ValidatableObject<T>` örneği ve `IsValid` özelliği `ValidatableObject<T>` örneği, doğrulama başarılı veya başarısız olup olmadığını belirtmek üzere güncelleştirilir.

Özellik değişikliği bildirimi tarafından sağlanan `ExtendedBindableObject` sınıfı ve bu nedenle bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetimi için bağlayabilirsiniz `IsValid` özelliği `ValidatableObject<T>` olsun veya olmasın bildirim almak için Görünüm model sınıfı örneği Girilen veriler geçerli değil.

## <a name="specifying-validation-rules"></a>Doğrulama kuralları belirtme

Doğrulama kuralları türeyen bir sınıf oluşturarak belirtilir `IValidationRule<T>` aşağıdaki kod örneğinde gösterildiği arabirimi:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Bu arabirim bir doğrulama kuralı sınıfı sağlamalısınız belirtir bir `boolean` `Check` gerekli doğrulama gerçekleştirmek için kullanılan yöntem ve bir `ValidationMessage` özellik değeri olan görüntülenir doğrulama hata iletisi doğrulama başarısız olur.

Aşağıdaki örnekte gösterildiği kod `IsNotNullOrEmptyRule<T>` kullanıcı adı ve kullanıcı tarafından girilen parola doğrulaması yapmak için kullanılan doğrulama kuralı `LoginView` sahte hizmetlerini eShopOnContainers mobil uygulamaya kullanırken:

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

`Check` Yöntemi döndürür bir `boolean` değer bağımsız değişkeninde olup olmadığını belirten `null`boş veya yalnızca boşluk karakterlerinden oluşur.

Aşağıdaki kod örneğinde eShopOnContainers mobil uygulama tarafından kullanılmıyor olsa da, e-posta adreslerini doğrulamak için bir doğrulama kuralı gösterir:

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

`Check` Yöntemi döndürür bir `boolean` değeri bağımsız değişkeni geçerli bir eposta adresi olup olmadığını belirten. Bu değer bağımsız değişkeninde belirtilen normal ifade deseni ilk geçtiği için arama yaparak sağlanır `Regex` Oluşturucusu. Normal ifade deseni giriş dizesi içinde olup olmadığını bulundu değerini denetleyerek belirlenebilir `Match` nesnenin `Success` özelliği.

> [!NOTE]
> Özellik doğrulama bazen bağımlı özellikler içerebilir. Özellik b ayarlanmış belirli değeri geçerli değerleri kümesi özelliği a bağlı olduğunda, bağımlı özellikleri örneğidir A özelliğinin değeri izin verilen değerlerden biri olduğunu denetlemek için b özelliğinin değeri alınırken içerir Ayrıca, B özelliğinin değeri değiştiğinde özelliği A yeniden doğrulanır gerekir.

## <a name="adding-validation-rules-to-a-property"></a>Bir özellik için doğrulama kuralları ekleme

Türünde olmasını bildirilen eShopOnContainers mobil uygulama doğrulama gerektiren görünüm model özellikleri `ValidatableObject<T>`, burada `T` doğrulanacak veri türüdür. Aşağıdaki kod örneği iki gibi özellikleri örneği gösterilmektedir:

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

Doğrulama kuralları gerçekleşmesi doğrulama için eklenmesi gerekir `Validations` her koleksiyonunu `ValidatableObject<T>` , aşağıdaki kod örneğinde gösterildiği şekilde örneği:

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Bu yöntem ekler `IsNotNullOrEmptyRule<T>` doğrulama kuralı için `Validations` her koleksiyonunu `ValidatableObject<T>` doğrulama kuralının değerleri belirtme örneği `ValidationMessage` görüntülenir doğrulama hata iletisini belirtir özelliği doğrulama başarısız olur.

## <a name="triggering-validation"></a>Tetikleyici doğrulama

Bir özelliği değiştiğinde eShopOnContainers mobil uygulamada kullanılan doğrulama yaklaşım bir özelliğin doğrulama ve otomatik olarak tetikleyici doğrulama el ile tetikleyebilirsiniz.

### <a name="triggering-validation-manually"></a>Doğrulama el ile tetikleme

Doğrulama için bir görünüm modeli özellik el ile tetiklenebilir. Örneğin, kullanıcı dokunur geldiğinde eShopOnContainers mobil uygulamasında gerçekleşir **oturum açma** düğmesini `LoginView`, sahte hizmetlerini kullanırken. Komut temsilci çağrılarını `MockSignInAsync` yönteminde `LoginViewModel`, hangi çağırır doğrulama yürüterek `Validate` aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

`Validate` Yöntemi kullanıcı adı ve kullanıcı tarafından girilen parola doğrulaması gerçekleştirir `LoginView`, her doğrulama yöntemini çağıran tarafından `ValidatableObject<T>` örneği. Aşağıdaki kod örneğinde elde edilen doğrulama yöntemi gösterilir `ValidatableObject<T>` sınıfı:

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Bu yöntem temizler `Errors` toplama ve tüm doğrulama kuralları nesnenin eklendi alır `Validations` koleksiyonu. `Check` Her alınan doğrulama kuralı için yöntemi yürütüldüğünde ve `ValidationMessage` verileri doğrulamak için başarısız hiçbir doğrulama kuralı için özellik değeri eklenen `Errors` koleksiyonu `ValidatableObject<T>` örneği. Son olarak, `IsValid` özelliği ayarlanmış ve doğrulama başarılı veya başarısız olduğunu belirten arama yöntemi için bu değer döndürülür.

### <a name="triggering-validation-when-properties-change"></a>Özelliklerini değiştirdiğinizde tetikleme doğrulama

Bağlı bir özellik değiştiğinde doğrulama da otomatik olarak tetiklenir. Örneğin, iki yönlü bir bağlama zaman `LoginView` ayarlar `UserName` veya `Password` özelliği, doğrulama tetiklenir. Aşağıdaki kod örneği, bu nasıl gerçekleştiğini gösterir:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Denetim bağlanır `UserName.Value` özelliği `ValidatableObject<T>` örneği ve denetimin `Behaviors` gruplandırmasında bir `EventToCommandBehavior` kendisine eklenmiş örneği. Bu davranış yürütür `ValidateUserNameCommand` yanıt olarak [`TextChanged`] üzerinde tetikleme olay `Entry`, hangi durumlarda tetiklenir metinde `Entry` değişiklikler. Buna karşılık, `ValidateUserNameCommand` temsilcisini yürütür `ValidateUserName` yürütür yöntemi `Validate` yöntemi `ValidatableObject<T>` örneği. Bu nedenle, her kullanıcının girdiği bir karakter `Entry` girilen veri doğrulaması kullanıcı adı için denetimi gerçekleştirilir.

Davranışları hakkında daha fazla bilgi için bkz: [uygulama davranışları](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Doğrulama hataları görüntüleme

Kırmızı bir çizgi ile geçersiz veri içeren denetimi vurgulayarak eShopOnContainers mobil uygulama tüm doğrulama hatalarını kullanıcı bildirir ve kullanıcı neden bildiren bir hata iletisi görüntüleyerek veri denetimi içeren aşağıda geçersiz Geçersiz veri. Geçersiz veri düzeltildiğinde, satır siyah olarak değiştirir ve hata iletisi kaldırılır. Doğrulama hataları mevcut olduğunda Şekil 6-2 eShopOnContainers mobil uygulamaya (link) gösterir.

![](validation-images/validation-login.png "Oturum açma sırasında doğrulama hataları görüntüleme")

**Şekil 6-2:** oturum açma sırasında doğrulama hataları görüntüleme

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Geçersiz veri içeren bir denetim vurgulama

`LineColorBehavior` Ekli davranışı vurgulamak için kullanılan [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetimleri nerede doğrulama hataları oluştu. Aşağıdaki örnekte gösterildiği kod nasıl `LineColorBehavior` ekli davranışı eklendiği bir `Entry` denetimi:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP, WinRT, WinPhone" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Denetimi aşağıdaki kod örneğinde gösterildiği bir açık stili kullanır:

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Bu stilini ayarlar `ApplyLineColor` ve `LineColor` özelliklerini bağlı `LineColorBehavior` davranışı üzerinde bağlı [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetim. Stilleri hakkında daha fazla bilgi için bkz: [stilleri](~/xamarin-forms/user-interface/styles/index.md).

Zaman değeri `ApplyLineColor` iliştirilmiş bir özelliktir kümesi ya da değişiklikler, `LineColorBehavior` ekli davranışı yürütür `OnApplyLineColorChanged` aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Bu yöntem parametrelerini davranışı iliştirildiği denetiminin örneği ve eski ve yeni değerleri sağlayın `ApplyLineColor` özelliği eklenmiş. `EntryLineColorEffect` Sınıfı denetimin eklenen [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu, `ApplyLineColor` ekli özellik `true`, denetimin kaldırılmadan Aksi takdirde `Effects` koleksiyonu. Davranışları hakkında daha fazla bilgi için bkz: [uygulama davranışları](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

`EntryLineColorEffect` Alt sınıfların [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) sınıfı ve aşağıdaki kod örneğinde gösterilir:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) Sınıfı, platforma özgü bir iç efekt sarmalar platformdan bağımsız etkisi temsil eder. Tür bilgisi için bir platforma özgü etkisi hiçbir derleme zamanı erişimi olduğundan etkisi kaldırma işlemi basitleştirir. `EntryLineColorEffect` Çözümleme grup adı ve her platforma özgü etkisi sınıf içinde belirtilmiş benzersiz kimliği birleşimini oluşan bir parametre olarak geçirme temel sınıf oluşturucu çağırır.

Aşağıdaki örnekte gösterildiği kod `eShopOnContainers.EntryLineColorEffect` iOS için uygulama:

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

`OnAttached` Yöntemi alır yerel denetimi için Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetlemek ve çizgi rengini çağırarak güncelleştirmeleri `UpdateLineColor` yöntemi. `OnElementPropertyChanged` Geçersiz kılma üzerinde bağlanabilirse özellik değişikliklerini yanıt `Entry` varsa çizgi rengini güncelleştirerek denetim ekli `LineColor` özellik değişikliklerini veya [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) özelliği`Entry`değişiklikler. Etkileri hakkında daha fazla bilgi için bkz: [efektler](~/xamarin-forms/app-fundamentals/effects/index.md).

Ne zaman geçerli veri girilen [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetimi, onu geçerli siyah bir çizgi hiçbir doğrulama hatası olduğunu belirtmek için bu denetimin altına. Şekil 6-3 bunun bir örneğini gösterir.

![](validation-images/validation-blackline.png "Doğrulama hatası olmaması gösteren siyah bir çizgi")

**Şekil 6-3**: doğrulama hatası olmaması gösteren siyah bir çizgi

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Denetimi de sahip bir [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) eklenen kendi [ `Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) koleksiyonu. Aşağıdaki örnekte gösterildiği kod `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

Bu [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) izleyiciler `UserName.IsValid` özellik ve değer ise olur `false`, yürütülmeden [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/), hangi değişikliklerin `LineColor` bağlı özelliği `LineColorBehavior` davranışı kırmızı bağlı. Şekil 6-4 bunun bir örneğini gösterir.

![](validation-images/validation-redline.png "Doğrulama hatası belirten kırmızı çizgi")

**Şekil 6-4**: doğrulama hatası belirten kırmızı çizgi

Satırda [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) girilen veriler geçersiz durumdayken denetim kırmızı kalır, aksi takdirde girilen verilerin geçerli olduğunu belirtmek için siyah olarak değişir.

Tetikleyiciler hakkında daha fazla bilgi için bkz: [Tetikleyicileri](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Hata iletilerini görüntüleme

Kullanıcı arabirimini etiket denetimleri verisini doğrulanamadı her denetim altındaki bir doğrulama hata iletisi görüntüler. Aşağıdaki örnekte gösterildiği kod [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) görüntüleyen bir doğrulama hatası iletisini kullanıcı geçerli bir kullanıcı adı girilmezse:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Her [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) bağlar `Errors` özelliği Doğrulanmakta olan Görünüm model nesnesi. `Errors` Özelliği tarafından sağlanan `ValidatableObject<T>` sınıfı ve türü `List<string>`. Çünkü `Errors` özelliği, birden çok doğrulama hataları içerebilir `FirstValidationErrorConverter` örnek koleksiyonundan görüntülenmesi için ilk hata almak için kullanılır.

## <a name="summary"></a>Özet

EShopOnContainers mobil uygulama görünüm modeli özelliklerinin eşzamanlı istemci tarafı doğrulama gerçekleştirir ve tüm doğrulama hatalarını kullanıcı geçersiz veri içeren denetimi vurgulama ve kullanıcıyı bilgilendirmek hata iletilerini göstererek bildirir neden verileri geçersiz.

Doğrulama gerektiren görünüm modeli özelliklerdir türü `ValidatableObject<T>`ve her `ValidatableObject<T>` örneği sahip eklenen doğrulama kuralları, `Validations` özelliği. Doğrulama çağrılır görünüm modelden çağırarak `Validate` yöntemi `ValidatableObject<T>` doğrulama alır örneği kuralları ve bunlara karşı yürütür `ValidatableObject<T>` `Value` özelliği. Tüm doğrulama hatalarını içine yerleştirilen `Errors` özelliği `ValidatableObject<T>`örneği ve `IsValid` özelliği `ValidatableObject<T>` örneği, doğrulama başarılı veya başarısız olup olmadığını belirtmek üzere güncelleştirilir.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
