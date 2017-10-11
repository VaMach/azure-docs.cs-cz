---
title: "Služba kvóty a omezení pro Azure Batch | Microsoft Docs"
description: "Další informace o výchozích Azure Batch kvót, omezení a omezení a zvyšuje jak požádat o kvótu"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3f69ed8d3a985afe07e648e7512a88b25278ced
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="batch-service-quotas-and-limits"></a>Kvóty a omezení služby Batch

Jako s jinými službami Azure neexistují omezení na některé prostředky spojené s touto službou Batch. Mnoho z těchto omezení je výchozí kvóty používané službou Azure na úrovni účet nebo předplatné. Tento článek popisuje tyto výchozí hodnoty a jak můžete vyžádat kvóty zvyšuje.

Pamatujte na tyto kvóty při navrhování a škálování zatížení vašich úloh Batch. Například pokud váš fond není dosažení cílovým počtem výpočetních uzlů, které jste určili, může bylo dosaženo maximální kvóta základní pro vašeho účtu Batch nebo regionální kvóta jader virtuálních počítačů pro vaše předplatné.

Můžete spustit několik dávkových úloh služby Batch v jednom účtu Batch najednou, nebo můžete úlohy rozložit mezi více účtů Batch, které jsou v jednom předplatném, ale v různých oblastech Azure.

Pokud plánujete spouštět úlohy v produkčním prostředí ve službě Batch, musíte zvýšit jeden nebo více kvót nad výchozí. Pokud chcete kvótu zvýšit, můžete otevřít online [zákazníka žádost o podporu](#increase-a-quota) zdarma.

> [!NOTE]
> Kvótu je limit platební, není záruku kapacity. Pokud máte požadavků na kapacitu ve velkém měřítku, obraťte se na podporu Azure.
> 
> 

## <a name="resource-quotas"></a>Kvóty prostředků
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a>Kvóty v uživatelském režimu předplatného

Pro účet Batch se režim přidělení fondu nastavený na **uživatele předplatné**, dávky virtuální počítače a další prostředky, například účty úložiště, jsou vytvořené přímo v rámci vašeho předplatného, když je vytvořen fond. Kvóta jader Azure Batch se nevztahuje na účet je vytvořen v tomto režimu. Místo toho kvóty pro vaše předplatné pro místní výpočetní jader a dalším prostředkům se použijí. Další informace o těchto kvót v [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).

Při plánování využití prostředků pro účet je vytvořen v uživatelském režimu předplatné, Všimněte si, že v následujících zdrojích informací Batch (kromě výpočetní jádra) jsou vyžadovány pro každých 40 virtuální počítače s Linuxem nebo 20 virtuálních počítačů Windows:

| Prostředek | Kvóta | Poskytovatel |
| --- | ---| --- |
| Jeden účet úložiště | Účty úložiště | Microsoft.Storage |
| Jedna veřejná IP adresa | Veřejné IP adresy | Microsoft.Network | 
| Jedna virtuální síť | Virtuální sítě | Microsoft.Network | 
| Jedna síť skupiny zabezpečení | Network Security Groups (Skupiny zabezpečení sítě) | Microsoft.Network | 
| Jeden škálovací sadu virtuálních počítačů | Škálovací sady virtuálních počítačů | Microsoft.Compute | 
| Jedna služba Vyrovnávání zatížení | Nástroje pro vyrovnávání zatížení | Microsoft.Network | 

Kvóta jader na místní úrovni nebo na virtuální počítač rodiny měli nastavit podle velikosti virtuálního počítače, které jsou vyžadovány pro fondu Batch nebo fondy:

| Kvóta | Poskytovatel |
| --- | ---- |
| Celkový počet jader místní | Microsoft.Compute |
| … Rodiny jader | Microsoft.Compute |



## <a name="other-limits"></a>Další omezení
| **Prostředek** | **Maximální omezení** |
| --- | --- |
| [Souběžné úlohy](batch-parallel-node-tasks.md) na výpočetním uzlu |4 x počet jader na uzel |
| [Aplikace](batch-application-packages.md) na účtu Batch |20 |
| Balíčky aplikací na aplikaci. |40 |
| Velikost balíčku aplikace, (všechny) |Poli 195GB<sup>1</sup> |
| Velikost maximální spuštění úloh | 32768 znaků<sup>2</sup> |

<sup>1</sup> azure Storage limit pro velikost objektu blob maximální bloku<br />
<sup>2</sup> zahrnuje soubory prostředků a proměnných prostředí

## <a name="view-batch-quotas"></a>Zobrazení dávky kvóty
Zobrazení vaší kvóty účtu Batch [portál Azure][portal].

1. Vyberte **účty Batch** na portálu, pak vyberte účet Batch, co vás zajímá.
2. Vyberte **vlastnosti** v okně účtu Batch nabídky.
3. Zobrazí v okně Vlastnosti **kvóty** aktuálně použité k účtu Batch
   
    ![Kvóty účtu batch][account_quotas]

Pro účet Batch vytvořit v uživatelském režimu předplatné můžete zobrazte kvóty související předplatné na portálu Azure.

1. Vyberte **odběry**a vyberte odběr, který používáte pro účet Batch.

2. Na **předplatné** vyberte **využití + kvóty**.



## <a name="increase-a-quota"></a>Navýšení kvóty
Postupujte podle těchto kroků k vyžádání kvótu zvýšit vašeho účtu Batch nebo předplatnému pomocí [portál Azure][portal]. Typ zvýšení kvóty závisí na režimu přidělení fondu vašeho účtu Batch.

### <a name="increase-a-batch-cores-quota"></a>Navýšení kvóty jader Batch 

Pokud je váš účet Batch vytvořený v **služba Batch** režim, postupujte podle těchto kroků k vyžádání Batch jader kvótu zvýšit:

1. Vyberte **Nápověda a podpora** dlaždici řídicího panelu portálu nebo otazník (**?**) v pravém horním rohu portálu.
2. Vyberte **nová žádost o podporu** > **Základy**.
3. Na **Základy** okno:
   
    a. **Vydávání typu** > **kvóty**
   
    b. Vyberte své předplatné.
   
    c. **Typ kvóty** > **Batch**
   
    d. **Podporují plán** > **kvóty podporu - zahrnuté**
   
    Klikněte na **Další**.
4. Na **problém** okno:
   
    a. Vyberte **závažnost** podle vaší [dopad na chod firmy][support_sev].
   
    b. V **podrobnosti**, zadejte každé kvóty, které chcete změnit, název účtu Batch a nový limit.
   
    Klikněte na **Další**.
5. Na **kontaktní informace** okno:
   
    a. Vyberte **způsob kontaktu preferované**.
   
    b. Zkontrolujte a zadejte požadované kontaktní údaje.
   
    Kliknutím na **Vytvořit** odešlete žádost o podporu.

Jakmile jste odeslali vaši žádost o podporu, bude vás kontaktovat podporu Azure. Všimněte si, že dokončení požadavku může trvat až 2 pracovních dnů.

### <a name="increase-a-subscription-cores-quota"></a>Navýšení kvóty předplatného jader

Pokud váš účet Batch byl vytvořen v **uživatele předplatné** režimu a potřebujete další místní nebo rodiny jader virtuálního počítače, žádost a kvótu zvýšit v rámci vašeho předplatného. Pokyny najdete v tématu [základní kvóta správce prostředků zvýšit požadavky](../azure-supportability/resource-manager-core-quotas-request.md).



## <a name="related-topics"></a>Související témata
* [Vytvoření účtu Azure Batch pomocí portálu Azure](batch-account-create-portal.md)
* [Přehled funkcí Azure Batch](batch-api-basics.md)
* [Předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
