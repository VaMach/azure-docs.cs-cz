---
title: "Stažení nástroje Azure zásobníku z Githubu | Microsoft Docs"
description: "Zjistěte, jak si chcete stáhnout nástroje, které jsou požadovány pro práci s Azure zásobníku."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: sngun
ms.openlocfilehash: c0a4f337c055f4b62d986e2a3c3ce7b962aceae9
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/21/2017
---
# <a name="download-azure-stack-tools-from-github"></a>Stažení nástroje Azure zásobníku z Githubu

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

**Nástroje AzureStack** je úložiště GitHub, který je hostitelem moduly Powershellu pro správu a nasazení prostředků do protokolů Azure. Pokud máte v úmyslu navázat připojení VPN, můžete tyto moduly Powershellu stáhnout Development Kit zásobník Azure nebo na externí klienta se systémem Windows. Pokud chcete získat tyto nástroje, naklonujte úložiště GitHub nebo stáhnout **AzureStack nástroje** složky. 

Klonovat úložiště, stáhněte si [Git pro Windows](https://git-scm.com/download/win), otevřete příkazový řádek a spusťte následující skript:

```PowerShell
# Change directory to the root directory. 
cd \

# Clone the repository.
git clone https://github.com/Azure/AzureStack-Tools.git --recursive

# Change to the tools directory.
cd AzureStack-Tools
```

Pokud chcete stáhnout nástroje pro složku, spusťte následující skript:

```PowerShell
# Change directory to the root directory. 
cd \

# Download the tools archive.
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
cd AzureStack-Tools-master

```

## <a name="functionality-provided-by-the-modules"></a>Funkce poskytované službou moduly

**AzureStack nástroje** úložiště obsahuje moduly Powershellu, které podporují následující funkce pro Azure zásobníku:  

| Funkce | Popis | Tento modul, který můžete použít? |
| --- | --- | --- |
| [Možností cloudu](user/azure-stack-validate-templates.md) | Tento modul slouží k získání možností cloudu cloudu. Například můžete pomocí tohoto modulu získat cloudové funkce jako je verze rozhraní API a prostředky Azure Resource Manager. Můžete také získat rozšíření virtuálního počítače pro zásobník Azure a Azure cloudy pomocí tohoto modulu. | Operátoři cloudů a uživatelé |
| [Správa výpočtů Azure zásobníku](azure-stack-add-vm-image.md) | Tento modul slouží k přidání nebo odebrání image virtuálního počítače v zásobníku Azure Marketplace. | Operátoři cloudu |
| [Správa infrastruktury Azure zásobníku](https://github.com/Azure/AzureStack-Tools/blob/master/Infrastructure/README.md) | Tento modul slouží ke správě infrastruktury Azure zásobníku virtuálních počítačů, výstrahy, aktualizace a tak dále. |  Operátoři cloudu|
| [Správce prostředků zásady pro Azure zásobníku](user/azure-stack-policy-module.md) | Tento modul slouží ke konfiguraci předplatné Azure nebo skupinu prostředků Azure pomocí stejné verze a služba dostupnosti jako zásobník Azure. | Operátoři cloudů a uživatelé |
| [Zaregistrovat Azure](azure-stack-register.md) | Tento modul slouží k registraci instance kit vývoj s Azure. Po registraci, můžete stáhnout položky marketplace z Azure a použít je v Azure zásobníku. | Operátoři cloudu |
| [Nasazení Azure zásobníku](azure-stack-run-powershell-script.md) | Tento modul slouží k přípravě hostitelský počítač zásobník Azure k nasazení a znovu nasaďte pomocí bitové kopie virtuálního pevného disku (VHD) Azure zásobníku. | Operátoři cloudu|
| [Připojení k Azure zásobníku](azure-stack-connect-powershell.md) | Tento modul slouží k připojení k instanci zásobník Azure pomocí prostředí PowerShell a ke konfiguraci připojení VPN do protokolů Azure. | Operátoři cloudů a uživatelé |
| [Správa služby Azure zásobníku](azure-stack-create-offer.md) | Tento modul slouží k vytvoření na výchozí klienta nabídku s neomezená kvóty napříč výpočty, úložiště Azure, sítě a služby Key Vault.   | Operátoři cloudu|
| [Ověření šablony](user/azure-stack-validate-templates.md) | Tento modul slouží k ověření, pokud existující nebo nové šablony lze nasadit do protokolů Azure. | Operátoři cloudů a uživatelé|


## <a name="next-steps"></a>Další kroky
* [Konfigurace prostředí PowerShell Azure zásobník uživatele](user/azure-stack-powershell-configure-user.md)   
* [Připojení k Azure zásobníku Development Kit přes síť VPN](azure-stack-connect-azure-stack.md)  
