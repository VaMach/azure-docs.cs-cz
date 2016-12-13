---
title: "Nasazení webové aplikace v Node.js do Azure během pěti minut | Dokumentace Microsoftu"
description: "Nasazením ukázkové aplikace zjistíte, jak snadné je spustit webové aplikace ve službě App Service. Pusťte se hned do skutečného vývoje. Výsledky uvidíte okamžitě."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 412cc786-5bf3-4e1b-b696-6a08cf46501e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/13/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 4fc33ba185122496661f7bc49d14f7522d6ee522
ms.openlocfilehash: 1aa591f73b1af225230549e5af78ba4d64cdf310


---
# <a name="deploy-your-first-nodejs-web-app-to-azure-in-five-minutes"></a>Nasazení první webové aplikace v Node.js do Azure během pěti minut
Tento kurz vám pomůže nasadit první webovou aplikaci v Node.js do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Službu App Service můžete používat při vytváření webových aplikací, [back-endů mobilních aplikací](/documentation/learning-paths/appservice-mobileapps/) a [aplikací API](../app-service-api/app-service-api-apps-why-best-platform.md).

Vaším úkolem je: 

* Vytvořit webovou aplikaci ve službě Azure App Service.
* Nasadit ukázkový kód Node.js.
* Sledovat spuštění kódu naživo v produkčním prostředí.
* Aktualizovat webovou aplikaci stejným způsobem, jakým byste [nuceně doručili (push) potvrzené změny do Git](https://git-scm.com/docs/git-push).

## <a name="prerequisites"></a>Požadavky
* [Git](http://www.git-scm.com/downloads)
* [Rozhraní příkazového řádku Azure](../xplat-cli-install.md).
* Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](http://go.microsoft.com/fwlink/?LinkId=523751) bez účtu Azure. Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.
> 
> 

## <a name="deploy-a-nodejs-web-app"></a>Nasazení webové aplikace v Node.js
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
4. Přejděte do pracovního adresáře (`CD`) a naklonujte ukázkovou aplikaci tímto způsobem:
   
        git clone https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git
5. Přejděte do úložiště ukázkové aplikace.
   
        cd app-service-web-nodejs-get-started
6. V Azure vytvořte prostředek aplikace služby App Service s jedinečným názvem aplikace a uživatelem nasazení, kterého jste nakonfigurovali v předchozích krocích. Po zobrazení výzvy zadejte číslo požadované oblasti.
   
        azure site create <app_name> --git --gitusername <username>
   
    ![Vytvoření prostředku Azure pro první webovou aplikaci v Azure](./media/app-service-web-get-started-languages/node-site-create.png)
   
    Aplikace je nyní vytvořena v Azure. Aktuální adresář je také inicializován pro Git a připojený k nové aplikaci služby App Service jako vzdálený adresář Git.
    Můžete přejít na adresu URL aplikace (http://&lt;název_aplikace>.azurewebsites.net) a prohlédnout si krásnou výchozí stránku HTML, ale nyní tam raději umístíme váš kód.
7. Nasaďte ukázkový kód do aplikace Azure stejným způsobem, jakým byste doručili bez vyžádání (push) libovolný kód prostřednictvím Gitu. Po zobrazení výzvy použijte heslo, které jste nakonfigurovali v předchozích krocích.
   
        git push azure master
   
    ![Nucené doručení (push) kódu do první webové aplikace v Azure](./media/app-service-web-get-started-languages/node-git-push.png)
   
    Příkaz `git push` nejen že vloží kód do Azure, ale také aktivuje úlohy nasazení v modulu nasazení. 
    Pokud máte v kořenovém adresáři (úložišti) projektu soubor package.json, skript nasazení obnoví požadované balíčky za vás. 

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
[Vytvořte, nakonfigurujte a nasaďte webovou aplikaci Node.js Express do Azure](app-service-web-nodejs-get-started.md). V tomto kurzu se naučíte základní dovednosti, které v Azure potřebujete ke spuštění libovolné webové aplikace v Node.js, jako je:

* Vytvoření a konfigurace aplikací v Azure z prostředí PowerShell/Bash
* Nastavení verze Node.js
* Použití spouštěcího souboru, který není v kořenovém adresáři aplikace
* Automatizace pomocí NPM
* Získání protokolů chyb a výstupu

Další možností je pokračovat v práci s první webovou aplikací. Příklad:

* Vyzkoušejte si [další způsoby nasazení kódu do Azure](web-sites-deploy.md). Pokud třeba chcete pro nasazení použít některé z úložišť GitHubu, stačí v části **Možnosti nasazení** místo **Místní úložiště Git** vybrat **GitHub**.
* Zdokonalte aplikaci Azure o další úroveň. Ověřte svoje uživatele. Škálujte ji na základě poptávky. Nastavte některá upozornění týkající se výkonu. To vše pomocí několika kliknutí. Viz téma [Přidání funkce do první webové aplikace](app-service-web-get-started-2.md).




<!--HONumber=Dec16_HO1-->


