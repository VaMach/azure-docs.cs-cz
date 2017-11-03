---
title: "Zákazník fakturace a vrácení peněz v Azure zásobníku | Microsoft Docs"
description: "Zjistěte, jak načíst informace o využití prostředků z Azure zásobníku."
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: a9afc7b6-43da-437b-a853-aab73ff14113
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: alfredop
ms.openlocfilehash: ea7510c239ee07a9a27f3e682e61a6b08eb5694d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="usage-and-billing-in-azure-stack"></a>Využití a cenách služby Azure zásobníku

Využití představuje objemu prostředky spotřebované uživatele. Azure zásobníku shromažďuje informace o využití pro každého uživatele a používá je k jejich vyúčtování. Tento článek popisuje, jak jsou uživatelé zásobník Azure účtuje využití prostředků a jak je přístupná fakturační informace pro analýzu, vrácení peněz, atd.

Obsahuje Azure zásobník infrastruktury pro shromažďování a agregace dat využití pro všechny prostředky a předávat tato data do Azure commerce. Přístup k těmto datům a exportovat je do fakturačních systémů pomocí fakturace adaptéru nebo exportovat je do nástroje business intelligence, jako je Microsoft Power BI. Po exportu, je tento fakturační informace použít pro analýzu nebo převedena do systému vrácení peněz.

![Konceptuální model fakturace adaptér propojíte zásobník Azure fakturace aplikace](media/azure-stack-billing-and-chargeback/image1.png)

## <a name="usage-pipeline"></a>Použití kanálu

Každý poskytovatel prostředků v Azure zásobníku vysílá data o využití podle využití prostředků. Služba Usage pravidelně (každou hodinu nebo denně) agreguje data o využití a ukládá je do databáze využití. Data uložená využití přístupná pomocí Azure zásobníku operátory a uživatelé místně pomocí využití rozhraní API. 

Pokud máte [vaší instanci Azure zásobníku a registrované v Azure](azure-stack-register.md), most využití je nakonfigurován pro odesílání dat o využití do obchodu Spojených států v Azure. Po dat je k dispozici v Azure, můžete přistupovat prostřednictvím portálu fakturace nebo pomocí Azure využití rozhraní API. Odkazovat [generování sestav dat využití](azure-stack-usage-reporting.md) tématu, dozvíte se více o jaké využití dat údajně do Azure. 

Následující obrázek znázorňuje klíčové komponenty v kanálu využití:

![Použití kanálu](media/azure-stack-billing-and-chargeback/usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Jaké informace o využití můžete najít a jak?

Azure zprostředkovatelé zásobníku prostředků, jako jsou výpočty, úložiště a sítě, generování dat o využití v hodinových intervalech pro každé předplatné. Data o využití obsahuje informace o prostředek, který používá jako název prostředku, předplatné použité, množství použít, atd. Další informace o ID prostředky měřidla, naleznete [využití nejčastější dotazy týkající se rozhraní API](azure-stack-usage-related-faq.md) článku. 

Po shromážděných dat o využití, je [oznamovány Azure](azure-stack-usage-reporting.md) ke generování faktury, které lze zobrazit pomocí portálu Azure fakturace. 

> [!NOTE]
> Generování sestav dat využití není nutné pro Azure zásobníku Development Kit a zásobník Azure integrované systému uživatele, kteří licencují v rámci modelu kapacity. Další informace o licencování v Azure zásobníku, najdete v článku [balení a ceny](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) dat listu.

Portál Azure fakturace zobrazuje data využití pouze pro fakturovatelné prostředky. Kromě fakturovatelné prostředků Azure zásobníku zaznamená data o využití pro širší sadě síťových prostředků, které dostanete ve vašem prostředí zásobníku Azure prostřednictvím rozhraní API REST nebo PowerShell. Operátory Azure zásobníku můžete načíst data o využití pro všechna předplatná uživatele, zatímco uživatel může získat pouze jejich podrobnosti o použití.

## <a name="retrieve-usage-information"></a>Načíst informace o využití

Ke generování dat o využití, měli byste mít prostředky, které jsou spuštěné a aktivně pomocí systému, například, aktivnímu virtuálnímu počítači nebo účet úložiště obsahující některá data atd. Pokud nejste jistí, jestli nějaké prostředky spuštěné v Azure Marketplace zásobníku, nasazení virtuálního počítače (VM) a ověřte virtuální počítač monitorování okno a ujistěte se, běží. Pro zobrazení dat o využití, použijte následující rutiny prostředí PowerShell:

1. [Instalace prostředí PowerShell pro Azure zásobníku.](azure-stack-powershell-install.md)
2. [Nakonfigurujte uživatele Azure zásobníku](user/azure-stack-powershell-configure-user.md) nebo [zásobník Azure operátor](azure-stack-powershell-configure-admin.md) prostředí PowerShell 

3. Pro načtení dat o využití, použijte [Get-UsageAggregates](/powershell/module/azurerm.usageaggregates/get-usageaggregates) rutiny prostředí PowerShell:

   ```powershell
   Get-UsageAggregates -ReportedStartTime "<Start time for usage reporting>" -ReportedEndTime "<end time for usage reporting>" -AggregationGranularity <Hourly or Daily>
   ```

## <a name="next-steps"></a>Další kroky

[Generování sestav Azure zásobníku využití dat do Azure](azure-stack-usage-reporting.md)

[Využití prostředků poskytovatele rozhraní API](azure-stack-provider-resource-api.md)

[Využití prostředků rozhraní API klienta](azure-stack-tenant-resource-usage-api.md)

[Nejčastější dotazy souvisí s využitím](azure-stack-usage-related-faq.md)

