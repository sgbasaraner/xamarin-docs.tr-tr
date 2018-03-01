## <a name="firewall-configuration"></a>Güvenlik duvarı yapılandırması

Xamarin Test Cloud gönderilmesi testler için sırayla testleri gönderme bilgisayarın Test bulut sunucuları ile iletişim kurabildiğinden olması gerekir. Güvenlik duvarları, konumunda bulunan sunucular gelen ve giden ağ trafiğine izin verecek şekilde yapılandırılmalıdır **testcloud.xamarin.com** 80 ve 443 numaralı bağlantı noktalarını. Bu uç noktaya DNS tarafından yönetilir ve IP adresi değiştirilebilir. 

Bazı durumlarda, bir test (veya test çalıştıran bir cihazda) bir güvenlik duvarı tarafından korunan web sunucularıyla iletişim kurması gerekir. Bu senaryoda, Güvenlik Duvarı'nı aşağıdaki IP adreslerinden gelen trafiğe izin verecek şekilde yapılandırılmalıdır:

* **195.249.159.238**
* **195.249.159.239**
