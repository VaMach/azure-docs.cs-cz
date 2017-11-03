---
title: "Povolit rozhraní příkazového řádku Azure pro uživatele Azure zásobníku | Microsoft Docs"
description: "Naučte se používat rozhraní příkazového řádku (CLI) a platformy pro správu a nasazení prostředků v Azure zásobníku"
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: sngun
ms.openlocfilehash: d854c106fbce7e3f01c2878bb9828bdffa4d42a5
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Povolit rozhraní příkazového řádku Azure pro uživatele Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Nejsou k dispozici žádné úlohy specifické pro operátor zásobník Azure, které můžete provádět pomocí rozhraní příkazového řádku Azure. Ale předtím, než mohou uživatelé spravovat prostředky přes rozhraní příkazového řádku, operátory zásobník Azure je nutné zadat s následujícími službami:

* **Certifikát kořenové certifikační Autority zásobník Azure** je povinný, pokud uživatelé používají rozhraní příkazového řádku z pracovní stanice mimo Azure zásobníku Development Kit.  

* **Koncový bod virtuálního počítače aliasy** poskytuje alias, jako je "UbuntuLTS" nebo "Win2012Datacenter", který odkazuje na image vydavatele, nabídky, SKU a verzi jako jeden parametr při nasazování virtuálních počítačů.  

Následující části popisují, jak získat tyto hodnoty.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Exportujte certifikát kořenové certifikační Autority Azure zásobníku

Certifikát kořenové certifikační Autority zásobník Azure je k dispozici v sadě pro vývoj a na virtuálním počítači klienta, který běží v rámci vývojového prostředí sady. Pokud chcete exportovat certifikát od kořenové zásobník Azure ve formátu PEM, přihlaste se k vaší development kit nebo klientský virtuální počítač a spusťte následující skript:

```powershell
$label = "AzureStackSelfSignedRootCert"
Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
$root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
if (-not $root)
{
    Log-Error "Certificate with subject CN=$label not found"
    return
}

Write-Host "Exporting certificate"
Export-Certificate -Type CERT -FilePath root.cer -Cert $root

Write-Host "Converting certificate to PEM format"
certutil -encode root.cer root.pem
```

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Nastavit koncový bod aliasy virtuálního počítače

Azure zásobníku operátory měli nastavit veřejně přístupném koncovém bodu, který je hostitelem alias soubor virtuálního počítače. Alias soubor virtuálního počítače je soubor JSON, který poskytuje běžný název pro bitovou kopii. Tento název je zadán následně při nasazení virtuálního počítače jako parametr příkazového řádku Azure CLI.  

Předtím, než přidáte položku do souboru alias, ujistěte se, že jste [stažení bitové kopie z Azure Marketplace](azure-stack-download-azure-marketplace-item.md), nebo mají [publikovat svoji vlastní image](azure-stack-add-vm-image.md). Pokud publikujete vlastní image, poznamenejte si vydavatele, nabídky, SKU a verzi informace, které jste zadali během publikování. Pokud je image z marketplace, můžete zobrazit informace pomocí ```Get-AzureVMImage``` rutiny.  
   
A [ukázkový soubor alias](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) mnoho běžných Image aliasy je k dispozici. Který můžete použít jako počáteční bod. Tento soubor v prostoru vaši klienti rozhraní příkazového řádku kde dosáhnout ho uložit. Jedním ze způsobů je hostitelem souboru v účtu úložiště blob a sdílet adresu URL s uživateli:

1. Stažení [ukázkový soubor](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) z Githubu.
2. Vytvořte nový účet úložiště v Azure zásobníku. Po dokončení vytvořte nový kontejner objektů blob. Nastavit zásady přístupu k "veřejná".  
3. Nahrání souboru JSON do nového kontejneru. Po dokončení můžete zobrazit adresu URL objektu blob klepnutím na název objektu blob a potom výběrem adresu URL z vlastností objektu blob.


## <a name="next-steps"></a>Další kroky

[Nasazení šablon pomocí rozhraní příkazového řádku Azure](azure-stack-deploy-template-command-line.md)

[Připojení přes PowerShell](azure-stack-connect-powershell.md)

[Správa uživatelských oprávnění](azure-stack-manage-permissions.md)

