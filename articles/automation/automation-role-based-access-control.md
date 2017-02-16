---
title: "Řízení přístupu na základě role ve službě Azure Automation | Dokumentace Microsoftu"
description: "Řízení přístupu na základě role (RBAC) umožňuje správu přístupu k prostředkům Azure. Tento článek popisuje způsob nastavení řízení přístupu na základě role ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: "rbac v automation, řízení přístupu na základě rolí, rbac v azure"
ms.assetid: 04b5625e-0ee8-4b5b-85cd-7734c1b3d4a3
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte;sngun
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 17c7e410a9c5b69ab450eb3affd192f1e3cb6e76


---
# <a name="role-based-access-control-in-azure-automation"></a>Řízení přístupu na základě role ve službě Azure Automation
## <a name="role-based-access-control"></a>Řízení přístupu na základě role
Řízení přístupu na základě role (RBAC) umožňuje správu přístupu k prostředkům Azure. Pomocí [řízení přístupu na základě role](../active-directory/role-based-access-control-configure.md) můžete povinnosti v rámci týmu oddělit a udělit uživatelům, skupinám a aplikacím jenom takový přístup, který k provádění svojí práce potřebují. Přístup na základě role můžete udělit uživatelům, kteří používají portál Azure, nástroje příkazového řádku Azure nebo rozhraní API pro správu Azure.

## <a name="rbac-in-automation-accounts"></a>Řízení přístupu na základě role v účtech Automation
Ve službě Azure Automation se přístup uděluje přiřazením příslušné role RBAC uživatelům, skupinám a aplikacím v rozsahu účtu Automation. Níže jsou uvedené vestavěné role, které účet Automation podporuje:  

| **Role** | **Popis** |
|:--- |:--- |
| Vlastník |Role vlastníka umožňuje přístup ke všem prostředkům a akcím v rámci účtu Automation, včetně poskytnutí přístupu dalším uživatelům, skupinám a aplikacím za účelem správy účtu Automation. |
| Přispěvatel |Role přispěvatele umožňuje spravovat všechno kromě úpravy oprávnění jiných uživatelů k přístupu k účtu Automation. |
| Čtenář |Role čtenáře vám umožní zobrazit všechny prostředky na účtu Automation, ale neumožní vám provádět změny. |
| Operátor služby Automation |Role operátora služby Automation umožňuje provádět provozní úlohy, například spuštění, zastavení, pozastavení, pokračování a plánování úloh. Tato role je užitečná, pokud chcete chránit prostředky na účtu Automation, například assety přihlašovacích údajů a runbooky, aby je nikdo nemohl zobrazit nebo upravit, ale aby členové vaší organizace mohli tyto runbooky stále spouštět. |
| Správce přístupu uživatelů |Role správce přístupu uživatelů umožňuje spravovat přístup uživatelů k účtům Azure Automation. |

> [!NOTE]
> Konkrétnímu runbooku nebo runbookům nemůžete udělit přístupová práva, můžete je přidělit jenom prostředkům a akcím v rámci účtu Automation.  
> 
> 

V tomto článku vás provedeme procesem nastavení řízení přístupu na základě role ve službě Azure Automation. Nejdřív se trochu podrobněji podíváme na jednotlivá oprávnění udělená přispěvateli, čtenáři, operátorovi služby Automation a správci přístupu uživatelů, abychom získali dostatečný přehled předtím, než někomu udělíme práva k účtu Automation.  V opačném případě by to mohlo přinést nezamýšlené nebo nežádoucí důsledky.     

## <a name="contributor-role-permissions"></a>Oprávnění role přispěvatele
Následující tabulka představuje konkrétní akce, které může role přispěvatele provádět na účtu Automation.

| **Typ prostředku** | **Čtení** | **Zápis** | **Odstranění** | **Další akce** |
|:--- |:--- |:--- |:--- |:--- |
| Účet Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset certifikátu Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset připojení Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset typu připojení Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset přihlašovacích údajů Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset plánu Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset proměnné Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Požadovaný stav konfigurace Automation | | | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Typ prostředku procesu Hybrid Runbook Worker |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Úloha Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Datový proud úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Plán úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Modul Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Runbook Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Koncept runbooku Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Testovací úloha konceptu runbooku Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Webhook služby Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |

## <a name="reader-role-permissions"></a>Oprávnění role čtenáře
Následující tabulka představuje konkrétní akce, které může role čtenáře provádět na účtu Automation.

| **Typ prostředku** | **Čtení** | **Zápis** | **Odstranění** | **Další akce** |
|:--- |:--- |:--- |:--- |:--- |
| Klasický správce předplatného |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Zámek pro správu |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Oprávnění |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Operace poskytovatele |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Přiřazení role |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Definice role |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="automation-operator-role-permissions"></a>Oprávnění role operátora služby Automation
Následující tabulka představuje konkrétní akce, které může role operátora služby Automation provádět na účtu Automation.

| **Typ prostředku** | **Čtení** | **Zápis** | **Odstranění** | **Další akce** |
|:--- |:--- |:--- |:--- |:--- |
| Účet Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset certifikátu Automation | | | | |
| Asset připojení Automation | | | | |
| Asset typu připojení Automation | | | | |
| Asset přihlašovacích údajů Automation | | | | |
| Asset plánu Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | |
| Asset proměnné Automation | | | | |
| Požadovaný stav konfigurace Automation | | | | |
| Typ prostředku procesu Hybrid Runbook Worker | | | | |
| Úloha Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Datový proud úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Plán úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | |
| Modul Automation | | | | |
| Runbook Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Koncept runbooku Automation | | | | |
| Testovací úloha konceptu runbooku Automation | | | | |
| Webhook služby Automation | | | | |

[Akce operátora automatizace](../active-directory/role-based-access-built-in-roles.md#automation-operator) uvádí seznam akcí podporovaných rolí operátora automatizace na účtu Automation a jejími prostředky.

## <a name="user-access-administrator-role-permissions"></a>Oprávnění role správce přístupu uživatelů
Následující tabulka představuje konkrétní akce, které může role správce přístupu uživatelů provádět na účtu Automation.

| **Typ prostředku** | **Čtení** | **Zápis** | **Odstranění** | **Další akce** |
|:--- |:--- |:--- |:--- |:--- |
| Účet Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset certifikátu Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset připojení Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset typu připojení Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset přihlašovacích údajů Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset plánu Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset proměnné Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Požadovaný stav konfigurace Automation | | | | |
| Typ prostředku procesu Hybrid Runbook Worker |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Úloha Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Datový proud úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Plán úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Modul Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Runbook Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Koncept runbooku Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Testovací úloha konceptu runbooku Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Webhook služby Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="configure-rbac-for-your-automation-account-using-azure-portal"></a>Konfigurace řízení přístupu na základě role u vašeho účtu Automation pomocí portálu Azure
1. Přihlaste se k [portálu Azure](https://portal.azure.com/) a v okně Účty Automation otevřete svůj účet Automation.  
2. V pravém horním rohu klikněte na ovládací prvek **Přístup**. Tím otevřete okno **Uživatelé**, ve kterém můžete přidat nové uživatele, skupiny a aplikace, aby mohli spravovat váš účet Automation a zobrazovat existující role, které můžou být pro účet Automation konfigurované.  
   
   ![Tlačítko Přístup](media/automation-role-based-access-control/automation-01-access-button.png)  

> [!NOTE]
> **Správci předplatného** už existují jako výchozí uživatel. Skupina služby Active Directory, která patří správcům předplatného, zahrnuje správce služeb a spolusprávce předplatného Azure. Správce služby je vlastníkem vašeho předplatného Azure a jeho prostředků a zdědí i roli vlastníka pro účty Automation. To znamená, že přístup se **dědí** v případě **správců a spolusprávců služby** předplatného a všem ostatním uživatelům se **přiřazuje**. Kliknutím na **Správci předplatného** zobrazíte další podrobnosti o jejich oprávněních.  
> 
> 

### <a name="add-a-new-user-and-assign-a-role"></a>Přidání nového uživatele a přiřazení role
1. V okně Uživatelé klikněte na **Přidat** a otevřete okno **Přidání přístupu**, ve kterém můžete přidat uživatele, skupiny nebo aplikace a přiřadit jim roli.  
   
   ![Přidání uživatele](media/automation-role-based-access-control/automation-02-add-user.png)  
2. V seznamu dostupných rolí vyberte jednu roli. Vybereme roli **Čtenář**, ale vy si můžete vybrat libovolnou vestavěnou roli, která je dostupná a kterou účet Automation podporuje, nebo jakoukoli vlastní roli, kterou jste definovali.  
   
   ![Výběr role](media/automation-role-based-access-control/automation-03-select-role.png)  
3. Kliknutím na **Přidat uživatele** otevřete okno **Přidání uživatelů**. Pokud jste přidali nějaké uživatele, skupiny nebo aplikace, aby spravovali vaše předplatné, budou tito uživatelé vypsáni a vy je budete moct vybrat a přidat jim přístup. Pokud žádné uživatele nevidíte nebo pokud požadovaný uživatel není uvedený, klikněte na **Pozvat** a otevřete okno **Pozvání hosta**, ve kterém můžete pozvat uživatele s platnou e-mailovou adresou k účtu Microsoft, například Outlook.com, OneDrive nebo Xbox Live ID. Po zadání e-mailové adresy uživatele klikněte na **Vybrat**, přidejte uživatele a potom klikněte na tlačítko **OK**. 
   
   ![Přidání uživatelů](media/automation-role-based-access-control/automation-04-add-users.png)  
   
   Teď by se měl přidaný uživatel zobrazit v okně **Uživatelé** s přiřazenou rolí **Čtenář**.  
   
   ![Vypsání uživatelů](media/automation-role-based-access-control/automation-05-list-users.png)  
   
   Roli můžete uživateli přiřadit také v okně **Role**. 
4. Klikněte v okně Uživatelé na **Role**, a otevřete tak okno **Role**. V tomto okně můžete vidět název role a počet uživatelů a skupin, kteří jsou k této roli přiřazení.
   
    ![Přiřazení role v okně Uživatelé](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
   > [!NOTE]
   > Řízení přístupu na základě role můžete nastavit jenom na úrovni účtu Automation, a ne u jakéhokoliv prostředku pod účtem Automation.
   > 
   > 
   
    Uživateli, skupině nebo aplikaci můžete přiřadit více než jednu roli. Pokud uživateli přidáme například roli **Operátor automatizace** spolu s rolí **Čtenář**, takový uživatel může zobrazit všechny prostředky Automation a může také spouštět úlohy runbooku. Pokud chcete zobrazit role přiřazené uživateli, můžete rozbalit rozevírací seznam.  
   
    ![Zobrazení více rolí](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)  

### <a name="remove-a-user"></a>Odebrání uživatele
Pokud uživatel nespravuje účet Automation nebo už v organizaci nepracuje, můžete odebrat jeho oprávnění k přístupu. Uživatele můžete odebrat následujícím postupem: 

1. V okně **Uživatelé** vyberte přiřazenou roli, kterou chcete odebrat.
2. V okně podrobností přiřazení klikněte na tlačítko **Odebrat**.
3. Kliknutím na **Ano** odebrání potvrďte. 
   
   ![Odebrání uživatelů](media/automation-role-based-access-control/automation-08-remove-users.png)  

## <a name="role-assigned-user"></a>Uživatel přiřazený k roli
Když se uživatel přiřazený k roli přihlásí k účtu Automation, může vidět účet vlastníka uvedený v seznamu **Výchozí adresáře**. Aby mohl vidět účet Automation, ke kterému byl přidán, musí přepnout výchozí adresář na výchozí adresář vlastníka.  

![Výchozí adresář](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)  

### <a name="user-experience-for-automation-operator-role"></a>Uživatelské prostředí pro roli operátora služby Automation
Pokud uživatel, který je přiřazený k roli Operátor služby Automation, zobrazí účet Automation, ke kterému je přiřazený, může zobrazit jenom seznam runbooků a úlohy a plány runbooků vytvořené v účtu Automation, ale nemůže zobrazit jejich definice. Může spustit, zastavit, pozastavit, obnovit nebo naplánovat úlohu runbooku. Uživatel nebude mít přístup k dalším prostředkům Automation, například ke konfiguracím, skupinám Hybrid Worker nebo uzlům DSC.  

![Žádný přístup k prostředkům](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

Když uživatel klikne na runbook, nebudou mu nabídnuty příkazy pro zobrazení nebo úpravu zdroje, protože role operátora služby Automation k nim nemá přístup.  

![Žádný přístup k úpravě runbooku](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)  

Uživatel bude mít přístup k zobrazení a vytváření plánů, ale nebudete mít přístup k žádným jiným typům assetů.  

![Žádný přístup k assetům](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)  

Tento uživatel také nemá přístup k zobrazení webhooků, které jsou přidružené k runbooku.

![Žádný přístup k webhookům](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## <a name="configure-rbac-for-your-automation-account-using-azure-powershell"></a>Konfigurace řízení přístupu na základě role u vašeho účtu Automation pomocí Azure PowerShellu
Přístup na základě role můžete pro účet Automation nakonfigurovat také pomocí následujících [rutin Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md).

• [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) vypíše všechny role funkce řízení přístupu na základě role, které jsou ve službě Azure Active Directory dostupné. Tento příkaz můžete použít spolu s vlastností **Název** k vypsání všech akcí, které může konkrétní role provádět.  
    **Příklad:**  
    ![Získání definice role](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)  

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) vypíše v zadaném rozsahu všechna přiřazení rolí funkce řízení přístupu na základě role v Azure AD. Pokud nezadáte žádné parametry, tento příkaz vrátí všechna přiřazení rolí v rámci předplatného. K vypsání přiřazení přístupu konkrétního uživatele nebo skupin, kterých je uživatel členem, použijte parametr **ExpandPrincipalGroups**.  
    **Příklad:** K vypsání všech uživatelů a jejich rolí v rámci účtu Automation použijte následující příkaz.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Získat přiřazení role](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) slouží k přiřazení přístupu v určitém rozsahu pro uživatele, skupiny nebo aplikace.  
    **Příklad:** K přiřazení role „Operátor služby Automation“ pro uživatele v rozsahu účtu Automation použijte následující příkaz.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Přiřazení nové role](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) použijte k odebrání přístupu v určitém rozsahu pro konkrétního uživatele, skupinu nebo aplikaci.  
    **Příklad:** K odebrání uživatele z role „Operátor služby Automation“ v rozsahu účtu Automation použijte následující příkaz.

    Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

Ve výše uvedených příkladech nahraďte **přihlašovací ID**, **ID předplatného**, **název skupiny prostředků** a **název účtu Automation** údaji ze svého účtu. Po zobrazení výzvy, která požádá o potvrzení odebrání přiřazené role uživatele, zvolte **Ano**.   

## <a name="next-steps"></a>Další kroky
* Další informace o různých způsobech konfigurace RBAC pro Azure Automation najdete v článku [Správa řízení přístupu na základě role pomocí Azure PowerShellu](../active-directory/role-based-access-control-manage-access-powershell.md).
* Podrobnosti o různých způsobech spouštění runbooků najdete v článku [Spuštění runbooku](automation-starting-a-runbook.md)
* Další informace o různých typech runbooků najdete v článku [Typy runbooků ve službě Azure Automation](automation-runbook-types.md).




<!--HONumber=Dec16_HO1-->


