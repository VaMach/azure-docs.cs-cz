<properties 
   pageTitle="Řízení přístupu na základě role ve službě Azure Automation | Microsoft Azure"
   description="Řízení přístupu na základě role (RBAC) umožňuje správu přístupu k prostředkům Azure. Tento článek popisuje způsob nastavení řízení přístupu na základě role ve službě Azure Automation."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn"
   keywords="automation rbac, role based access control, azure rbac" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/10/2016"
   ms.author="magoedte;sngun"/>

# Řízení přístupu na základě role ve službě Azure Automation

## Řízení přístupu na základě role

Řízení přístupu na základě role (RBAC) umožňuje správu přístupu k prostředkům Azure. Pomocí [řízení přístupu na základě role](../active-directory/role-based-access-control-configure.md) můžete povinnosti v rámci týmu oddělit a udělit uživatelům, skupinám a aplikacím jenom takový přístup, který k provádění svojí práce potřebují. Přístup na základě role můžete udělit uživatelům, kteří používají portál Azure, nástroje příkazového řádku Azure nebo rozhraní API pro správu Azure.

## Řízení přístupu na základě role v účtech Automation

Ve službě Azure Automation se přístup uděluje přiřazením příslušné role RBAC uživatelům, skupinám a aplikacím v rozsahu účtu Automation. Níže jsou uvedené vestavěné role, které účet Automation podporuje:  

|**Role** | **Popis** |
|:--- |:---|
| Vlastník | Role vlastníka umožňuje přístup ke všem prostředkům a akcím v rámci účtu Automation, včetně poskytnutí přístupu dalším uživatelům, skupinám a aplikacím za účelem správy účtu Automation. |
| Přispěvatel | Role přispěvatele umožňuje spravovat všechno kromě úpravy oprávnění jiných uživatelů k přístupu k účtu Automation. |
| Čtenář | Role čtenáře vám umožní zobrazit všechny prostředky na účtu Automation, ale neumožní vám provádět změny. |
| Operátor služby Automation | Role operátora služby Automation umožňuje provádět provozní úlohy, například spuštění, zastavení, pozastavení, pokračování a plánování úloh. Tato role je užitečná, pokud chcete chránit prostředky na účtu Automation, například assety přihlašovacích údajů a runbooky, aby je nikdo nemohl zobrazit nebo upravit, ale aby členové vaší organizace mohli tyto runbooky stále spouštět. [Akce operátora automatizace](../active-directory/role-based-access-built-in-roles.md#automation-operator) uvádí seznam akcí podporovaných rolí operátora automatizace na účtu Automation a jejími prostředky. |
| Správce přístupu uživatelů | Role správce přístupu uživatelů umožňuje spravovat přístup uživatelů k účtům Azure Automation. |

V tomto článku vás provedeme procesem nastavení řízení přístupu na základě role ve službě Azure Automation. 

## Konfigurace řízení přístupu na základě role u vašeho účtu Automation pomocí portálu Azure

1.  Přihlaste se k [portálu Azure](https://portal.azure.com/) a v okně Účty Automation otevřete svůj účet Automation.  

2.  V pravém horním rohu klikněte na ovládací prvek **Přístup**. Tím otevřete okno **Uživatelé**, ve kterém můžete přidat nové uživatele, skupiny a aplikace, aby mohli spravovat váš účet Automation a zobrazovat existující role, které můžou být pro účet Automation konfigurované.  

    ![Tlačítko Přístup](media/automation-role-based-access-control/automation-01-access-button.png)  

>[AZURE.NOTE]  **Správci předplatného** už existují jako výchozí uživatel. Skupina služby Active Directory, která patří správcům předplatného, zahrnuje správce služeb a spolusprávce předplatného Azure. Správce služby je vlastníkem vašeho předplatného Azure a jeho prostředků a zdědí i roli vlastníka pro účty Automation. To znamená, že přístup se **dědí** v případě **správců a spolusprávců služby** předplatného a všem ostatním uživatelům se **přiřazuje**. Kliknutím na **Správci předplatného** zobrazíte další podrobnosti o jejich oprávněních.  

### Přidání nového uživatele a přiřazení role

1.  V okně Uživatelé klikněte na **Přidat** a otevřete okno **Přidání přístupu**, ve kterém můžete přidat uživatele, skupiny nebo aplikace a přiřadit jim roli.  

    ![Přidání uživatele](media/automation-role-based-access-control/automation-02-add-user.png)  

2.  V seznamu dostupných rolí vyberte jednu roli. Vybereme roli **Čtenář**, ale vy si můžete vybrat libovolnou vestavěnou roli, která je dostupná a kterou účet Automation podporuje, nebo jakoukoli vlastní roli, kterou jste definovali.  

    ![Výběr role](media/automation-role-based-access-control/automation-03-select-role.png)  

3.  Kliknutím na **Přidat uživatele** otevřete okno **Přidání uživatelů**. Pokud jste přidali nějaké uživatele, skupiny nebo aplikace, aby spravovali vaše předplatné, budou tito uživatelé vypsáni a vy je budete moct vybrat a přidat jim přístup. Pokud žádné uživatele nevidíte nebo pokud požadovaný uživatel není uvedený, klikněte na **Pozvat** a otevřete okno **Pozvání hosta**, ve kterém můžete pozvat uživatele s platnou e-mailovou adresou k účtu Microsoft, například Outlook.com, OneDrive nebo Xbox Live ID. Po zadání e-mailové adresy uživatele klikněte na **Vybrat**, přidejte uživatele a potom klikněte na tlačítko **OK**. 

    ![Přidání uživatelů](media/automation-role-based-access-control/automation-04-add-users.png)  
 
Teď by se měl přidaný uživatel zobrazit v okně **Uživatelé** s přiřazenou rolí **Čtenář**.  

![Vypsání uživatelů](media/automation-role-based-access-control/automation-05-list-users.png)  

Roli můžete uživateli přiřadit také v okně **Role**. Klikněte v okně Uživatelé na **Role**, a otevřete tak okno **Role**. V tomto okně můžete vidět název role a počet uživatelů a skupin, kteří jsou k této roli přiřazení.

![Přiřazení role v okně Uživatelé](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
>[AZURE.NOTE] Řízení přístupu na základě role můžete nastavit jenom na úrovni účtu Automation, a ne u jakéhokoliv prostředku pod účtem Automation.

Uživateli, skupině nebo aplikaci můžete přiřadit více než jednu roli. Pokud uživateli přidáme například roli **Operátor automatizace** spolu s rolí **Čtenář**, takový uživatel může zobrazit všechny prostředky Automation a může také spouštět úlohy runbooku. Pokud chcete zobrazit role přiřazené uživateli, můžete rozbalit rozevírací seznam.  

![Zobrazení více rolí](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)  
 
### Odebrání uživatele

Pokud uživatel nespravuje účet Automation nebo už v organizaci nepracuje, můžete odebrat jeho oprávnění k přístupu. Uživatele můžete odebrat následujícím postupem: 

1.  V okně **Uživatelé** vyberte přiřazenou roli, kterou chcete odebrat.

2.  V okně podrobností přiřazení klikněte na tlačítko **Odebrat**.

3.  Kliknutím na **Ano** odebrání potvrďte. 

    ![Odebrání uživatelů](media/automation-role-based-access-control/automation-08-remove-users.png)  

## Uživatel přiřazený k roli

Když se uživatel přiřazený k roli přihlásí k účtu Automation, může vidět účet vlastníka uvedený v seznamu **Výchozí adresáře**. Aby mohl vidět účet Automation, ke kterému byl přidán, musí přepnout výchozí adresář na výchozí adresář vlastníka.  

![Výchozí adresář](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)  

### Uživatelské prostředí pro roli operátora služby Automation

Pokud uživatel, který je přiřazený k roli Operátor automatizace, zobrazí účet Automation, ke kterému je přiřazený, může zobrazit jenom seznam runbooků a úlohy a plány runbooků vytvořené v účtu Automation, ale nemůže zobrazit jejich definice. Může spustit, zastavit, pozastavit, obnovit nebo naplánovat úlohu runbooku. Uživatel nebude mít přístup k dalším prostředkům Automation, například ke konfiguracím, skupinám Hybrid Worker nebo uzlům DSC.  

![Žádný přístup k prostředkům](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

Když uživatel klikne na runbook, nebudou mu nabídnuty příkazy pro zobrazení nebo úpravu zdroje, protože role operátora služby Automation k nim nemá přístup.  

![Žádný přístup k úpravě runbooku](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)  

Uživatel bude mít přístup k zobrazení a vytváření plánů, ale nebudete mít přístup k žádným jiným typům assetů.  

![Žádný přístup k assetům](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)  

Tento uživatel také nemá přístup k zobrazení webhooků, které jsou přidružené k runbooku.

![Žádný přístup k webhookům](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## Konfigurace řízení přístupu na základě role u vašeho účtu Automation pomocí Azure PowerShellu

Přístup na základě role můžete pro účet Automation nakonfigurovat také pomocí následujících [rutin Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md).

• [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) vypíše všechny role, které jsou v RBAC služby Azure Active Directory dostupné. Tento příkaz můžete použít spolu s vlastností **Název** k vypsání uživatelů s konkrétní rolí.  
    **Příklad:**  
    ![Získání definice role](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)  

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) vypíše přiřazení rolí pro RBAC v zadaném rozsahu. Pokud nezadáte žádné parametry, tento příkaz vrátí všechna přiřazení rolí v rámci předplatného. K vypsání přiřazení přístupu konkrétního uživatele nebo skupin, kterých je uživatel členem, použijte parametr **ExpandPrincipalGroups**.  
    **Příklad:** K vypsání všech uživatelů a jejich rolí v rámci účtu Automation použijte následující příkaz.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Získat přiřazení role](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) slouží k udělení přístupu v určitém rozsahu pro uživatele, skupiny nebo aplikace.  
    **Příklad:** K vytvoření nové role „Operátor automatizace“ pro uživatele v rozsahu účtu Automation použijte následující příkaz.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Přiřazení nové role](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) použijte k odebrání přístupu v určitém rozsahu pro konkrétního uživatele, skupinu nebo aplikaci.
    **Příklad:** K vytvoření nové role „Operátor automatizace“ pro uživatele v rozsahu účtu Automation použijte následující příkaz.

    Remove-AzureRmRoleAssignment -SignInName "<sign-in Id of a user you wish to remove>" -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

Ve výše uvedených rutinách nahraďte přihlašovací jméno, ID předplatného, název skupiny prostředků a název účtu Automation údaji ze svého účtu. Po zobrazení výzvy k pokračování s odstraněním přiřazení role zvolte **Ano**.   


## Další kroky
-  Další informace o různých způsobech konfigurace RBAC pro Azure Automation najdete v článku [Správa řízení přístupu na základě role pomocí Azure PowerShellu](../active-directory/role-based-access-control-manage-access-powershell.md).
- Podrobnosti o různých způsobech spouštění runbooků najdete v článku [Spuštění runbooku](automation-starting-a-runbook.md)
- Informace o různých typech najdete v článku [Typy runbooků ve službě Azure Automation](automation-runbook-types.md).




<!--HONumber=Jun16_HO2-->


