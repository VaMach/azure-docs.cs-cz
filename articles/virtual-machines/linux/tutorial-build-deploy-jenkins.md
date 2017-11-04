---
title: "CI/CD z volaných pro virtuální počítače Azure pomocí služby VSTS | Microsoft Docs"
description: "Nastavte průběžnou integraci (CI) a průběžné nasazování (CD) aplikace Node.js pomocí volaných k virtuálním počítačům Azure ze správy verzí v aplikaci Visual Studio Team Services (VSTS) nebo Microsoft Team Foundation Server (TFS)"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/19/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: d5c15d6ab36a8454d1c69bac498be89b990c7e33
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-vsts"></a>Nasazení aplikace do virtuální počítače s Linuxem pomocí volaných a služby VSTS

Průběžnou integraci (CI) a průběžné nasazování (CD) je kanál, pomocí kterého můžete sestavit, verzi a nasadíte tak svůj kód. Visual Studio Team Services (VSTS) poskytuje úplný, plně funkční sadu nástrojů automatizace CI nebo CD pro nasazení do Azure. Volaných je Oblíbené 3. stran CI/CD na serveru nástroj, který poskytuje také CI/CD automatizace. Můžete i společně k přizpůsobení jak poskytovat cloudové aplikace nebo služby.

V tomto kurzu použijete k sestavení volaných **webové aplikace Node.js**a služby VSTS nebo Team Foundation Server (TFS) nasadit ji [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) obsahující virtuální počítače s Linuxem.

Vaším úkolem je:

> [!div class="checklist"]
> * Načíst ukázková aplikace
> * Konfigurace volaných moduly plug-in
> * Konfigurace projektu volný volaných styl pro Node.js
> * Konfigurace volaných pro integraci služby VSTS
> * Vytvoření koncového bodu služby volaných
> * Vytvořit skupinu nasazení pro virtuální počítače Azure
> * Vytvoření verze definice služby VSTS
> * Spustit ruční a aktivaci položky konfigurace nasazení

## <a name="before-you-begin"></a>Než začnete

* Potřebujete přístup k serveru volaných. Pokud jste ještě nevytvořili volaných server, přečtěte si téma [vytvořit hlavní volaných na virtuální počítač Azure](https://docs.microsoft.com/en-us/azure/jenkins/install-jenkins-solution-template). 

* Přihlaste se k účtu služby VSTS (`https://{youraccount}.visualstudio.com`). 
  Můžete získat [bezplatný účet služby VSTS](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Další informace najdete v tématu [připojit k služby VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

*  Je nutné virtuální počítač s Linuxem pro cíl nasazení.  Další informace najdete v tématu [vytvořit a spravovat virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm).

*  Otevřete příchozí port 80 pro virtuální počítač.  Další informace najdete v tématu [vytvoření skupin zabezpečení sítě pomocí portálu Azure](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>Načíst ukázková aplikace

Je nutné aplikaci k nasazení uložené v úložišti Git.
V tomto kurzu doporučujeme, abyste použili [této ukázkové aplikace, které jsou k dispozici z Githubu](https://github.com/azooinmyluggage/fabrikam-node).  Tento kurz obsahuje ukázkový skript používá pro instalaci Node.js a aplikace.  Pokud chcete pracovat s vlastní úložiště, byste měli nakonfigurovat podobně jako ukázka.

1. Vytvoření větve tuto aplikaci a poznamenejte si umístění (URL) pro použití v dalších krocích tohoto kurzu.  Další informace najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/)    

> [!NOTE]
> Aplikace bylo vytvořeno prostřednictvím [Yeoman](http://yeoman.io/learning/index.html); používá **Express**, **bower**, a **grunt**; a má některé **npm** balíčky jako závislosti.
> Ukázka také obsahuje skript, který nastaví Nginx a nasadí aplikaci. Spuštění u virtuálních počítačů. Konkrétně skript nainstaluje uzlu, Nginx a PM2; Nakonfiguruje Nginx a PM2; pak spustí aplikaci uzlu.

## <a name="configure-jenkins-plugins"></a>Konfigurace volaných moduly plug-in

Nejdřív musíte nakonfigurovat dvě volaných moduly plug-in pro **NodeJS** a **VS Team Services průběžné nasazování**.

1. Otevřete váš účet volaných a zvolte **spravovat volaných**.
2. V **spravovat volaných** vyberte **Správa modulů plug-in**.
3. Filtrování seznamu a vyhledejte **NodeJS** modulů plug-in a zvolte **nainstalovat bez restartování** možnost.
    ![Přidání modulu plug-in NodeJS do volaných](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Filtrovat seznam a vyhledat **VS Team Services průběžné nasazování** modulů plug-in a zvolte **nainstalovat bez restartování** možnost.
5. Vraťte se na řídicím panelu volaných a zvolte **spravovat volaných**.
6. Zvolte **globální nástroj Konfigurace**.  Najít **NodeJS** a klikněte na tlačítko **NodeJS instalace**.
7. Povolit **instalovat automaticky** možnost a potom zadejte **název** hodnotu.
8. Klikněte na **Uložit**.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Konfigurace projektu volný volaných styl pro Node.js

1. Klikněte na tlačítko **novou položku**.  Zadejte **název položky**.
2. Zvolte **volný styl projektu**.  Klikněte na **OK**.
3. V **správu zdrojového kódu** vyberte **Git** a zadejte podrobnosti o úložišti a větev obsahující kódu aplikace.    
    ![Přidat úložišti do vaší sestavení](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. V **sestavení aktivační události** vyberte **dotazování SCM** a zadejte plán `H/03 * * * *` k dotazování úložiště Git pro změny každé tři minuty. 
5. V **sestavení prostředí** vyberte **poskytují uzlu &amp; npm bin / složky cesta** a vyberte **NodeJS instalace** hodnotu. Nechte **npmrc souboru** nastavena na "použít výchozí systémové nastavení."
6. V **sestavení** , zvolte **spustit prostředí** a zadejte příkaz `npm install` zajistit jsou aktualizovány všechny závislosti.


## <a name="configure-jenkins-for-vsts-integration"></a>Konfigurace volaných pro integraci služby VSTS

  > [!NOTE]
  Ujistěte se, osobní přístupový token (Jan), použijte následující postup obsahuje **verze (čtení, zápis, spuštění a spravovat) oprávnění v služby VSTS**.
 
1.  Pokud již nemáte, vytvořte v účtu služby VSTS PAT. Volaných vyžaduje zadání těchto informací pro přístup k účtu služby VSTS.  Ujistěte se, můžete **ukládání** informace o tokenu pro nadcházející kroky v této části.
  Čtení [vytvoření osobního přístupového tokenu služby VSTS a TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) se dozvíte, jak jej vygenerovat.
2. V **akce po sestavení** , klikněte na **přidat akci po sestavení**. Zvolte **archivu artefakty**.
3. Pro **soubory k archivaci**, zadejte `**/*` zahrnout všechny soubory.
4. Chcete-li vytvořit další akci, klikněte na tlačítko **přidat akci po sestavení**.
5. Zvolte **aktivovat verze v TFS/Team Services**, jako například zadejte identifikátor uri pro váš účet služby VSTS: `https://{your-account-name}.visualstudio.com`.
6. Zadejte **týmového projektu** název.
7. Zvolte název **verze definice** (vytvořit tuto definici verze později v služby VSTS).
8. Vyberte přihlašovací údaje pro připojení k prostředí služby VSTS nebo TFS.  Ponechte **uživatelské jméno** prázdný, pokud používáte služby VSTS.
   Zadejte **uživatelské jméno a heslo** Pokud se používá místní verzi sady TFS.    
    ![Konfigurace akce volaných po sestavení](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. **Uložit** volaných projektu.

## <a name="create-a-jenkins-service-endpoint"></a>Vytvoření koncového bodu služby volaných

Koncový bod služby umožňuje služby VSTS pro připojení k volaných.

1. Otevřete **služby** stránku služby VSTS, otevřete **nový koncový bod služby** seznam a vyberte **volaných**.
    ![Přidání koncového bodu volaných](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Zadejte název pro připojení.
3. Zadejte adresu URL vašeho serveru volaných a značek **přijetí nedůvěryhodných certifikátů SSL** možnost.  Příkladem je adresa URL:`http://{YourJenkinsURL}.westcentralus.cloudapp.azure.com`
4. Zadejte **uživatelské jméno a heslo** pro váš účet volaných.
5. Zvolte **ověření připojení** zkontrolujte správnost informací.
6. Zvolte **OK** vytvořit koncový bod služby.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Vytvořte skupinu nasazení pro virtuální počítače Azure

Je nutné [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) se registrovat agenta služby VSTS, aby definice vydání, můžete nasadit k virtuálnímu počítači.  Nasazení skupiny usnadňují definovat logické skupiny cílových počítačů pro nasazení a nainstalujte požadované agent na každém počítači.

   > [!NOTE]
   > V následujících krocích, ujistěte se, nainstalujte součásti a **nemáte spustit skript s oprávněními sudo.**

1. Otevřete **verze** kartě **sestavení &amp; verze** rozbočovače, otevřete **nasazení skupiny**a zvolte **+ nový**.
2. Zadejte název skupiny nasazení a volitelný popis.
   Zvolte **vytvořit**.
3. Vyberte operační systém pro vaše nasazení cílového virtuálního počítače.  Například vyberte **Ubuntu 16.04 +**.
4. Značky **používat osobní přístupový token ve skriptu pro ověřování**.
5. Vyberte **požadované součásti systému** odkaz.  Instalace požadovaných součástí pro operační systém.
6. Vyberte **zkopírujte skript do schránky** skript zkopírovat.
7. **Přihlaste se** pro vaše nasazení cílového virtuálního počítače a **provést** skript.  **Nemáte** spusťte skript s oprávněními sudo.
8. Po instalaci zobrazí se výzva pro nasazení skupiny značky.  Přijměte výchozí hodnoty.
9. V služby VSTS, zkontrolujte pro nově zaregistrovaný virtuálního počítače v **cíle** pod **nasazení skupiny**.

## <a name="create-a-vsts-release-definition"></a>Vytvoření verze definice služby VSTS

Verze definice určuje proces služby VSTS provede k nasazení aplikace.  V tomto příkladu je spustit skript prostředí.

K vytvoření definice verze v služby VSTS:

1. Otevřete **verze** na **sestavení &amp; verze** rozbočovače a zvolte **vytvořit verze definice**. 
2. Vyberte **prázdný** šablony výběrem začněte s **prázdný proces**.
3. V **artefakty** části, klikněte na **+ přidat artefaktů** a zvolte **volaných** pro **typ zdroje**. Vyberte připojení volaných koncový bod služby. Pak vyberte úlohu volaných zdroje a zvolte **přidat**.
4.  Klikněte na tlačítko se třemi tečkami vedle **prostředí 1**.  Klikněte na tlačítko **fáze skupiny přidat nasazení**.
5.  Zvolte vaše **skupiny nasazení**.
5. Klikněte na tlačítko  **+**  přidat úloha **fáze nasazení skupiny**.
6. Vyberte **skript prostředí** úloh a klikněte na tlačítko **přidat**.    
    **Skript prostředí** úloha slouží k poskytování konfigurace pro skript běžet na každém serveru nainstalovat Node.js a spusťte aplikaci. Nakonfigurujte následujícím způsobem:
8. **Skript cesta**:     
    `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`
9.  Klikněte na tlačítko **Upřesnit**a pak povolte **zadat pracovní adresář**.
10.  **Pracovní adresář**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
11. Upravit název definice verze na název, který jste zadali v **akce po sestavení** kartě sestavení v volaných. Volaných vyžaduje tento název moct aktivovat novou verzi, když jsou aktualizovány zdrojové artefakty.
12. Klikněte na tlačítko **Uložit**a klikněte na tlačítko **OK** se uložit definici verze.

## <a name="execute-manual-and-ci-triggered-deployments"></a>Spustit ruční a aktivaci položky konfigurace nasazení

1. Zvolte **+ verze** a vyberte **vytvořit verzi**.
2. Vyberte sestavení, můžete dokončit v zvýrazněná rozevíracího seznamu a zvolte **fronty**.
3. Vyberte odkaz verze v místní zpráva. Například: "verze **verze 1** byla vytvořena."
4. Otevřete **protokoly** a podívejte se na výstup konzoly verze.
5. V prohlížeči otevřete adresu URL jednoho ze serverů, které jste přidali do vaší skupiny nasazení. Zadejte například`http://{your-server-ip-address}`
6. Přejít na zdrojové úložiště Git a upravovat obsah **h1** záhlaví v souboru app/views/index.jade změněné textem.
7. **Potvrdit** změny.
8. Po několika minutách, zobrazí se nová verze vytvořené v **verze** stránka služby VSTS nebo TFS. Otevřete verzi zobrazíte probíhající nasazení. Blahopřejeme!

## <a name="next-steps"></a>Další kroky

V tomto kurzu automatické nasazení aplikace do Azure pomocí volaných sestavení a služby VSTS pro verzi. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Sestavení aplikace ve volaných
> * Konfigurace volaných pro integraci služby VSTS
> * Vytvořit skupinu nasazení pro virtuální počítače Azure
> * Vytvořit definici verze, která slouží ke konfiguraci virtuálních počítačů a nasadí aplikaci

Přechod na další kurzu se dozvíte další informace o tom, jak nasadit SVÍTILNU zásobníku (Linux, Apache, MySQL a PHP,).

> [!div class="nextstepaction"]
> [Nasazení zásobníku LAMP](tutorial-lamp-stack.md)