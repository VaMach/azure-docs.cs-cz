---
title: "Místní nasazení z Gitu do služby Azure App Service"
description: "Zjistěte, jak povolit místní nasazení Git do služby Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 19681564ef48649d4d44da1d0a2a65044f494ce4
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2018
---
# <a name="local-git-deployment-to-azure-app-service"></a>Místní nasazení z Gitu do služby Azure App Service
V tomto kurzu se dozvíte, jak nasadit aplikaci, kterou chcete [Azure Web Apps](app-service-web-overview.md) z úložiště Git v místním počítači. App Service podporuje tento přístup s **místní Git** v možnost nasazení [portálu Azure].  
Mnoho příkazů Git popsané v tomto článku se provádí automaticky při vytváření aplikace služby App Service pomocí [rozhraní příkazového řádku Azure] popsané [zde](app-service-web-get-started-dotnet.md).

## <a name="prerequisites"></a>Požadavky
Pro absolvování tohoto kurzu potřebujete:

* Git. Můžete si stáhnout binární instalace [zde](http://www.git-scm.com/downloads).  
* Základní znalosti o Git.
* Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte na [vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní aplikaci ve službě App Service. Není vyžadována platební karta a nevzniká žádný závazek.  
> 
> 

## <a name="Step1"></a>Krok 1: Vytvoření místní úložiště
Proveďte následující kroky k vytvoření nového úložiště Git.

1. Spusťte nástroj příkazového řádku, jako například **Git Bash** (Windows) nebo **Bash** (Unix prostředí). V systémech OS X dostanete příkazového řádku pomocí **Terminálové** aplikace.
2. Přejděte do adresáře, kde by umístění obsahu pro nasazení.
3. Použijte následující příkaz k chybě při inicializaci nového úložiště Git:

```bash  
git init
```

## <a name="Step2"></a>Krok 2: Potvrzení obsahu
App Service podporuje aplikace vytvořené v různých programovacích jazyků. 

1. Pokud úložiště už obsahuje obsahu Přeskočit tento bod a přesunout do bodu 2 níže. Pokud úložiště už neobsahuje obsah jednoduše naplnění soubor statické .html následujícím způsobem: 
   
   * Pomocí textového editoru, vytvořte nový soubor s názvem **index.html** v kořenovém adresáři úložiště Git
   * Přidejte následující text jako obsah pro index.html souboru a uložte jej: *Hello Git!*
2. Z příkazového řádku ověřte, že jste v kořenovém adresáři úložiště Git. Pak použijte následující příkaz pro přidání souborů do úložiště:

```bash  
git add -A
```
3. V dalším kroku potvrďte změny do úložiště pomocí následujícího příkazu:

```bash  
git commit -m "Hello Azure App Service"
```  

## <a name="Step3"></a>Krok 3: Povolení úložišti aplikace služby App Service
Pomocí následujících kroků povolte úložiště Git pro vaši aplikaci služby App Service.

1. Přihlaste se k [portálu Azure].
2. V okně aplikace App Service, klikněte na tlačítko **Nastavení > zdroj nasazení**. Klikněte na tlačítko **vybrat zdroj**, pak klikněte na tlačítko **místní úložiště Git**a potom klikněte na **OK**.  
   
    ![Místní úložiště Git](./media/app-service-deploy-local-git/local_git_selection.png)
3. Pokud je vaše první čas nastavení úložiště v Azure, budete muset vytvořit přihlašovací údaje pro ni. Budete je používat k přihlášení do Azure změny úložiště a posílejte nabízená oznámení z místního úložiště Git. V okně vaší aplikace, klikněte na tlačítko **nasazení > přihlašovací údaje nasazení**, nakonfigurujte nasazení uživatelské jméno a heslo. Když jste hotovi, klikněte na tlačítko **Uložit**.
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Krok 4: Nasazení projektu
Pomocí následujících kroků k publikování aplikace do služby App Service pomocí místní Git.

1. V okně vaší aplikace na portálu Azure, klikněte na tlačítko **Nastavení > vlastnosti** pro **adresy URL pro Git**.
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    **Adresy URL pro Git** je vzdálený odkaz k nasazení z místního úložiště. V následujících krocích použijete tuto adresu URL.
2. Pomocí příkazového řádku, ověřte, že jste v kořenu místní úložiště Git.
3. Použití `git remote` přidat odkaz na vzdálené uvedené v **adresy URL pro Git** z kroku 1. Váš příkaz bude vypadat nějak takto:
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > **Vzdáleného** příkaz přidá odkaz s názvem do vzdáleného úložiště. V tomto příkladu se vytvoří odkaz úložiště vaší webové aplikace s názvem "azure".
   > 
   > 
4. Push svůj obsah do služby App Service pomocí nové **azure** vzdálené jste právě vytvořili.

```bash  
git push azure master
```
    You will be prompted for the password you created earlier when you reset your deployment credentials in the Azure Portal. Enter the password (note that Gitbash does not echo asterisks to the console as you type your password). 
5. Vraťte se zpátky do vaší aplikace na portálu Azure. Položka protokolu z vaší poslední nabízené má být zobrazena v **nasazení** okno. 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. Klikněte **Procházet** tlačítka v horní části okna aplikace k ověření nasazený obsah. 

## <a name="Step5"></a>Řešení potíží
Tady jsou chyby nebo problémy, které jsou běžně došlo při použití Git pro publikování do aplikace služby App Service v Azure:

- - -
**Příznaky**: Nelze získat přístup k [siteURL]: Nepodařilo se připojit k [scmAddress]

**Příčina**: této chybě může dojít, pokud aplikace není spuštěná.

**Řešení**: Spusťte aplikaci na portálu Azure. Nasazení Git nebude fungovat, pokud aplikace běží. 

- - -
**Příznaky**: nelze přeložit název hostitele, hostitele.

**Příčina**: k této chybě může dojít, pokud bylo nesprávné informace o adrese zadali při vytváření vzdálené služby azure.

**Řešení**: použití `git remote -v` příkazu zobrazíte všechny dálkové ovladače, společně s přidružené adresy URL. Ověřte správnost adresy URL pro "azure" vzdálené. V případě potřeby odeberte a znovu tento vzdálené pomocí správnou adresu URL.

- - -
**Příznaky**: žádné odolný systém souborů v běžné a žádná zadaný; žádným způsobem. Možná musíte zadat větve, jako je například "hlavní".

**Příčina**: k této chybě může dojít, pokud při provádění git push operace a nebyly nastavte hodnotu push.default používá Git nezadávejte větev.

**Řešení**: proveďte nabízenou akci, zadání hlavní větve. Příklad:

```bash  
git push azure master
```
- - -
**Příznaky**: src refspec [branchname] neodpovídá žádnému.

**Příčina**: k této chybě může dojít, pokud se pokusíte nabízená větev pouze hlavní na "azure" vzdálené.

**Řešení**: proveďte nabízenou akci, zadání hlavní větve. Příklad:

```bash  
git push azure master
```
- - -
**Příznaky**: RPC se nezdařilo; způsobit = 22, kód HTTP = 502.

**Příčina**: k této chybě může dojít, pokud se pokusíte úložiště velké git push přes protokol HTTPS.

**Řešení**: Změna konfigurace git v místním počítači, chcete-li postBuffer zvětšit

```bash  
git config --global http.postBuffer 524288000
```
- - -
**Příznaky**: Chyba: změny potvrzeny do vzdáleného úložiště, ale neaktualizuje webové aplikace.

**Příčina**: této chybě může dojít, pokud nasazujete aplikaci Node.js obsahující soubor package.json, který určuje další požadované moduly.

**Řešení**: další zprávy obsahující 'npm ERR!. by měl být před přihlášeného k této chybě a může poskytnout další kontext při selhání. Následující seznam uvádí známé příčiny této chyby a odpovídající 'npm ERR!. zpráva:

* **Soubor package.json chybná**: npm ERR! Nepodařilo se přečíst závislosti.
* **Nativní modul, který nemá binární distribuce pro systém Windows**:
  
  * npm ERR! \`cmd "/ c" "uzlu gyp opětovné sestavení"\` se nezdařilo s 1
    
      NEBO
  * npm ERR! [modulename@version] předinstalovat: \`zkontrolujte || gmake\`

## <a name="additional-resources"></a>Další prostředky
* [Dokumentace pro Git](http://git-scm.com/documentation)
* [Dokumentace Kudu projektu](https://github.com/projectkudu/kudu/wiki)
* [Průběžné nasazování do Azure App Service](app-service-continuous-deployment.md)
* [Způsob používání prostředí PowerShell pro Azure](/powershell/azure/overview)
* [Jak používat rozhraní příkazového řádku Azure](../cli-install-nodejs.md)

[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[portálu Azure]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[rozhraní příkazového řádku Azure]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
