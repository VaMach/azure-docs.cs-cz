<properties
    pageTitle="Úvodní příručka k prostředí Node.js | Microsoft Azure"
    description="Naučte se vytvořit jednoduchou webovou aplikaci Node.js a nasadit ji v cloudové službě Azure Cloud Service."
    services="cloud-services"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


# Sestavení a nasazení aplikace Node.js ve službě Azure Cloud Service

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

V tomto návodu se dozvíte, jak vytvořit jednoduchou aplikaci Node.js, běžící v cloudové službě Azure Cloud Service. Cloud Services jsou stavebními bloky škálovatelných cloudových aplikací v systému Azure. Umožňují oddělení, nezávislou správu a škálování front-endových a back-endových komponent vaší aplikace.  Cloud Services poskytují výkonný vyhrazený virtuální počítač, který spolehlivě hostuje jednotlivé role.

Další informace o službách Cloud Services a jejich porovnání se službami Azure Websites a Virtual Machines najdete v tématu [Porovnání webů Azure, služby Cloud Services a služby Virtual Machines].

>[AZURE.TIP] Chcete si vytvořit jednoduchý web? Pokud váš scénář zahrnuje jen jednoduchý front-endový web, zvažte [Použití jednoduché webové aplikace]. V případě potřeby budete moci snadno upgradovat na Cloud Service, až vaše webová aplikace naroste a vaše požadavky se změní.

V následujícím kurzu si vytvoříte jednoduchou webovou aplikaci, hostovanou v rámci webové role. Přes výpočetní emulátor si budete moci svou aplikaci lokálně otestovat a pak ji přes nástroje příkazového řádku PowerShellu nasadit.

Jedná se o jednoduchou aplikaci "hello world":

![A web browser displaying the Hello World web page][A web browser displaying the Hello World web page]

## Požadavky

> [AZURE.NOTE] Tento kurz používá prostředí Azure PowerShell, které vyžaduje systém Windows.

- Nainstalujte a nakonfigurujte [Azure Powershell].
- Stáhněte a nainstalujte sadu [Azure SDK pro .NET 2.7]. V nastavení instalace vyberte:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## Vytvoření projektu Azure Cloud Service

Proveďte následující kroky, a vytvořte tak nový projekt Azure Cloud Service a vygenerujte základní uživatelské rozhraní Node.js:

1. Jako správce spusťte **Windows PowerShell** – v **nabídce Start** nebo na **obrazovce Start** vyhledejte **Windows PowerShell**.

2. Proveďte [připojení PowerShellu] ke svému předplatnému.

3. Zadejte následující rutiny PowerShellu, a vytvořte tak projekt.

        New-AzureServiceProject helloworld

    ![The result of the New-AzureService helloworld command][The result of the New-AzureService helloworld command]

    Rutina **New-AzureServiceProject** generuje základní strukturu pro publikování aplikace Node.js na službě Cloud Service. Obsahuje konfigurační soubory, které jsou nezbytné pro publikování v systému Azure. Rutina také změní pracovní adresář na adresář pro službu.

    Rutina vytvoří následující soubory:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** a **ServiceDefinition.csdef**: soubory specifické pro Azure, které jsou zapotřebí pro publikování aplikace. Další informace najdete v tématu [Přehled vytváření hostované služby pro Azure].

    -   **deploymentSettings.json**: uloží místní nastavení, které používají rutiny Azure PowerShellu sloužící k nasazení.

4.  Zadejte následující příkaz pro přidání nové webové role:

        Add-AzureNodeWebRole

    ![The output of the Add-AzureNodeWebRole command][The output of the Add-AzureNodeWebRole command]

    Rutina **Add-AzureNodeWebRole** vytvoří základní aplikaci Node.js. Také upraví soubory **.csfg** a **.csdef** pro přidání konfiguračních údajů pro novou roli.

    > [AZURE.NOTE] Pokud nezadáte název role, použije se výchozí název. Název můžete zadat jako první parametr rutiny: `Add-AzureNodeWebRole MyRole`

Aplikace Node.js je definována v souboru **server.js**, který je umístěn v adresáři pro webovou roli (ve výchozím nastavení **WebRole1**). Zde je kód:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Tento kód je v podstatě stejný jako u příkladu "Hello World" na webu [nodejs.org] až na to, že používá číslo portu přiřazené cloudovému prostředí.

## Nasazení aplikace v Azure

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### Stažení nastavení publikování v systému Azure

Než aplikaci nasadíte v systému Azure, musíte si nejdřív stáhnout nastavení publikování pro své předplatné Azure.

1.  Spusťte následující rutiny Azure PowerShellu:

        Get-AzurePublishSettingsFile

    Tím ve svém prohlížeči přejdete na stránku, odkud můžete stáhnout nastavení publikování. Můžete být vyzváni k přihlášení pomocí účtu Microsoft Account. Pokud k tomu dojde, použijte účet spojený s předplatným Azure.

    Soubor se staženým profilem uložte do umístění, kam se snadno dostanete.

2.  Spusťte následující rutinu, která importuje stažený profil publikování.

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Po importu nastavení publikování zvažte, zda byste neměli smazat soubor .publishSettings. Obsahuje totiž informace, které by někomu mohly umožnit přístup na váš účet.

### Publikování aplikace

Pokud chcete aplikaci publikovat, spusťte následující příkazy:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **-ServiceName** určuje název pro nasazení. Název musí být jedinečný, jinak se proces publikování nezdaří. Příkaz **Get-Date** přiřadí k řetězci datum/čas, aby byl název jedinečný.

- **-Location** určuje datové centrum, kde bude aplikace hostována. Pokud chcete zobrazit seznam dostupných datových center, použijte rutinu **Get-AzureLocation**.

- **-Launch** otevře okno prohlížeče a po dokončení nasazení přejde na hostovanou službu.

Po úspěšném publikování uvidíte odpověď podobnou této:

![The output of the Publish-AzureService command][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Při prvním publikování může trvat několik minut, než se aplikace nasadí a bude k dispozici.

Po dokončení nasazení se otevře okno prohlížeče a zobrazí stránku cloudové služby.

![A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Aplikace je nyní spuštěna v systému Azure.

Rutina **Publish-AzureServiceProject** provede následující kroky:

1.  Vytvoří balíček pro nasazení. Tento balíček obsahuje všechny soubory ve složce vaší aplikace.

2.  Vytvoří nový **účet úložiště**, pokud žádný neexistuje. Účet úložiště Azure slouží k ukládání balíčku aplikace během nasazení. Po dokončení nasazení můžete účet úložiště bezpečně smazat.

3.  Vytvoří novou **cloudovou službu**, pokud žádná neexistuje. **Cloudová služba** je kontejner, ve kterém je vaše aplikace hostována, když ji nasadíte v systému Azure. Další informace najdete v tématu [Přehled vytváření hostované služby pro Azure].

4.  Publikuje balíček pro nasazení v systému Azure.


## Zastavení a odstranění vaší aplikace

Může se stát, že po nasazení budete chtít aplikaci zastavit a vyhnout se tak dalším poplatkům. Azure účtuje poplatky za instance webových rolí podle času (v hodinách), který na serveru spotřebují. Čas serveru se spotřebovává, jakmile je vaše aplikace nasazena, i když jsou instance zrovna zastaveny a neběží.

1.  V okně prostředí Windows PowerShell pomocí následující rutiny ukončete nasazení služby vytvořené v předchozí části:

        Stop-AzureService

    Zastavování služby může trvat několik minut. Až bude služba zastavená, obdržíte zprávu s oznámením, že se tak stalo.

    ![The status of the Stop-AzureService command][The status of the Stop-AzureService command]

2.  Pokud chcete službu odstranit, zavolejte následující rutinu:

        Remove-AzureService

    Po zobrazení výzvy zadejte **Y**, a službu tak odstraňte.

    Odstraňování služby může trvat několik minut. Po odstranění služby obdržíte zprávu s oznámením, že se tak stalo.

    ![The status of the Remove-AzureService command][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Odstraněním služby nedojde k odstranění účtu úložiště, který byl vytvořen při prvním publikování služby. Budou vám tedy i nadále účtovány poplatky za využívání úložiště. Pokud se úložiště nepoužívá pro nic jiného, můžete je odstranit.

## Další kroky

Další informace najdete v tématu [Centrum pro vývojáře Node.js].

<!-- URL List -->

[Porovnání webů Azure, služby Cloud Services a služby Virtual Machines]: ../app-service-web/choose-web-site-cloud-service-vm.md
[Použití jednoduché webové aplikace]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md">
[Azure Powershell]: ../powershell-install-configure.md
[Azure SDK pro .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[připojení PowerShellu]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Přehled vytváření hostované služby pro Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[Centrum pro vývojáře Node.js]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png



<!--HONumber=Oct16_HO1-->


