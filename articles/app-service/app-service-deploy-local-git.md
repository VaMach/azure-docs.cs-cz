---
title: "Místní nasazení z Gitu do služby Azure App Service"
description: "Zjistěte, jak povolit místní nasazení Git do služby Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: cfowler
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 7667f94ac4e7edd4e94d6605adefea469102a0c4
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="local-git-deployment-to-azure-app-service"></a>Místní nasazení z Gitu do služby Azure App Service

V tomto kurzu se dozvíte, jak nasadit aplikaci, kterou chcete [Azure Web Apps](app-service-web-overview.md) z úložiště Git v místním počítači. App Service podporuje tento přístup s **místní Git** v možnost nasazení [portál Azure].

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* Git. Můžete si stáhnout binární instalace [zde](http://www.git-scm.com/downloads).
* Základní znalosti o Git.
* Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte na [vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní aplikaci ve službě App Service. Není vyžadována platební karta a nevzniká žádný závazek.
>

## <a name="Step1"></a>Krok 1: Vytvoření místní úložiště

Proveďte následující kroky k vytvoření nového úložiště Git.

1. Spusťte nástroj příkazového řádku, jako například **Git Bash** (Windows) nebo **Bash** (Unix prostředí). V systémech OS X, můžete přístup k příkazovému řádku prostřednictvím **Terminálové** aplikace.
1. Přejděte do adresáře, kde by umístění obsahu pro nasazení.
1. Použijte následující příkaz k chybě při inicializaci nového úložiště Git:

  ```bash
  git init
  ```

## <a name="Step2"></a>Krok 2: Potvrzení obsahu

App Service podporuje aplikace vytvořené v různých programovacích jazyků.

1. Pokud úložiště už neobsahuje obsah, přidejte soubor statické .html následujícím způsobem; nebo můžete tento krok přeskočit:
   * Pomocí textového editoru, vytvořte nový soubor s názvem **index.html** v kořenovém adresáři úložiště Git
   * Přidejte následující text jako obsah pro index.html souboru a uložte jej: *Hello Git!*
1. Z příkazového řádku ověřte, že jste v kořenovém adresáři úložiště Git. Pak použijte následující příkaz pro přidání souborů do úložiště:

    ```bash
    git add -A
    ```
    
1. V dalším kroku potvrďte změny do úložiště pomocí následujícího příkazu:

    ```bash
    git commit -m "Hello Azure App Service"
    ```

## <a name="Step3"></a>Krok 3: Povolení úložišti aplikace služby App Service

Pomocí následujících kroků povolte úložiště Git pro vaši aplikaci služby App Service.

1. Přihlaste se k portálu [portál Azure].
1. V zobrazení aplikaci aplikační služby, klikněte na tlačítko **Nastavení > zdroj nasazení**. Klikněte na tlačítko **vybrat zdroj**, pak klikněte na tlačítko **místní úložiště Git**a potom klikněte na **OK**.

    ![Místní úložiště Git](./media/app-service-deploy-local-git/local_git_selection.png)

1. Pokud je vaše první čas nastavení úložiště v Azure, budete muset vytvořit přihlašovací údaje pro ni. Je používáte k přihlášení do Azure změny úložiště a posílejte nabízená oznámení z místního úložiště Git. Ze zobrazení vaší webové aplikace, klikněte na tlačítko **Nastavení > přihlašovací údaje nasazení**, nakonfigurujte nasazení uživatelské jméno a heslo. Když jste hotovi, klikněte na tlačítko **Uložit**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Krok 4: Nasazení projektu

Pomocí následujících kroků k publikování aplikace do služby App Service pomocí místní Git.

1. V zobrazení vaší webové aplikace v portálu Azure, klikněte na tlačítko **Nastavení > vlastnosti** pro **adresy URL pro Git**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Adresy URL pro Git** je vzdálený odkaz k nasazení z místního úložiště. Pomocí této adresy URL v dalších krocích.
1. Pomocí příkazového řádku, ověřte, že jste v kořenu místní úložiště Git.
1. Použití `git remote` přidat odkaz na vzdálené uvedené v **adresy URL pro Git** z kroku 1. Váš příkaz vypadá podobně jako následující:

    ```
    git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git
    ```

   > [!NOTE]
   > **Vzdáleného** příkaz přidá odkaz s názvem do vzdáleného úložiště. V tomto příkladu se vytvoří odkaz úložiště vaší webové aplikace s názvem "azure".
   >

1. Push svůj obsah do služby App Service pomocí nové **azure** vzdálené jste vytvořili.

    ```bash
    git push azure master
    ```

    Zobrazí se výzva k zadání hesla, které jste předtím vytvořili při resetování přihlašovací údaje nasazení na portálu Azure. Zadejte heslo (Všimněte si, že Git Bash není odezvu hvězdičky ke konzole jako typ heslo). 
1. Vraťte se zpátky do vaší aplikace na portálu Azure. Položka protokolu z vaší poslední nabízené má být zobrazena v **nasazení** zobrazení.

    ![](./media/app-service-deploy-local-git/deployment_history.png)

1. Klikněte **Procházet** tlačítka v horní části webové stránky aplikace k ověření nasazený obsah.

## <a name="Step5"></a>Řešení potíží

Tady jsou chyby nebo problémy, které jsou běžně došlo při použití Git pro publikování do aplikace služby App Service v Azure:

---
**Příznaky**:`Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Příčina**: této chybě může dojít, pokud aplikace není spuštěná.

**Řešení**: Spusťte aplikaci na portálu Azure. Nasazení Git není dostupná, když webové aplikace je zastavena.

---
**Příznaky**:`Couldn't resolve host 'hostname'`

**Příčina**: k této chybě může dojít, pokud bylo nesprávné informace o adrese zadali při vytváření vzdálené služby azure.

**Řešení**: použití `git remote -v` příkazu zobrazíte všechny dálkové ovladače, společně s přidružené adresy URL. Ověřte správnost adresy URL pro "azure" vzdálené. V případě potřeby odeberte a znovu tento vzdálené pomocí správnou adresu URL.

---
**Příznaky**:`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Příčina**: k této chybě může dojít, pokud nezadáte větev během `git push`, nebo pokud jste nenastavili `push.default` hodnotu `.gitconfig`.

**Řešení**: proveďte nabízenou akci, zadání hlavní větve. Příklad:

```bash
git push azure master
```

---
**Příznaky**:`src refspec [branchname] does not match any.`

**Příčina**: k této chybě může dojít, pokud se pokusíte nabízená větev pouze hlavní na "azure" vzdálené.

**Řešení**: proveďte nabízenou akci, zadání hlavní větve. Příklad:

```bash
git push azure master
```

---
**Příznaky**:`RPC failed; result=22, HTTP code = 5xx.`

**Příčina**: k této chybě může dojít, pokud se pokusíte úložiště velké git push přes protokol HTTPS.

**Řešení**: Změna konfigurace git v místním počítači, chcete-li postBuffer zvětšit

```bash
git config --global http.postBuffer 524288000
```

---
**Příznaky**:`Error - Changes committed to remote repository but your web app not updated.`

**Příčina**: této chybě může dojít, pokud nasazujete aplikaci Node.js obsahující soubor package.json, který určuje další požadované moduly.

**Řešení**: další zprávy obsahující 'npm ERR!. by měl být před přihlášeného k této chybě a může poskytnout další kontext při selhání. Následující seznam uvádí známé příčiny této chyby a odpovídající 'npm ERR!. zpráva:

* **Soubor package.json chybná**: npm ERR! Nepodařilo se přečíst závislosti.
* **Nativní modul, který nemá binární distribuce pro systém Windows**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      NEBO
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Další prostředky

* [Dokumentace pro Git](http://git-scm.com/documentation)
* [Dokumentace Kudu projektu](https://github.com/projectkudu/kudu/wiki)
* [Průběžné nasazování do Azure App Service](app-service-continuous-deployment.md)
* [Způsob používání prostředí PowerShell pro Azure](/powershell/azure/overview)
* [Jak používat rozhraní příkazového řádku Azure](../cli-install-nodejs.md)

[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[portál Azure]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure Command-Line Interface]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
