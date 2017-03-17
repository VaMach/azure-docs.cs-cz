---
title: "Konfigurace účtu Spustit v Azure jako | Dokumentace Microsoftu"
description: "Kurz vás provede vytvořením, otestováním a ukázkovým použitím ověření objektu zabezpečení ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "název objektu služby, název instančního objektu, setspn, ověřování azure"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 7230fb1a8d27708c40040950e3ec8950c6c04780
ms.lasthandoff: 03/07/2017


---
# <a name="authenticate-runbooks-with-azure-run-as-account"></a>Ověření runbooků pomocí účtu Spustit v Azure jako
V tomto tématu se seznámíte se způsobem konfigurace účtu Automation na webu Azure Portal pomocí funkce účtu Spustit jako za účelem ověření prostředků, které spravují runbooky, buď v Azure Resource Manageru nebo ve službě Azure Service Management.

Když na portálu Azure vytvoříte nový účet Automation, účet automaticky vytvoří následující:

* Účet Spustit jako, který vytvoří nový objekt služby v Azure Active Directory (certifikát) a přiřadí přispěvateli řízení přístupu na základě rolí (RBAC), které se bude používat ke správě prostředků Resource Managera pomocí runbooků.   
* Účet Spustit jako pro Azure Classic tím, že odešle certifikát správy, který bude použit ke správě služby Azure Service Management nebo klasických prostředků pomocí runbooků.  

Tato funkce zjednodušuje proces a pomůže vám rychle začít vytvářet a nasazovat runbooky na podporu vašich automatizačních potřeb.      

Při používání účtu Spustit jako a účtu Spustit jako pro Azure Classic můžete následující:

* Zajistit standardizovaný způsob ověření pomocí Azure při správě prostředků Azure Resource Manageru nebo prostředků služby Azure Service Management pomocí runbooků na portálu Azure.  
* Automatizovat používání globálních runbooků nakonfigurovaných ve výstrahách Azure.

> [!NOTE]
> [Funkce integrace výstrah](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) Azure pomocí globálních runbooků Automation vyžaduje účet Automation, který je nakonfigurovaný pomocí účtů Spustit jako a Spustit jako pro Azure Classic. Můžete vybrat účet Automation, který už má definovaného uživatele účtů Spustit jako a Spustit jako pro Azure Classic, nebo můžete vytvořit nový.
>  

Ukážeme vám, jak na webu Azure Portal vytvořit účet Automation, jak aktualizovat účet Automation pomocí PowerShellu, jak spravovat konfiguraci účtu a předvedeme vám ověření v runboocích.

Než se do toho pustíme, je tu několik věcí, které potřebujete pochopit a vzít do úvahy.

1. Toto neovlivní existující účty Automation, které už byly vytvořeny v klasickém modelu nasazení nebo v modelu nasazení Resource manageru.  
2. Bude to fungovat jenom na účty Automation, které vytvoříte na portálu Azure.  Pokus o vytvoření účtu na portálu Classic nebude úspěšný při replikaci konfigurace účtu Spustit jako.
3. Pokud v současnosti máte dříve vytvořené runbooky a assety (tj. plány, proměnné atd.) pro správu klasických prostředků a chcete, aby tyto runbooky ověřily nový účet Spustit jako pro Azure Classic, musíte vytvořit účet Spustit jako pro Azure Classic pomocí Správy účtu Spustit jako nebo stávající účet aktualizovat pomocí níže uvedeného skriptu PowerShellu.  
4. Abyste mohli ověřit nový účet Spustit jako a účet Spustit jako pro Azure Classic na účtu Automation, upravte svoje existující runbooky pomocí níže uvedeného ukázkového kódu.  **Všimněte si**, že účet Spustit jako slouží k ověřování v prostředcích Resource manageru pomocí certifikovaného objektu služby a účet Spustit jako pro Azure Classic slouží k ověřování v prostředcích služby správy pomocí certifikátu pro správu.     

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Vytvoření nového účtu Automation na webu Azure Portal
V této části provedete následující kroky a vytvoříte nový účet Azure Automation na portálu Azure.  Tím vytvoříte účet Spustit jako i účet Spustit jako pro Azure Classic.  

> [!NOTE]
> Uživatel provádějící tyto kroky musí být členem role Správci služeb nebo spolusprávcem předplatného, což danému uživateli uděluje přístup k předplatnému. Také je nutné přidat uživatele do výchozí služby Active Directory takového předplatného jako Uživatele. Účtu nemusíte přiřazovat privilegovanou roli. Uživatelé, kteří před přidáním do role Spolusprávce předplatného nejsou členy služby Active Directory tohoto předplatného, budou přidáni do služby Active Directory jako Hosté a zobrazí se jim upozornění „Nemáte oprávnění k vytvoření...“ v okně **Přidání účtu Automation**. Uživatele, kteří byli nejprve přidáni do role spolusprávce, je možné odebrat ze služby Active Directory předplatného a znovu je přidat – tak se z nich stanou úplní Uživatelé ve službě Active Directory. Takovou situaci můžete ověřit v podokně **Azure Active Directory** na webu Azure Portal. Vyberte **Uživatelé a skupiny**, potom **Všichni uživatelé** a po výběru konkrétního uživatele vyberte **Profil**.  Hodnota atributu **Typ uživatele** v profilu uživatele by neměla být **Host**.  
> 

1. Přihlaste se k webu Azure Portal pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.
2. Vyberte **Účty Automation**.
3. V okně Účty Automation klikněte na **Přidat**.<br>![Přidání účtu Automation](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)
   
   > [!NOTE]
   > Pokud se v okně **Přidat účet Automation** zobrazí následující upozornění, důvodem je to, že váš účet není členem role správců předplatného a není spolusprávcem předplatného.<br>![Přidání upozornění do účtu Automation](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   > 
   > 
4. V okně **Přidat účet Automation** do pole **Název** zadejte název nového účtu Automation.
5. Pokud máte více než jedno předplatné, zadejte předplatné pro nový účet a také novou nebo existující **skupinu prostředků** a **umístění** datového centra Azure.
6. U možnosti **Vytvořit účet Spustit v Azure jako** zkontrolujte, jestli je vybraná položka **Ano**, a klikněte na tlačítko **Vytvořit**.  
   
   > [!NOTE]
   > Pokud se rozhodnete nevytvořit účet Spustit v Azure jako tím, že nevyberete možnost **Ne**, zobrazí se v okně **Přidání účtu Automation** zpráva upozornění.  Při vytváření účtu na portálu Azure nebude mít účet odpovídající identitu ověřování v rámci adresářové služby klasického předplatného nebo předplatného Resource Manageru a předplatné proto nebude mít přístup k prostředkům.  Všechny runbooky odkazující na tento účet kvůli tomu nebudou moct ověřit a provádět úlohy s prostředky v těchto modelech nasazení.
   > 
   > ![Přidání upozornění do účtu Automation](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)<br>
   > Pokud nevytvoříte objekt služby, nebude přiřazená role přispěvatele.
   > 

7. Zatímco Azure vytváří účet Automation, můžete průběh sledovat v nabídce v části **Oznámení**.

### <a name="resources-included"></a>Zahrnuté prostředky
Po úspěšném vytvoření účtu Automation se pro vaší potřebu automaticky vytvoří několik prostředků.  Následující tabulka shrnuje prostředky pro účet Spustit jako.<br>

| Prostředek | Popis |
| --- | --- |
| Runbook AzureAutomationTutorial |Ukázkový grafický runbook, který předvádí ověření pomocí účtu Spustit jako a získává všechny prostředku Resource Manageru. |
| Runbook AzureAutomationTutorialScript |Ukázkový runbook PowerShellu, který předvádí ověření pomocí účtu Spustit jako a získává všechny prostředku Resource Manageru. |
| AzureRunAsCertificate |Asset certifikátu vytvořený automaticky během vytváření účtu Automation, nebo když jste použili níže uvedený powershellový skript pro existující účet.  Umožňuje ověření pomocí Azure, abyste mohli spravovat prostředky Azure Resource Manageru pomocí runbooků.  Tento certifikát má životnost jeden rok. |
| AzureRunAsConnection |Asset připojení vytvořený automaticky během vytváření účtu Automation, nebo když jste použili níže uvedený powershellový skript pro existující účet. |

Následující tabulka shrnuje prostředky pro účet Spustit jako pro Azure Classic.<br>

| Prostředek | Popis |
| --- | --- |
| Runbook AzureClassicAutomationTutorial |Ukázkový grafický runbook, který získá všechny klasické virtuální počítače v rámci předplatného pomocí účtu Spustit jako pro Azure Classic (certifikát) a potom vypíše název a stav virtuálního počítače. |
| Runbook se skriptem AzureClassicAutomationTutorial |Ukázkový runbook PowerShellu, který získá všechny klasické virtuální počítače v rámci předplatného pomocí účtu Spustit jako pro Azure Classic (certifikát) a potom vypíše název a stav virtuálního počítače. |
| AzureClassicRunAsCertificate |Automaticky vytvořený asset certifikátu, který se používá k ověřování pomocí Azure, abyste mohli spravovat klasické prostředky Azure pomocí runbooků.  Tento certifikát má životnost jeden rok. |
| AzureClassicRunAsConnection |Automaticky vytvořený asset připojení, který se používá k ověřování pomocí Azure, abyste mohli spravovat klasické prostředky Azure pomocí runbooků. |

## <a name="verify-run-as-authentication"></a>Kontrola ověřování pomocí účtu Spustit jako
V dalším kroku provedeme malý test, abychom potvrdili, že můžete provádět ověření pomocí nového účtu Spustit jako.     

1. Na webu Azure Portal otevřete účet Automation, který jste vytvořili dříve.  
2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.
3. Vyberte runbook **AzureAutomationTutorialScript** a kliknutím na tlačítko **Spustit** runbook spusťte.  Zobrazí se výzva s dotazem, jestli chcete runbook spustit.
4. Vytvoří se [úloha runbooku](automation-runbook-execution.md), zobrazí se okno Úloha a na dlaždici **Souhrn úlohy** se zobrazí stav úlohy.  
5. Počáteční stav úlohy bude *Zařazeno ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu. Když pracovní proces úlohu přijme, změní se stav na *Spouštění*, a když se runbook skutečně spustí, změní se na *Spuštěno*.  
6. Po dokončení úlohy runbooku by se měla zobrazit zpráva **Dokončeno**.<br> ![Test runbooku objektu zabezpečení](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)<br>
7. Pokud chcete zobrazit podrobné výsledky runbooku, klikněte na dlaždici **Výstup**.
8. V okně **Výstup** byste měli vidět, že se úspěšně ověřil a vrátil seznam všech prostředků, které jsou ve skupině prostředků dostupné.
9. Zavřením okna **Výstup** se vrátíte do okna **Souhrn úlohy**.
10. Zavřete **Souhrn úlohy** a odpovídající okno runbooku **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Kontrola ověřování pomocí účtu Spustit jako pro Azure Classic
V dalším kroku provedeme malý test, abychom potvrdili, že můžete provádět ověření pomocí nového účtu Spustit jako pro Azure Classic.     

1. Na webu Azure Portal otevřete účet Automation, který jste vytvořili dříve.  
2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.
3. Vyberte runbook **AzureClassicAutomationTutorialScript** a kliknutím na tlačítko **Spustit** runbook spusťte.  Zobrazí se výzva s dotazem, jestli chcete runbook spustit.
4. Vytvoří se [úloha runbooku](automation-runbook-execution.md), zobrazí se okno Úloha a na dlaždici **Souhrn úlohy** se zobrazí stav úlohy.  
5. Počáteční stav úlohy bude *Zařazeno ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu. Když pracovní proces úlohu přijme, změní se stav na *Spouštění*, a když se runbook skutečně spustí, změní se na *Spuštěno*.  
6. Po dokončení úlohy runbooku by se měla zobrazit zpráva **Dokončeno**.<br> ![Test runbooku objektu zabezpečení](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
7. Pokud chcete zobrazit podrobné výsledky runbooku, klikněte na dlaždici **Výstup**.
8. V okně **Výstup** byste měli vidět, že se úspěšně ověřil a vrátil seznam všech klasických virtuálních počítačů v rámci předplatného.
9. Zavřením okna **Výstup** se vrátíte do okna **Souhrn úlohy**.
10. Zavřete **Souhrn úlohy** a odpovídající okno runbooku **AzureClassicAutomationTutorialScript**.

## <a name="managing-azure-run-as-account"></a>Správa účtu Spustit v Azure jako
Po celou dobu životnosti vašeho účtu Automation je nutné obnovovat certifikát před vypršením platnosti. Nebo můžete účet Spustit jako odstranit a znovu vytvořit, pokud věříte, že došlo k ohrožení bezpečnosti účtu.  V této části najdete pokyny, jak tyto operace provést.  

### <a name="self-signed-certificate-renewal"></a>Obnovení certifikátu podepsaného svým držitelem
Certifikát podepsaný svým držitelem vytvořený pro účet Spustit v Azure jako můžete obnovit kdykoli až do vypršení jeho platnosti, což je&1; rok od data vytvoření.  Když jej obnovíte, starý platný certifikát se uchová pro zajištění, že nebudou ovlivněny runbooky ověřované účtem Spustit jako, které jsou právě ve frontě nebo aktivně spuštěné.  Certifikát bude existovat až do vypršení platnosti.    

> [!NOTE]
> Pokud jste svůj účet Automation Spustit jako nakonfigurovali k používání certifikátu vydaného certifikační autoritou organizace a použijete tuto možnost, tento certifikát se nahradí certifikátem podepsaným svým držitelem.  

1. Na webu Azure Portal otevřete účet Automation.  
2. V okně účtu Automation v podokně vlastností účtu vyberte **Účty Spustit jako** v části **Nastavení účtů**.<br><br> ![Podokno vlastností účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)<br><br>
3. V okně vlastností **Účty Spustit jako** vyberte účet Spustit jako nebo účet Spustit jako pro Azure Classic, pro který chcete obnovit certifikát, a v okně vlastností vybraného účtu klikněte na **Obnovit certifikát**.<br><br> ![Obnovení certifikátu pro účet Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)<br><br> Zobrazí se výzva s dotazem, jestli chcete pokračovat.  
4. Zatímco se certifikát obnovuje, můžete průběh sledovat v nabídce v části **Oznámení**.

### <a name="delete-run-as-account"></a>Odstranění účtu Spustit jako
Následující postup popisuje, jak odstranit a znovu vytvořit účet Spustit v Azure jako nebo účet Spustit jako pro Azure Classic.  Po provedení této akce se uchová účet Automation.  Po odstranění účtu Spustit jako nebo účtu Spustit jako pro Azure Classic jej můžete znovu vytvořit na portálu.  

1. Na webu Azure Portal otevřete účet Automation.  
2. V okně účtu Automation v podokně vlastností účtu vyberte **Účty Spustit jako** v části **Nastavení účtů**.
3. V okně vlastností **Účty Spustit jako** vyberte účet Spustit jako nebo účet Spustit jako pro Azure Classic, který chcete odstranit, a v okně vlastností vybraného účtu klikněte na **Odstranit**.<br><br> ![Odstranění účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)<br><br>  Zobrazí se výzva s dotazem, jestli chcete pokračovat.
4. Zatímco se účet odstraňuje, můžete průběh sledovat v nabídce v části **Oznámení**.  Po dokončení odstraňování můžete účet znovu vytvořit v okně vlastností **Účty Spustit jako** výběrem možnosti Vytvořit v části **Účet Spustit v Azure jako**.<br><br> ![Znovuvytvoření účtu Automation Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)<br> 

### <a name="misconfiguration"></a>Chybná konfigurace
Pokud se během prvotního nastavení nesprávně vytvoří nebo později odstraní některá z položek konfigurace nezbytných pro správné fungování účtu Spustit jako nebo účtu Spustit jako pro Azure Classic, jako například:

* Asset certifikátu 
* Asset připojení 
* Účet Spustit jako byl odebrán z role přispěvatele
* Instanční objekt nebo aplikace v Azure AD

Služba Automation tyto změny zjistí a upozorní vás zobrazením stavu **Nedokončeno** u příslušného účtu v okně vlastností **Účty Spustit jako**.<br><br> ![Zpráva o stavu nedokončené konfigurace účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)<br><br>Pokud vyberete tento účet Spustit jako, v podokně vlastností účtu se zobrazí následující chyba:<br><br> ![Zpráva upozornění o stavu nedokončené konfigurace účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).<br>  
Pokud je váš účet Spustit jako chybně nakonfigurovaný, můžete to rychle vyřešit jeho odstraněním a znovuvytvořením.   

## <a name="update-an-automation-account-using-powershell"></a>Aktualizace účtu Automation pomocí PowerShellu
Tady vám nabízíme možnost aktualizace účtu Automation pomocí PowerShellu v případě, že jste provedli následující:

1. Vytvořili jste účet Automation, ale odmítli jste vytvoření účtu Spustit jako. 
2. Už máte účet Automation pro správu prostředků Resource manageru a chcete ho aktualizovat, aby zahrnoval účet Spustit jako pro ověřování runbooků.
4. Už máte účet Automation pro správu klasických prostředků a chcete ho aktualizovat, abyste mohli používat účet Spustit jako pro Azure Classic místo vytváření nového účtu a migrace runbooků a assetů na nový účet.   
5. Rozhodli jste se vytvořit účet Spustit v Azure jako a účet Spustit jako pro Azure Classic pomocí certifikátu, který vydala CA organizace.

Tento skript má následující požadavky:

1. Tento skript podporuje spuštění pouze v systémech Windows 10 a Windows Server 2016 s nainstalovanými moduly Azure Resource Manageru verze 2.01 nebo novější.  V předchozích verzích Windows není podporován.  
2. Azure PowerShell 1.0 nebo novější. Informace o této verzi a její instalaci najdete v článku [Postup instalace a konfigurace Azure PowerShellu](/powershell/azureps-cmdlets-docs).
3. Vytvořili jste účet Automation.  Na tento účet se bude v níže uvedeném skriptu odkazovat jako na hodnotu pro parametry - AutomationAccountName a -ApplicationDisplayName.

Pokud chcete získat hodnoty pro *SubscriptionID*, *ResourceGroup* a *AutomationAccountName*, které jsou požadovanými parametry skriptů, vyberte na portálu Azure v okně **Účet automatizace** svůj účet Automation a vyberte **Všechna nastavení**.  V okně **Všechna nastavení** v části **Nastavení účtu** vyberte **Vlastnosti**.  V okně **Vlastnosti** si můžete tyto hodnoty opsat.<br><br> ![Vlastnosti účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-run-as-account-powershell-script"></a>Vytvoření powershellového skriptu pro účet Spustit jako
Tento skript PowerShellu zahrnuje podporu následujících konfigurací: 

* Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem
* Vytvoření účtu Spustit v Azure jako a účtu Spustit jako pro Azure Classic pomocí certifikátu podepsaného svým držitelem
* Vytvoření účtu Spustit v Azure jako a účtu Spustit jako pro Azure Classic pomocí podnikového certifikátu
* Vytvoření účtu Spustit v Azure jako a účtu Spustit jako pro Azure Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government

Skript v závislosti na vybrané možnosti konfigurace provede následující:

* Vytvoří aplikaci Azure AD, která bude ověřena pomocí certifikátu podepsaného svým držitelem nebo pomocí podnikového certifikátu, vytvoří účet instančního objektu pro tuto aplikaci v Azure AD a přiřadí roli přispěvatele (toto můžete změnit na roli vlastníka nebo jinou roli) pro tento účet v aktuálním předplatném.  Další informace najdete v článku [Řízení přístupu na základě rolí ve službě Azure Automation](automation-role-based-access-control.md).
* Asset certifikátu Automation na určeném účtu Automation s názvem **AzureRunAsCertificate**, který obsahuje certifikát použitý objektem služby.
* Asset připojení Automation v určeném účtu Automation s názvem **AzureRunAsConnection**, který obsahuje applicationId, tenantId, subscriptionId a kryptografický otisk certifikátu.    

Pro účet Spustit jako pro Azure Classic:

* Asset certifikátu Automation v určeném účtu Automation s názvem **AzureClassicRunAsCertificate**, který obsahuje certifikát podepsaný svým držitelem nebo vydaný certifikační autoritou organizace, použitý k ověření vašich runbooků.
* Asset připojení Automation v určeném účtu Automation s názvem **AzureClassicRunAsConnection**, který obsahuje název předplatného, subscriptionId a název assetu certifikátu.

Pokud pro účet Spustit jako pro Azure Classic vyberete možnost použít certifikát podepsaný svým držitelem, skript vytvoří certifikát pro správu podepsaný svým držitelem a uloží ho do složky dočasných souborů v počítači v rámci profilu uživatele, který používáte ke spouštění relace PowerShellu – *%USERPROFILE%\AppData\Local\Temp*.  Po spuštění skriptu budete muset odeslat certifikát pro správu Azure do úložiště správy předplatného, ve kterém byl účet Automation vytvořený.  Následující postup vás provede procesem spuštění skriptu a odesláním certifikátu.  

1. Uložte následující skript do počítače.  V tomto příkladu ho uložte s názvem **New-RunAsAccount.ps1**.  
   
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
        [int] $NoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,[string] $certPath, [string] $certPathCer, [string] $noOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"

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
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use Key credentials and create AAD Application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId 
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id
   
        # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
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

        # Create Run As Account using Service Principal
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
           CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $NoOfMonthsUntilExpired
        }

        # Create Service Principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

         # Create the automation certificate asset
         CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

         # Populate the ConnectionFieldValues
         $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
         $TenantID = $SubscriptionInfo | Select TenantId -First 1
         $Thumbprint = $PfxCert.Thumbprint
         $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId} 

         # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
         CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
           # Create Run As Account using Service Principal
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
            CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $NoOfMonthsUntilExpired
         }

         # Create the automation certificate asset
         CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

         # Populate the ConnectionFieldValues
         $SubscriptionName = $subscription.Subscription.SubscriptionName
         $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

         # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
         CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

         Write-Host -ForegroundColor red $UploadMessage
         }

2. Na svém počítači na obrazovce **Start** spusťte **Windows PowerShell** se zvýšenými uživatelskými právy.
3. Z prostředí příkazového řádku PowerShellu se zvýšenými oprávněními přejděte do složky, která obsahuje skript vytvořený v kroku 1, a spusťte tento skript se změněnými hodnotami požadovaných parametrů v závislosti na požadované konfiguraci.  

    **Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword>` 

    **Vytvoření účtu Spustit v Azure jako a účtu Spustit jako pro Azure Classic pomocí certifikátu podepsaného svým držitelem**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Vytvoření účtu Spustit v Azure jako a účtu Spustit jako pro Azure Classic pomocí podnikového certifikátu**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Vytvoření účtu Spustit v Azure jako a účtu Spustit jako pro Azure Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`
 
    > [!NOTE]
    > Po spuštění skriptu budete vyzváni k ověření pomocí Azure. Musíte se přihlásit k portálu Azure pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.
    > 
    > 

Pokud jste vytvořili účet Spustit jako pro Azure Classic, po úspěšném dokončení skriptu budete muset zkopírovat vytvořený certifikát do složky **Temp** v profilu uživatele.  Postupujte podle kroků pro [odeslání certifikátu rozhraní API pro správu](../azure-api-management-certs.md) na portál Azure Classic a potom použijte [ukázkový kód](#sample-code-to-authenticate-with-service-management-resources) k ověření konfigurace přihlašovacích údajů pomocí prostředků správy služeb.  Pokud jste nevytvořili účet Spustit jako pro Azure Classic, použijte níže uvedený [ukázkový kód](#sample-code-to-authenticate-with-resource-manager-resources) k ověření pomocí prostředků Resource Manageru a ke kontrole konfigurace přihlašovacích údajů. Nebo použijte [ukázkový kód](#sample-code-to-authenticate-with-service-management-resources) k ověření konfigurace přihlašovacích údajů pomocí prostředků správy služeb.

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Ukázkový kód pro ověření pomocí prostředků Resource Manageru
Můžete použít níže uvedený aktualizovaný ukázkový kód z ukázkového runbooku **AzureAutomationTutorialScript** a provést ověření pomocí účtu Spustit jako, abyste mohli prostředky Resource Manageru spravovat pomocí svých runbooků.   

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


Skript obsahuje dva další řádky kódu, které podporují odkazování na kontext předplatného, abyste mohli lépe pracovat s několika předplatnými. Proměnný asset s názvem SubscriptionId obsahuje ID předplatného a po příkazu rutiny Add-AzureRmAccount bude [rutina Set-AzureRmContext](https://msdn.microsoft.com/library/mt619263.aspx) uvedená v sadě parametrů *-SubscriptionId*. Pokud je název proměnné příliš obecný, můžete ho upravit přidáním předpony nebo pomocí jiné zásady vytváření názvů, abyste proměnnou dokázali lépe identifikovat. Alternativně můžete místo -SubscriptionId použít sadu parametrů -SubscriptionName s odpovídajícím proměnným assetem.    

Všimněte si, že rutina používaná pro ověřování v runbooku – **Add-AzureRmAccount**, používá sadu parametrů *ServicePrincipalCertificate*.  Ověřování provádí pomocí certifikátu objektu služby a ne pomocí přihlašovacích údajů.  

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Ukázkový kód pro ověření pomocí prostředků správy služby
Můžete použít níže uvedený aktualizovaný ukázkový kód z ukázkového runbooku **AzureClassicAutomationTutorialScript** a provést ověření pomocí účtu Spustit jako pro Azure Classic, abyste mohli klasické prostředky spravovat pomocí svých runbooků.

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
* Další informace o objektech služby najdete v článku [Objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).
* Další informace o řízení přístupu na základě rolí ve službě Azure Automation najdete v článku [Řízení přístupu na základě rolí ve službě Azure Automation](automation-role-based-access-control.md).
* Další informace o certifikátech a službách Azure najdete v článku [Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md)


