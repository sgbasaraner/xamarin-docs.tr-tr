
Yayın derlemesi çözümün belirtmek için aşağıdaki komut satırını **SOLUTION_FILE.sln** iPhone. IPA konumunu belirterek ayarlanabilir `IpaPackageDir` özelliği komut satırında:

 - Mac üzerinde kullanarak **xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

**Xbuild** komutu genellikle dizinde bulunan **/Library/Frameworks/Mono.framework/Commands**.

 - Kullanarak Windows'da **msbuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**MSBuild** değil otomatik olarak genişletecek `$( )` ifadeleri geçirilen komut satırı tarafından. Bu nedenle önerilir ayarlarken bir tam yol kullanmak için `IpaPackageDir` komut satırında.


Bkz: [iOS 9.8 için sürüm notları](https://developer.xamarin.com/releases/ios/xamarin.ios_9/xamarin.ios_9.8/#New_MSBuild_property_IpaPackageDir_to_customize_.ipa_output_location) hakkında daha fazla ayrıntı için `IpaPackageDir` özelliği.
