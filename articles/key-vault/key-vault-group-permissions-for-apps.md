---
title: "Udělení oprávnění ke mnoho aplikací pro přístup Azure trezoru klíčů | Microsoft Docs"
description: "Zjistěte, jak chcete udělit oprávnění k mnoha aplikace pro přístup k trezoru klíčů"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: 14da9256def60d678ef5cae795fef1c373914b5c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a>Udělit oprávnění k mnoha aplikace pro přístup k trezoru klíčů

## <a name="q-i-have-several-over-16-applications-that-need-to-access-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a>Otázka: je nutné několik (více než 16) aplikace, které potřebují přístup k trezoru klíčů. Vzhledem k tomu, že Key Vault umožňuje jenom 16 položek řízení přístupu, co mám dělat?

Zásada řízení přístupu Key Vault podporuje pouze 16 položky. Ale můžete vytvořit skupiny zabezpečení služby Azure Active Directory. Přidejte všechny objekty přidružené služby do této skupiny zabezpečení a pak udělují přístup k této skupině zabezpečení Key Vault.

Tady jsou požadavky:
* [Instalace modulu Azure Active Directory V2 PowerShell](https://www.powershellgallery.com/packages/AzureAD).
* [Nainstalujte prostředí Azure PowerShell](/powershell/azure/overview).
* Pokud chcete spustit následující příkazy, potřebujete oprávnění k vytvoření nebo úpravě skupin v klientovi služby Azure Active Directory. Pokud nemáte oprávnění, budete muset obraťte se na správce služby Azure Active Directory.

Nyní spusťte následující příkazy v prostředí PowerShell.

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionToKeys all –PermissionToSecrets all –PermissionToCertificates all 
 
# Of course you can adjust the permissions as required 
```

Pokud potřebujete udělit jinou sadu oprávnění pro skupinu aplikací, vytvořte samostatnou skupinu zabezpečení služby Azure Active Directory pro tyto aplikace.

## <a name="next-steps"></a>Další kroky

Další informace o tom, jak [zabezpečit váš trezor klíčů](key-vault-secure-your-key-vault.md).
