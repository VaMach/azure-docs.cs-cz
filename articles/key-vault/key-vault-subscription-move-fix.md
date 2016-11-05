---
title: Změna ID tenanta trezoru klíčů po přesunu předplatného | Microsoft Docs
description: Zjistěte, jak přepnout ID tenanta pro trezor klíčů po přesunu předplatného na jiného tenanta
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager

ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/13/2016
ms.author: ambapat

---
# Změna ID tenanta trezoru klíčů po přesunu předplatného
### Otázka: Moje předplatné bylo přesunuto z tenanta A na tenanta B. Jak změním ID tenanta pro stávající trezor klíčů a nastavím správné seznamy ACL pro objekty zabezpečení v tenantu B?
Když v rámci předplatného vytvoříte nový trezor klíčů, je automaticky vázán na výchozí ID tenanta služby Azure Active Directory pro dané předplatné. Zároveň jsou k tomuto ID tenanta vázány i všechny položky zásad přístupu. Když přesunete předplatné Azure z tenanta A na tenanta B, stávající trezory klíčů budou pro objekty zabezpečení (uživatelé a aplikace) v tenantu B nepřístupné. Chcete-li tento problém vyřešit, je třeba

* změnit ID tenanta přidružené ke všem stávajícím trezorům klíčů v tomto předplatném na tenanta B,
* odebrat všechny stávající položky zásad přístupu,
* přidat nové položky zásad přístupu, které jsou přidružené k tenantu B.

Například pokud máte trezor klíčů s názvem „muj_trezor“ v předplatném, které bylo přesunuto z tenanta A na tenanta B, následujícím postupem změníte ID tenanta pro tento trezor klíčů a odeberete staré zásady přístupu.

<pre>
$vaultResourceId = (Get-AzureRmKeyVault -VaultName muj_trezor).ResourceId $vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties $vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId $vault.Properties.AccessPolicies = @() Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Protože byl tento trezor před přesunem v tenantu A, původní hodnota proměnné **$vault.Properties.TenantId** je tenant A, zatímco **(Get-AzureRmContext).Tenant.TenantId** je tenant B.

Nyní, když je trezor přiřazen ke správnému ID tenanta a staré položky zásad přístupu jsou odebrány, nastavte nové položky zásad přístupu pomocí [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## Další kroky
* Máte-li dotazy ke službě Key Vault, navštivte [fóra Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

<!--HONumber=Sep16_HO3-->


