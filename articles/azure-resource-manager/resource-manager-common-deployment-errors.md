---
title: "Odstraňování běžných chyb nasazení Azure | Microsoft Docs"
description: "Popisuje postup řešení běžných chyb při nasazování prostředků do Azure pomocí Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Chyba nasazení, nasazení azure nasazení do azure"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: tomfitz
ms.openlocfilehash: d6dc0ffac1a3e688f93af903f51f798270b63711
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru

Toto téma popisuje některé běžné chyby nasazení Azure můžete setkat a obsahuje informace, které případné chyby opravte. Pokud nemůžete najít kód chyby pro vaše nasazení chyby, přečtěte si téma [najít kód chyby](#find-error-code).

## <a name="error-codes"></a>Kódy chyb

| Kód chyby | Omezení rizik | Další informace |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Postupujte podle omezení pro pojmenovávání pro účty úložiště. | [Přeložit název účtu úložiště](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | Zkontrolujte vlastnosti účtu úložiště k dispozici. | [storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AnotherOperationInProgress | Počkejte na dokončení souběžná operace. | |
| AuthorizationFailed | Váš účet nebo instanční objekt nemá dostatečný přístup k dokončení nasazení. Zkontrolujte roli, kterou váš účet patří do a přístup pro obor nasazení. | [Řízení přístupu Azure na základě rolí](../active-directory/role-based-access-control-configure.md) |
| Struktura BadRequest | Jste poslali hodnot nasazení, které neodpovídají očekávané pomocí Správce prostředků. Zkontrolujte zprávy vnitřní stav a nápovědu k řešení potíží. | [Odkaz na šablonu](/azure/templates/) a [podporované umístění](resource-manager-template-location.md) |
| Konflikt | Požadujete operace, který není povolen v aktuálním stavu prostředku. Například změna velikosti disku je povolená pouze při vytváření virtuálního počítače, nebo když je virtuální počítač navrácený. | |
| DeploymentActive | Počkejte, než pro souběžné nasazení do této skupiny prostředků pro dokončení. | |
| DnsRecordInUse | Název záznamu DNS musí být jedinečný. Buď zadejte jiný název, nebo upravte stávající záznamu. | |
| ImageNotFound | Zkontrolujte nastavení bitové kopie virtuálního počítače. | [Řešení potíží s obrázky Linux](../virtual-machines/linux/troubleshoot-deployment-new-vm.md) a [Image pro řešení potíží s Windows](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) |
| InUseSubnetCannotBeDeleted | Této chybě může dojít při pokusu o aktualizaci prostředek, ale je požadavek zpracován odstranit a vytvoření prostředku. Nezapomeňte zadat všechny hodnoty beze změny. | [Aktualizace prostředku](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Získání tokenu přístupu pro příslušné klienta. Pouze můžete získat token z klienta, který váš účet patří do. | |
| InvalidContentLink | Pokusili jste s největší pravděpodobností propojení vnořené šablony, která není k dispozici. Překontrolujte identifikátor URI, který jste zadali pro vnořené šablonu. Pokud šablona již existuje v účtu úložiště, zkontrolujte, zda že identifikátor URI je přístupný. Potřebujete předat SAS token. | [Propojených šablon](resource-group-linked-templates.md) |
| InvalidParameter | Jedna z hodnot, které jste zadali pro zdroj neodpovídá očekávané hodnotě. Tato chyba může způsobit z mnoha různých podmínkách. Například může být nedostatek heslo nebo název objektu blob může být nesprávný. Zkontrolujte chybové zprávy k určení, která hodnota je třeba opravit. | |
| InvalidRequestContent | Vaše nasazení hodnoty buď patří hodnoty, které nejsou očekávané nebo chybí požadované hodnoty. Zkontrolujte hodnoty pro váš typ prostředku. | [Odkaz na šablonu](/azure/templates/) |
| InvalidRequestFormat | Povolit protokolování ladění při provádění nasazení a ověřte obsah žádosti. | [Ladění protokolování](resource-manager-troubleshoot-tips.md#enable-debug-logging) |
| InvalidResourceNamespace | Zkontrolujte obor názvů prostředků, které jste zadali v **typ** vlastnost. | [Odkaz na šablonu](/azure/templates/) |
| InvalidResourceReference | Prostředek ještě neexistuje nebo je nesprávně odkazuje. Zkontrolujte, jestli je potřeba přidat závislost. Ověřte, že používání **odkaz** požadované parametry pro váš scénář zahrnuje funkce. | [Vyřešte závislosti](resource-manager-not-found-errors.md) |
| InvalidResourceType | Zadejte kontrola prostředku, kterou jste zadali v **typ** vlastnost. | [Odkaz na šablonu](/azure/templates/) |
| InvalidTemplate | Zkontrolujte syntaxi šablony chyb. | [Neplatná šablona řešení](resource-manager-invalid-template-errors.md) |
| LinkedAuthorizationFailed | Zkontrolujte, pokud váš účet patří do stejné klienta jako pro skupinu prostředků, které nasazujete. | |
| LinkedInvalidPropertyId | ID prostředku pro prostředek není správně řešení. Zkontrolujte, že poskytujete všechny požadované hodnoty pro ID prostředku, včetně ID odběru, název skupiny prostředků, typ prostředku, název nadřazené prostředků (v případě potřeby) a název prostředku. | |
| LocationRequired | Zadejte umístění prostředku. | [Nastavení umístění](resource-manager-template-location.md) |
| MissingRegistrationForLocation | Zkontrolujte stav registrace poskytovatele prostředků a podporovaných umístění. | [Vyřešte registrace](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | Zaregistrujte své předplatné s poskytovatelem prostředků. | [Vyřešte registrace](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | Zkontrolujte stav registrace poskytovatele prostředků. | [Vyřešte registrace](resource-manager-register-provider-errors.md) |
| notFound | Může se pokoušíte nasadit závislý prostředek paralelně s nadřazený prostředek. Zkontrolujte, pokud potřebujete přidat závislost. | [Vyřešte závislosti](resource-manager-not-found-errors.md) |
| OperationNotAllowed | Nasazení se pokouší operace, která překračuje kvótu pro předplatné, skupinu prostředků nebo oblasti. Pokud je to možné zkontrolovat, jestli vaše nasazení zůstane v rámci kvóty. Zvažte, jinak hodnota požadavku na změnu vaší kvóty. | [Vyřešte kvóty](resource-manager-quota-errors.md) |
| ParentResourceNotFound | Ujistěte se, že nadřazený prostředek existuje před vytvořením podřízené prostředky. | [Vyřešte nadřazený prostředek](resource-manager-parent-resource-errors.md) |
| PrivateIPAddressInReservedRange | Zadaná IP adresa obsahuje rozsah adres, vyžaduje Azure. Změna IP adresy, aby se zabránilo vyhrazený rozsah. | [IP adresy](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | Zadaná IP adresa je mimo rozsah podsítě. Změnit IP adresu, která spadá do rozsahu podsítě. | [IP adresy](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PropertyChangeNotAllowed | Některé vlastnosti nelze změnit na nasazené prostředku. Při aktualizaci prostředku, omezte vaše změny povolených vlastností. | [Aktualizace prostředku](/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | Vaše předplatné zahrnuje zásady prostředků, které brání akce, které se pokoušíte provést během nasazení. Najít zásady, které blokuje akce. Pokud je to možné změňte nasazení tak, aby splňovaly omezení ze zásad. | [Vyřešte zásady](resource-manager-policy-requestdisallowedbypolicy-error.md) |
| ResourceGroupBeingDeleted | Počkejte na dokončení odstranění. | |
| ResourceGroupNotFound | Zkontrolujte název cílové skupiny prostředků pro nasazení. Již musí existovat v rámci vašeho předplatného. Zkontrolujte kontext vašeho předplatného. | [Rozhraní příkazového řádku Azure](/cli/azure/account?#az_account_set) [prostředí PowerShell](/powershell/module/azurerm.profile/set-azurermcontext) |
| ResourceNotFound | Nasazení odkazuje na prostředek, který nelze vyřešit. Ověřte, že používání **odkaz** parametrů požadovaných pro váš scénář zahrnuje funkce. | [Překlad odkazů](resource-manager-not-found-errors.md) |
| ResourceQuotaExceeded | Nasazení se pokouší vytvořit prostředky, které překročí kvótu pro předplatné, skupinu prostředků nebo oblasti. Pokud je to možné upraveno infrastrukturu tak, aby zůstat v rámci kvóty. Zvažte, jinak hodnota požadavku na změnu vaší kvóty. | [Vyřešte kvóty](resource-manager-quota-errors.md) |
| SkuNotAvailable | Vyberte SKU (například velikost virtuálního počítače), která je k dispozici pro umístění, které jste vybrali. | [Vyřešte SKU](resource-manager-sku-not-available-errors.md) |
| StorageAccountAlreadyExists | Zadejte jedinečný název pro účet úložiště. | [Přeložit název účtu úložiště](resource-manager-storage-account-name-errors.md)  |
| StorageAccountAlreadyTaken | Zadejte jedinečný název pro účet úložiště. | [Přeložit název účtu úložiště](resource-manager-storage-account-name-errors.md) |
| StorageAccountNotFound | Zkontrolujte předplatné, skupinu prostředků a název účtu úložiště, ke které chcete použít. | |
| SubnetsNotInSameVnet | Virtuální počítač může mít pouze jednu virtuální síť. Pokud nasazujete víc síťových karet, ujistěte se, že patří do stejné virtuální síti. | [Několik síťových adaptérů](../virtual-machines/windows/multiple-nics.md) |

## <a name="find-error-code"></a>Najít kód chyby

Když dojde k chybě během nasazení, Resource Manager vrátí kód chyby. Zobrazí se chybová zpráva prostřednictvím portálu, prostředí PowerShell nebo rozhraní příkazového řádku Azure. Vnější chybová zpráva může být příliš obecné řešení potíží. Podívejte se na vnitřní zprávu, která obsahuje podrobné informace o této chybě. Další informace najdete v tématu [zjistit kód chyby](resource-manager-troubleshoot-tips.md#determine-error-code).

## <a name="next-steps"></a>Další kroky
* Další informace o auditování akce najdete v tématu [auditovat operace s Resource Managerem](resource-group-audit.md).
* Další informace o akce k určení chyby během nasazení najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).
