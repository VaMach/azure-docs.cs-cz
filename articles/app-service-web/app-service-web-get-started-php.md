---
title: "Nasazení první webové aplikace v PHP do Azure během pěti minut (CLI 2.0 Preview) | Dokumentace Microsoftu"
description: "Nasazením ukázkové aplikace v PHP zjistíte, jak snadné je spustit webové aplikace ve službě App Service. Pusťte se hned do skutečného vývoje. Výsledky uvidíte okamžitě."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/27/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 68298208d2e2cc1fe7ab4050afecb25ca7d619cd
ms.lasthandoff: 03/01/2017


---
# <a name="deploy-your-first-php-web-app-to-azure-in-five-minutes-cli-20-preview"></a>Nasazení první webové aplikace v PHP do Azure během pěti minut (CLI 2.0 Preview)
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Tento kurz vám pomůže nasadit první webovou aplikaci v PHP do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Službu App Service můžete používat při vytváření webových aplikací, [back-endů mobilních aplikací](/documentation/learning-paths/appservice-mobileapps/) a [aplikací API](../app-service-api/app-service-api-apps-why-best-platform.md).

Vaším úkolem je: 

* Vytvořit webovou aplikaci ve službě Azure App Service.
* Nasadit ukázkový kód PHP.
* Sledovat spuštění kódu naživo v produkčním prostředí.
* Aktualizovat webovou aplikaci stejným způsobem, jakým byste [nuceně doručili (push) potvrzené změny do Git](https://git-scm.com/docs/git-push).

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

- [Azure CLI 1.0](app-service-web-get-started-php-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
- [Azure CLI 2.0](app-service-web-get-started-php.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

## <a name="prerequisites"></a>Požadavky
* [Git](http://www.git-scm.com/downloads)
* [Azure CLI 2.0 Preview](/cli/azure/install-az-cli2)
* Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure. Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.
> 
> 

## <a name="deploy-a-php-web-app"></a>Nasazení webové aplikace v PHP
1. Otevřete nový příkazový řádek systému Windows, okno prostředí PowerShell, prostředí Linux nebo terminál OS X. Spusťte `git --version` a `az --version` ověřte, zda jsou v počítači nainstalovány Git a rozhraní příkazového řádku Azure CLI.
   
    ![Test instalace nástrojů rozhraní příkazového řádku pro první webovou aplikaci v Azure](./media/app-service-web-get-started-languages/1-test-tools-2.0.png)
   
    Pokud jste nástroje ještě nenainstalovali, odkazy na stažení naleznete v části [Požadavky](#Prerequisites).
2. Přihlaste se k Azure tímto způsobem:
   
        az login
   
    Postupujte podle zprávy nápovědy a pokračujte v procesu přihlášení.
   
    ![Přihlášení k Azure a vytvoření první webové aplikace](./media/app-service-web-get-started-languages/3-azure-login-2.0.png)

3. Nastavte uživatele nasazení pro App Service. Později pomocí těchto přihlašovacích údajů nasadíte kód.
   
        az appservice web deployment user set --user-name <unique-username> --password <8-char-or-longer-password-letters-and-numbers>

3. Vytvořte novou [skupinu prostředků](../azure-resource-manager/resource-group-overview.md). V tomto prvním kurzu ke službě App Service ještě ani nepotřebujete vědět, co to je.

        az group create --location "<location>" --name my-first-app-group

    K zobrazení možných hodnot, které se dají použít pro `<location>`, použijte příkaz `az appservice list-locations` rozhraní příkazového řádku.

3. Vytvořte nový BEZPLATNÝ [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). Pro tento první kurz ke službě App Service stačí vědět, že se vám za webové aplikace v tomto plánu nebude nic účtovat.

        az appservice plan create --name my-free-appservice-plan --resource-group my-first-app-group --sku FREE

4. Vytvořte novou webovou aplikaci s jedinečným názvem ve značce `<app_name>`.

        az appservice web create --name <app_name> --resource-group my-first-app-group --plan my-free-appservice-plan

4. Teď získáte ukázkový kód PHP, který chcete nasadit. Přejděte do pracovního adresáře (`CD`) a naklonujte ukázkovou aplikaci tímto způsobem:
   
        cd <working_directory>
        git clone https://github.com/Azure-Samples/app-service-web-php-get-started.git

5. Přejděte do úložiště ukázkové aplikace. Například:
   
        cd app-service-web-php-get-started
5. Ke konfiguraci místního nasazení Gitu pro webovou aplikaci App Service použijete následující příkaz:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-first-app-group

    Získáte výstup JSON podobný tomuto. To znamená, že vzdálené úložiště Git je nakonfigurované:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Přidejte adresu URL v kódu JSON jako vzdálené úložiště Git pro vaše místní úložiště (pro zjednodušení nazvané `azure`).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Nasaďte ukázkový kód do aplikace Azure stejným způsobem, jakým byste doručili bez vyžádání (push) libovolný kód prostřednictvím Gitu. Po zobrazení výzvy použijte heslo, které jste nakonfigurovali v předchozích krocích.
   
        git push azure master
   
    ![Nucené doručení (push) kódu do první webové aplikace v Azure](./media/app-service-web-get-started-languages/php-git-push.png)
   
    Příkaz `git push` nejen že vloží kód do Azure, ale také aktivuje úlohy nasazení v modulu nasazení. Můžete také  [povolit rozšíření Autora](web-sites-php-mysql-deploy-use-git.md#composer) a automaticky zpracovávat soubory composer.json v aplikaci PHP.

Blahopřejeme, úspěšně jste nasadili aplikaci do služby Azure App Service.

## <a name="see-your-app-running-live"></a>Sledování živého běhu aplikace
Chcete-li sledovat živý běh v Azure, spusťte následující příkaz z libovolného adresáře v úložišti:

    az appservice web browse --name <app_name> --resource-group my-first-app-group

## <a name="make-updates-to-your-app"></a>Provádění aktualizací aplikace
Nyní můžete pomocí Git kdykoli provádět nucené doručení (push) z kořenového adresáře projektu (úložiště) a aktualizovat živý web. Postup je stejný jako při prvním nasazení kódu. Například pokaždé, když chcete nuceně doručit (push) novou změnu, kterou jste místně otestovali, stačí spustit následující příkazy z kořenového adresáře projektu (úložiště):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Další kroky
[Vytvořte, nakonfigurujte a nasaďte webovou aplikaci Laravel do Azure](app-service-web-php-get-started.md). V tomto kurzu se naučíte základní dovednosti, které v Azure potřebujete ke spuštění libovolné webové aplikace v PHP, jako je:

* Vytvoření a konfigurace aplikací v Azure z prostředí PowerShell/Bash
* Nastavení verze PHP
* Použití spouštěcího souboru, který není v kořenovém adresáři aplikace
* Povolení automatizace nástroje Composer
* Přístup k proměnným pro konkrétní prostředí
* Odstraňování běžných chyb

Další možností je pokračovat v práci s první webovou aplikací. Příklad:

* Vyzkoušejte si [další způsoby nasazení kódu do Azure](web-sites-deploy.md). Pokud třeba chcete pro nasazení použít některé z úložišť GitHubu, stačí v části **Možnosti nasazení** místo **Místní úložiště Git** vybrat **GitHub**.
* Zdokonalte aplikaci Azure o další úroveň. Ověřte svoje uživatele. Škálujte ji na základě poptávky. Nastavte některá upozornění týkající se výkonu. To vše pomocí několika kliknutí. Viz téma [Přidání funkce do první webové aplikace](app-service-web-get-started-2.md).


