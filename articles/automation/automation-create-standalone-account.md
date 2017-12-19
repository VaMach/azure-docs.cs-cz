---
title: "Vytvoření samostatného účtu Azure Automation | Dokumentace Microsoftu"
description: "Kurz vás provede vytvořením, otestováním a ukázkovým použitím ověření objektu zabezpečení ve službě Azure Automation."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: dc4bfa4a94eaa2fb4e0e821c4931dcd1963f3109
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="create-a-standalone-azure-automation-account"></a>Vytvoření samostatného účtu Azure Automation
Toto téma ukazuje, jak vytvořit účet Automation z webu Azure Portal, pokud se chcete seznámit se službou Azure Automation a vyzkoušet si ji bez zahrnutí dalších řešení pro správu nebo integrace s OMS Log Analytics k poskytování pokročilého monitorování úloh runbooků.  Tato řešení pro správu nebo integraci s Log Analytics můžete přidat kdykoli v budoucnu.  S účtem Automation můžete ověřit runbooky, které spravují prostředky buď v Azure Resource Manageru, nebo v nasazení Azure Classic.

Když na webu Azure Portal vytvoříte účet Automation, účet automaticky vytvoří následující:

* Účet Spustit jako, který vytvoří nový instanční objekt v Azure Active Directory (certifikát) a přiřadí přispěvateli řízení přístupu na základě rolí (RBAC), které se používá ke správě prostředků Resource Manageru pomocí runbooků.   
* Účet Spustit jako pro Azure Classic tím, že nahraje certifikát pro správu, který se používá ke správě prostředků Classic pomocí runbooků.  

Tato funkce zjednodušuje proces a pomůže vám rychle začít vytvářet a nasazovat runbooky na podporu vašich automatizačních potřeb.  

## <a name="permissions-required-to-create-automation-account"></a>Oprávnění požadovaná k vytvoření účtu Automation
Pokud chcete vytvořit nebo aktualizovat účet Automation, musíte mít následující specifická oprávnění vyžadovaná k dokončení tohoto tématu.   
 
* Abyste mohli vytvořit účet Automation, váš uživatelský účet AD musí být přidán do role se stejnými oprávněními jako role vlastníka pro prostředky Microsoft.Automation, jak je uvedeno v článku [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).  
* Pokud je nastavení Registrace aplikací nastaveno na **Ano**, uživatelé ve vašem tenantovi Azure AD, kteří nejsou správci, můžou [registrovat aplikace služby AD](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions).  Pokud je nastavení Registrace aplikací nastaveno na **Ne**, uživatel provádějící tuto akci musí být globálním správcem služby Azure AD. 

Pokud před přidáním do role globálního správce nebo spolusprávce nejste členem instance Active Directory příslušného předplatného, budete do služby Active Directory přidaní jako host. V takové situaci se zobrazí upozornění Nemáte oprávnění k vytvoření... v okně **Přidání účtu Automation**. Uživatele, kteří byli nejdřív přidaní do role globálního správce nebo spolusprávce, je možné z instance Active Directory předplatného odebrat a potom je znovu přidat – tak se z nich ve službě Active Directory stanou úplní uživatelé. Takovou situaci můžete ověřit v podokně **Azure Active Directory** na webu Azure Portal. Vyberte **Uživatelé a skupiny**, potom **Všichni uživatelé** a po výběru konkrétního uživatele vyberte **Profil**. Hodnota atributu **Typ uživatele** v profilu uživatele by neměla být **Host**.

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Vytvoření nového účtu Automation na webu Azure Portal
V této části provedete následující kroky a vytvoříte účet Azure Automation na webu Azure Portal.    

1. Přihlaste se k webu Azure Portal pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.
2. Klikněte na možnost **Nové**.<br><br> ![Výběr možnosti Nové na webu Azure Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Vyhledejte **Automation** a potom ve výsledcích hledání vyberte **Automation and Control***.<br><br> ![Vyhledání a výběr služby Automation na Marketplace](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)<br> 
3. V okně Účty Automation klikněte na **Přidat**.<br><br>![Přidání účtu Automation](media/automation-create-standalone-account/automation-create-automationacct-properties.png)


   > [!NOTE]
   > Pokud se v okně **Přidat účet Automation** zobrazí následující upozornění, důvodem je to, že váš účet není členem role správců předplatného a není spolusprávcem předplatného.<br><br>![Přidání upozornění do účtu Automation](media/automation-create-standalone-account/create-account-without-perms.png)
   > 
   > 
4. V okně **Přidat účet Automation** do pole **Název** zadejte název nového účtu Automation.
5. Pokud máte více než jedno předplatné, zadejte předplatné pro nový účet, novou nebo existující **skupinu prostředků** a **umístění** datového centra Azure.
6. U možnosti **Vytvořit účet Spustit v Azure jako** zkontrolujte, jestli je vybraná položka **Ano**, a klikněte na tlačítko **Vytvořit**.  
   
   > [!NOTE]
   > Pokud se rozhodnete nevytvořit účet Spustit v Azure jako tím, že nevyberete možnost **Ne**, zobrazí se v okně **Přidání účtu Automation** zpráva upozornění.  Při vytváření účtu na portálu Azure nemá účet odpovídající identitu ověřování v rámci adresářové služby klasického předplatného nebo předplatného Resource Manageru a předplatné proto nebude mít přístup k prostředkům.  Všechny runbooky odkazující na tento účet kvůli tomu nebudou moct ověřit a provádět úlohy s prostředky v těchto modelech nasazení.
   > 
   > ![Přidání upozornění do účtu Automation](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)<br>
   > Pokud nevytvoříte objekt služby, nebude přiřazená role přispěvatele.
   > 

7. Zatímco Azure vytváří účet Automation, můžete průběh sledovat v nabídce v části **Oznámení**.

### <a name="resources-included"></a>Zahrnuté prostředky
Po úspěšném vytvoření účtu Automation se pro vaší potřebu automaticky vytvoří několik prostředků.  Následující tabulka shrnuje prostředky pro účet Spustit jako.<br>

| Prostředek | Popis |
| --- | --- |
| Runbook AzureAutomationTutorial |Ukázkový grafický runbook, který předvádí ověření pomocí účtu Spustit jako a získává všechny prostředku Resource Manageru. |
| Runbook AzureAutomationTutorialScript |Ukázkový runbook PowerShellu, který předvádí ověření pomocí účtu Spustit jako a získává všechny prostředku Resource Manageru. |
| Runbook AzureAutomationTutorialPython2 |Ukázkový runbook Pythonu, který předvádí ověření pomocí účtu Spustit jako a následný výpis skupin prostředků, které se nacházejí v zadaném předplatném. |
| AzureRunAsCertificate |Asset certifikátu vytvořený automaticky během vytváření účtu Automation, nebo když jste použili níže uvedený powershellový skript pro existující účet.  Umožňuje ověření pomocí Azure, abyste mohli spravovat prostředky Azure Resource Manageru pomocí runbooků.  Tento certifikát má životnost jeden rok. |
| AzureRunAsConnection |Asset připojení vytvořený automaticky během vytváření účtu Automation, nebo když jste použili níže uvedený powershellový skript pro existující účet. |

Následující tabulka shrnuje prostředky pro účet Spustit jako pro Azure Classic.<br>

| Prostředek | Popis |
| --- | --- |
| Runbook AzureClassicAutomationTutorial |Ukázkový grafický runbook, který získá všechny klasické virtuální počítače v rámci předplatného pomocí účtu Spustit jako pro Azure Classic (certifikát) a potom vypíše název a stav virtuálního počítače. |
| Runbook se skriptem AzureClassicAutomationTutorial |Ukázkový runbook PowerShellu, který získá všechny klasické virtuální počítače v rámci předplatného pomocí účtu Spustit jako pro Azure Classic (certifikát) a potom vypíše název a stav virtuálního počítače. |
| AzureClassicRunAsCertificate |Automaticky vytvořený asset certifikátu, který se používá k ověřování pomocí Azure, abyste mohli spravovat klasické prostředky Azure pomocí runbooků.  Tento certifikát má životnost jeden rok. |
| AzureClassicRunAsConnection |Automaticky vytvořený asset připojení, který se používá k ověřování pomocí Azure, abyste mohli spravovat klasické prostředky Azure pomocí runbooků. |


## <a name="next-steps"></a>Další kroky
* Další informace o vytváření grafického obsahu najdete v článku [Vytváření grafického obsahu v Azure Automation](automation-graphical-authoring-intro.md).
* První kroky s powershellovými runbooky najdete v článku [Můj první powershellový runbook](automation-first-runbook-textual-powershell.md).
* První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).
* První kroky s runbooky Python2 najdete v článku [Můj první runbook Python2](automation-first-runbook-textual-python2.md).
