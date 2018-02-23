---
title: "Nasazení aplikace do Azure a Azure zásobníku | Microsoft Docs"
description: "Zjistěte, jak nasadit aplikace do Azure a Azure zásobníku se zřetězením příkazů hybridní CI/CD."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: 
ms.custom: mvc
ms.openlocfilehash: 6c073376db196b7d6c73c38d6a0a7b2c24949528
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2018
---
# <a name="deploy-apps-to-azure-and-azure-stack"></a>Nasazení aplikací do Azure a Azure zásobníku
*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Hybrid [průběžnou integraci](https://www.visualstudio.com/learn/what-is-continuous-integration/)/[nastavené průběžné doručování](https://www.visualstudio.com/learn/what-is-continuous-delivery/)kanálu (CI/CD) umožňuje vytvoření, testování a nasazení aplikace do víc cloudů.  V tomto kurzu vytvoříte ukázkové prostředí se dozvíte, jak kanálu CI/CD hybridní můžete:
 
> [!div class="checklist"]
> * Zahájení nové sestavení podle potvrzení kód do úložiště Visual Studio Team Services (VSTS).
> * Automaticky nasaďte nově vytvořený kód do Azure pro testování přijetí uživateli.
> * Po uplynutí kódu testování, automaticky nasaďte do Azure zásobníku. 


## <a name="prerequisites"></a>Požadavky
Několik součásti jsou nutné k vytvoření kanálu hybridní CI/CD a může trvat delší dobu přípravy.  Pokud již máte některé z těchto součástí, ujistěte se, že splňují požadavky před zahájením.

Toto téma také předpokládá, že máte některé informace o Azure a Azure zásobníku. Pokud chcete další informace, než budete pokračovat, ujistěte se, že jste začínat tato témata:

- [Úvod do Azure](https://docs.microsoft.com/azure/fundamentals-introduction-to-azure)
- [Klíčové koncepty Azure zásobníku](../azure-stack-key-features.md)

### <a name="azure"></a>Azure
 - Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.
 - Vytvoření [webové aplikace](../../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md)a nakonfigurovat ji pro [publikování FTP](../../app-service/app-service-deploy-ftp.md).  Poznamenejte si nové URL webové aplikace, jako je později použít.


### <a name="azure-stack"></a>Azure Stack
 - [Nasazení Azure zásobníku](../azure-stack-run-powershell-script.md).  Instalace obvykle trvá několik hodin k dokončení, takže Plánujte odpovídajícím způsobem.
 - Nasazení [služby App Service](../azure-stack-app-service-deploy.md) služby PaaS do protokolů Azure.
 - Vytvořit webovou aplikaci a nakonfigurovat ji pro [publikování FTP](../azure-stack-app-service-enable-ftp.md).  Poznamenejte si nové URL webové aplikace, jako je později použít.  

### <a name="developer-tools"></a>Vývojářské nástroje
 - Vytvoření [prostoru služby VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).  Proces registrace vytvoří projekt s názvem "MyFirstProject."  
 - [Nainstalovat Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) a [Přihlaste se do služby VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#connect-and-share-code-from-visual-studio)
 - Připojení k projektu a [klonovat místně](https://www.visualstudio.com/docs/git/gitquickstart).
 - Vytvoření [fondu agenta](https://www.visualstudio.com/docs/build/concepts/agents/pools-queues#creating-agent-pools-and-queues) v služby VSTS.
 - Instalace sady Visual Studio a nasazení [služby VSTS sestavení agenta](https://www.visualstudio.com/docs/build/actions/agents/v2-windows) k virtuálnímu počítači v Azure zásobníku. 
 

## <a name="create-app--push-to-vsts"></a>& Vytvořit aplikaci nabízená služby VSTS

### <a name="create-application"></a>Vytvořit aplikaci
V této části vytvoříte jednoduchou aplikaci ASP.NET a poslat ho přímo služby VSTS.  Tyto kroky představují normální vývojáře pracovní postup a může se přizpůsobit pro nástroje pro vývojáře a jazyky. 

1.  Otevřete sadu Visual Studio.
2.  Z prostoru Team Explorer a **řešení...**  oblast, klikněte na tlačítko **nový**.
3.  Vyberte **Visual C#** > **webové** > **webové aplikace ASP.NET (rozhraní .NET Framework)**.
4.  Zadejte název aplikace a klikněte na tlačítko **OK**.
5.  Na další obrazovce, ponechejte výchozí hodnoty (webových formulářů) a klikněte na tlačítko **OK**.

### <a name="commit-and-push-changes-to-vsts"></a>Potvrďte a odešlete změny do služby VSTS
1.  Průzkumník týmových projektů v sadě Visual Studio, vyberte rozevíracího seznamu a klikněte na **změny**.
2.  Zadejte zprávu o potvrzení a vyberte **potvrdit všechny**. Můžete být vyzváni k uložení souboru řešení, klikněte na tlačítko Ano uložte všechny.
3.  Jakmile potvrzené, Visual Studio nabízí synchronizaci změn do projektu. Vyberte **Synchronizovat**.

    ![Obrázek znázorňující potvrzení obrazovky po dokončení potvrzení](./media/azure-stack-solution-pipeline/image1.png)

4.  Na kartě Synchronizace v části *odchozí*, zobrazí vaše nové potvrzení.  Vyberte **Push** synchronizovat změnu služby VSTS.

    ![Obrázek znázorňující synchronizace kroky](./media/azure-stack-solution-pipeline/image2.png)

### <a name="review-code-in-vsts"></a>Zkontrolujte kód služby VSTS
Jednou jste potvrzeny změnu a instaluje do služby VSTS, zkontrolujte kódu z portálu služby VSTS.  Vyberte **kód**a potom **soubory** z rozevírací nabídky.  Zobrazí se řešení, které jste vytvořili.

## <a name="create-build-definition"></a>Vytvořit definici sestavení
Proces sestavení definuje vytvořené a zabalené pro nasazení na každém potvrzení změny kódu aplikace. V našem příkladu jsme šablona součástí konfigurace procesu sestavení pro aplikaci ASP.NET, i když tato konfigurace může být přizpůsobena v závislosti na vaší aplikace.

1.  Přihlaste se do pracovního prostoru služby VSTS z webového prohlížeče.
2.  Nápis, vyberte **sestavení a verze** a potom **sestavení**.
3.  Klikněte na tlačítko **+ novou definici**.
4.  V seznamu šablon vyberte **ASP.NET (Preview)** a vyberte **použít**.
5.  Změnit *argumenty MSBuild* pole *sestavit řešení* tak, aby:

    `/p:DeployOnBuild=True /p:WebPublishMethod=FileSystem /p:DeployDefaultTarget=WebPublish /p:publishUrl="$(build.artifactstagingdirectory)\\"`

6.  Vyberte **možnosti** a vyberte fronty agenta pro agenta sestavení jste nasadili do virtuálního počítače v Azure zásobníku. 
7.  Vyberte **aktivační události** kartě a povolte **průběžnou integraci**.
7.  Klikněte na tlačítko **Uložit & fronty** a pak vyberte **Uložit** z rozevíracího seznamu. 

## <a name="create-release-definition"></a>Vytvoření definice verze
Proces verze definuje, jak se nasadí sestavení z předchozího kroku do prostředí.  V tomto kurzu jsme naše aplikace ASP.NET pomocí FTP publikovat do webové aplikace Azure. Ke konfiguraci verze do Azure, použijte následující kroky:

1.  Služby VSTS banner, vyberte **sestavení a verze** a potom **verze**.
2.  Klikněte na tlačítko se zeleným **+ novou definici**.
3.  Vyberte **prázdný** a klikněte na tlačítko **Další**.
4.  Zaškrtněte políčko pro *průběžné nasazování*a potom klikněte na **vytvořit**.

Teď, když jste vytvořili definici prázdný verze a vázané na sestavení, přidáme kroky pro prostředí Azure:

1.  Klikněte na tlačítko se zeleným  **+**  přidat úlohy.
2.  Vyberte **všechny**a pak ze seznamu, přidejte **FTP nahrát** a vyberte **Zavřít**.
3.  Vyberte **FTP nahrát** úkolů jste právě přidali a nakonfigurujte následující parametry:
    
    | Parametr | Hodnota |
    | ----- | ----- |
    |Metoda ověřování| Zadejte přihlašovací údaje|
    |URL serveru | Adresa URL webové aplikace FTP načíst z portálu Azure |
    |Uživatelské jméno | Uživatelské jméno, které jste nakonfigurovali při vytváření pověření FTP pro webovou aplikaci |
    |Heslo | Heslo, které jste vytvořili při navazování FTP pověření pro webovou aplikaci|
    |Zdrojový adresář | $(System.DefaultWorkingDirectory)\**\ |
    |Vzdálený adresář | /Site/wwwroot / |
    |Zachovat cesty k souborům | Povoleno (zaškrtnuto)|

4.  Klikněte na tlačítko **uložit**

Nakonec můžete nakonfigurovat definici verze používat agenta fond, který obsahuje agent nasazen pomocí následujících kroků:
1.  Vyberte definici verzi a klikněte na **upravit**.
2.  Vyberte **spustit na agentovi** ze střední sloupce.  V pravém sloupci vyberte frontu agenta obsahující sestavení agenta spuštěného v zásobníku Azure.  
    ![Obrázek znázorňující konfigurace verze definice používat konkrétní fronty](./media/azure-stack-solution-pipeline/image3.png)


## <a name="deploy-your-app-to-azure"></a>Nasazení aplikace do Azure
Tento krok používá vaše nově vytvořený kanál CI/CD nasazení aplikace ASP.NET do webové aplikace v Azure. 

1.  Informační zpráva v služby VSTS, vyberte **sestavení a verze**a potom vyberte **sestavení**.
2.  Klikněte na tlačítko **...**  v definici sestavení vytvořené a vyberte možnost **fronty nového sestavení**.
3.  Přijměte výchozí hodnoty a klikněte na tlačítko **Ok**.  Sestavení začne a zobrazí se průběh.
4.  Po dokončení sestavení může sledovat stav výběrem **sestavení a verze** a výběrem **verze**.
5.  Po dokončení sestavení navštivte webovou stránku pomocí adresy URL uvedené při vytváření webové aplikace.    


## <a name="add-azure-stack-to-pipeline"></a>Přidat zásobník Azure do kanálu
Teď, když vaše CI/CD kanálu otestujete nasazením do Azure, je čas přidání zásobník Azure do kanálu.  V následujících krocích vytvořte nové prostředí a přidejte nahrání FTP úloh k nasazení aplikace do Azure zásobníku.  Můžete také přidat schvalovatel verze, která slouží jako způsob, jak simulovat "podepisování" na verze kódu do protokolů Azure.  

1.  V definici verze vyberte **+ přidat prostředí** a **vytvořit nové prostředí**.
2.  Vyberte **prázdný**, klikněte na tlačítko **Další**.
3.  Vyberte **konkrétním uživatelům** a zadejte svůj účet.  Vyberte **Vytvořit**.
4.  Přejmenovat prostředí vyberete existující název a zadáte *zásobník Azure*.
5.  Nyní, pak vyberte výběr prostředím Azure zásobníku **přidání úkolů**.
6.  Vyberte **FTP nahrát** úkolů a vyberte **přidat**, pak vyberte **Zavřít**.


### <a name="configure-ftp-task"></a>Konfigurace úloh FTP
Teď, když jste vytvořili verze, nakonfigurujete kroky potřebné k publikování do webové aplikace v Azure zásobníku.  Stejně jako jste nakonfigurovali úlohu nahrát FTP pro Azure, nakonfigurujete úlohu pro Azure zásobníku:

1.  Vyberte **FTP nahrát** úkolů jste právě přidali a nakonfigurujte následující parametry:
    
    | Parametr | Hodnota |
    | -----     | ----- |
    |Metoda ověřování| Zadejte přihlašovací údaje|
    |URL serveru | Adresa URL webové aplikace FTP načíst z portálu Azure zásobníku |
    |Uživatelské jméno | Uživatelské jméno, které jste nakonfigurovali při vytváření pověření FTP pro webovou aplikaci |
    |Heslo | Heslo, které jste vytvořili při navazování FTP pověření pro webovou aplikaci|
    |Zdrojový adresář | $(System.DefaultWorkingDirectory)\**\ |
    |Vzdálený adresář | /Site/wwwroot /|
    |Zachovat cesty k souborům | Povoleno (zaškrtnuto)|

2.  Klikněte na tlačítko **uložit**

Nakonec nakonfigurujte definici verze používat agenta fond, který obsahuje agent nasazen pomocí následujících kroků:
1.  Vyberte definici verzi a klikněte na **upravit**
2.  Vyberte **spustit na agentovi** ze střední sloupce. V pravém sloupci vyberte frontu agenta obsahující sestavení agenta spuštěného v zásobníku Azure.  
    ![Obrázek znázorňující konfigurace verze definice používat konkrétní fronty](./media/azure-stack-solution-pipeline/image3.png)

## <a name="deploy-new-code"></a>Nasazování nového kódu
Nyní můžete otestovat kanálu CI/CD hybridní, se v posledním kroku publikování do protokolů Azure.  V této části zápatí lokality změnit a spusťte nasazení prostřednictvím kanálu.  Po dokončení uvidíte změny nasadit do Azure ke kontrole a pak po správné verze, jsou publikovány do protokolů Azure.

1. V sadě Visual Studio, otevřete *site.master* soubor a změňte tento řádek:
    
    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application</p>
    `

    měli změnit na:

    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application delivered by VSTS, Azure, and Azure Stack</p>
    `
3.  Provedení změn a synchronizaci služby VSTS.  
4.  V pracovním prostoru služby VSTS zkontrolovat stav sestavení výběrem **sestavení a verze** > **sestavení**
5.  Zobrazí se v průběhu sestavení.  Dvakrát klikněte na stav, a můžete sledovat průběh sestavení.  Jakmile se zobrazí "Dokončení sestavení" v konzole, přejít k zkontrolujte verzi z **sestavení a verze** > **verze**.  Dvakrát klikněte na vydání.
6.  Vám bude zasláno oznámení, že verze vyžaduje kontrolu. Zkontrolujte adresu URL webové aplikace a ověřte, zda že jsou k dispozici nové změny.  Verze v služby VSTS schvalte.
    ![Obrázek znázorňující konfigurace verze definice používat konkrétní fronty](./media/azure-stack-solution-pipeline/image4.png)
    
7.  Ověření publikování do protokolů Azure je dokončená, přepínáním navštěvující web pomocí adresy URL uvedené při vytváření webové aplikace.
    ![Obrázek znázorňující ASP. NEt aplikace pomocí zápatí změnit](./media/azure-stack-solution-pipeline/image5.png)


Nyní můžete svůj nový kanál CI/CD hybridní jako stavební blok pro jiné vzorů hybridního cloudu.

## <a name="next-steps"></a>Další postup
V tomto kurzu jste zjistili, jak vytvořit hybridní CI/CD kanálu, který:

> [!div class="checklist"]
> * Zahájí nové sestavení na základě kódu potvrdí se svým úložištěm Visual Studio Team Services (VSTS).
> * Automaticky nasadí nově vytvořený kód do Azure pro testování přijetí uživateli.
> * Po uplynutí kódu testování, se automaticky nasadí do zásobníku Azure. 

Teď, když máte hybridní CI/CD kanál, pokračujte naučit vyvíjet aplikace pro Azure zásobníku.

> [!div class="nextstepaction"]
> [Vývoj pro Azure Stack](azure-stack-developer.md)


