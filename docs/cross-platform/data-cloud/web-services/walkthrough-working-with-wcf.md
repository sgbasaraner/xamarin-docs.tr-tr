---
title: İzlenecek yol - WCF ile çalışma
description: Bu kılavuz, Xamarin ile oluşturulan bir mobil uygulama BasicHttpBinding sınıfı kullanarak bir WCF web hizmeti nasıl tüketebileceği kapsar.
ms.prod: xamarin
ms.assetid: D0E83342-2E79-4D25-BD57-43718F5628C4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/17/2018
ms.openlocfilehash: 7f6885415e1b5e0c988d13fe331703213b9b8fb7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---working-with-wcf"></a>İzlenecek yol - WCF ile çalışma

_Bu kılavuz, Xamarin ile oluşturulan bir mobil uygulama BasicHttpBinding sınıfı kullanarak bir WCF web hizmeti nasıl tüketebileceği kapsar._


Arka uç sistemleri ile iletişim kurabilmesi mobil uygulamalar için ortak bir gereksinimdir. Birçok seçenek ve arka uç çerçeveleri, biri olan seçenekleri vardır [Windows Communication Foundation](http://msdn.microsoft.com/en-us/library/ms731082.aspx) (WCF). Bu kılavuzu kullanarak bir WCF hizmeti bir Xamarin mobil uygulamayı nasıl tüketebileceği örneği sağlayacak `BasicHttpBinding` sınıfı. İzlenecek yol aşağıdaki konuları içerir:

1.  **Bir WCF hizmeti oluşturma** -bu bölümdeki iki yöntem sahip çok basit bir WCF Hizmeti oluşturacağız. Bir C# nesnesi başka bir yöntem sürer ilk yöntem bir dize parametresi olur. Bu bölümde ayrıca WCF Hizmeti Uzaktan erişime izin vermek için bir geliştiricinin iş istasyonu yapılandırmak nasıl ele alınacaktır.
1.  **Bir Xamarin.Android uygulaması oluşturma** -WCF Hizmeti oluşturulduktan sonra WCF hizmetini kullanacak olan basit bir Xamarin.Android uygulaması oluşturacağız. Bu bölümde, WCF Hizmeti ile iletişimi kolaylaştırmak için bir WCF hizmeti proxy sınıfı oluşturmak nasıl ele alınacaktır.
1.  **Bir Xamarin.iOS uygulaması oluşturma** -Bu öğreticinin son bölümü, WCF hizmetini kullanacak olan basit bir Xamarin.iOS uygulaması oluşturursunuz.

<a name="Requirements" />

## <a name="requirements"></a>Gereksinimler

Bu kılavuzda oluşturulması ve WCF hizmetlerini kullanarak bazı bilgisi olduğu varsayılır.

<a name="Creating_a_WCF_Service" />

## <a name="creating-a-wcf-service"></a>Bir WCF hizmeti oluşturma

İlk önce bize bir WCF Hizmeti ile iletişim kurmak mobil uygulamaları oluşturmak için bir görevdir.

1. Visual Studio 2017 başlatın ve yeni bir proje oluşturun.
1. İçinde **yeni proje** iletişim kutusunda **WCF > WCF Hizmeti Kitaplığı** şablonu ve ad çözümü `HelloWorldService`:

    ![](walkthrough-working-with-wcf-images/new-wcf-service.png "Yeni bir WCF Hizmeti kitaplığı oluşturun")

1. İçinde **Çözüm Gezgini**, adlı yeni bir sınıf ekleyin `HelloWorldData` projeye:

    ```csharp
        using System.Runtime.Serialization;

        namespace HelloWorldService
        {
            [DataContract]
            public class HelloWorldData
            {
                [DataMember]
                public bool SayHello { get; set; }

                [DataMember]
                public string Name { get; set; }

                public HelloWorldData()
                {
                    Name = "Hello ";
                    SayHello = false;
                }
            }
        }
    ```


1. İçinde **Çözüm Gezgini**, yeniden adlandırma `IService1.cs` için `IHelloWorldService.cs`ve yeniden adlandırma `Service1.cs` için `HelloWorldService.cs`.
1. İçinde **Çözüm Gezgini**, açık `IHelloWorldService.cs` ve kodu aşağıdaki kodla değiştirin:

    ```csharp
        using System.ServiceModel;

        namespace HelloWorldService
        {
            [ServiceContract]
            public interface IHelloWorldService
            {
                [OperationContract]
                string SayHelloTo(string name);

                [OperationContract]
                HelloWorldData GetHelloData(HelloWorldData helloWorldData);
            }
        }
    ```
  
    Bu hizmet için iki yöntem sunar: bir dizeyi bir parametre ve başka bir .NET nesnesini alır.

1. İçinde **Çözüm Gezgini**, açık `HelloWorldService.cs` ve kodu aşağıdaki kodla değiştirin:

    ```csharp
        using System;

        namespace HelloWorldService
        {
            public class HelloWorldService : IHelloWorldService
            {
                public HelloWorldData GetHelloData(HelloWorldData helloWorldData)
                {
                    if (helloWorldData == null)
                        throw new ArgumentException("helloWorldData");

                    if (helloWorldData.SayHello)
                        helloWorldData.Name = "Hello World to {helloWorldData.Name}";

                    return helloWorldData;
                }

                public string SayHelloTo(string name)
                {
                    return "Hello World to you, {name}";
                }
            }
        }
    ```

1. İçinde **Çözüm Gezgini**açın `App.config`, güncelleştirme `name` özniteliği `<service>` düğümü, `contract` özniteliği `<endpoint>` düğümünü ve `baseAddress` özniteliği`<add>` düğümü:

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            ...
            <services>
              <service name="HelloWorldService.HelloWorldService">
                <endpoint address="" binding="basicHttpBinding" contract="HelloWorldService.IHelloWorldService">
                  <identity>
                    <dns value="localhost" />
                  </identity>
                </endpoint>
                <endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange" />
                <host>
                  <baseAddresses>
                    <add baseAddress="http://localhost:8733/Design_Time_Addresses/HelloWorldService/" />
                  </baseAddresses>
                </host>
              </service>
            </services>
            ...
        </configuration>
    ```

1. Derleme ve WCF Hizmeti çalıştırın. WCF test istemcisi tarafından barındırılan hizmetin:

    ![](walkthrough-working-with-wcf-images/hosted-wcf-service.png "Test istemcisinde çalışan WCF Hizmeti")

1. Çalışan WCF test istemcisi ile bir tarayıcıyı başlatın ve WCF hizmetine gidin:

    ![](walkthrough-working-with-wcf-images/wcf-service-browser.png "WCF hizmet tarayıcı bilgileri sayfası")

> [!IMPORTANT]
> Aşağıdaki bölüm, yalnızca Windows 10 iş istasyonunda uzak bağlantıları kabul etmesi gerekiyorsa gereklidir. WCF Hizmeti dağıtmak için alternatif bir platformda varsa bölüm göz ardı edilebilir.

<a name="Allow_Remote_Access_to_IIS_Express" />

## <a name="configuring-remote-access-to-iis-express"></a>IIS Express için yapılandırma uzaktan erişim

Yerel olarak bir WCF barındırma bağlantıları yalnızca yerel makineden geldiğinizde yeterlidir. Ancak, uzak cihazlara (örneğin, bir Android cihaz veya iPhone) herhangi bir yerel WCF Hizmeti erişebilir değil. Bu nedenle, bu bölümde Windows 10 ve IIS Express uzak bağlantıları kabul edecek şekilde nasıl yapılandırılacağı açıklanır:

1.  **IIS Express kabul uzak bağlantıları yapılandırmak** -belirli bir bağlantı noktasını uzak bağlantıları kabul etmek IIS Express için yapılandırma dosyasını düzenleyerek ve ardından gelen trafiği kabul etmek IIS Express için bir kural ayarlama bu adımı içerir.
1.  **Windows Güvenlik Duvarı'nda bir özel durum ekleyin** -Windows Uzak uygulamaları WCF Hizmeti ile iletişim için kullandığı güvenlik duvarı aracılığıyla bağlantı noktası açmanız gerekir.

    İş istasyonunuzu IP adresini bilmeniz gerekir. Bu örneğin amaçları doğrultusunda, iş istasyonu 192.168.1.143 IP adresi olduğunu varsayıyoruz.

1. Dış isteklerini dinlemek için IIS Express yapılandırarak başlayalım. Bu en IIS Express için yapılandırma dosyasını düzenleyerek yapabiliriz `[solutiondirectory]\.vs\config\applicationhost.config`aşağıdaki ekran görüntüsünde gösterildiği gibi:

    [![](walkthrough-working-with-wcf-images/image05.png "Bu yapılandırma dosyası için IIS Express solutiondirectory.vsconfigapplicationhost.config düzenleyerek bu ekran görüntüsünde gösterildiği gibi yapabiliriz")](walkthrough-working-with-wcf-images/image05.png#lightbox)


    Bulun `site` adı bir öğesiyle `HelloWorldWcfHost`. Aşağıdaki XML parçacığını gibi görünmelidir:

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
            </bindings>
        </site>
    ```
 
    Başka bir eklemeniz gerekir `binding` dış trafiği 8734 noktasına açın. Aşağıdaki XML eklemek `bindings` öğesi, kendi IP adresiyle IP adresini değiştirme:

    ```xml
    <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
    ```
    
    Bu bağlantı noktası 8734 bilgisayarın dış IP adresi üzerinde uzak tüm IP adreslerinden gelen HTTP trafiği kabul etmek için IIS Express yapılandırabilir. Yukarıdaki kod parçacığında bu IIS Express çalıştıran bilgisayarın IP adresini 192.168.1.143 olduğunu varsayar. Değişikliklerden sonra `bindings` öğesi aşağıdaki gibi görünmelidir:

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
                <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
            </bindings>
        </site>
    ```

1. Ardından, IIS Express'i yapılandırmak ihtiyacımız 8734 bağlantı noktasından gelen bağlantıları kabul edin. Başlangıç bir yönetici komut istemi ayarlama ve şu komutu çalıştırın:

    `> netsh http add urlacl url=http://192.168.1.143:9608/ user=everyone`

1. Windows Güvenlik Duvarı'nı 8734 bağlantı noktasındaki dış trafiğe izin verecek şekilde yapılandırmak için son adım olacaktır. Bir yönetici komut isteminden aşağıdaki komutu çalıştırın:

    `> netsh advfirewall firewall add rule name="IISExpressXamarin" dir=in protocol=tcp localport=8734 profile=private remoteip=localsubnet action=allow`

    Bu komut Windows 10 iş istasyonu olarak aynı alt ağdaki tüm cihazlardan 8734 bağlantı noktasında gelen trafiğe izin verir.

Diğer cihazlar veya bizim alt ağdaki bilgisayarlar gelen bağlantıları kabul edecek IIS Express barındırılan çok basit bir WCF Hizmeti oluşturdunuz. Bu dışarı uygulamanızı çalıştıran ve ziyaret sınayabilirsiniz `http://localhost:8733/Design_Time_Addresses/HelloWorldService/` istasyonunuzda ve `http://192.168.1.143:8734/Design_Time_Addresses/HelloWorldService/` , alt ağdaki başka bir bilgisayardan.

Çalıştığından ve hizmetin hizmet veren tutmak IIS Express izin vermek için kapatmak **Düzenle ve devam et** seçeneğini *proje özellikleri > Web > hata ayıklayıcıları*.

## <a name="creating-a-proxy-for-the-web-service"></a>Web Hizmeti Proxy oluşturma

Bir uygulama hizmeti kullanabilmeniz için önce bir web hizmeti proxy'si WCF hizmeti için oluşturulması gerekir. Bu şekilde gerçekleştirilebilir:

1. .NET standart sınıfı adlı Kitaplığı eklemek `HelloWorldServiceProxy`ve projedeki tüm sınıfların silin.
1. Çalıştırma `HelloWorldService` projesi.
1. İle `HelloWorldService` projesi çalıştırma, yeni bir ekleme **bağlı hizmet** projesine kullanarak **Microsoft WCF Web hizmeti başvuru sağlayıcısı**.
1. İçinde **Hizmeti uç noktası** sekmesinde **yapılandırma WCF Web hizmeti başvuru** iletişim kutusunda, tıklatın **bulma** düğmesini tıklatın, silmek `mex` algılanan sonundan uç **URI** açılan listesinde, girin `HelloWorldServiceProxy` olarak **Namespace**, tıklatıp **sonraki** düğmesi.
1. İçinde **veri türü seçenekleri** sekmesinde **yapılandırma WCF Web hizmeti başvuru** iletişim kutusunda, tıklayarak Varsayılanları kabul **sonraki** düğmesi.
1. İçinde **istemci seçenekleri** sekmesinde **WCF Web hizmeti başvuru yapılandırmak** iletişim kutusunda, emin **ortak** onay kutusu seçilidir ve tıklatın **son**  düğmesi.
1. Yapı `HelloWorldServiceProxy` projesi.

> [!NOTE]
> Visual Studio 2017 içinde Microsoft WCF Web hizmeti başvuru sağlayıcısı kullanarak proxy oluşturma alternatif ServiceModel meta veri yardımcı Programracı (svcutil.exe) kullanmaktır. Daha fazla bilgi için bkz: [ServiceModel meta veri yardımcı Programracı (Svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Creating_a_Xamarin_Android_Application" />

## <a name="creating-a-xamarinandroid-application"></a>Bir Xamarin.Android uygulaması oluşturma

WCF Hizmeti Proxy'si şu şekilde bir Xamarin.Android uygulaması tarafından kullanılabilecek:

1. Visual Studio'da çözüme yeni bir boş Android projesi ekleyin ve adını `HelloWorld.Android`.
1. İçinde `HelloWorld.Android` proje, bir başvuru ekleyin `HelloWorldServiceProxy` proje ve başvuru `System.ServiceModel` ad alanı.
1. İçinde **Çözüm Gezgini**, açık `Resources/layout/main.axml` ve varolan XML aşağıdaki XML ile değiştirin:

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                  android:orientation="vertical"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent">
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/sayHelloWorldButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/say_hello_world" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/sayHelloWorldTextView" />
            </LinearLayout>
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/getHelloWorldDataButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/get_hello_world_data" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/getHelloWorldDataTextView" />
            </LinearLayout>
        </LinearLayout>
    ```
    
    Aşağıdaki ekran Tasarımcısı'nda kullanıcı arabirimini gösterir:

    [![](walkthrough-working-with-wcf-images/image09.png "Bu UI nasıl Tasarımcısı'nda göründüğünü, ekran budur")](walkthrough-working-with-wcf-images/image09.png#lightbox)
    
1. İçinde **Çözüm Gezgini**, açık `Resources/values/Strings.xml` ve aşağıdaki XML ekleyin:

    ```xml
    <string name="say_hello_world">Say Hello World</string>
    <string name="get_hello_world_data">Get Hello World data</string>
    ```
    
1. İçinde **Çözüm Gezgini**, açık `MainActivity.cs` ve var olan kodu aşağıdaki kodla değiştirin:

    ```csharp
        [Activity(Label = "HelloWorld.Android", MainLauncher = true)]
        public class MainActivity : Activity
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");

            HelloWorldServiceClient _client;
            Button _getHelloWorldDataButton;
            TextView _getHelloWorldDataTextView;
            Button _sayHelloWorldButton;
            TextView _sayHelloWorldTextView;
            ...
        }
    ```

    Değiştir `<insert_WCF_service_endpoint_here>` WCF Bitiş adresi.

1. İçinde `MainActivity.cs`, değişiklik `OnCreate` aşağıdaki kod, BT'nin içerecek şekilde yöntemi:

    ```csharp
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            InitializeHelloWorldServiceClient();

            // This button will invoke the GetHelloWorldData - the method that takes a C# object as a parameter.
            _getHelloWorldDataButton = FindViewById<Button>(Resource.Id.getHelloWorldDataButton);
            _getHelloWorldDataButton.Click += GetHelloWorldDataButtonOnClick;
            _getHelloWorldDataTextView = FindViewById<TextView>(Resource.Id.getHelloWorldDataTextView);

            // This button will invoke SayHelloWorld - this method takes a simple string as a parameter.
            _sayHelloWorldButton = FindViewById<Button>(Resource.Id.sayHelloWorldButton);
            _sayHelloWorldButton.Click += SayHelloWorldButtonOnClick;
            _sayHelloWorldTextView = FindViewById<TextView>(Resource.Id.sayHelloWorldTextView);
        }
    ```
    
    Yukarıdaki kod sınıfı için örnek değişkenleri başlatır ve bazı olay işleyicilerini bağlayan.

1. İçinde `MainActivity.cs`, aşağıdaki iki yöntemden ekleyerek istemci proxy sınıfının örneği:

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
    
    Yukarıdaki kod oluşturur ve başlatır bir `HelloWorldServiceClient` nesnesi.

1. İçinde `MainActivity.cs`, iki düğme bile işleyicileri eklemek `Activity`:

    ```csharp
        async void GetHelloWorldDataButtonOnClick(object sender, EventArgs e)
        {
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            _getHelloWorldDataTextView.Text = "Waiting for WCF...";
            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                _getHelloWorldDataTextView.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButtonOnClick(object sender, EventArgs e)
        {
            _sayHelloWorldTextView.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                _sayHelloWorldTextView.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```
  
1. Uygulamayı çalıştırın, WCF hizmeti çalışıyor ve iki düğmeleri tıklatın emin olun. Uygulamayı WCF zaman uyumsuz olarak koşuluyla çağıracak `Endpoint` alanını doğru şekilde ayarlayın:

    [![](walkthrough-working-with-wcf-images/image08.png "30 saniye içinde her WCF yöntemi bir yanıt alınmalıdır ve uygulamamızı bu ekran görüntüsüne benzer görünmelidir")](walkthrough-working-with-wcf-images/image08.png#lightbox)

<a name="Creating_a_Xamarin_iOS_Application" />

## <a name="creating-a-xamarinios-application"></a>Bir Xamarin.iOS uygulaması oluşturma

WCF Hizmeti Proxy'si şu şekilde bir Xamarin.iOS uygulaması tarafından kullanılabilecek:

1. Visual Studio'da yeni bir iPhone eklemek **tek görünüm uygulaması** proje çözüme ve adlandırın `HelloWorld.iOS`.
1. İçinde `HelloWorld.iOS` proje, bir başvuru ekleyin `HelloWorldServiceProxy` proje ve başvuru `System.ServiceModel` ad alanı.
1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` iOS Tasarımcısı'nda dosyayı açmak için. Ardından, aşağıdaki ekleyin `UIButton` ve `UITextView` denetimleri:

    ||Ad|Başlık|
    |--- |--- |--- |
    |`UIButton`|`sayHelloWorldButton`|"Hello, World" söyleyin|
    |`UITextView`|`sayHelloWorldText`||
    |`UIButton`|`getHelloWorldDataButton`|"Hello, World" get veri|
    |`UITextView`|`getHelloWorldDataText`||

    Kullanıcı Arabirimi denetimlerini eklendikten sonra aşağıdaki ekran görüntüsü benzemelidir:

    [![](walkthrough-working-with-wcf-images/image12.png "Denetimleri eklendikten sonra kullanıcı arabirimini bu ekran benzemelidir")](walkthrough-working-with-wcf-images/image12.png#lightbox)

1. İçinde **Çözüm Gezgini**, açık `ViewController.cs` ve aşağıdaki kodu ekleyin:

    ```xml
        public partial class ViewController : UIViewController
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");
            HelloWorldServiceClient _client;
            ...
        }
    ```
  
    Değiştir `<insert_WCF_service_endpoint_here>` WCF Bitiş adresi.

1. İçinde `ViewController.cs`, güncelleştirme `ViewDidLoad` şekilde aşağıdakine benzer şekilde yöntemi:

    ```csharp
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();
            InitializeHelloWorldServiceClient();

            getHelloWorldDataButton.TouchUpInside += GetHelloWorldDataButton_TouchUpInside;
            sayHelloWorldButton.TouchUpInside += SayHelloWorldButton_TouchUpInside;
        }
    ```
  
1. İçinde `ViewController.cs`, ekleme `InitializeHelloWorldServiceClient` ve `CreateBasicHttpBinding` yöntemleri:

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
  
1. İçinde `ViewController.cs`, olay işleyicileri ekleme `TouchUpInside` iki olaylarına `UIButton` örnekleri:

    ```csharp
        async void GetHelloWorldDataButton_TouchUpInside(object sender, EventArgs e)
        {
            getHelloWorldDataText.Text = "Waiting for WCF...";
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                getHelloWorldDataText.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButton_TouchUpInside(object sender, EventArgs e)
        {
            sayHelloWorldText.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                sayHelloWorldText.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```

1. Uygulamayı çalıştırın, WCF hizmeti çalışıyor ve iki düğmeleri tıklatın emin olun. Uygulamayı WCF zaman uyumsuz olarak koşuluyla çağıracak `Endpoint` alanını doğru şekilde ayarlayın:

    [![](walkthrough-working-with-wcf-images/image10.png "30 saniye içinde her WCF yöntemi bir yanıt alınmalıdır ve uygulamamızı bu ekran görüntüsüne görünmelidir")](walkthrough-working-with-wcf-images/image10.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Özet

Bu öğretici Xamarin.Android ve Xamarin.iOS kullanarak mobil uygulamada bir WCF Hizmeti ile nasıl çalışılacağını ele. Bir WCF hizmeti nasıl oluşturulacağını gösterir ve Windows 10 ve IIS Express uzak aygıtlardan bağlantılarını kabul edecek şekilde nasıl yapılandırılacağı açıklanmıştır. WCF proxy istemcisi oluşturmak nasıl açıklandığı ve proxy istemcisi Xamarin.Android ve Xamarin.iOS uygulamalarında kullanma gösterilmektedir.


## <a name="related-links"></a>İlgili bağlantılar

- [HelloWorld (örnek)](https://developer.xamarin.com/samples/mobile/WCF-Walkthrough/)
- [WCF ile hizmet odaklı uygulamalar geliştirme](https://docs.microsoft.com/en-us/dotnet/framework/wcf/index)
- [Nasıl yapılır: bir Windows Communication Foundation istemcisi oluşturma](https://docs.microsoft.com/en-us/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [ServiceModel meta veri yardımcı Programracı (svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
