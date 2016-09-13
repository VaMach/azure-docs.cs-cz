<properties
    pageTitle="Nasazení vaší první webové aplikace do Azure během pěti minut | Microsoft Azure"
    description="Zjistěte, jak snadno lze spustit webové aplikace v službě App Service, prostřednictvím nasazení ukázkové aplikace v pouhých několika krocích. Začněte provádět skutečný vývoj během pěti minut a ihned sledujte výsledky."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# Nasazení vaší první webové aplikace do Azure během pěti minut

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Tento kurz vám pomůže nasadit první webovou aplikaci do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Službu App Service můžete používat při vytváření webových aplikací, [back-endů mobilních aplikací](/documentation/learning-paths/appservice-mobileapps/) a [aplikací API](../app-service-api/app-service-api-apps-why-best-platform.md).

Stačí, abyste se nepatrně zapojili, a budete moci:

- Nasadit ukázkovou webovou aplikaci (zvolte mezi ASP.NET, PHP, Node.js, Java nebo Python).
- Během pár sekund sledovat živý běh aplikace.
- Aktualizovat webovou aplikaci stejným způsobem, jakým byste [nuceně doručili (push) potvrzené změny do Git](https://git-scm.com/docs/git-push).

Letmo se také seznámíte s [portálem Azure](https://portal.azure.com) a prozkoumáte funkce, které jsou zde k dispozici.

## Požadavky

- [Nainstalovat Git](http://www.git-scm.com/downloads).
- [Nainstalovat rozhraní příkazového řádku Azure CLI](../xplat-cli-install.md).
- Získat účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Sledujte webovou aplikaci v akci. Ihned [vyzkoušejte službu App Service](http://go.microsoft.com/fwlink/?LinkId=523751) a vytvořte krátkodobou úvodní aplikaci – aniž by byla požadována platební karta a bez jakýchkoli závazků.

## Nasazení webové aplikace

Nyní nasadíme webovou aplikaci do služby Azure App Service.

1. Otevřete nový příkazový řádek systému Windows, okno prostředí PowerShell, prostředí Linux nebo terminál OS X. Spusťte `git --version` a `azure --version` ověřte, zda jsou v počítači nainstalovány Git a rozhraní příkazového řádku Azure CLI.

    ![Test instalace nástrojů rozhraní příkazového řádku pro první webovou aplikaci v Azure](./media/app-service-web-get-started/1-test-tools.png)

    Pokud jste nástroje ještě nenainstalovali, odkazy na stažení naleznete v části [Požadavky](#Prerequisites).

1. Přejděte do pracovního adresáře (`CD`) a naklonujte ukázkovou aplikaci tímto způsobem:

        git clone <github_sample_url>

    ![Klonování ukázkového kódu aplikace pro první webovou aplikaci v Azure](./media/app-service-web-get-started/2-clone-sample.png)

    Jako adresu *&lt;github_sample_url>* použijte jednu z následujících adres URL, a to v závislosti na vámi upřednostňovaném prostředí:

    - HTML+CSS+JS: [https://github.com/Azure-Samples/app-service-web-html-get-started.git](https://github.com/Azure-Samples/app-service-web-html-get-started.git)
    - ASP.NET: [https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git](https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git)
    - PHP (CodeIgniter): [https://github.com/Azure-Samples/app-service-web-php-get-started.git](https://github.com/Azure-Samples/app-service-web-php-get-started.git)
    - Node.js (Express): [https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git](https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git)
    - Java: [https://github.com/Azure-Samples/app-service-web-java-get-started.git](https://github.com/Azure-Samples/app-service-web-java-get-started.git)
    - Python (Django): [https://github.com/Azure-Samples/app-service-web-python-get-started.git](https://github.com/Azure-Samples/app-service-web-python-get-started.git)

2. Přejděte do úložiště ukázkové aplikace. Například:

        cd app-service-web-html-get-started

3. Přihlaste se k Azure tímto způsobem:

        azure login

    Postupujte podle zprávy nápovědy a pokračujte v procesu přihlášení.

    ![Přihlášení k Azure a vytvoření první webové aplikace](./media/app-service-web-get-started/3-azure-login.png)

4. Pomocí následujícího příkazu vytvořte v Azure prostředek aplikace služby App Service s jedinečným názvem aplikace. Po zobrazení výzvy zadejte číslo požadované oblasti.

        azure site create --git <app_name>

    ![Vytvoření prostředku Azure pro první webovou aplikaci v Azure](./media/app-service-web-get-started/4-create-site.png)

    >[AZURE.NOTE] Pokud jste u předplatného Azure dosud nenastavili přihlašovací údaje nasazení, budete vyzváni k jejich vytvoření. Služba App Service používá tyto přihlašovací údaje (tedy nikoli vše přihlašovací údaje účtu Azure) pouze k nasazením Git a přihlášením FTP.

    Aplikace je nyní vytvořena v Azure. Aktuální adresář je také inicializován pro Git a připojený k nové aplikaci služby App Service jako vzdálený adresář Git.
    Můžete procházením přejít k adrese URL aplikace (http://&lt;app_name>.azurewebsites.net) a prohlédnout si krásnou výchozí stránku HTML, ale nyní tam raději umístíme váš vlastní kód.

4. Nasaďte ukázkový kód nové aplikace služby App Service stejným způsobem, jakým byste nuceně doručili (push) libovolný kód prostřednictvím Git:

        git push azure master

    ![Nucené doručení (push) kódu do první webové aplikace v Azure](./media/app-service-web-get-started/5-push-code.png)    

    Pokud jste použili jedno z jazykových rozhraní, zobrazí se jiný výstup. Důvodem je, že `git push` nejen vloží kód v Azure, ale také aktivuje úlohy nasazení v modulu pro nasazení. Máte-li v kořenovém adresáři projektu (úložiště) libovolné soubory package.json (Node.js) nebo requirements.txt (Python) nebo máte-li v projektu ASP.NET soubor packages.config, skripty nasazení vám obnoví požadované balíčky. Můžete také [povolit rozšíření Autora](web-sites-php-mysql-deploy-use-git.md#composer) a automaticky zpracovávat soubory composer.json v aplikaci PHP.

Blahopřejeme, úspěšně jste nasadili aplikaci do služby Azure App Service.

## Sledování živého běhu aplikace

Chcete-li sledovat živý běh v Azure, spusťte následující příkaz z libovolného adresáře v úložišti:

    azure site browse

## Provádění aktualizací aplikace

Nyní můžete pomocí Git kdykoli provádět nucené doručení (push) z kořenového adresáře projektu (úložiště) a aktualizovat živý web. Postup je stejný jako při prvním nasazení aplikace do Azure. Například pokaždé, když chcete nuceně doručit (push) novou změnu, kterou jste místně otestovali, spusťte následující příkazy z kořenového adresáře projektu (úložiště):

    git add .
    git commit -m "<your_message>"
    git push azure master

## Zobrazení aplikace na webu Azure Portal

Nyní přejdeme na portál Azure a prohlédneme si, co jste vytvořili:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) pomocí účtu Microsoft, který obsahuje předplatné Azure.

2. V levém panelu klikněte na položku **App Services**.

3. Kliknutím na aplikaci, kterou jste právě vytvořili, otevřete její stránku na portálu (nazývá se [okno](../azure-portal-overview.md)). Ve výchozím nastavení se rovněž otevře okno **Nastavení**, které vám usnadní práci.

    ![Zobrazení portálu první webové aplikace v Azure](./media/app-service-web-get-started/portal-view.png)

Okno portálu aplikace služby App Service nabízí bohatou sadu nastavení a nástrojů, které vám pomohou při konfiguraci, monitorování, zabezpečení či řešení problémů aplikace. Doporučujeme věnovat chvíli seznámení se s tímto rozhraním tím, že provedete několik jednoduchých úloh. (Číslo úlohy odpovídá číslu na snímku obrazovky.)

1. Zastavte aplikaci.
2. Restartujte aplikaci.
3. Kliknutím na odkaz **Skupina prostředků** zobrazte všechny prostředky, které jsou ve skupině prostředků nasazeny.
4. Kliknutím na položku **Nastavení** > **Vlastnosti** zobrazte další informace o aplikaci.
5. Kliknutím na položku **Nástroje** získejte přístup k užitečným nástrojům pro sledování a řešení problémů.  

## Další kroky

- Zdokonalte aplikaci Azure o další úroveň. Zvyšte míru jejího zabezpečení pomocí ověřování. Škálujte ji na základě poptávky. Nastavte některá upozornění týkající se výkonu. To vše pomocí několika kliknutí. Viz téma [Přidání funkce do první webové aplikace](app-service-web-get-started-2.md).
- Kromě Git a rozhraní příkazového řádku Azure CLI existují i další způsoby jak nasadit webové aplikace do Azure. Viz [Nasazení vaší aplikace do Azure App Service](../app-service-web/web-sites-deploy.md).
Chcete-li nalézt upřednostňovaný postup vývoje a nasazení pro své jazykové rozhraní, vyberte požadované rozhraní v horní části článku.



<!--HONumber=sep16_HO1-->


