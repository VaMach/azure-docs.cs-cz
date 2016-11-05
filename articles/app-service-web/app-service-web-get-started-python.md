---
title: Nasazení první webové aplikace v Pythonu do Azure během pěti minut | Microsoft Docs
description: Nasazením ukázkové aplikace zjistíte, jak snadné je spustit webové aplikace ve službě App Service. Pusťte se hned do skutečného vývoje. Výsledky uvidíte okamžitě.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: wpickett
editor: ''

ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/16/2016
ms.author: cephalin

---
# Nasazení první webové aplikace v Pythonu do Azure během pěti minut
Tento kurz vám pomůže nasadit první webovou aplikaci v Pythonu do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Službu App Service můžete používat při vytváření webových aplikací, [back-endů mobilních aplikací](/documentation/learning-paths/appservice-mobileapps/) a [aplikací API](../app-service-api/app-service-api-apps-why-best-platform.md).

Vaším úkolem je: 

* Vytvořit webovou aplikaci ve službě Azure App Service.
* Nasadit ukázkový kód Pythonu.
* Sledovat spuštění kódu naživo v produkčním prostředí.
* Aktualizovat webovou aplikaci stejným způsobem, jakým byste [nuceně doručili (push) potvrzené změny do Git](https://git-scm.com/docs/git-push).

## Požadavky
* [Nainstalovat Git](http://www.git-scm.com/downloads). Ověřit, že instalace proběhla úspěšně, a to spuštěním `git --version` z nového příkazového řádku Windows, okna PowerShellu, prostředí Linuxu nebo terminálu OS X.
* Získat účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](http://go.microsoft.com/fwlink/?LinkId=523751) bez účtu Azure. Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.
> 
> 

<a name="create"></a>

## Vytvoření webové aplikace
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) pomocí svého účtu Azure.
2. V levé nabídce klikněte na **Nové** > **Web + mobilní zařízení** > **Webová aplikace**.
   
    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)
3. V okně pro vytvoření aplikace použijte pro novou aplikaci následující nastavení:
   
   * **Název aplikace**: Zadejte jedinečný název.
   * **Skupina prostředků**: Vyberte **Vytvořit novou** a zadejte název nové skupiny prostředků.
   * **Plán/umístění služby App Service**: Kliknutím na tuto položku přejděte ke konfiguraci a potom klikněte na **Vytvořit nový** a nastavte název, umístění a cenovou úroveň plánu služby App Service. Nebojte se využít cenovou úroveň **Free**.
     
     Až skončíte, mělo by okno pro vytvoření aplikace vypadat takto:
     
     ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)
4. V dolní části klikněte na **Vytvořit**. Kliknutím na ikonu **Oznámení** v horní části můžete zobrazit průběh akce.
   
    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)
5. Až se nasazení dokončí, mělo by se zobrazit tato oznámení. Kliknutím na tuto zprávu otevřete okno vašeho nasazení.
   
    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)
6. V okně **Nasazení bylo úspěšné** klikněte na odkaz **Prostředek**. Otevře se okno nové webové aplikace.
   
    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## Nasazení kódu do webové aplikace
Teď do Azure pomocí Gitu nasadíme kód.

1. V okně webové aplikace se posuňte dolů k položce **Možnosti nasazení** (nebo ji vyhledejte) a potom na ni klikněte. 
   
    ![](./media/app-service-web-get-started-languages/deploy-web-app-deployment-options.png)
2. Klikněte na **Zvolit zdroj** > **Místní úložiště Git** > **OK**.
3. V okně webové aplikace klikněte na **Přihlašovací údaje pro nasazení**.
4. Nastavte přihlašovací údaje pro nasazení a klikněte na **Uložit**.
5. V okně webové aplikace se posuňte dolů k položce **Vlastnosti** (nebo ji vyhledejte) a potom na ni klikněte. Vedle položky **Adresa URL pro Git** klikněte na tlačítko **Kopírovat**.
   
    ![](./media/app-service-web-get-started-languages/deploy-web-app-properties.png)
   
    Teď jste připraveni nasadit svůj kód pomocí Gitu.
6. V terminálu příkazového řádku přejděte do pracovního adresáře (`CD`) a naklonujte ukázkovou aplikaci tímto způsobem:
   
        git clone https://github.com/Azure-Samples/app-service-web-python-get-started.git
   
    ![Klonování ukázkového kódu aplikace pro první webovou aplikaci v Azure](./media/app-service-web-get-started-languages/python-git-clone.png)
   
    Jako adresu *&lt;github_sample_url>* použijte jednu z následujících adres URL, a to v závislosti na vámi upřednostňovaném prostředí:
7. Přejděte do úložiště ukázkové aplikace. Například: 
   
        cd app-service-web-html-get-started
8. Nakonfigurujte vzdálené úložiště Git pro aplikaci Azure pomocí adresy URL pro Git, kterou jste zkopírovali z portálu v jednom z předchozích kroků.
   
        git remote add azure <giturlfromportal>
9. Nasaďte ukázkový kód do aplikace Azure stejným způsobem, jakým byste doručili bez vyžádání (push) libovolný kód prostřednictvím Gitu:
   
        git push azure master
   
    ![Nucené doručení (push) kódu do první webové aplikace v Azure](./media/app-service-web-get-started-languages/python-git-push.png)    
   
    Pokud jste použili jedno z jazykových rozhraní, zobrazí se jiný výstup. Důvodem je, že `git push` nejen vloží kód v Azure, ale také aktivuje úlohy nasazení v modulu pro nasazení. Pokud máte v kořenovém adresáři (úložišti) projektu soubor requirements.txt, skript nasazení obnoví požadované balíčky za vás. 

A to je vše! Váš kód teď živě běží v Azure. V prohlížeči přejděte na http://*&lt;název_aplikace>*.azurewebsites.net a prohlédněte si ho. 

## Provádění aktualizací aplikace
Nyní můžete pomocí Git kdykoli provádět nucené doručení (push) z kořenového adresáře projektu (úložiště) a aktualizovat živý web. Postup je stejný jako při prvním nasazení kódu. Například pokaždé, když chcete nuceně doručit (push) novou změnu, kterou jste místně otestovali, stačí spustit následující příkazy z kořenového adresáře projektu (úložiště):

    git add .
    git commit -m "<your_message>"
    git push azure master

## Další kroky
[Vytvořte, nakonfigurujte a nasaďte webovou aplikaci Django do Azure pomocí sady Visual Studio](web-sites-python-ptvs-django-mysql.md). V tomto kurzu se naučíte základní dovednosti, které v Azure potřebujete ke spuštění libovolné webové aplikace v Pythonu, jako je:

* Vytvoření a nasazení aplikace v Pythonu pomocí šablony
* Nastavení verze Pythonu
* Vytvoření virtuálních prostředí
* Připojení k databázi

Další možností je pokračovat v práci s první webovou aplikací. Příklad:

* Vyzkoušejte si [další způsoby nasazení kódu do Azure](web-sites-deploy.md). Pokud třeba chcete pro nasazení použít některé z úložišť GitHubu, stačí v části **Možnosti nasazení** místo **Místní úložiště Git** vybrat **GitHub**.
* Zdokonalte aplikaci Azure o další úroveň. Ověřte svoje uživatele. Škálujte ji na základě poptávky. Nastavte některá upozornění týkající se výkonu. To vše pomocí několika kliknutí. Viz téma [Přidání funkce do první webové aplikace](app-service-web-get-started-2.md).

<!--HONumber=Sep16_HO3-->


