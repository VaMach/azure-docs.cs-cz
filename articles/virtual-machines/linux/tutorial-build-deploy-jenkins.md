---
title: "CI/CD z volaných pro virtuální počítače Azure se Team Services | Microsoft Docs"
description: "Nastavení průběžnou integraci (CI) a průběžné nasazování (CD) aplikace Node.js pomocí volaných k virtuálním počítačům Azure ze správy verzí v sadě Visual Studio Team Services nebo Microsoft Team Foundation Server"
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
ms.openlocfilehash: bfda0475b58556db1236c8b051c59393384720f7
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-your-app-to-linux-vms-by-using-jenkins-and-team-services"></a>Nasazení aplikace do virtuální počítače s Linuxem pomocí volaných a Team Services

Průběžnou integraci (CI) a průběžné nasazování (CD) představují kanálu, pomocí kterého můžete sestavit, verzi a nasadíte tak svůj kód. Visual Studio Team Services poskytuje úplný, plně funkční sadu nástrojů automatizace CI nebo CD pro nasazení do Azure. Volaných je Oblíbené třetích stran CI/CD na serveru nástroj, který poskytuje také CI/CD automatizace. Team Services a volaných můžete použít společně k přizpůsobení jak poskytovat cloudové aplikace nebo služby.

V tomto kurzu použijete volaných k sestavení webové aplikace Node.js. Potom použít Team Services nebo Team Foundation Server pro její nasazení [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) obsahující Linuxové virtuální počítače (VM).

Vaším úkolem je:

> [!div class="checklist"]
> * Získáte ukázkovou aplikaci.
> * Nakonfigurujte volaných moduly plug-in.
> * Konfigurace projektu volný volaných styl pro Node.js.
> * Nakonfigurujte volaných pro integraci produktů Team Services.
> * Vytvoření koncového bodu služby volaných.
> * Vytvořte skupinu nasazení pro virtuální počítače Azure.
> * Vytvořte definici verze Team Services.
> * Spusťte ruční a aktivovaného CI nasazení.

## <a name="before-you-begin"></a>Než začnete

* Potřebujete přístup k serveru volaných. Pokud jste ještě nevytvořili volaných server, přečtěte si téma [vytvořit hlavní volaných na virtuální počítač Azure](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template). 

* Přihlaste se ke svému účtu Team Services (**https://{youraccount}.visualstudio.com**). 
  Můžete získat [bezplatný účet Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Další informace najdete v tématu [připojit k Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

*  Je nutné virtuální počítač s Linuxem pro cíl nasazení.  Další informace najdete v tématu [vytvořit a spravovat virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm).

*  Otevřete příchozí port 80 pro virtuální počítač. Další informace najdete v tématu [vytvoření skupin zabezpečení sítě pomocí portálu Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>Načíst ukázková aplikace

Je nutné aplikaci k nasazení, uložené v úložišti Git.
V tomto kurzu doporučujeme používat [této ukázkové aplikace, které jsou k dispozici z Githubu](https://github.com/azooinmyluggage/fabrikam-node). Tento kurz obsahuje ukázkový skript, který se používá pro instalaci Node.js a aplikace. Pokud chcete pracovat s vlastní úložiště, byste měli nakonfigurovat podobně jako ukázka.

Vytvoření větve tuto aplikaci a poznamenejte si umístění (URL) pro použití v dalších krocích tohoto kurzu. Další informace najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/).    

> [!NOTE]
> Aplikace byla vytvořena prostřednictvím [Yeoman](http://yeoman.io/learning/index.html). Používá Express, bower a grunt. A má některé balíčky npm jako závislosti.
> Ukázka také obsahuje skript, který nastaví Nginx a nasadí aplikaci. Spuštění u virtuálních počítačů. Konkrétně tento skript:
> 1. Nainstaluje uzlu, Nginx a PM2.
> 2. Nakonfiguruje Nginx a PM2.
> 3. Spuštění aplikace uzlu.

## <a name="configure-jenkins-plug-ins"></a>Konfigurace volaných moduly plug-in

Nejdřív musíte nakonfigurovat dvě volaných moduly plug-in: **NodeJS** a **VS Team Services průběžné nasazování**.

1. Otevřete váš účet volaných a vyberte **spravovat volaných**.
2. Na **spravovat volaných** vyberte **Správa modulů plug-in**.
3. Filtrování seznamu a vyhledejte **NodeJS** modul plug-in a vyberte možnost **nainstalovat bez restartování** možnost.
    ![Přidání modulu plug-in NodeJS do volaných](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Filtrovat seznam a vyhledat **VS Team Services průběžné nasazování** modulu plug-in a vyberte **nainstalovat bez restartování** možnost.
5. Přejděte zpět na řídicí panel volaných a vyberte **spravovat volaných**.
6. Vyberte **globální nástroj Konfigurace**. Najít **NodeJS** a vyberte **NodeJS instalace**.
7. Vyberte **instalovat automaticky** možnost a potom zadejte **název** hodnotu.
8. Vyberte **Uložit**.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Konfigurace projektu volný volaných styl pro Node.js

1. Vyberte **novou položku**. Zadejte název položky.
2. Vyberte **volný styl projektu**. Vyberte **OK**.
3. Na **správu zdrojového kódu** vyberte **Git** a zadejte podrobnosti o úložišti a větve, které obsahují kódu aplikace.    
    ![Přidat úložišti do vaší sestavení](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. Na **sestavení aktivační události** vyberte **dotazování SCM** a zadejte plán `H/03 * * * *` k dotazování úložiště Git pro změny každé tři minuty. 
5. Na **sestavení prostředí** vyberte **poskytují uzlu &amp; npm bin / složky cesta** a vyberte **NodeJS instalace** hodnotu. Nechte **npmrc soubor** nastavena na **použít výchozí systémové nastavení**.
6. Na **sestavení** vyberte **spustit prostředí** a zadejte příkaz `npm install` zajistit, že jsou aktualizovány všechny závislosti.


## <a name="configure-jenkins-for-team-services-integration"></a>Konfigurace volaných pro integraci produktů Team Services

> [!NOTE]
> Zajistěte, aby obsahoval osobní přístupový token (Jan), použijte následující postup *verze* (čtení, zápis, spuštění a spravovat) oprávnění v Team Services.
 
1.  Pokud jste ještě nemáte, vytvořte PAT ve vašem účtu Team Services. Volaných vyžaduje zadání těchto informací pro přístup k účtu Team Services. Nezapomeňte uložit informace o tokenu pro nadcházející kroky v této části.
  
    Zjistěte, jak vygenerovat token, přečtěte si téma [vytvoření osobního přístupového tokenu služby VSTS a TFS?](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate).
2. V **akce po sestavení** vyberte **přidat akci po sestavení**. Vyberte **archivu artefakty**.
3. Pro **soubory k archivaci**, zadejte `**/*` zahrnout všechny soubory.
4. Chcete-li vytvořit další akci, vyberte **přidat akci po sestavení**.
5. Vyberte **aktivovat verze v TFS/Team Services**. Zadejte identifikátor URI pro vašeho účtu Team Services, jako například **https://{your-account-name}.visualstudio.com**.
6. Zadejte **týmového projektu** název.
7. Zvolte název pro definici verze. (Dále v Team Services vytvořit tuto definici verzi.)
8. Vyberte pověření pro připojení k prostředí Team Services nebo Team Foundation Server:
   - Nechte **uživatelské jméno** prázdný, pokud používáte Team Services. 
   - Zadejte uživatelské jméno a heslo, pokud se používá místní verzi sady Team Foundation Server.    
   ![Konfigurace akce volaných po sestavení](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. Uložte volaných projektu.


## <a name="create-a-jenkins-service-endpoint"></a>Vytvoření koncového bodu služby volaných

Koncový bod služby umožňuje Team Services pro připojení k volaných.

1. Otevřete **služby** stránky v Team Services, otevřete **nový koncový bod služby** seznam a vyberte **volaných**.
   ![Přidání koncového bodu volaných](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Zadejte název pro připojení.
3. Zadejte adresu URL serveru volaných a vyberte **přijetí nedůvěryhodných certifikátů SSL** možnost. Příklad adresy URL je **http://{YourJenkinsURL}.westcentralus.cloudapp.azure.com**.
4. Zadejte uživatelské jméno a heslo účtu volaných.
5. Vyberte **ověření připojení** zkontrolujte správnost informací.
6. Vyberte **OK** vytvořit koncový bod služby.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Vytvořte skupinu nasazení pro virtuální počítače Azure

Je nutné [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) k registraci agenta Team Services, aby definice verzi můžete nasadit na virtuální počítač. Nasazení skupin usnadnit definování logických skupin cílových počítačů pro nasazení a nainstalujte požadované agent na každém počítači.

   > [!NOTE]
   > V následujícím postupu, nezapomeňte nainstalovat požadované součásti a *nespouštět skript s oprávněními sudo.*

1. Otevřete **verze** kartě **sestavení &amp; verze** rozbočovače, otevřete **nasazení skupiny**a vyberte **+ nový**.
2. Zadejte název skupiny nasazení a volitelný popis. Potom vyberte **Vytvořit**.
3. Vyberte operační systém pro vaše nasazení cílového virtuálního počítače. Vyberte například **Ubuntu 16.04 +**.
4. Vyberte **používat osobní přístupový token ve skriptu pro ověřování**.
5. Vyberte **požadované součásti systému** odkaz. Instalace požadovaných součástí pro operační systém.
6. Vyberte **zkopírujte skript do schránky** skript zkopírovat.
7. Přihlaste se k nasazení cílový virtuální počítač a spusťte skript. Nespouštět skript s oprávněními sudo.
8. Po instalaci zobrazí se výzva pro nasazení skupiny značky. Přijměte výchozí hodnoty.
9. V Team Services, zkontrolujte pro nově zaregistrovaný virtuálního počítače v **cíle** pod **nasazení skupiny**.

## <a name="create-a-team-services-release-definition"></a>Vytvoření definice Team Services verze

Verze definice určuje proces, který Team Services používá k nasazení aplikace. V tomto příkladu je spustit skript prostředí.

K vytvoření definice verze v Team Services:

1. Otevřete **verze** kartě **sestavení &amp; verze** rozbočovače a vyberte **vytvořit verze definice**. 
2. Vyberte **prázdný** šablony výběrem začínat **prázdný proces**.
3. V **artefakty** vyberte **+ přidat artefaktů** a zvolte **volaných** pro **typ zdroje**. Vyberte připojení volaných koncový bod služby. Potom vyberte úlohu volaných zdroj a vyberte **přidat**.
4. Vyberte se třemi tečkami vedle **prostředí 1**. Vyberte **fáze skupiny přidat nasazení**.
5. Zvolte skupinu vaše nasazení.
5. Vyberte  **+**  přidat úloha **fáze nasazení skupiny**.
6. Vyberte **skript prostředí** úkolů a vyberte **přidat**. **Skript prostředí** úloha poskytuje konfigurace pro skript běžet na každém serveru, aby bylo možné nainstalovat Node.js a spusťte aplikaci.
8. Pro **cestu ke skriptu**, zadejte **$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh**.
9. Vyberte **Upřesnit**a pak povolte **zadat pracovní adresář**.
10. Pro **pracovní adresář**, zadejte **$(System.DefaultWorkingDirectory) nebo uzel Fabrikam**.
11. Upravit název definice verze na název, který jste zadali na **akce po sestavení** kartě sestavení v volaných. Volaných vyžaduje tento název moct aktivovat novou verzi, když jsou aktualizovány zdrojové artefakty.
12. Vyberte **Uložit** a vyberte **OK** se uložit definici verze.

## <a name="execute-manual-and-ci-triggered-deployments"></a>Provést ruční a aktivovaného položky konfigurace nasazení

1. Vyberte **+ verze** a vyberte **vytvořit verzi**.
2. Vyberte sestavení, které jste dokončili v zvýrazněná rozevíracího seznamu a vyberte **fronty**.
3. Vyberte verzi odkaz v automaticky otevřeném okně. Například: "verze **verze 1** byla vytvořena."
4. Otevřete **protokoly** a podívejte se na výstup konzoly verze.
5. V prohlížeči otevřete adresu URL jednoho ze serverů, které jste přidali do vaší skupiny nasazení. Zadejte například **http://{your-server-ip-address}**.
6. Přejít na zdrojové úložiště Git a upravovat obsah **h1** záhlaví v souboru app/views/index.jade změněné textem.
7. Potvrďte změny.
8. Po několika minutách, zobrazí se na vytvořit novou verzi **verze** Team Services nebo Team Foundation Server. Otevřete verzi zobrazíte probíhající nasazení. Blahopřejeme!

## <a name="next-steps"></a>Další kroky

V tomto kurzu automatické nasazení aplikace do Azure pomocí volaných pro sestavení a Team Services pro verzi. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Sestavení aplikace ve volaných.
> * Nakonfigurujte volaných pro integraci produktů Team Services.
> * Vytvořte skupinu nasazení pro virtuální počítače Azure.
> * Vytvořte definici verze, která slouží ke konfiguraci virtuálních počítačů a nasadí aplikaci.

Další informace o tom, jak nasadit svítilna (Linux, Apache, MySQL a PHP) zásobníku, přechodu na v dalším kurzu.

> [!div class="nextstepaction"]
> [Nasazení zásobníku LAMP](tutorial-lamp-stack.md)
