---
title: Model-View-ViewModel desenini
description: Bu bölümde, MVVM düzenini hizmetine mobil uygulamayı düzgün bir şekilde iş ve bir sunu mantıksal uygulamanın, kullanıcı arabiriminden ayırmak için nasıl kullandığını açıklar.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: c947ec0c2fffbd9038ee58211c77bd947c445b6e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998448"
---
# <a name="the-model-view-viewmodel-pattern"></a>Model-View-ViewModel desenini

Xamarin.Forms geliştirici deneyimi, genellikle XAML içinde bir kullanıcı arabirimi oluşturmak ve ardından, kullanıcı arabiriminde çalışan arka plan kod ekleyerek içerir. Uygulama değiştirilir ve boyut ve kapsam içinde büyütme gibi karmaşık bakım sorunları ortaya çıkabilir. Bu sorunlar, UI denetimleri ve kullanıcı Arabirimi değişiklikleri ve bu tür kod birim testi zorluk maliyetini artıran iş mantığı arasında sıkı eşleştirme içerir.

Model-View-ViewModel (MVVM) desenini düzgün bir şekilde iş ve bir sunu mantıksal uygulamanın, kullanıcı arabiriminden (UI) ayrı yardımcı olur. Uygulama mantığı ve UI arasında NET bir ayrım koruma çok sayıda geliştirme sorunları gidermeye yardımcı olur ve uygulama sınamak için korumak ve geliştirmek daha kolay hale getirebilirsiniz. Kodu yeniden kullanma fırsatlarını büyük ölçüde geliştirebilir ve geliştiricilerin sağlar ve daha fazla bilgi için kullanıcı Arabirimi tasarımcıları, ilgili bölümleri Uygulama geliştirirken kolayca işbirliği yapın.

## <a name="the-mvvm-pattern"></a>MVVM düzenini

MVVM desen üç temel bileşeni vardır: model görünümü ve görünüm modeli. Her, farklı bir amaca hizmet eder. Şekil 2-1, üç bileşeni arasındaki ilişkiler gösterilmektedir.

![](mvvm-images/mvvm.png "MVVM düzenini")

**Şekil 2-1**: MVVM desen

Her bileşenleri sorumluluklarını anlamanın yanı sıra, birbirleri ile nasıl etkileşimde bulunduğunu anlamak önemlidir. Yüksek bir düzeyde görünümü "Görünüm modeli bildiği" Görünüm modeli "modeli bildiği" ancak model görünüm modeli farkında değildir ve görünüm modeli görünümünü farkında değildir. Bu nedenle, görünüm modeli modeli görünümünden yalıtır ve Görünüm bağımsız olarak yararlanmayan modeli sağlar.

MVVM düzenini kullanmanın avantajları şunlardır:

-   Mevcut iş mantığı kapsülleyen var olan model uygulaması ise, zor veya değiştirmek için riskli olabilir. Bu senaryoda, görünüm modeli model sınıfları için bir bağdaştırıcı görür ve modeli kodu herhangi bir önemli değişiklik yapmadan kaçınmanızı sağlar.
-   Geliştiriciler, görünüm kullanılmadan görünüm modeli ve modelin için birim testleri oluşturabilirsiniz. Görünüm modeli için birim testleri, tam olarak aynı işlevleri görünümü tarafından kullanılan alıştırma yapabilirsiniz.
-   Görünüm tamamen XAML içinde uygulanan koşuluyla uygulama UI koda dokunmadan tasarlanması. Bu nedenle, görünüm yeni bir sürümü mevcut görünüm model ile çalışması gerekir.
-   Tasarımcılar ve geliştiriciler bileşenleri üzerinde geliştirme sürecinde bağımsız olarak ve eşzamanlı olarak çalışabilir. Geliştiricilerin model bileşenleri ve görünüm model üzerinde çalışabilir tasarımcıları görünümü üzerinde odaklanabilirsiniz.

MVVM etkili bir şekilde kullanmanın anahtarı, uygulama kodu doğru sınıflara faktörü nasıl anlamak ve sınıfların nasıl etkileşimde bulunduğunu anlamak arasındadır. Aşağıdaki bölümlerde, her birinin MVVM düzenini sınıflarda sorumlulukları açıklanmaktadır.

### <a name="view"></a>Görüntüle

Görünüm, yapısı, Düzen ve ekranda hangi kullanıcının gördüğü görünümünü tanımlamaktan sorumludur. İdeal olarak her görünümün bir sınırlı iş mantığı içermeyen gerideki kod ile XAML içinde tanımlanır. Ancak, bazı durumlarda, animasyonları gibi XAML ifade etmek zordur visual davranışı uygulayan kullanıcı Arabirimi mantığı arka plan kod içerebilir.

Xamarin.Forms uygulamada, genellikle bir görünümü olan bir [ `Page` ](xref:Xamarin.Forms.Page)-türetilmiş veya [ `ContentView` ](xref:Xamarin.Forms.ContentView)-türetilmiş sınıf. Ancak, görünümleri ayrıca bir nesne görüntülendiğinde görsel olarak göstermek için kullanılacak olan kullanıcı Arabirimi öğeleri belirten bir veri şablonu tarafından temsil edilebilir. Veri şablonu bir görünüm olarak tüm arka plan kod yok ve bir özel görünüm modeli türüne bağlamak için tasarlanmıştır.

> [!TIP]
> Arka plan kod içinde UI öğelerini devre dışı bırakma ve etkinleştirme kaçının. Görünüm modelleri görünümün görüntüleme, bir komut olup veya bir işlem beklemede göstergesidir gibi bazı yönlerini etkilemek mantıksal durum değişikliklerini tanımlamak için sorumlu olduğundan emin olun. Bu nedenle, etkinleştirme ve kullanıcı Arabirimi öğeleri model özellikleri yerine etkinleştirme ve gerideki kod devre dışı bırakma görüntülemek için bağlama tarafından devre dışı.

Bir düğmeye tıklatma gibi yanıt etkileşimleri görünüm, görünüm modeli kodu yürüten veya öğe seçimi için birkaç seçenek vardır. Denetim komutları, denetime ait destekliyorsa `Command` özelliği veri bağlama için bir `ICommand` özelliği görünüm modeli. Denetimin komut çağrıldığında, görünüm modeli kodda yürütülecek. Ek komutlar davranışları görünümünde bir nesneye bağlı ve çağrılacak komut veya tetiklenecek olayı için dinleyebilirsiniz. Yanıt olarak davranışı ardından çağırabilirsiniz bir `ICommand` görünüm modeli veya bir yöntem görünüm modeli.

### <a name="viewmodel"></a>ViewModel

Görünüm modeli, özellikler ve komutlar view için veri bağlamak için uygular ve görünüm durumu değişiklikleri değişiklik bildirimi olaylar ile uyarır. Özellikler ve görünüm modeli sağlayan komutlar UI tarafından sunulacak işlevselliği tanımlamak, ancak görünümü işlevselliğini nasıl görüntüleneceğini belirler.

> [!TIP]
> Hızlı yanıt veren zaman uyumsuz işlemleri ile UI tutun. Mobil uygulamalar, performans, bir kullanıcının algılamasını iyileştirmek için engeli kaldırılmış UI iş parçacığı tutmanız gerekir. Bu nedenle, görünüm modeli g/ç işlemleri için zaman uyumsuz yöntemleri kullanın ve özellik değişiklikleri görünümlerini zaman uyumsuz olarak bildirmek için olaylar da oluşturur.

Görünüm modeli için gerekli olan herhangi bir model sınıfları görünümün etkileşim koordine sorumludur. Genellikle görünüm modeli ile model sınıfları arasındaki bir-çok ilişkisi yoktur. Görünüm modeli denetimleri görünümünde doğrudan için veri bağlama böylece model sınıfları görüntülemek için doğrudan kullanıma sunmak isteyebilirsiniz. Bu durumda, model sınıfları veri bağlamayı destekler ve bildirim olayları değiştirmek için tasarlanmış olması gerekir.

Her görünüm modeli görünümü bir kolayca tüketebileceği bir formda bir model verileri sağlar. Bunu yapmak için veri dönüştürme bazen görünüm modeli gerçekleştirir. Görünümü bağlayabilirsiniz özellikleri sağlar çünkü bu veri dönüştürme görünüm modelinde yerleştirmek iyi bir fikirdir. Örneğin, görünüm modeli görüntülemek için Görünüm tarafından kolaylaştırmak için iki özellik değerlerini birleştirebilirsiniz.

> [!TIP]
> Veri dönüştürme dönüştürme katmanındaki tek bir merkezden yönetin. Görünüm modeli ve görünüm arasında yer alan ayrı bir veri dönüştürme katmanı olarak dönüştürücüler kullanmak da mümkündür. Örneğin, veri görünüm modeli sağlamaz özel biçimlendirme gerektirdiğinde bu gerekli olabilir.

Görünümü ile iki yönlü veri bağlaması katılmak görünüm modeli için sırada özelliklerini oluşturmalıdır `PropertyChanged` olay. Görünüm modelleri uygulayarak bu gereksinimi karşılayan `INotifyPropertyChanged` arabirim ve oluşturma `PropertyChanged` bir özelliği değiştirildiğinde olay.

Koleksiyon görünümü dostu `ObservableCollection<T>` sağlanır. Bu koleksiyon değiştirilmiş toplama bildirim, geliştirici uygulamak zorunda yoğun uygulayan `INotifyCollectionChanged` koleksiyonlarda arabirimi.

### <a name="model"></a>Model

Model, uygulama verilerini kapsayan görsel olmayan sınıflar sınıflardır. Bu nedenle, model, genellikle iş ve Doğrulama mantığı ile birlikte bir veri modeli içeren uygulama etki alanı modeli temsil eden olarak düşünülebilir. Veri aktarımı nesneleri (Dto), düz eski CLR nesnelerini (POCOs) ve oluşturulan varlığı ve proxy nesneleri model nesneleri örnekleridir.

Model sınıfları, genellikle Hizmetleri ya da veri erişimi ve önbelleğe alma kapsülleyen depoları ile birlikte kullanılır.

## <a name="connecting-view-models-to-views"></a>Görünüm için Görünüm modelleri bağlanma

Görünüm modelleri, görünümleri Xamarin.Forms veri bağlama özellikleri kullanarak bağlanabilir. Görünümleri oluşturmak ve modelleri görüntüleyin ve çalışma zamanında ilişkilendirmek için kullanılan birçok yaklaşım vardır. Bu yaklaşımların bilinen Görünüm ilk oluşumunu ve görünüm modeli ilk oluşumunu iki kategoriye ayrılır. Görünüm ilk oluşumunu ile ilk bileşim modeli tercihi ve karmaşıklık söz konusu görünümü arasında seçim yapma. Ancak, kendi BindingContext özelliğe atanmış bir görünüm modeli görünüme için aynı bir yandan tüm yaklaşımlardan paylaşır.

Görünüm ile ilk bileşim uygulama kavramsal olarak, bağlı oldukları görünüm modelleri bağlanan görünümlerinin oluşur. Bu yaklaşımın birincil avantajı, görünüm modelleri, görünümleri hiçbir bağımlılığı olduğundan gevşek oluşturmak kolayca birim test edilebilir uygulamalar sağlar, ' dir. Görsel yapısı aşağıdaki yerine sınıfları nasıl oluşturulduğunu ve ilişkili anlamak için kod yürütmeyi izlemek zorunda yapısını anlamak kolaydır. Ayrıca, karmaşık ve platformuyla yanlış hizalanmış bir görünüm modeli ilk oluşumunu getiren Gezinti oluştuğunda oluşturma sayfaları için sorumlu Xamarin.Forms gezinti sistemi Görünüm ilk yapım hizalar.

Görünüm ile ilk model oluşturma uygulama kavramsal olarak görünüm modelleri, görünüm için Görünüm modeli konumlandırmak için sorumlu olan bir hizmet ile oluşur. Görünüm oluşturma yerine, uygulama UI olmayan mantıksal yapısını odaklanmanıza olanak soyutlanması olduğundan görünüm modeli ilk oluşumunu bazı geliştiriciler için daha doğal. Ayrıca, diğer görünüm modelleri tarafından oluşturulması görünüm modelleri sağlar. Ancak, bu yaklaşım genellikle karmaşıktır ve uygulama çeşitli kısımlarını nasıl oluşturulduğunu ve ilişkili anlamak zor olabilir.

> [!TIP]
> Görünüm modelleri ve görünümleri bağımsız tutun. Görünüm veri kaynağındaki bir özelliğe bağlama görünümün, karşılık gelen görünüm modeli asıl bağımlılığı olmalıdır. Görünüm türleri gibi özellikle başvurmuyor [ `Button` ](xref:Xamarin.Forms.Button) ve [ `ListView` ](xref:Xamarin.Forms.ListView), görünüm modellerinden. Burada özetlenen ilkeleri uygulayarak, görünüm modelleri yalıtım, bu nedenle kapsamı sınırlayarak yazılım kusurlarını olasılığını azaltır, test edilebilir.

Aşağıdaki bölümlerde, görünüm için Görünüm modelleri bağlanmaya ana yaklaşım açıklanmaktadır.

### <a name="creating-a-view-model-declaratively"></a>Bir görünüm modeli bildirimli olarak oluşturma

XAML, karşılık gelen görünüm modelinde bildirimli olarak örneklemek görünümün en basit yaklaşımdır bakın. Görünüm oluşturulduğunda, karşılık gelen görünüm modeli nesnesi de oluşturulur. Bu yaklaşım aşağıdaki kod örneğinde gösterilmiştir:

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

Zaman [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) oluşturulur, örneği `LoginViewModel` otomatik olarak oluşturulur ve görünümün ayarlamak [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext).

Bu bildirim temelli yapım ve görünüm modeli görünümü tarafından ataması, basit bir işlemdir ancak görünüm modeli varsayılan (parametresiz) Oluşturucu gerektirdiğini olumsuz sahip avantajı vardır.

### <a name="creating-a-view-model-programmatically"></a>Bir görünüm modeli program aracılığıyla oluşturma

Bir görünümü atanan görünüm modeli sonuçlanır arka plan kod dosyasında kod olabilir, [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) özelliği. Bu genellikle görünümün oluşturucusunun içinde aşağıdaki kod örneğinde gösterildiği gibi gerçekleştirilir:

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

Programlı oluşturma ve atama görünümün arka plan kod içinde görünüm modeli, basit olduğunu avantajı vardır. Ancak, bu yaklaşım dezavantajı görünümü Görünüm modeli ile tüm gerekli bağımlılıkları sağlamak gereken olur. Bir bağımlılık ekleme kapsayıcısını kullanarak görünümü ve görünüm modeli eşlemeyi gevşek korunmasına yardımcı olabilir. Daha fazla bilgi için [bağımlılık ekleme](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

### <a name="creating-a-view-defined-as-a-data-template"></a>Veri şablonu tanımlanmış bir görünüm oluşturma

Görünüm veri şablon olarak tanımlanabilir ve bir görünüm modeli türü ile ilişkili. Veri şablonları kaynaklar olarak tanımlanabilir veya Görünüm modeli görüntüler denetimi içinde satır içi olabilirler. Denetimin içeriğini görünüm modeli örnektir ve veri şablonunun görsel olarak göstermek için kullanılır. Bu teknik, görünüm modeli oluşturulması görünüm tarafından izlenen ilk olarak, örneği bir durum örneğidir.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>Otomatik olarak bir görünüm modeli bulucuyla bir görünüm modeli oluşturma

Bir görünüm modeli Bulucu görünüm modelleri ve bunların görünümlere örneğinin yöneten özel bir sınıftır. Hizmetine mobil uygulamasında `ViewModelLocator` sınıfında ekli özelliği, `AutoWireViewModel`, görünüm modelleri, görünümleri ile ilişkilendirmek için kullanılır. Görünüm'ün XAML içinde ekli bu özellik, görünüm modeli görünüme otomatik olarak bağlanmalıdır aşağıdaki kod örneğinde gösterildiği gibi belirtmek için true olarak ayarlanır:

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel` Özelliği false olarak başlatılır ve değeri değiştiğinde bir bağlanılabilir özellik `OnAutoWireViewModelChanged` olay işleyicisi çağrılır. Bu yöntem görünüm için Görünüm modeli çözümler. Aşağıdaki kod örneği bunu nasıl elde edildiğini gösterir:

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

`OnAutoWireViewModelChanged` Kural tabanlı bir yaklaşım kullanarak görünüm modeli çözmek yöntem çalışır. Bu kuralı olduğunu varsayar:

-   Görünüm modelleri görünüm türleri aynı bütünleştirilmiş kod için geçerlidir.
-   Görünümleri olan bir. Görünüm alt ad alanı.
-   Görünüm modelleri olan bir. Viewmodel'lar alt ad alanı.
-   Görünüm modeli adları karşılık gelen görünüm adları ile ve "ViewModel" ile bitmelidir.

Son olarak, `OnAutoWireViewModelChanged` yöntemi kümeleri [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) çözümlenen görünüm model türü için Görünüm türü. Görünüm modeli türü çözümleme hakkında daha fazla bilgi için bkz. [çözümleme](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution).

Bu yaklaşım, bir uygulama için Görünüm modelleri ve görünümlere kendi bağlantı örneğinin sorumlu tek bir sınıf olduğunu avantajına sahiptir.

> [!TIP]
> Bir görünüm modeli Bulucu değiştirme kolaylığı için kullanın. Bir görünüm modeli Bulucu da değişim bağımlılık diğer uygulamaları için bir nokta olarak gibi birim testi veya tasarım zamanı veri için kullanılabilir.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>Güncelleştirme görünümleri temel değişikliklere yanıt olarak Model veya Model görüntüleme

Tüm görünüm modeli ve bir görünüm için erişilebilir olan model sınıfları uygulamalıdır `INotifyPropertyChanged` arabirimi. Bir görünüm modeli veya model sınıfı bu arabirimi uygulayan temel özellik değeri değiştiğinde görünümünde herhangi bir veriye bağlı denetimler için değişiklik bildirimleri sağlamak sınıf sağlar.

Uygulamalar için doğru kullanımını özellik değişikliği bildirimi, aşağıdaki gereksinimleri karşılayarak desteklemesi:

-   Her zaman yükseltme bir `PropertyChanged` bir genel özelliğinin değeri değiştiğinde bir olay. Bu yükseltme varsaymayın `PropertyChanged` olay nedeniyle XAML bağlama nasıl gerçekleştirildiğini bilgisi gözardı.
-   Her zaman yükseltme bir `PropertyChanged` herhangi bir olay görünümünde diğer özellikleri tarafından kullanılan değerleri özellikleri hesaplanan modeli veya modeli.
-   Her zaman yükseltme `PropertyChanged` bir özelliği değiştirmek yapar veya ne zaman nesne bilinen güvenli bir durumda olmasını yönteminin sonunda olay. Olayı eş zamanlı olarak olay işleyicileri çağırarak işlemi kesintiye uğratır. Bir işlem gerçekleştiriyorsanız böyle bir durumda bir güvenli, kısmen güncelleştirilmiş durumda olduğunda bir nesneye geri çağırma işlevleri doğurabilir. Ayrıca, geçişli değişikliklerin tarafından tetiklenmesi olası `PropertyChanged` olayları. Geçişli değişiklikleri genellikle basamaklı değişiklik yürütmek güvenli hale gelmeden önce tamamlanması için güncelleştirmeleri gerektirir.
-   Asla oluşturma bir `PropertyChanged` özelliği değişmezse olay. Tetiklenmeden önce eski ve yeni değerleri karşılaştırmanız gerekir yani `PropertyChanged` olay.
-   Asla oluşturma `PropertyChanged` olayı sırasında bir özellik başlatırken, bir görünüm Modeli Oluşturucu. Verilere bağlı denetimler görünümünde bu noktada değişiklik bildirimleri almak için abone değil.
-   Hiçbir zaman birden fazla yükseltme `PropertyChanged` olay tek bir zaman uyumlu çağırma bir sınıfın ortak bir yöntemin içinde aynı özellik adı bağımsız değişkeni. Örneğin, belirtilen bir `NumberOfItems` özelliği olan yedekleme deposu `_numberOfItems` yöntemi artırır, alan `_numberOfItems` elli kez döngü yürütülmesi sırasında bunu yalnızca özellik değişikliği bildirimi üzerinde tetiklemelidir `NumberOfItems` özelliği bir kez İş tamamlandıktan sonra. Zaman uyumsuz yöntemler için yükseltmek `PropertyChanged` olayı için her zaman uyumlu bir zaman uyumsuz bir devamlılık zincirini parçası olarak belirtilen özellik adı.

Hizmetine mobil uygulama kullandığı `ExtendedBindableObject` sınıfı sağlamak için değişiklik bildirimleri, aşağıdaki kod örneğinde gösterilen:

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

Xamarin.Form'ın [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) sınıfının Implements `INotifyPropertyChanged` arabirim ve sağlayan bir [ `OnPropertyChanged` ](xref:Xamarin.Forms.BindableObject.OnPropertyChanged(System.String)) yöntemi. `ExtendedBindableObject` Sağlar sınıfını `RaisePropertyChanged` özelliği çağrılacak yöntem bildirimi değiştirme ve bunu yaparken tarafından sağlanan işlevselliği kullandığı `BindableObject` sınıfı.

Hizmetine mobil uygulamasında her görünüm modeli sınıfın türetildiği `ViewModelBase` sırayla türetilen sınıf `ExtendedBindableObject` sınıfı. Bu nedenle, her görünüm model sınıfı kullanır `RaisePropertyChanged` yönteminde `ExtendedBindableObject` özellik değişikliği bildirimi sağlamak için sınıf. Aşağıdaki kod örneği, bir lambda ifadesi kullanarak özellik değişikliği bildirimi hizmetine mobil uygulamayı nasıl çağırır gösterir:

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

Bir lambda ifadesi bu şekilde kullanılması lambda ifadesindeki her çağrı için değerlendirilecek olduğundan maliyet küçük bir performans içerdiğini unutmayın. Performans maliyeti, küçük ve uygulama normal şekilde etkiler değil olsa da, birçok değişiklik bildirimleri olduğunda maliyet tahakkuk. Ancak, bu yaklaşımın avantajı, derleme zamanı tür güvenliği ve yeniden düzenleme desteği özellikleri yeniden adlandırılırken sağlamasıdır.

## <a name="ui-interaction-using-commands-and-behaviors"></a>Komutlar ve davranışları kullanarak kullanıcı Arabirimi etkileşimi

Mobil uygulamalar, Eylemler gibi arka plan kod dosyasında bir olay işleyicisi oluşturarak uygulanabilir düğmesini tıklatın, bir kullanıcı eylemi için yanıt genellikle çağrılır. Ancak, MVVM düzenini görünüm model ile eylemi uygulamak için sorumluluk arasındadır ve arka plan kod yerleştirme kodda kaçınılmalıdır.

Komutlar, kullanıcı arabiriminde denetimlere bağlı eylemleri temsil etmek için kullanışlı bir yol sağlar. Bunlar eylemi uygulayan kod kapsüllemek ve görünümünde görsel kendi gösterimden ayrılmış kalmasını sağlamak için Yardım. Kullanıcı denetimle etkileşim kurduğunda bu denetimleri komutu çağırır ve Xamarin.Forms bildirimli olarak komuta bağlı denetimleri içerir.

Davranışlar, komut bildirimli olarak bağlanması denetimler de izin verir. Ancak, davranışları denetimi tarafından oluşturulan olayları aralığı ile ilişkili bir eylemi çağırmak için kullanılabilir. Bu nedenle, davranışları denetimleri, komut etkin olarak aynı senaryoların çoğunun yüksek düzeyde esneklik ve denetim sağlarken adresi. Ayrıca, davranışları komut nesneleri veya yöntemleri komutları ile etkileşim kurmak için özel olarak tasarlanmamıştır denetimleri ile ilişkilendirmek için de kullanılabilir.

### <a name="implementing-commands"></a>Uygulama komutları

Görünüm modelleri uygulayan nesne örneklerini komut özellikleri, görünümden bağlama için genellikle kullanıma `ICommand` arabirimi. Xamarin.Forms denetimleri çok sayıda sağlar bir `Command` verilerin özelliğe bağlı bir `ICommand` görünüm modeli tarafından sağlanan nesne. `ICommand` Arabirimi tanımlar bir `Execute` işlemi kapsülleyen yöntemi bir `CanExecute` komutu çağrılabilir olup olmadığını ve bir gösteren yöntemi `CanExecuteChanged` etkileyen olup değişiklikler olduğunda gerçekleşen olay komutun çalışacağını belirtir. [ `Command` ](xref:Xamarin.Forms.Command) Ve [ `Command<T>` ](xref:Xamarin.Forms.Command) Xamarin.Forms tarafından sağlanan sınıfları uygulayan `ICommand` arabirimi, burada `T` bağımsızdeğişkentürü`Execute`ve `CanExecute`.

Bir görünüm modeli içinde olması gerektiğini türünde bir nesne [ `Command` ](xref:Xamarin.Forms.Command) veya [ `Command<T>` ](xref:Xamarin.Forms.Command) her bir ortak özellik türü, görünüm modeli için `ICommand`. `Command` Veya `Command<T>` Oluşturucusu gerektirir bir `Action` ne zaman çağrılan geri çağırma nesnesi `ICommand.Execute` yöntemi çağrılır. `CanExecute` Yöntemi bir isteğe bağlı Oluşturucu parametresi ve bir `Func` döndüren bir `bool`.

Aşağıdaki kodda gösterildiği nasıl bir [ `Command` ](xref:Xamarin.Forms.Command) kayıt komutu temsil eden örneği oluşturulan bir temsilciye belirterek `Register` görüntüleme modeli yöntemi:

```csharp
public ICommand RegisterCommand => new Command(Register);
```

Komut görünüme bir başvuru döndürür bir özelliği aracılığıyla kullanıma sunulan bir `ICommand`. Zaman `Execute` yöntemi çağrıldığında [ `Command` ](xref:Xamarin.Forms.Command) nesnesi, bunu yalnızca iletir görünüm modeli aracılığıyla belirtilen temsilci yöntemi çağrısı `Command` Oluşturucusu.

Zaman uyumsuz bir yöntem kullanarak bir komut tarafından çağrılabilir `async` ve `await` komut belirtirken anahtar sözcükleri `Execute` temsilci. Bu geri çağırma olduğunu belirten bir `Task` ve bekleniyor olabilir. Örneğin, aşağıdaki gösterildiği nasıl kod bir [ `Command` ](xref:Xamarin.Forms.Command) bir oturum açma komutu temsil eder, örneğin, bir temsilciye belirterek yapılandırılmıştır `SignInAsync` görüntüleme modeli yöntemi:

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

Parametreleri geçilebilir `Execute` ve `CanExecute` kullanarak Eylemler [ `Command<T>` ](xref:Xamarin.Forms.Command) komutu oluşturmak için sınıf. Örneğin, aşağıdaki gösterildiği nasıl kod bir `Command<T>` göstermek için kullanılan örnek `NavigateAsync` yöntemi türünde bir bağımsız değişken gerektirir `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

Hem de [ `Command` ](xref:Xamarin.Forms.Command) ve [ `Command<T>` ](xref:Xamarin.Forms.Command) sınıfları, temsilciye `CanExecute` her Oluşturucu yönteminde, isteğe bağlıdır. Bir temsilci belirtilmezse, `Command` döndüreceği `true` için `CanExecute`. Ancak, görünüm modeli, komutun bir değişiklik gösterebilir `CanExecute` çağırarak durumu `ChangeCanExecute` metodunda `Command` nesne. Bu neden `CanExecuteChanged` olay oluşturulur. Komutuna bağlı tüm denetimler kullanıcı arabiriminde, ardından etkinleştirilmiş durumlarını verilere bağlı komutu kullanılabilirliğini yansıtacak şekilde güncelleştirilir.

#### <a name="invoking-commands-from-a-view"></a>Komutları bir görünümden çağırma

Aşağıdaki örnekte gösterildiği nasıl kod bir [ `Grid` ](xref:Xamarin.Forms.Grid) içinde `LoginView` bağlar `RegisterCommand` içinde `LoginViewModel` sınıfı kullanarak bir [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) örneği:

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

Bir komut parametresi de isteğe bağlı olarak kullanılarak tanımlanabilir [ `CommandParameter` ](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) özelliği. Beklenen değişken türü belirtilen `Execute` ve `CanExecute` hedef yöntemleri. [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Kullanıcının eklenen bir denetimle etkileşim kurduğunda otomatik olarak hedef komutu çağırır. Komutun komut parametresi belirtilmezse, bağımsız değişken olarak geçirilen `Execute` temsilci.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>Davranışları uygulama

Davranışlar alt sınıfı için bunları zorunda kalmadan bir UI denetimine eklenecek işlevselliği sağlar. Bunun yerine, işlevselliği bir davranış sınıfta uygulanır ve bu denetimi bir parçası olarak ise denetimine bağlı. Davranışları doğrudan API denetimin kısaca denetimine bağlı ve olması için yeniden kullanılması, birden fazla görünümü veya uygulama paketlenmiş emin bir şekilde etkileşimde bulunduğundan genellikle arka plan, kod yazmanız gerekir kodu uygulamalarına olanak sağlar. MVVM bağlamında davranışları komutları denetimlerini bağlamak için kullanışlı bir yaklaşım olan.

Ekli özellikler aracılığıyla bir denetimine bağlı bir davranış olarak bilinen bir *davranışı bağlı*. Davranışı ardından bu denetime veya görsel ağaç görünümünün başka denetimler de işlevselliği eklemek için bağlı öğenin açık bir API de kullanabilirsiniz. Hizmetine mobil uygulamayı içeren `LineColorBehavior` sınıfını ekli bir davranıştır. Bu davranış hakkında daha fazla bilgi için bkz. [doğrulama hataları görüntüleme](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors).

Türetilen bir sınıf bir Xamarin.Forms davranıştır [ `Behavior` ](xref:Xamarin.Forms.Behavior) veya [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) sınıfı burada `T `davranışı uygulanacağını denetim türü. Bu sınıfları sağlar `OnAttachedTo` ve `OnDetachingFrom` yöntemleri davranışı bağlı ve ayrılmış denetimlerden yürütülecek mantığını sağlamak için geçersiz kılınmalıdır.

Hizmetine mobil uygulamasında `BindableBehavior<T>` sınıf türetilir [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) sınıfı. Amacı `BindableBehavior<T>` sınıfı, temel sınıf için gerektiren Xamarin.Forms davranışları sağlamak için [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) ekli denetime ayarlamak için davranış.

`BindableBehavior<T>` Geçersiz kılınabilir bir sağlar sınıfını `OnAttachedTo` ayarlar yönteminin [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) davranış ve geçersiz kılınabilir bir `OnDetachingFrom` temizler yöntemi `BindingContext`. Ayrıca, eklenen denetimi başvuru sınıfı depolar `AssociatedObject` özelliği.

Hizmetine mobil uygulamayı içeren bir `EventToCommandBehavior` yanıt, bir olayın gerçekleşmesi için bir komut yürüttüğünde sınıfı. Bu sınıfın türetildiği `BindableBehavior<T>` davranışı bağlamak ve yürütme sınıfı bir `ICommand` tarafından belirtilen bir `Command` davranışı kullanıldığında özelliği. Aşağıdaki örnekte gösterildiği kod `EventToCommandBehavior` sınıfı:

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

`OnAttachedTo` Ve `OnDetachingFrom` kaydetmek ve bir olay işleyicisi için tanımlanan olay kaydını silmek için kullanılan yöntemleri `EventName` özelliği. Ardından, olay başlatıldığında `OnFired` yöntemi çağrılır, komutu yürütür.

Kullanmanın avantajı `EventToCommandBehavior` bir olay oluşturulduğunda, bir komut yürütmek için olan komutları komutları ile etkileşime geçmek için tasarlanmış olmayan denetimleri ile ilişkili olabilir. Ayrıca, bu olay işleme kodu görünüm modelleri için birim test burada olabilir taşır.

#### <a name="invoking-behaviors-from-a-view"></a>Davranışlar bir görünümden çağırma

`EventToCommandBehavior` Komutları desteklemeyen bir denetim için bir komut eklemek için özellikle yararlıdır. Örneğin, `ProfileView` kullanan `EventToCommandBehavior` yürütülecek `OrderDetailCommand` olduğunda [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) olayı tetikler üzerinde [ `ListView` ](xref:Xamarin.Forms.ListView) gösterildiği gibi kullanıcının sipariş listeleyen Aşağıdaki kodda:

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

Çalışma zamanında, `EventToCommandBehavior` etkileşim yanıtlar [ `ListView` ](xref:Xamarin.Forms.ListView). İçinde bir öğe seçildiğinde `ListView`, [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) olay yangın, hangi yürütecek `OrderDetailCommand` içinde `ProfileViewModel`. Varsayılan olarak, olayının olay değişkenlerini komutuna geçirilir. Belirtilen dönüştürücü tarafından kaynak ve hedef arasında başarılı gibi bu verileri dönüştürülür `EventArgsConverter` döndüren özellik [ `Item` ](xref:Xamarin.Forms.ItemTappedEventArgs.Item) , `ListView` gelen [ `ItemTappedEventArgs` ](xref:Xamarin.Forms.ItemTappedEventArgs). Bu nedenle, `OrderDetailCommand` yürütüldüğünde, seçili `Order` kayıtlı eylem için bir parametre olarak geçirilir.

Davranışlar hakkında daha fazla bilgi için bkz. [davranışları](~/xamarin-forms/app-fundamentals/behaviors/index.md).

## <a name="summary"></a>Özet

Model-View-ViewModel (MVVM) desenini düzgün bir şekilde iş ve bir sunu mantıksal uygulamanın, kullanıcı arabiriminden (UI) ayrı yardımcı olur. Uygulama mantığı ve UI arasında NET bir ayrım koruma çok sayıda geliştirme sorunları gidermeye yardımcı olur ve uygulama sınamak için korumak ve geliştirmek daha kolay hale getirebilirsiniz. Kodu yeniden kullanma fırsatlarını büyük ölçüde geliştirebilir ve geliştiricilerin sağlar ve daha fazla bilgi için kullanıcı Arabirimi tasarımcıları, ilgili bölümleri Uygulama geliştirirken kolayca işbirliği yapın.

MVVM düzeni, uygulamanın kullanıcı arabirimini ve temel alınan sunu ve iş mantığı, üç ayrı sınıflara ayrılmış: kullanıcı Arabirimi ve kullanıcı arabirimini Kapsüller görünümü mantıksal uygulama Sunum mantığı ve durumu yalıtan görünüm modeli; ve modeli, uygulamanın iş mantığı ve verileri saklar.


## <a name="related-links"></a>İlgili bağlantılar

- [(2 Mb PDF) e-kitabı indir](https://aka.ms/xamarinpatternsebook)
- [Hizmetine (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
