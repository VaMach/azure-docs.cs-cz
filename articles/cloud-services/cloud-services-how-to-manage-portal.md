---
title: "Běžné úlohy správy cloudové služby | Microsoft Docs"
description: "Naučte se spravovat cloudové služby na portálu Azure. Tyto příklady použití portálu Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 9af1fdeb5cfe69631cabe13bd341b43319175aae
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="how-to-manage-cloud-services"></a>Jak spravovat cloudové služby
V **cloudové služby (klasické)** oblasti Azure portálu, můžete aktualizovat roli služby nebo nasazení, zvýšení úrovně dvoufázového nasazení do produkčního prostředí, prostředky propojit cloudové služby, aby mohli zobrazit závislosti prostředků a škálování prostředky společně a odstraňte cloudovou službu nebo nasazení.

Další informace o tom, jak škálování cloudové služby jsou k dispozici [zde](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Postupy: aktualizovat roli služby cloudu nebo nasazení
Pokud je potřeba aktualizovat kód aplikace pro cloudové služby, použijte **aktualizace** v okně cloudové služby. Můžete aktualizovat roli jednoho nebo všech rolí. Pokud chcete aktualizovat, můžete nahrát nový balíček služby nebo konfigurační soubor služby.

1. V [portál Azure][Azure portal], vyberte cloudovou službu, kterou chcete aktualizovat. Tento krok se otevře okno instance služby pro cloud.
2. V okně klikněte **aktualizace** tlačítko.

    ![Tlačítko Aktualizovat](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Aktualizujte nasazení nového souboru balíku služeb (.cspkg) a konfigurační soubor služby (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Volitelně** aktualizovat označení nasazení a účet úložiště.
5. Pokud žádné role mít pouze jednu instanci role, vyberte **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** povolit upgradu pokračovat.

    Azure můžete pouze zaručit 99,95 % dostupnost služby při aktualizaci služby cloud pokud každá role má aspoň dvě instance role (virtuální počítače). Se dvěma instancemi role jeden virtuální počítač zpracovává požadavky na klienta během dalších je aktualizována.

6. Zkontrolujte **spuštění nasazení** tak, aby měl použijí po dokončení nahrávání balíčku aktualizace.
7. Klikněte na tlačítko **OK** spustíte aktualizaci služby.

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Postupy: Prohodit nasazení na podporu dvoufázového nasazení do produkčního prostředí
Pokud se rozhodnete nasadit novou verzi cloudové služby, fáze a otestovat novou verzi v prostředí s pracovní cloudové služby. Použití **Prohodit** přepnout adresy URL, které dvě nasazení se podrobněji a povyšte novou verzí do produkčního prostředí.

Můžete zaměnit nasazení z **cloudové služby** stránku nebo řídicí panel.

1. V [portál Azure][Azure portal], vyberte cloudovou službu, kterou chcete aktualizovat. Tento krok se otevře okno instance služby pro cloud.
2. V okně klikněte **Prohodit** tlačítko.

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Otevře se následující potvrzovací výzvu.

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Po ověření, informace o nasazení, klikněte na tlačítko **OK** chcete Prohodit nasazení.

    Swap nasazení dochází rychle jediné, co se změní je virtuální IP adresy (VIP) pro nasazení.

    Pokud chcete uložit výpočetní náklady, můžete odstranit pracovní nasazení po ověření, že produkčního nasazení funguje podle očekávání.

### <a name="common-questions-about-swapping-deployments"></a>Časté otázky týkající se nasazení vzájemná záměna

**Jaké jsou požadavky pro odkládací nasazení?**

Existují dva klíče požadavky pro úspěšné nasazení prohození:

- Pokud chcete používat statickou IP adresu pro vaše produkční slot, jeden pro přípravný slot také musíte rezervovat. Prohození, jinak selže.

- Všechny instance role musí být spuštěna, než bude možné provést prohození. Můžete zkontrolovat stav vašimi instancemi v okně Přehled portálu Azure. Alternativně můžete použít [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) příkazu v prostředí Windows PowerShell.

Všimněte si, že hostovaného operačního systému, aktualizace a opravy operace služby může také způsobit nasazení záměna selhání. Další informace najdete v tématu [Poradce při potížích se nasazení služby cloud](cloud-services-troubleshoot-deployment-problems.md).

**Nesnižuje prohození výpadek své aplikaci? Jak by měly zpracovávat ho?**

Jak je popsáno v poslední části, je obvykle rychlé prohození nasazení, protože je právě změnu konfigurace pro vyrovnávání zatížení Azure. V některých případech ale ho můžete trvat deset nebo víc sekund a způsobit selhání přechodný připojení. Chcete-li omezit vliv na vaše zákazníky, zvažte implementaci [logika opakovaných pokusů klienta](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Postupy: odkaz prostředek cloudové služby
Portál Azure neobsahuje odkazy prostředky společně jako nemá na aktuálním portálu Azure classic. Místo toho nasaďte další prostředky do stejné skupiny prostředků používá cloudové služby.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Postupy: odstranění nasazení a cloudové služby
Před odstraněním cloudové služby, je nutné odstranit každé existující nasazení.

Pokud chcete uložit výpočetní náklady, můžete odstranit pracovní nasazení po ověření, že produkčního nasazení funguje podle očekávání. Fakturuje se výpočetní náklady pro instance nasazené rolí, které jsou zastaveny.

Pomocí následujícího postupu můžete odstranit nasazení nebo cloudové služby.

1. V [portál Azure][Azure portal], vyberte cloudovou službu, kterou chcete odstranit. Tento krok se otevře okno instance služby pro cloud.
2. V okně klikněte **odstranit** tlačítko.

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Celý cloudové služby můžete odstranit kontrolou **Cloudová služba a její nasazení** nebo zvolit buď **produkční nasazení** nebo **pracovní nasazení**.

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Klikněte **odstranit** tlačítko dole.
5. Chcete-li odstranit cloudovou službu, klikněte na tlačítko **odstranění Cloudová služba**. Potom v potvrzovací výzvu, klikněte na **Ano**.

> [!NOTE]
> Pokud Cloudová služba je Odstraněná a podrobné monitorování je nastavena, musíte odstranit data ručně z vašeho účtu úložiště. Informace o tom, kde najít metriky tabulky najdete v tématu [to](cloud-services-how-to-monitor.md) článku.


## <a name="how-to-find-more-information-about-failed-deployments"></a>Postupy: Další informace o selhání nasazení
**Přehled** okno obsahuje stavového řádku v horní části. Po kliknutí na tlačítko panelu, nové okno se zobrazí veškeré informace o chybě. Pokud nasazení neobsahuje žádné chyby, v okně informace je prázdný.

![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Další kroky
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).
* Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).
