---
title: "Azure AD Domain Services: Povolení synchronizace hesel | Dokumentace Microsoftu"
description: "Začínáme se službou Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8731f2b2-661c-4f3d-adba-2c9e06344537
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/20/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 9c43a831768684c6458d5f62557c8a06b2c5ca6b


---
# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Povolení synchronizace hesel do služby Azure AD Domain Services
V předchozích úlohách jste pro svého tenanta Azure AD povolili službu Azure AD Domain Service. Dalším úkolem je povolení synchronizace hesel do služby Azure AD Domain Services. Jakmile je synchronizace přihlašovacích údajů nastavená, uživatelé se do spravované domény mohou přihlásit pomocí podnikových přihlašovacích údajů.

Potřebný postup se liší podle toho, jestli má vaše organizace výhradně cloudového tenanta Azure AD, nebo jestli má nastavenou synchronizaci s místním adresářem pomocí služby Azure AD Connect.

<br>

> [!div class="op_single_selector"]
> * [Výhradně cloudový tenant Azure AD](active-directory-ds-getting-started-password-sync.md)
> * [Synchronizovaný tenant Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)
> 
> 

<br>

## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Úloha 5: Povolení synchronizace hesel do služby AAD Domain Services u synchronizovaného tenanta služby Azure AD
Synchronizovaný tenant služby Azure AD je nastavený na synchronizaci s místním adresářem vaší organizace pomocí služby Azure AD Connect. Azure AD Connect ve výchozím nastavení do služby Azure AD nesynchronizuje hodnoty hash přihlašovacích údajů protokolů NTLM a Kerberos. Chcete-li použít službu Azure AD Domain Services, je potřeba nakonfigurovat službu Azure AD Connect na synchronizaci hodnot hash přihlašovacích údajů požadovaných pro ověřování protokolů NTLM a Kerberos. Následujícím postupem povolíte synchronizaci požadovaných hodnot hash přihlašovacích údajů do svého tenanta služby Azure AD.

### <a name="install-or-update-azure-ad-connect"></a>Instalace nebo aktualizace služby Azure AD Connect
Na počítač připojený k doméně nainstalujte nejnovější doporučenou verzi služby Azure AD Connect. Máte-li existující instanci instalace služby Azure AD Connect, je nutné ji aktualizovat tak, aby používala nejnovější verzi služby Azure AD Connect. Ujistěte se, že vždy používáte nejnovější verzi služby Azure AD Connect – předejdete tak známým problémům nebo chybám, které už pravděpodobně byly opraveny.

**[Stažení služby Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**

Doporučená verze: **1.1.281.0** – publikováno 7. září 2016.

> [!WARNING]
> Chcete-li povolit synchronizaci starších verzí přihlašovacích hesel (požadovaných pro ověřování protokolů NTLM a Kerberos) do svého tenanta služby Azure AD, JE NUTNÉ nainstalovat nejnovější doporučenou verzi služby Azure AD Connect. V předchozích verzích služby Azure AD Connect ani u starší verze nástroje DirSync není tato funkce dostupná.
> 
> 

Pokyny k instalaci služby Azure AD Connect jsou dostupné v následujícím článku – [Začínáme se službou Azure AD Connect](../active-directory/active-directory-aadconnect.md).

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Povolení synchronizace hodnot hash přihlašovacích údajů protokolů NTLM a Kerberos do služby Azure AD
Chcete-li vynutit úplnou synchronizaci hesel a povolit synchronizaci hodnot hash přihlašovacích údajů všech místních uživatelů do svého tenanta služby Azure AD, spusťte následující skript prostředí PowerShell v každé doménové struktuře služby AD. Tímto skriptem povolíte synchronizaci hodnot hash přihlašovacích údajů požadovaných pro ověřování protokolů NTLM a Kerberos do svého tenanta služby Azure AD.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Synchronizace hodnot hash přihlašovacích údajů do služby Azure AD bude v závislosti na velikosti adresáře (počtu uživatelů, skupin atd.) chvíli trvat. Hesla bude možné použít ve spravované doméně služby Azure AD Domain Services zanedlouho poté, co se hodnoty hash přihlašovacích údajů synchronizují do služby Azure AD.

<br>

## <a name="related-content"></a>Související obsah
* [Povolení synchronizace hesel do služby AAD Domain Services u výhradně cloudového adresáře služby Azure AD](active-directory-ds-getting-started-password-sync.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Připojení virtuálního počítače s Windows k spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Připojení virtuálního počítače se systémem Red Hat Enterprise Linux k spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)




<!--HONumber=Dec16_HO1-->


