---
title: "Nasazení první webové aplikace v Pythonu do Azure během pěti minut | Dokumentace Microsoftu"
description: "Nasazením ukázkové aplikace v Pythonu zjistíte, jak snadné je spustit webové aplikace ve službě App Service. Pusťte se hned do skutečného vývoje. Výsledky uvidíte okamžitě."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 845cfeab-6744-46e6-94a8-aef12657376e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 05e61d2fc751c4239aef4b10ad897765c59fe928
ms.openlocfilehash: 0fd92ba7ef394fd5266d37f64d0dd3e9f1c19c3a


---
# <a name="deploy-your-first-python-web-app-to-azure-in-five-minutes"></a>Nasazení první webové aplikace v Pythonu do Azure během pěti minut

> [!div class="op_single_selector"]
> * [První web v HTML](app-service-web-get-started-html-cli-nodejs.md)
> * [První aplikace v .NET](app-service-web-get-started-dotnet-cli-nodejs.md)
> * [První aplikace v PHP](app-service-web-get-started-php-cli-nodejs.md)
> * [První aplikace v Node.js](app-service-web-get-started-nodejs-cli-nodejs.md)
> * [První aplikace v Pythonu](app-service-web-get-started-python-cli-nodejs.md)
> * [První aplikace v Javě](app-service-web-get-started-java.md)
> 
> 

Tento kurz vám pomůže nasadit první webovou aplikaci v Pythonu do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Službu App Service můžete používat při vytváření webových aplikací, [back-endů mobilních aplikací](/documentation/learning-paths/appservice-mobileapps/) a [aplikací API](../app-service-api/app-service-api-apps-why-best-platform.md).

Vaším úkolem je: 

* Vytvořit webovou aplikaci ve službě Azure App Service.
* Nasadit ukázkový kód Pythonu.
* Sledovat spuštění kódu naživo v produkčním prostředí.
* Aktualizovat webovou aplikaci stejným způsobem, jakým byste [nuceně doručili (push) potvrzené změny do Git](https://git-scm.com/docs/git-push).

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

- [Azure CLI 1.0](app-service-web-get-started-python-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
- [Azure CLI 2.0 (Preview)](app-service-web-get-started-python.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

## <a name="prerequisites"></a>Požadavky
* [Git](http://www.git-scm.com/downloads)
* [Rozhraní příkazového řádku Azure](../xplat-cli-install.md).
* Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](http://go.microsoft.com/fwlink/?LinkId=523751) bez účtu Azure. Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.
> 
> 

## <a name="deploy-a-python-web-app"></a>Nasazení webové aplikace v Pythonu
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
4. Přejděte do pracovního adresáře (`CD`) a naklonujte ukázkovou aplikaci v Pythonu tímto způsobem:
   
        git clone https://github.com/Azure-Samples/app-service-web-python-get-started.git
5. Přejděte do úložiště ukázkové aplikace. Například:
   
        cd app-service-web-python-get-started
6. V Azure vytvořte prostředek aplikace služby App Service s jedinečným názvem aplikace a uživatelem nasazení, kterého jste nakonfigurovali v předchozích krocích. Po zobrazení výzvy zadejte číslo požadované oblasti.
   
        azure site create <app_name> --git --gitusername <username>
   
    ![Vytvoření prostředku Azure pro první webovou aplikaci v Azure](./media/app-service-web-get-started-languages/python-site-create.png)
   
    Aplikace je nyní vytvořena v Azure. Aktuální adresář je také inicializován pro Git a připojený k nové aplikaci služby App Service jako vzdálený adresář Git.
    Můžete přejít na adresu URL aplikace (http://&lt;název_aplikace>.azurewebsites.net) a prohlédnout si krásnou výchozí stránku HTML, ale nyní tam raději umístíme váš kód.
7. Nasaďte ukázkový kód do aplikace Azure stejným způsobem, jakým byste doručili bez vyžádání (push) libovolný kód prostřednictvím Gitu. Po zobrazení výzvy použijte heslo, které jste nakonfigurovali v předchozích krocích.
   
        git push azure master
   
    ![Nucené doručení (push) kódu do první webové aplikace v Azure](./media/app-service-web-get-started-languages/python-git-push.png)
   
    Příkaz `git push` nejen že vloží kód do Azure, ale také aktivuje úlohy nasazení v modulu nasazení. 
    Pokud máte v kořenovém adresáři (úložišti) projektu nějaký soubor requirements.txt (v Pythonu), skript nasazení obnoví požadované balíčky za vás. 

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
[Vytvořte, nakonfigurujte a nasaďte webovou aplikaci Django do Azure pomocí sady Visual Studio](web-sites-python-ptvs-django-mysql.md). V tomto kurzu se naučíte základní dovednosti, které v Azure potřebujete ke spuštění libovolné webové aplikace v Pythonu, jako je:

* Vytvoření a nasazení aplikace v Pythonu pomocí šablony
* Nastavení verze Pythonu
* Vytvoření virtuálních prostředí
* Připojení k databázi

Další možností je pokračovat v práci s první webovou aplikací. Příklad:

* Vyzkoušejte si [další způsoby nasazení kódu do Azure](web-sites-deploy.md). Pokud třeba chcete pro nasazení použít některé z úložišť GitHubu, stačí v části **Možnosti nasazení** místo **Místní úložiště Git** vybrat **GitHub**.
* Zdokonalte aplikaci Azure o další úroveň. Ověřte svoje uživatele. Škálujte ji na základě poptávky. Nastavte některá upozornění týkající se výkonu. To vše pomocí několika kliknutí. Viz téma [Přidání funkce do první webové aplikace](app-service-web-get-started-2.md).




<!--HONumber=Jan17_HO1-->


