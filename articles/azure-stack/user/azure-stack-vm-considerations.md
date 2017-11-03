---
title: "Rozdíly a důležité informace pro virtuální počítače v Azure zásobníku | Microsoft Docs"
description: "Další informace o rozdílech a důležité informace při práci s virtuálními počítači v Azure zásobníku."
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
ms.date: 9/25/2017
ms.author: sngun
ms.openlocfilehash: 7d841dba798c2b706c26dcf51361ce0447710b12
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="considerations-for-virtual-machines-in-azure-stack"></a>Důležité informace pro virtuální počítače v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Virtuální počítače jsou na vyžádání, škálovatelných výpočetních prostředků, které nabízí Azure zásobníku. Použijete-li virtuální počítače, je potřeba pochopit, že jsou rozdíly mezi funkce, které jsou dostupné v Azure a Azure zásobníku. Tento článek obsahuje přehled aspektů jedinečné pro virtuální počítače a jeho funkce v zásobníku Azure. Další informace o nejvýraznějších rozdílů mezi zásobník Azure a Azure, najdete v článku [klíčové aspekty](azure-stack-considerations.md) tématu.

## <a name="cheat-sheet-virtual-machine-differences"></a>Tahák: rozdíly virtuálního počítače

| Funkce | Azure (globální) | Azure Stack |
| --- | --- | --- |
| Bitové kopie virtuálních počítačů | Azure Marketplace obsahuje bitové kopie, které můžete použít k vytvoření virtuálního počítače. Najdete v článku [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) stránky pro zobrazení seznamu bitových kopií, které jsou k dispozici v Azure Marketplace. | Ve výchozím nastavení nejsou k dispozici všechny Image, k dispozici v zásobníku Azure marketplace. Správce cloudu Azure zásobníku by měla publikovat nebo před uživatelé používají stáhli bitové kopie do zásobníku Azure marketplace. |
| Velikosti virtuálních počítačů | Azure podporuje širokou škálu velikostí pro virtuální počítače. Další informace o dostupných velikostí a možnosti, naleznete [velikosti virtuálních počítačů Windows](../../virtual-machines/virtual-machines-windows-sizes.md) a [velikostí virtuálních počítačů Linux](../../virtual-machines/linux/sizes.md) témata. | Azure zásobníku podporuje podmnožinu velikostí virtuálních počítačů, které jsou k dispozici v Azure. Chcete-li zobrazit seznam podporovaných velikostí, naleznete [velikostí virtuálních počítačů](#virtual-machine-sizes) tohoto článku. |
| Kvóty virtuálního počítače | [Maximální kvóty](../../azure-subscription-service-limits.md#service-specific-limits) jsou nastavené microsoftem. | Správce cloudu Azure zásobníku musí přiřadit kvóty, než virtuální počítače nabízejí uživatelům. |
| Rozšíření virtuálního počítače |Azure podporuje širokou škálu rozšíření virtuálního počítače. Další informace o dostupných rozšíření, naleznete [rozšíření virtuálního počítače a funkce](../../virtual-machines/windows/extensions-features.md) tématu.| Azure zásobníku podporuje podmnožinu rozšíření, které jsou k dispozici v Azure a rozšíření mají konkrétních verzí. Správce cloudu Azure zásobníku může rozhodnout, jaká rozšíření bylo k dispozici na pro své uživatele. Chcete-li zobrazit seznam podporovaných rozšíření, naleznete [rozšíření virtuálního počítače](#virtual-machine-extensions) tohoto článku. |
| Síť virtuálních počítačů | Veřejné IP adresy, které jsou přiřazené k virtuálnímu počítači klienta jsou přístupné přes Internet.<br><br><br>Virtuální počítače Azure má pevný název DNS | Veřejné IP adresy, které jsou přiřazeny k virtuálnímu počítači klienta jsou přístupné pouze na prostředí Azure zásobníku Development Kit. Uživatel musí mít přístup k Azure zásobníku Development Kit prostřednictvím [RDP](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) nebo [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) pro připojení k virtuálnímu počítači, který je vytvořen v zásobníku Azure.<br><br>Virtuální počítače vytvořené v rámci konkrétní instanci Azure zásobníku mít název DNS na základě hodnoty, který je nakonfigurovaný pomocí Správce cloudu. |
| Úložiště virtuálního počítače | Podporuje [spravované disky.](../../virtual-machines/windows/managed-disks-overview.md) | Spravované disky se ještě nepodporují v zásobníku Azure. |
| Verze rozhraní API | Azure má vždy nejnovější verze rozhraní API pro všechny součásti virtuálního počítače. | Azure zásobníku podporuje konkrétní služby Azure a konkrétní verze rozhraní API pro tyto služby. Chcete-li zobrazit seznam podporovaných verzí rozhraní API, naleznete [verze rozhraní API](#api-versions) tohoto článku. |
|Skupiny dostupnosti virtuálního počítače|Více domén selhání (2 nebo 3 každou oblast)<br>Více domén aktualizace<br>Podpora disku spravovaného|Doména jednoho selhání<br>Domény jedné aktualizace<br>Bez podpory spravovaných disků|
|Škálovací sady virtuálních počítačů|Podporované automatickému škálování|Automatické škálování není podporována.<br>Přidáte další instance škálování, nastavit pomocí portálu, šablony Resource Manageru nebo prostředí PowerShell.

## <a name="virtual-machine-sizes"></a>Velikosti virtuálních počítačů 

Development Kit zásobník Azure podporuje následující formáty: 

| Typ | Velikost | Rozsah podporovaných velikostí |
| --- | --- | --- |
|Obecné účely |Basic A|A0 A4|
|Obecné účely |Standardní A|A0 A7|
|Obecné účely |Standardní D|D1 D4|
|Obecné účely |Standardní Dv2|D1v2 D5v2|
|Optimalizované z hlediska paměti|D-series|D11 D14|
|Optimalizované z hlediska paměti |Dv2-series|D11v2 D14v2|

Velikosti virtuálních počítačů a jejich přidružených prostředků počty jsou konzistentní mezi zásobník Azure a Azure. Například to zahrnuje množství paměti, počet jader a číslo nebo velikost datových disků, které lze vytvořit. Výkon stejnou velikost virtuálního počítače v Azure zásobníku však závisí na základní charakteristiky konkrétní prostředí Azure zásobníku.

## <a name="virtual-machine-extensions"></a>Rozšíření virtuálního počítače 

 Development Kit zásobník Azure podporuje následující verze rozšíření virtuálního počítače:

![Rozšíření virtuálních počítačů](media/azure-stack-vm-considerations/vm-extensions.png)

Pomocí následujícího skriptu prostředí PowerShell získat seznam rozšíření virtuálního počítače, které jsou k dispozici ve vašem prostředí Azure zásobníku:

```powershell 
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize 
```

## <a name="api-versions"></a>Verze rozhraní API 

Funkce virtuálních počítačů v Azure zásobníku Development Kit podporují následující verze rozhraní API:

![Typy prostředků virtuálních počítačů](media/azure-stack-vm-considerations/vm-resoource-types.png)

Následující skript prostředí PowerShell můžete použít k získání verze rozhraní API pro funkce virtuálních počítačů, které jsou k dispozici ve vašem prostředí Azure zásobníku:

```powershell 
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like “Microsoft.compute”}
```
Seznam podporovaných prostředků typy a verze rozhraní API se může lišit, pokud operátor cloudu aktualizuje na novější verzi prostředí Azure zásobníku.

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell v Azure zásobníku](azure-stack-quick-create-vm-windows-powershell.md)
