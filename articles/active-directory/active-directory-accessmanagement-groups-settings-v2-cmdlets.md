---
title: "Příklady prostředí PowerShell pro správu skupin v Azure Active Directory | Microsoft Docs"
description: "Tato stránka obsahuje příklady prostředí PowerShell, které vám pomohou spravovat vaše skupiny ve službě Azure Active Directory"
keywords: "Azure AD, Azure Active Directory, prostředí PowerShell, správu skupin, skupiny"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7a5023dc-2727-4c25-8254-b531fc3244ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: 5cad44dc7bf415002b3c9872fffdcf0d54bb6ad6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Verze 2 rutiny služby Azure Active Directory pro správu skupin
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Tento článek obsahuje příklady, jak pomocí prostředí PowerShell ke správě skupin v Azure Active Directory (Azure AD).  Je také vysvětluje, jak získat nastavit modulu Azure AD PowerShell. Nejdřív musíte [stáhnout modulu Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="installing-the-azure-ad-powershell-module"></a>Instalace modulu Azure AD PowerShell
K instalaci modulu Azure AD PowerShell, použijte následující příkazy:

    PS C:\Windows\system32> install-module azuread

Pokud chcete ověřit, zda byl nainstalován modul, použijte následující příkaz:

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

Teď můžete začít používat rutiny v modulu. Úplný popis rutin v modulu Azure AD, naleznete v dokumentaci online referencí pro [Azure Active Directory PowerShell verze 2](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="connecting-to-the-directory"></a>Připojení k adresáři
Předtím, než můžete začít spravovat skupiny pomocí rutin prostředí Azure AD PowerShell, je nutné připojit relace prostředí PowerShell k adresáři, který chcete spravovat. Použijte následující příkaz:

    PS C:\Windows\system32> Connect-AzureAD

Rutina vás vyzve k zadání přihlašovacích údajů, které chcete použít pro přístup k adresáři. V tomto příkladu používáme karen@drumkit.onmicrosoft.com pro přístup k adresáři demonstrační. Rutina vrátí potvrzení jestli že relace byl úspěšně připojen do vašeho adresáře:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Teď můžete začít používat rutiny AzureAD ke správě skupin ve vašem adresáři.

## <a name="retrieving-groups"></a>Načítání skupin
Načíst existující skupiny z adresáře můžete použít rutinu Get-AzureADGroups. Pokud chcete načíst všechny skupiny v adresáři, použijte rutinu bez parametrů:

    PS C:\Windows\system32> get-azureadgroup

Rutina vrátí všechny skupiny v adresáři připojené.

Pomocí parametru - objectID načíst konkrétní skupinu, pro který určíte objectID skupiny:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Rutina vrátí teď skupiny, jejichž objectID odpovídá hodnotě parametru, které jste zadali:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Můžete vyhledat konkrétní skupinu pomocí parametru - filtru. Tento parametr přebere klauzuli filtru ODATA a vrátí všechny skupiny, které odpovídají filtru, jako v následujícím příkladu:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

> [!NOTE] 
> Rutiny prostředí AzureAD PowerShell implementovat standard dotazu OData. Další informace najdete v tématu **$filter** v [možností dotazu systému OData pomocí koncový bod OData](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Vytváření skupin
Pokud chcete vytvořit novou skupinu v adresáři, použijte rutinu New-AzureADGroup. Tato rutina vytvoří novou skupinu zabezpečení s názvem "Marketing":

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Aktualizace skupiny
Pokud chcete aktualizovat existující skupiny, použijte rutinu Set-AzureADGroup. V tomto příkladu jsme změnit vlastnost DisplayName skupiny "Správci Intune." Nejprve se hledání skupiny pomocí rutiny Get-AzureADGroup a filtrovat pomocí atributu DisplayName:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

V dalším kroku jsme změnit vlastnosti Popis na novou hodnotu "Správci Intune zařízení":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Nyní když jsme najít skupinu vidíme, že je vlastnost Popis aktualizovat tak, aby odrážely novou hodnotu:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Odstranění skupiny
Pokud chcete odstranit skupiny z adresáře, použijte rutinu Remove-AzureADGroup následujícím způsobem:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Správa členů skupin
Pokud potřebujete přidat nové členy do skupiny, použijte rutinu Add-AzureADGroupMember. Tento příkaz přidá členem skupiny Intune Administrators, které jsme použili v předchozím příkladu:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Parametr - ObjectId ObjectID skupinu, do které jsme chcete člena přidat, a - RefObjectId je ObjectID uživatele, kterého chcete přidat jako člena do skupiny.

Získat stávající členy skupiny, použijte rutinu Get-AzureADGroupMember jako v následujícím příkladu:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Odebrat člen, který jsme dříve přidány do skupiny, použijte rutinu Remove-AzureADGroupMember, jak je znázorněno zde:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Pokud chcete ověřit membership(s) skupiny uživatele, použijte rutinu AzureADGroupIdsUserIsMemberOf vyberte. Tato rutina se používá jako jeho parametry ObjectId uživatele, pro kterého chcete zkontrolovat členství ve skupinách a seznamu skupin, pro které chcete zkontrolovat členství. Seznam skupin musí být zadaný ve formě proměnné komplexního typu "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck", proto jsme nejprve musíte vytvořit proměnné typu:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

V dalším kroku poskytujeme hodnoty pro identifikátory skupiny do atribut "Identifikátory skupiny" Tato proměnná komplexní se změnami:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Nyní Pokud nám chcete zkontrolovat členství ve skupinách uživatele s ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea proti skupin v $g, bychom měli použít:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Hodnota vrácená je seznam skupin, které je tento uživatel členem. Můžete taky použít tuto metodu za účelem zkontrolujte kontakty, skupiny nebo objekty služby členství pro daný seznam skupin, pomocí vyberte-AzureADGroupIdsContactIsMemberOf, vyberte AzureADGroupIdsGroupIsMemberOf nebo Vyberte AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="managing-owners-of-groups"></a>Správa vlastníků skupiny
Vlastníci přidat do skupiny, použijte rutinu Add-AzureADGroupOwner:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Parametr - ObjectId je ObjectID skupiny, do které chceme přidat vlastníka a - RefObjectId je ObjectID uživatele, kterého chcete přidat jako vlastníka skupiny.

Pro načtení vlastníků skupiny, použijte rutinu Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Rutina vrátí seznam vlastníků pro zadanou skupinu:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Pokud chcete odebrat vlastníka ze skupiny, použijte rutinu Remove-AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a>Vyhrazené aliasy 
Pokud skupinu je vytvořen, určité koncové body umožňují koncovému uživateli umožňují zadat mailNickname nebo alias, který se má použít jako součást e-mailovou adresu skupiny. Skupiny s následující aliasy vysoce privilegované e-mailu lze vytvořit pouze globální správce Azure AD. 
  
* zneužití 
* Správce 
* Správce 
* hostmaster 
* majordomo 
* správce pošty 
* kořenové 
* Zabezpečení 
* Zabezpečení 
* Správce protokolu SSL 
* správce webového serveru 

## <a name="next-steps"></a>Další kroky
Můžete najít další dokumentaci k Azure Active Directory PowerShell na [rutiny služby Azure Active Directory](/powershell/azure/install-adv2?view=azureadps-2.0).

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
