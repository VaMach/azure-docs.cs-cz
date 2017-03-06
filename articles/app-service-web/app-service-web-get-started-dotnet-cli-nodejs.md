---
title: "Nasazení první webové aplikace v ASP.NET do Azure během pěti minut | Dokumentace Microsoftu"
description: "Nasazením ukázkové aplikace v ASP.NET zjistíte, jak snadné je spustit webové aplikace ve službě App Service. Pusťte se hned do skutečného vývoje. Výsledky uvidíte okamžitě."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 0458cc3b-b630-4632-8cd0-76e2ed96498b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: d24e78a826de9fb72e0eedc48877c83e867f70ca
ms.openlocfilehash: 58683e50ab57e0d162dde92c2f9558d5c2379d5b
ms.lasthandoff: 03/01/2017


---
# <a name="deploy-your-first-aspnet-web-app-to-azure-in-five-minutes"></a>Nasazení první webové aplikace v ASP.NET do Azure během pěti minut
[!INCLUDE [app-service-web-selector-get-started-nodejs-cli](../../includes/app-service-web-selector-get-started-nodejs-cli.md)]  

Tento kurz vám pomůže nasadit jednoduchou webovou aplikaci v ASP.NET do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Službu App Service můžete používat při vytváření webových aplikací, [back-endů mobilních aplikací](/documentation/learning-paths/appservice-mobileapps/) a [aplikací API](../app-service-api/app-service-api-apps-why-best-platform.md).

Vaším úkolem je: 

* Vytvořit webovou aplikaci ve službě Azure App Service.
* Nasadit ukázkový kód ASP.NET.
* Sledovat spuštění kódu naživo v produkčním prostředí.
* Aktualizovat webovou aplikaci stejným způsobem, jakým byste [nuceně doručili (push) potvrzené změny do Git](https://git-scm.com/docs/git-push).

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

- [Azure CLI 1.0](app-service-web-get-started-dotnet-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
- [Azure CLI 2.0](app-service-web-get-started-dotnet.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

## <a name="prerequisites"></a>Požadavky
* [Git](http://www.git-scm.com/downloads)
* [Rozhraní příkazového řádku Azure](../xplat-cli-install.md).
* Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure. Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.
> 
> 

## <a name="deploy-an-aspnet-web-app"></a>Nasazení webové aplikace v ASP.NET
1. Otevřete nový příkazový řádek systému Windows, okno prostředí PowerShell, prostředí Linux nebo terminál OS X. Spusťte `git --version` a `azure --version` ověřte, zda jsou v počítači nainstalovány Git a rozhraní příkazového řádku Azure CLI.
   
    ![Test instalace nástrojů rozhraní příkazového řádku pro první webovou aplikaci v Azure](./media/app-service-web-get-started/1-test-tools.png)
   
    Pokud jste nástroje ještě nenainstalovali, odkazy na stažení naleznete v části [Požadavky](#Prerequisites).
2. Přihlaste se k Azure tímto způsobem:
   
        azure login
   
    Postupujte podle zprávy nápovědy a pokračujte v procesu přihlášení.
   
    ![Přihlášení k Azure a vytvoření první webové aplikace](./media/app-service-web-get-started/3-azure-login.png)

3. Přepněte rozhraní příkazového řádku Azure do režimu ASM a potom nastavte uživatele nasazení pro službu App Service. Později pomocí těchto přihlašovacích údajů nasadíte kód.
   
        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

4. Přejděte do pracovního adresáře (`CD`) a naklonujte ukázkovou aplikaci v ASP.NET.
   
        git clone https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git

5. Přejděte do úložiště ukázkové aplikace. 
   
        cd app-service-web-dotnet-get-started

6. V Azure vytvořte prostředek aplikace služby App Service s jedinečným názvem aplikace a uživatelem nasazení, kterého jste nakonfigurovali v předchozích krocích. Po zobrazení výzvy zadejte číslo požadované oblasti.
   
        azure site create <app_name> --git --gitusername <username>
   
    ![Vytvoření prostředku Azure pro první webovou aplikaci v Azure](./media/app-service-web-get-started-languages/dotnet-site-create.png)
   
    Aplikace je nyní vytvořena v Azure. Aktuální adresář je také inicializován pro Git a připojený k nové aplikaci služby App Service jako vzdálený adresář Git.
    Můžete přejít na adresu URL aplikace (http://&lt;název_aplikace>.azurewebsites.net) a prohlédnout si krásnou výchozí stránku HTML, ale nyní tam raději umístíme váš kód.

7. Nasaďte ukázkový kód do aplikace Azure stejným způsobem, jakým byste doručili bez vyžádání (push) libovolný kód prostřednictvím Gitu. Po zobrazení výzvy použijte heslo, které jste nakonfigurovali v předchozích krocích.
   
        git push azure master
   
    ![Nucené doručení (push) kódu do první webové aplikace v Azure](./media/app-service-web-get-started-languages/dotnet-git-push.png)
   
    `git push`nejenom vloží kód do Azure, ale také obnoví požadované balíčky a sestaví binární soubory ASP.NET. 

Blahopřejeme, úspěšně jste nasadili aplikaci do služby Azure App Service.

## <a name="see-your-app-running-live"></a>Sledování živého běhu aplikace
Chcete-li sledovat živý běh v Azure, spusťte následující příkaz z libovolného adresáře v úložišti:

    azure site browse

## <a name="make-updates-to-your-app"></a>Provádění aktualizací aplikace
Nyní můžete pomocí Git kdykoli provádět nucené doručení (push) z kořenového adresáře projektu (úložiště) a aktualizovat živý web. Postup je stejný jako při prvním nasazení kódu. Například pokaždé, když chcete nuceně doručit (push) novou změnu, kterou jste místně otestovali, stačí spustit následující příkazy z kořenového adresáře projektu (úložiště):

    git add .
    git commit -m "<your_message>"
    git push azure master


## <a name="next-steps"></a>Další kroky
Informace o vytváření, vývoji a nasazování webových aplikací .NET do Azure přímo v sadě Visual Studio najdete v tématu [Nasazení webové aplikace ASP.NET do služby Azure App Service pomocí sady Visual Studio](web-sites-dotnet-get-started.md).

Další možností je pokračovat v práci s první webovou aplikací. Příklad:

* Vyzkoušejte si [další způsoby nasazení kódu do Azure](web-sites-deploy.md). Pokud třeba chcete pro nasazení použít některé z úložišť GitHubu, stačí v části **Možnosti nasazení** místo **Místní úložiště Git** vybrat **GitHub**.
* Zdokonalte aplikaci Azure o další úroveň. Ověřte svoje uživatele. Škálujte ji na základě poptávky. Nastavte některá upozornění týkající se výkonu. To vše pomocí několika kliknutí. Viz téma [Přidání funkce do první webové aplikace](app-service-web-get-started-2.md).


