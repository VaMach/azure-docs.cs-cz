<properties
    pageTitle="Azure AD Domain Services: Povolení synchronizace hesel | Microsoft Azure"
    description="Začínáme se službou Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/06/2016"
    ms.author="maheshu"/>

# Azure AD Domain Services *(Preview)* – Povolení synchronizace hesel do služby Azure AD Domain Services

## Úloha 5: Povolení synchronizace hesel do služby AAD Domain Services u synchronizovaného klienta služby Azure AD
Jakmile jste u svého adresáře Azure AD povolili službu Azure AD Domain Services, dalším krokem je povolit synchronizaci hesel do služby Azure AD Domain Services. To uživatelům umožní přihlásit se k doméně pomocí podnikových přihlašovacích údajů.

Potřebný postup se liší podle toho, jestli má vaše organizace výhradně cloudový adresář Azure AD, nebo jestli má nastavenou synchronizaci s místním adresářem pomocí služby Azure AD Connect.

<br>

> [AZURE.SELECTOR]
- [Výhradně cloudový adresář Azure AD](active-directory-ds-getting-started-password-sync.md)
- [Synchronizovaný adresář Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>

### Synchronizovaní klienti – Povolení synchronizace hodnot hash přihlašovacích údajů protokolů NTLM a Kerberos do služby Azure AD
Pokud má klient služby Azure AD vaší organizace nastavenou synchronizaci s místním adresářem pomocí služby Azure AD Connect, službu Azure AD Connect bude nutné nakonfigurovat tak, aby synchronizovala hodnoty hash přihlašovacích údajů potřebných k ověřování pomocí protokolů NTLM a Kerberos. Ve výchozím nastavení tyto hodnoty hash nejsou do služby Azure AD synchronizovány a následující postup vám umožní povolit synchronizaci hodnot hash do klienta služby Azure AD.

#### Instalace nebo aktualizace služby Azure AD Connect

Bude nutné nainstalovat nejnovější doporučenou verzi služby Azure AD Connect na počítač připojený k doméně. Pokud máte existující instanci instalace služby Azure AD Connect, bude nutné ji aktualizovat tak, aby používala sestavení Azure AD Connect GA. Ujistěte se, že používáte nejnovější verzi Azure AD Connect – předejdete tak známým problémům nebo chybám, které už pravděpodobně byly opraveny.

**[Stažení služby Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**

Doporučená verze: **1.1.189.0** – publikováno 3. června 2016

  > [AZURE.WARNING] Pokud chcete povolit synchronizaci starší verze hesla (nutné pro ověřování protokolů NTLM a Kerberos) do klienta služby Azure AD, JE NUTNÉ nainstalovat nejnovější doporučenou verzi služby Azure AD Connect. V předchozích verzích služby Azure AD Connect ani u starší verze nástroje DirSync není tato funkce dostupná.

Pokyny k instalaci služby Azure AD Connect jsou dostupné v následujícím článku – [Začínáme se službou Azure AD Connect](../active-directory/active-directory-aadconnect.md).


#### Vynucení úplné synchronizace hesel do služby Azure AD

Chcete-li vynutit úplnou synchronizaci a povolit synchronizaci hodnot hash hesel všech místních uživatelů (včetně hodnot hash přihlašovacích údajů nezbytných k ověřování pomocí protokolu NTLM nebo Kerberos) do vašeho klienta služby Azure AD, spusťte v každé doménové struktuře služby AD následující skript prostředí PowerShell.

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

Synchronizace přihlašovacích údajů do služby Azure AD bude v závislosti na velikosti adresáře (počtu uživatelů, skupin atd.) chvíli trvat. Hesla bude možné použít ve spravované doméně služby Azure AD Domain Services zanedlouho poté, co se hodnoty hash přihlašovacích údajů synchronizují do služby Azure AD.


<br>

## Související obsah

- [Povolení synchronizace hesel do služby AAD Domain Services u výhradně cloudového adresáře služby Azure AD](active-directory-ds-getting-started-password-sync.md)

- [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)

- [Připojení virtuálního počítače s Windows k spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Připojení virtuálního počítače se systémem Red Hat Enterprise Linux k spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)



<!---HONumber=Aug16_HO4-->


