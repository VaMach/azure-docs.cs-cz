<properties 
    pageTitle="Upgrade agenta PhoneFactor na server Azure Multi-Factor Authentication" 
    description="Tento dokument popisuje, jak začít pracovat se serverem Azure MFA a postup upgradu ze staršího agenta phonefactor." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>

# Upgrade agenta PhoneFactor na server Azure Multi-Factor Authentication

Upgrade z PhoneFactor agenta v5.x nebo starší na server Azure Multi-Factor Authentication vyžaduje odinstalaci PhoneFactor agenta a přidružených součásti před instalací serveru Multi-Factor Authentication a jeho přidružených součásti. 

## Postup upgradu agenta PhoneFactor na server Azure Multi-Factor Authentication
<ol>
<li>Nejprve zálohujte datový soubor PhoneFactor. Výchozí umístění instalace je C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Pokud je nainstalován portál uživatele:</li>
<ol>
<li>Přejděte do složky instalace a zálohujte soubor web.config. Výchozí umístění instalace je C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Pokud jste přidali vlastní motivy na portál, zálohujte vlastní složky pod adresářem C:\inetpub\wwwroot\PhoneFactor\App_Themes.</li>


<li>Odinstalujte rozhraní User Portal prostřednictvím agenta PhoneFactor (dostupné, pouze pokud je nainstalován na stejném serveru jako agent PhoneFactor) nebo pomocí programů a funkcí systému Windows.</li></ol>




<li>Pokud je nainstalována webová služba mobilní aplikace:
<ol>
<li>Přejděte do složky instalace a zálohujte soubor web.config. Výchozí umístění instalace je C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Odinstalujte službu mobilní webové aplikace prostřednictvím programů a funkcí systému Windows.</li></ol>

<li>Pokud je nainstalována sada Web Service SDK, odinstalujte ji prostřednictvím agenta PhoneFactor nebo programů a funkcí systému Windows.

<li>Odinstalujte službu agent PhoneFactor prostřednictvím programů a funkcí systému Windows.

<li>Instalace serveru Multi-Factor Authentication. Všimněte si, že cesta instalace je převzata z registru z předchozí instalace agenta PhoneFactor, takže by se měla nainstalovat do stejného umístění (například C:\Program Files\PhoneFactor). Nové instalace budou mít různé výchozí instalační cesty (například C:\Program Files\Multi-Factor Authentication Server). Datový soubor zanechaný předchozí agentem PhoneFactor musí být upgradován během instalace, takže nastavení uživatelů by mělo být po instalaci nového serveru Multi-Factor Authentication stále přítomno.

<li>Pokud budete vyzváni, aktivovujte server Multi-Factor Authentication a ujistěte se, že je přiřazen do správné replikační skupiny.

<li>Pokud byla sada Web Service SDK dříve nainstalována, nainstalujte novou sadu Web Service SDK prostřednictvím uživatelského rozhraní serveru Multi-Factor Authentication. Všimněte si, že výchozí název virtuálního adresáře je nyní „MultiFactorAuthWebServiceSdk“ místo „PhoneFactorWebServiceSdk“. Pokud chcete použít předchozí název, musíte změnit název virtuálního adresáře během instalace. Jinak pokud povolíte instalaci použít nový výchozí název, budete muset změnit adresu URL ve všech aplikacích, které odkazují na sadu Web Service SDK, například portál User Portal a Webová služba mobilní aplikace, aby odkazovaly na správné místo.

<li>Pokud bylo rozhraní User Portal dříve nainstalováno na serveru agenta PhoneFactor, nainstalujte nový uživatelský portál Multi-Factor Authentication prostřednictvím uživatelského rozhraní serveru Multi-Factor Authentication. Všimněte si, že výchozí název virtuálního adresáře je nyní „MultiFactorAuth“ místo „PhoneFactor“. Pokud chcete použít předchozí název, musíte změnit název virtuálního adresáře během instalace. Jinak pokud povolíte instalaci pro použití nového výchozího názvu, doporučujeme kliknout na ikonu portálu pro uživatele na serveru Multi-Factor Authentication a aktualizovat adresu URL portálu pro uživatele na kartě Nastavení. 

<li>Pokud byly portál pro uživatele nebo webové služby mobilní aplikace dříve nainstalovány na jiném serveru než agent PhoneFactor:
<ol>
<li>Přejděte do umístění instalace (např. C:\Program Files\PhoneFactor) a zkopírujte příslušné instalační programy na jiný server. Existují 32bitové a 64bitové verze instalačních programů pro portál pro uživatele a webové služby mobilní aplikace. Nazývají se MultiFactorAuthenticationUserPortalSetupXX.msi a MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi v uvedeném pořadí.</li>
<li>Chcete-li nainstalovat portál pro uživatele na webovém serveru, otevřete příkazový řádek jako správce a spusťte soubor MultiFactorAuthenticationUserPortalSetupXX.msi. Všimněte si, že výchozí název virtuálního adresáře je nyní „MultiFactorAuth“ místo „PhoneFactor“. Pokud chcete použít předchozí název, musíte změnit název virtuálního adresáře během instalace. Jinak pokud povolíte instalaci pro použití nového výchozího názvu, doporučujeme kliknout na ikonu portálu pro uživatele na serveru Multi-Factor Authentication a aktualizovat adresu URL portálu pro uživatele na kartě Nastavení. Stávající uživatelé budou muset být informováni o nové adrese URL.</li>
<li>Přejděte do umístění instalace portálu pro uživatele (např. C:\inetpub\wwwroot\MultiFactorAuth) a upravte soubor web.config. Zkopírujte hodnoty v oddílech appSettings a applicationSettings z původního souboru web.config, který byl zálohován před upgradem, do nového souboru web.config. Pokud byl při instalaci sady Web Service SDK zachován výchozí název virtuálního adresáře, změňte adresu URL v části applicationSettings tak, aby odkazovala na správné místo. Pokud byly v předchozím soubor web.config změněny ostatní výchozí hodnoty, tyto změny budou stejně použity na nový soubor web.config.</li>
<li>Chcete-li nainstalovat webové služby mobilní aplikace na webovém serveru, otevřete příkazový řádek jako správce a spusťte soubor MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi. Všimněte si, že výchozí název virtuálního adresáře je nyní „MultiFactorAuthMobileAppWebService“ místo „PhoneFactorPhoneAppWebService“. Pokud chcete použít předchozí název, musíte změnit název virtuálního adresáře během instalace. Můžete vybrat kratší název pro usnadnění zadávání na mobilních zařízeních koncovým uživatelům. Jinak pokud povolíte instalaci pro použití nového výchozího názvu, doporučujeme kliknout na ikonu mobilní aplikace na serveru Multi-Factor Authentication a aktualizovat adresu URL webových služeb mobilní aplikace.</li>
<li>Přejděte do umístění instalace webové služby mobilní aplikace (např. C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) a upravte soubor web.config. Zkopírujte hodnoty v oddílech appSettings a applicationSettings z původního souboru web.config, který byl zálohován před upgradem, do nového souboru web.config. Pokud byl při instalaci sady Web Service SDK zachován výchozí název virtuálního adresáře, změňte adresu URL v části applicationSettings tak, aby odkazovala na správné místo. Pokud byly v předchozím soubor web.config změněny ostatní výchozí hodnoty, tyto změny budou stejně použity na nový soubor web.config.</li></ol>


 


 



<!---HONumber=Aug16_HO4-->


