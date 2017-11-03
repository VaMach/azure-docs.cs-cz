Většinu času chyby ověřování v důsledku nastavení konfigurace nesprávné nebo jsou nekonzistentní. Tady jsou některé konkrétní návrhy pro co je potřeba zkontrolovat.

* Ujistěte se, že nebyla byste zapomněli **Uložit** tlačítko odkudkoli. Obvykle je snadné provést a výsledkem je, že je budete vidí správné hodnoty na stránky portálu, ale ve skutečnosti se nebyly uloženy v prostředí Azure nebo aplikaci Azure AD.
* Pro nastavení nakonfigurovaná v **nastavení aplikace** okno portálu Azure, ujistěte se, že správné rozhraní API aplikaci nebo webové aplikaci byl vybrán při nastavení byla zadána.  Také se ujistěte, že nastavení byla zadána jako **nastavení aplikace** a není **připojovací řetězce**, jako je podobný formát dvě části.
* Pro ověřování JavaScript front-endu, stáhněte manifest znovu a ověřte, že `oauth2AllowImplicitFlow` byla úspěšně změněna na `true`.
* Ověřte, používá protokol HTTPS, bez ohledu na nakonfigurované adresy URL:
  
  * V projektu kódu
  * V CORS
  * V prostředí Azure nastavení aplikace pro každou aplikaci API a webové aplikace
  * V nastavení aplikace služby Azure AD.
    
    Všimněte si, že pokud zkopírujete adresu URL aplikace API z portálu, často má `http://` a vy musíte ručně změnit jeho `https://`.
* Ujistěte se, že byly úspěšně nasazeny změny kódu. Například v řešení více projekty je možné změnit kód projekt a vyberte jednu z jiné omylem, pokud máte v úmyslu nasazení příslušné změny.
* Ujistěte se, že se chystáte adresy URL typu HTTPS v prohlížeči není adres URL protokolu HTTP. Ve výchozím nastavení, Visual Studio vytvoří publikovat profily pomocí adres URL protokolu HTTP, a který je co otevře v prohlížeči po nasazení projektu.
* Pro ověřování JavaScript front-endu Ujistěte se, zda CORS správně nakonfigurovaná na aplikaci API, který volá kód jazyka JavaScript. Pokud máte pochybnosti, zda je problém související se sdílením CORS, zkuste "*" jako adresu URL povolený původ. 
* Pro JavaScript front-end otevřete kartu konzoly vývojářských nástrojů prohlížeče získat další informace o chybě a prozkoumat požadavků HTTP v síti. Konzole chybové zprávy však může být zavádějící. Pokud se zobrazí zprávu s upozorněním na chybu CORS, skutečné problém může být ověřování. Můžete se podívat, pokud je tomu tak, že spustíte aplikaci pomocí ověřování dočasně dočasně zakázána.
* Pro aplikace .NET API, ujistěte se, co nejvíce informací se zobrazuje v chybových zprávách nejdříve nastavením [customErrors režim vypnutém](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* Pro aplikace .NET API, spusťte [vzdálené relace ladění](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)a zkontrolujte hodnoty proměnných, které se budou předávat kód, který využívá ADAL k získání tokenu nosiče nebo kód, který kontroluje deklarace identity s očekávanou službu hlavní ID. Všimněte si, že váš kód můžete vyzvednutí hodnoty konfigurace z mnoha různých zdrojů, takže je možné najít výskyt nečekaných událostí tímto způsobem. Například, pokud jste překlep `ida:ClientId` jako `ida:ClientID` při konfiguraci nastavení prostředí Azure App Service, může získat kód `ida:ClientId` hodnotu, která hledá ze souboru Web.config nastavení služby Azure App Service je ignorována. 
* Pokud věcí nefungují v normálním okně Internet Exploreru, existující protokolu v může vadit; Zkuste InPrivate a zkuste to Chrome nebo Firefox.

