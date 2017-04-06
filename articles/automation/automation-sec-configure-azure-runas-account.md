---
title: "Konfigurace účtu Spustit jako pro Azure | Dokumentace Microsoftu"
description: "Tento kurz vás provede vytvořením, otestováním a ukázkovým použitím ověření objektu zabezpečení ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
keywords: "název objektu služby, název instančního objektu, setspn, ověřování azure"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/06/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: fbca3d195290551d37606e231b997a40a602351f
ms.lasthandoff: 03/28/2017


---

# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a>Ověření runbooků pomocí účtu Spustit jako pro Azure
V tomto tématu se seznámíte se způsobem konfigurace účtu Azure Automation na webu Azure Portal. Pomocí funkce účtu Spustit jako za účelem ověříte prostředky, které spravují runbooky, a to buď v Azure Resource Manageru, nebo ve službě Azure Service Management.

Když na webu Azure Portal vytvoříte účet Automation, automaticky se vytvoří dva účty:

* Účet Spustit jako. Tento účet vytvoří instanční objekt ve službě Azure Active Directory (Azure AD) a certifikát. Přiřadí také řízení přístupu na základě role Přispěvatel (RBAC), které spravuje prostředky Resource Manageru pomocí runbooků.
* Účet Spustit jako pro Azure Classic. Tento účet nahraje certifikát pro správu, který se používá ke správě prostředků správy služeb nebo klasických prostředků pomocí runbooků.

Vytvoření účtu Automation zjednodušuje tento proces a pomůže vám rychle začít vytvářet a nasazovat runbooky na podporu vašich automatizačních potřeb.

Pomocí účtu Spustit jako a účtu Spustit jako pro Azure Classic můžete:

* Zajistit standardizovaný způsob ověřování pomocí Azure při správě prostředků Azure Resource Manageru nebo prostředků správy služeb pomocí runbooků na webu Azure Portal.
* Automatizovat používání globálních runbooků, které se dají nakonfigurovat ve výstrahách Azure.

> [!NOTE]
> [Funkce integrace výstrah Azure](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) pomocí globálních runbooků Automation vyžaduje účet Automation, který je nakonfigurovaný s využitím účtů Spustit jako a Spustit jako pro Azure Classic. Můžete vybrat účet Automation, který už má definované účty Spustit jako a Spustit jako pro Azure Classic, nebo můžete vytvořit nový účet Automation.
>  

V tomto článku si ukážeme, jak na webu Azure Portal vytvořit účet Automation, jak tento účet aktualizovat pomocí Azure PowerShellu, jak spravovat jeho konfiguraci účtu a jak ověřovat v runboocích.

Ještě než začnete vytvářet účet Automation, je vhodné seznámit se s následujícími aspekty a vzít je v úvahu:

* Vytvoření účtu Automation neovlivní existující účty Automation, které už jste případně vytvořili v modelu nasazení Classic nebo v modelu nasazení Resource Manageru.
* Proces funguje jenom pro účty Automation, které vytvoříte na webu Azure Portal. Pokus o vytvoření účtu na portálu Azure Classic nezreplikuje konfiguraci účtu Spustit jako.
* Pokud už máte runbooky a prostředky (třeba plány nebo proměnné) pro správu klasických prostředků a chcete, aby runbooky k ověřování používaly nový účet Spustit jako pro Classic, proveďte jednu z těchto akcí:

  * Pokud chcete vytvořit účet Spustit jako pro Classic, postupujte podle pokynů v části Správa účtu Spustit jako. 
  * Pokud chcete aktualizovat existující účet, použijte skript PowerShellu v části Aktualizace účtu Automation pomocí PowerShellu.
* Abyste mohli ověřovat pomocí nového účtu Automation Spustit jako a účtu Spustit jako pro Classic, musíte stávající runbooky upravit pomocí ukázkového kódu uvedeného v části [Příklady kódu pro ověřování](#authentication-code-examples).

    >[!NOTE]
    >Účet Spustit jako slouží k ověřování pomocí prostředků Resource Manageru s využitím instančního objektu založeného na certifikátech. Účet Spustit jako pro Classic slouží k ověřování pomocí prostředků správy služeb s využitím certifikátu pro správu.

## <a name="create-an-automation-account-from-the-azure-portal"></a>Vytvoření účtu Automation na webu Azure Portal
V této části vytvoříte na webu Azure Portal účet služby Azure Automation, který potom vytvoří jak účet Spustit jako, tak účet Spustit jako pro Classic.

>[!NOTE]
>Abyste mohli vytvořit účet Automation, musíte být členem role Správci služeb nebo spolusprávcem předplatného, který k tomuto předplatnému uděluje přístup. K výchozí instanci Active Directory tohoto předplatného musíte být přihlášení jako uživatel. Účtu nemusí mít přiřazenou privilegovanou roli.
>
>Pokud před přidáním k roli spolusprávce nejste členem instance Active Directory příslušného předplatného, budete do služby Active Directory přidaní jako host. V takovém případě se na kartě **Přidat účet Automation** zobrazí upozornění typu Nemáte oprávnění k vytvoření...
>
>Uživatele, kteří byli nejdřív přidaní do role spolusprávce, je možné z instance Active Directory předplatného odebrat a potom je znovu přidat – tak se z nich ve službě Active Directory stanou úplní uživatelé. Takovou situaci můžete ověřit v podokně **Azure Active Directory** na webu Azure Portal. Vyberte **Uživatelé a skupiny**, potom **Všichni uživatelé** a po výběru konkrétního uživatele vyberte **Profil**. Hodnota atributu **Typ uživatele** v profilu uživatele by neměla být **Host**.
>

1. Přihlaste se k webu Azure Portal pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.

2. Vyberte **Účty Automation**.

3. V okně **Účty Automation** klikněte na **Přidat**.
Otevře se okno **Přidat účet Automation**.

 ![Okno Přidat účet Automation](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > Pokud váš účet není členem role správců předplatného a spolusprávcem předplatného, v okně **Přidat účet Automation** se zobrazí následující upozornění:
   >
   >![Přidání upozornění k účtu Automation](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. V okně **Přidat účet Automation** zadejte název nového účtu Automation do pole **Název**.

5. Pokud máte víc předplatných, postupujte takto:

    a. V části **Předplatné** zadejte předplatné pro nový účet.

    b. V části **Skupina prostředků** klikněte na **Vytvořit novou** nebo **Použít stávající**.

    c. V části **Umístění** zadejte datové centrum Azure.

6. V části **Vytvořit účet Spustit v Azure jako** vyberte **Ano** a potom klikněte na **Vytvořit**.

   > [!NOTE]
   > Pokud se rozhodnete nevytvářet účet Spustit jako a vyberete **Ne**, zobrazí se v okně **Přidat účet Automation** zpráva s upozorněním. Přestože je účet vytvořený na webu Azure Portal, nemá odpovídající identitu ověřování v rámci adresářové služby klasického předplatného nebo předplatného Resource Manageru. Následkem toho účet nemá přístup k prostředkům ve vašem předplatném. Všechny runbooky odkazující na tento účet kvůli tomu nebudou moct ověřit a provádět úlohy s prostředky v těchto modelech nasazení.
   >
   > ![Zpráva s upozorněním v okně Přidání účtu Automation](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > Navíc protože není vytvořený instanční objekt, není přiřazená role přispěvatele.
   >

7. Zatímco Azure vytváří účet Automation, můžete průběh sledovat v nabídce v části **Oznámení**.

### <a name="resources"></a>Zdroje a prostředky
Po úspěšném vytvoření účtu Automation se pro vaší potřebu automaticky vytvoří několik prostředků. Prostředky jsou shrnuté v následujících dvou tabulkách:

#### <a name="run-as-account-resources"></a>Prostředky účtu Spustit jako

| Prostředek | Popis |
| --- | --- |
| Runbook AzureAutomationTutorial | Ukázkový grafický runbook, který předvádí ověření pomocí účtu Spustit jako a získává všechny prostředku Resource Manageru. |
| Runbook AzureAutomationTutorialScript | Ukázkový runbook PowerShellu, který předvádí ověření pomocí účtu Spustit jako a získává všechny prostředku Resource Manageru. |
| AzureRunAsCertificate | Prostředek certifikátu vytvořený automaticky během vytváření účtu Automation. Pro stávající účet použijte následující skript PowerShellu. Certifikát umožňuje ověření pomocí Azure, abyste mohli spravovat prostředky Azure Resource Manageru pomocí runbooků. Tento certifikát má životnost jeden rok. |
| AzureRunAsConnection | Prostředek připojení vytvořený automaticky během vytváření účtu Automation. Pro stávající účet použijte skript PowerShellu. |

#### <a name="classic-run-as-account-resources"></a>Prostředky účtu Spustit jako pro Classic

| Prostředek | Popis |
| --- | --- |
| Runbook AzureClassicAutomationTutorial | Ukázkový grafický runbook, který získá všechny virtuální počítače vytvořené v rámci předplatného pomocí modelu nasazení Classic s využitím účtu Spustit jako pro Classic (certifikát) a potom zapíše název a stav virtuálního počítače. |
| Runbook se skriptem AzureClassicAutomationTutorial | Ukázkový runbook PowerShellu, který získá všechny klasické virtuální počítače v rámci předplatného pomocí účtu Spustit jako pro Azure Classic (certifikát) a potom vypíše název a stav virtuálního počítače. |
| AzureClassicRunAsCertificate | Automaticky vytvořený prostředek certifikátu, který použijete k ověřování pomocí Azure, abyste mohli spravovat klasické prostředky Azure pomocí runbooků. Tento certifikát má životnost jeden rok. |
| AzureClassicRunAsConnection | Automaticky vytvořený prostředek připojení, který použijete k ověřování pomocí Azure, abyste mohli spravovat klasické prostředky Azure pomocí runbooků. |

## <a name="verify-run-as-authentication"></a>Kontrola ověřování pomocí účtu Spustit jako
Proveďte malý test, abyste potvrdili, že pomocí nového účtu Spustit jako můžete provádět ověření.

1. Na webu Azure Portal otevřete účet Automation, který jste vytvořili dřív.

2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.

3. Vyberte runbook **AzureAutomationTutorialScript** a kliknutím na **Spustit** spusťte tento runbook. Dojde k následující chybě:
 * Vytvoří se [úloha runbooku](automation-runbook-execution.md), zobrazí se okno **Úloha** a na dlaždici **Souhrn úlohy** se zobrazí stav úlohy.
 * Počáteční stav úlohy bude **e frontě**. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu.
 * Když pracovní proces úlohu přijme, změní se stav na **Spouštění**.
 * Když se runbook skutečně spustí, změní se stav na **Spuštěno**.
 * Po dokončení úlohy runbooku by se měl zobrazit stav **Dokončeno**.

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. Pokud chcete zobrazit podrobné výsledky runbooku, klikněte na dlaždici **Výstup**.  
Zobrazí se okno **Výstup**, ve kterém se ukazuje, že se runbook úspěšně ověřil a vrátil seznam všech prostředků, které jsou ve skupině prostředků dostupné.

5. Zavřením okna **Výstup** se vrátíte do okna **Souhrn úlohy**.

6. Zavřete okno **Souhrn úlohy** a odpovídající okno runbooku **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Kontrola ověřování pomocí účtu Spustit jako pro Azure Classic
Proveďte podobný malý test, abyste potvrdili, že pomocí nového účtu Spustit jako pro Classic můžete provádět ověření.

1. Na webu Azure Portal otevřete účet Automation, který jste vytvořili dřív.

2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.

3. Vyberte runbook **AzureClassicAutomationTutorialScript** a kliknutím na tlačítko **Spustit** tento runbook spusťte. Dojde k následující chybě:

 * Vytvoří se [úloha runbooku](automation-runbook-execution.md), zobrazí se okno **Úloha** a na dlaždici **Souhrn úlohy** se zobrazí stav úlohy.
 * Počáteční stav úlohy bude **e frontě**. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu.
 * Když pracovní proces úlohu přijme, změní se stav na **Spouštění**.
 * Když se runbook skutečně spustí, změní se stav na **Spuštěno**.
 * Po dokončení úlohy runbooku by se měl zobrazit stav **Dokončeno**.

    ![Test runbooku objektu zabezpečení](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. Pokud chcete zobrazit podrobné výsledky runbooku, klikněte na dlaždici **Výstup**.  
Zobrazí se okno **Výstup**, ve kterém se ukazuje, že se runbook úspěšně ověřil a vrátil seznam všech klasických virtuálních prostředků v předplatném.

5. Zavřením okna **Výstup** se vrátíte do okna **Souhrn úlohy**.

6. Zavřete okno **Souhrn úlohy** a odpovídající okno runbooku **AzureAutomationTutorialScript**.

## <a name="managing-your-run-as-account"></a>Správa účtu Spustit jako
V určitém okamžiku před vypršením platnosti účtu Automation musíte obnovit certifikát. Pokud se domníváte, že zabezpečení účtu Spustit jako je ohrožené, můžete ho odstranit a vytvořit znovu. Tato část popisuje, jak tyto operace provést.

### <a name="self-signed-certificate-renewal"></a>Obnovení certifikátu podepsaného svým držitelem
Platnost certifikátu podepsaného svým držitelem, který jste vytvořili pro účet Spustit jako, vyprší jeden rok od data jeho vytvoření. Před vypršením platnosti ho můžete kdykoli obnovit. Když ho obnovíte, aktuální platný certifikát se uchová, aby se zajistilo, že to negativně neovlivní runbooky ověřované účtem Spustit jako, které jsou právě ve frontě nebo aktivně spuštěné. Certifikát zůstane platný až do data vypršení jeho platnosti.

> [!NOTE]
> Pokud jste svůj účet Automation Spustit jako nakonfigurovali k používání certifikátu vydaného podnikovou certifikační autoritou a použijete tuto možnost, tento podnikový certifikát se nahradí certifikátem podepsaným svým držitelem.

Pokud chcete certifikát obnovit, postupujte takto:

1. Na webu Azure Portal otevřete účet Automation.

2. V okně **Účet Automation** v podokně **Vlastnosti účtu** v části **Nastavení účtů** vyberte **Účty Spustit jako**.

    ![Podokno vlastností účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. V okně vlastností **Účty Spustit jako** vyberte účet Spustit jako nebo účet Spustit jako pro Classic, pro který chcete obnovit certifikát.

4. V okně **Vlastnosti** vybraného účtu klikněte na **Obnovit certifikát**.

    ![Obnovení certifikátu pro účet Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. Zatímco se certifikát obnovuje, můžete průběh sledovat v nabídce v části **Oznámení**.

### <a name="delete-a-run-as-or-classic-run-as-account"></a>Odstranění účet Spustit jako nebo Spustit jako pro Classic
Tato část popisuje, jak odstranit a znovu vytvořit účet Spustit jako nebo účet Spustit jako pro Classic. Při provedení této akce se účet Automation uchová. Odstraněný účet Spustit jako nebo účet Spustit jako pro Classic můžete znovu vytvořit na webu Azure Portal.

1. Na webu Azure Portal otevřete účet Automation.

2. V okně **Účet Automation** v podokně vlastností účtu vyberte **Účty Spustit jako**.

3. V okně vlastností **Účty Spustit jako** vyberte účet Spustit jako nebo účet Spustit jako pro Classic, který chcete odstranit. Potom v okně **Vlastnosti** vybraného účtu klikněte na **Odstranit**.

 ![Odstranění účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. Zatímco se účet odstraňuje, můžete průběh sledovat v nabídce v části **Oznámení**.

5. Účet po odstranění můžete znovu vytvořit v okně vlastností **Účty Spustit jako** výběrem možnosti Vytvořit v části **Účet Spustit jako**.

 ![Znovuvytvoření účtu Automation Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a>Chybná konfigurace
Může se stát, že se během prvotního nastavení nesprávně vytvoří nebo později odstraní některá z položek konfigurace nezbytných pro správné fungování účtu Spustit jako nebo Spustit jako pro Classic. Mezi tyto položky patří:

* Asset certifikátu
* Asset připojení
* Účet Spustit jako byl odebrán z role přispěvatele
* Instanční objekt nebo aplikace v Azure AD

V předchozí a dalších instancích chybné konfigurace účet Automation zjistí změny a v okně vlastností *Účty Spustit jako* příslušného účtu zobrazí stav **Nedokončeno**.

![Nedokončená konfigurace účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

Pokud vyberete tento účet Spustit jako, v podokně **Vlastnosti** účtu se zobrazí následující chybová zpráva:

![Zpráva upozornění o nedokončené konfiguraci účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).

Tyto potíže s účtem Spustit jako můžete rychle vyřešit jeho odstraněním a znovuvytvořením.

## <a name="update-your-automation-account-by-using-powershell"></a>Aktualizace účtu Automation pomocí PowerShellu
Účet Automation můžete aktualizovat pomocí PowerShellu, pokud jste postupovali takto:

* Vytvořili jste účet Automation, ale odmítli jste vytvoření účtu Spustit jako.
* Už máte účet Automation pro správu prostředků Resource Manageru a chcete ho aktualizovat, aby zahrnoval účet Spustit jako pro ověřování runbooků.
* Už máte účet Automation pro správu klasických prostředků a chcete ho aktualizovat, abyste mohli použít účet Spustit jako pro Classic a nemuseli vytvářet nový účet a migrovat na něj runbooky a prostředky.   
* Rozhodli jste se vytvořit účet Spustit jako a účet Spustit jako pro Classic pomocí certifikátu, který vydala vaše podniková certifikační autorita.

Tento skript má následující požadavky:

* Tento skript je možné spustit jenom v systémech Windows 10 a Windows Server 2016 s nainstalovanými moduly Azure Resource Manageru verze 2.01 nebo novější. V předchozích verzích Windows není podporován.
* Azure PowerShell 1.0 nebo novější. Informace o vydání PowerShellu 1.0 najdete v článku [Postup instalace a konfigurace Azure PowerShellu](/powershell/azureps-cmdlets-docs).
* Účet Automation, na který se odkazuje jako na hodnotu parametrů *-AutomationAccountName* a *-ApplicationDisplayName* v následujícím skriptu PowerShellu.

Abyste získali hodnoty pro parametry *SubscriptionID*, *ResourceGroup* a *AutomationAccountName*, které jsou pro skripty povinné, postupujte takto:
1. Na webu Azure Portal v okně **Účet Automation** vyberte příslušný účet Automation a potom vyberte **Všechna nastavení**. 
2. V okně **Všechna nastavení** v části **Nastavení účtu** vyberte **Vlastnosti**. 
3. Hodnoty v okně **Vlastnosti** si poznamenejte.

 ![Podokno vlastností účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a>Vytvoření powershellového skriptu pro účet Spustit jako
Tento skript PowerShellu zahrnuje podporu následujících konfigurací:

* Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem
* Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem
* Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu
* Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government

V závislosti na možnosti konfigurace, kterou vyberete, skript vytvoří následující položky.

**Pro účty Spustit jako:**

* Vytvoří aplikaci Azure AD, která se exportuje s veřejným klíčem certifikátu podepsaného svým držitelem nebo podnikového certifikátu, vytvoří účet instančního objektu pro tuto aplikaci v Azure AD a přiřadí roli přispěvatele pro tento účet v aktuálním předplatném. Toto nastavení můžete změnit na roli Vlastník nebo libovolnou jinou roli. Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).
* V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureRunAsCertificate*. Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá aplikace Azure AD.
* V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureRunAsConnection*. Prostředek připojení obsahuje parametry applicationId, tenantId, subscriptionId a certificate thumbprint.

**Pro účet Spustit jako pro Classic:**

* V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureClassicRunAsCertificate*. Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá certifikát pro správu.
* V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureClassicRunAsConnection*. Prostředek propojení obsahuje název a ID předplatného a název prostředku certifikátu.

>[!NOTE]
> Pokud při vytváření účtu Spustit jako pro Classic vyberete libovolnou z těchto možností, po spuštění skriptu nahrajte veřejný certifikát (soubor s příponou .cer) do úložiště správy předplatného, ve kterém byl účet Automation vytvořený.
> 

Pokud chcete spustit skript a nahrát certifikát, postupujte takto:

1. Uložte následující skript do počítače. V tomto příkladu ho uložte s názvem *New-RunAsAccount.ps1*.

        #Requires -RunAsAdministrator
         Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }

        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create a Run As account by using a service principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"

        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
          $CertificateName = $AutomationAccountName+$CertifcateAssetName
          $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
          $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
          $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}


        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#"

             if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
             $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
             $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
             $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
             $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
             $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
             $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
             $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
             $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Na svém počítači klikněte na **Start** a potom spusťte **Windows PowerShell** se zvýšenými uživatelskými oprávněními.

3. V prostředí příkazového řádku PowerShellu se zvýšenými oprávněními přejděte do složky, která obsahuje skript vytvořený v kroku 1.

4. Spusťte tento skript s využitím hodnot parametrů pro požadovanou konfiguraci.

    **Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Po spuštění skriptu se zobrazí výzva k ověření pomocí Azure. Přihlaste se pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.
    >
    >

Po úspěšném spuštění skriptu je třeba počítat s následujícím:
* Pokud jste vytvořili účet Spustit jako pro Classic s využitím veřejného certifikátu podepsaného svým držitelem (soubor .cer), skript ho vytvoří a uloží ve složce dočasných souborů ve vašem počítači pod profilem uživatele *%USERPROFILE%\AppData\Local\Temp*, který používáte ke spuštění relace PowerShellu.
* Pokud jste vytvořili účet Spustit jako pro Classic s využitím podnikového veřejného certifikátu (soubor .cer), použijte tento certifikát. Postupujte podle kroků pro [odeslání certifikátu rozhraní API pro správu na portál Azure Classic](../azure-api-management-certs.md) a potom použijte [ukázkový kód pro ověření pomocí prostředků správy služeb](#sample-code-to-authenticate-with-service-management-resources) k ověření konfigurace přihlašovacích údajů pomocí prostředků správy služeb. 
* Pokud jste *nevytvořili* účet Spustit jako pro Classic, použijte k ověření pomocí prostředků Resource Manageru a ke kontrole konfigurace přihlašovacích údajů [ukázkový kód pro ověření s využitím prostředků správy služeb](#sample-code-to-authenticate-with-resource-manager-resources).

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Ukázkový kód pro ověření pomocí prostředků Resource Manageru
Můžete použít následující aktualizovaný ukázkový kód z ukázkového runbooku *AzureAutomationTutorialScript* a provést ověření pomocí účtu Spustit jako, abyste mohli prostředky Resource Manageru spravovat pomocí svých runbooků.

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }

Skript obsahuje dva další řádky kódu, které podporují odkazování na kontext předplatného, abyste mohli lépe pracovat v prostředí s několika předplatnými. Prostředek proměnné s názvem *SubscriptionId* obsahuje ID předplatného. Po příkazu rutiny `Add-AzureRmAccount` bude uvedená rutina [`Set-AzureRmContext`](https://msdn.microsoft.com/library/mt619263.aspx) s nastaveným parametrem *-SubscriptionId*. Pokud je název proměnné příliš obecný, můžete ho upravit tak, že přidáte předponu nebo použijete jinou zásadu vytváření názvů, abyste tuto proměnnou dokázali lépe identifikovat. Alternativně můžete místo *-SubscriptionId* použít sadu parametrů *-SubscriptionName* s odpovídajícím prostředkem proměnné.

Rutina, kterou používáte pro ověřování v runbooku, `Add-AzureRmAccount`, používá sadu parametrů *ServicePrincipalCertificate*. Ověřování provádí pomocí certifikátu instančního objektu, a ne pomocí přihlašovacích údajů uživatele.

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Ukázkový kód pro ověření pomocí prostředků správy služby
Můžete použít následující aktualizovaný ukázkový kód z ukázkového runbooku *AzureClassicAutomationTutorialScript* a provést ověření pomocí účtu Spustit jako pro Classic, abyste mohli klasické prostředky spravovat pomocí svých runbooků.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a>Další kroky
* [Aplikační a instanční objekty v Azure AD](../active-directory/active-directory-application-objects.md)
* [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md)
* [Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md)

