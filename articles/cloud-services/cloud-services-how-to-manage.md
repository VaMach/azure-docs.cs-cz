---
title: "Běžné cloudové služby úlohy správy (klasické) | Microsoft Docs"
description: "Naučte se spravovat cloudové služby na portálu Azure classic."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 41a30e50-067c-485b-96fd-434a6d057abc
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 2ee76dfcb579e53975b1f61a6590f8d78dc0961b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-cloud-services"></a>Jak spravovat cloudové služby
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-manage-portal.md)
> * [Portál Azure Classic](cloud-services-how-to-manage.md)
>
>

V **cloudové služby** oblasti Azure classic portálu, můžete aktualizovat roli služby nebo nasazení, zvýšení úrovně dvoufázového nasazení do produkčního prostředí, propojte prostředky ke cloudové službě, můžete zobrazit závislosti prostředků a škálování prostředky společně a odstranit cloudovou službu nebo nasazení.

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Postupy: aktualizovat roli služby cloudu nebo nasazení
Pokud je potřeba aktualizovat kód aplikace pro cloudové služby, použijte **aktualizace** na řídicím panelu **cloudové služby** stránky, nebo **instance** stránky. Můžete aktualizovat roli jednoho nebo všech rolí. Budete muset nahrát nový balíček služby a konfigurační soubor služby.

1. V [portál Azure classic](https://manage.windowsazure.com/), na řídicím panelu **cloudové služby** stránky, nebo **instance** klikněte na tlačítko **aktualizace**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. V **označení nasazení**, zadejte název pro identifikaci nasazení (například mycloudservice4). Označení nasazení v části zjistíte **úvodní** na řídicím panelu.
3. V **balíček**, použijte **Procházet** pro nahrání souboru balíku služeb (.cspkg).
4. V **konfigurace**, použijte **Procházet** nahrát konfigurační soubor služby (.cscfg).
5. V **Role**, vyberte **všechny** Pokud chcete upgradovat všechny role v rámci cloudové služby. Pokud chcete provést aktualizaci s jedinou rolí, vyberte roli, kterou chcete aktualizovat. I když vyberete určité role aktualizace, aktualizace v konfiguračním souboru služby platí pro všechny role.
6. Pokud se aktualizace změní počet rolí nebo velikost žádnou roli, vyberte **povolit aktualizaci, pokud se změní velikost role nebo počet rolí** zaškrtávacího políčka umožníte aktualizace, aby bylo možné pokračovat.

    Uvědomte si, že pokud změníte velikost role (to znamená, velikost virtuálního počítače, který je hostitelem role instance) nebo počet rolí, každá instance role (virtuálním počítači) musí být znovu s vytvořenými bitovými kopiemi a všechny místní data budou ztracena.

7. Pokud všechny služby role mít pouze jednu instanci role, vyberte **aktualizovat i když jeden nebo více rolí obsahuje jednu instanci zaškrtávací políčko** povolit upgradu pokračovat.

    Azure můžete pouze zaručit 99,95 % dostupnost služby při aktualizaci služby cloud pokud každá role má aspoň dvě instance role (virtuální počítače). Umožňující jeden virtuální počítač ke zpracování požadavků klientů dalších během aktualizace.

8. Klikněte na tlačítko **OK** (zaškrtnutí) spustíte aktualizaci služby.

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Postupy: Prohodit nasazení na podporu dvoufázového nasazení do produkčního prostředí
Použití **Prohodit** ke zvýšení úrovně pracovní nasazení cloudové služby do produkčního prostředí. Pokud se rozhodnete nasadit novou verzi cloudové služby, můžete dvoufázové instalace a otestovat novou verzi v pracovní prostředí vaší cloudové služby, když vaši zákazníci používají aktuální verzi v produkčním prostředí. Až budete připraveni k povýšení nové verze do produkčního prostředí, můžete použít **Prohodit** přepnout adresy URL, které jsou určeny dvě nasazení.

Můžete zaměnit nasazení z **cloudové služby** stránku nebo řídicí panel.

1. V [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**.
2. V seznamu cloudové služby klikněte na cloudové služby za účelem vyberte ho.
3. Klikněte na tlačítko **odkládacího souboru**.

    Otevře se následující potvrzovací výzvu.

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Po ověření, informace o nasazení, klikněte na tlačítko **Ano** chcete Prohodit nasazení.

    Swap nasazení dochází rychle jediné, co se změní je virtuální IP adresy (VIP) pro nasazení.

    Abyste ušetřili náklady výpočetní, můžete odstranit nasazení v testovacím prostředí, jakmile se přesvědčíte, že nové produkční nasazení funguje podle očekávání.

### <a name="common-questions-about-swapping-deployments"></a>Časté otázky týkající se nasazení vzájemná záměna

**Jaké jsou požadavky pro odkládací nasazení?**

Existují dva klíče požadavky pro úspěšné nasazení prohození:

- Pokud chcete používat statickou IP adresu pro vaše produkční slot, jeden pro přípravný slot také musíte rezervovat. Prohození, jinak nebude úspěšná.

- Všechny instance role musí být spuštěna, než bude možné provést prohození. Můžete zkontrolovat stav vaší instance na portálu Azure classic nebo pomocí [příkaz Get-AzureRole v prostředí Windows PowerShell](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0).

Všimněte si, že aktualizace operačního systému hosta a službou opravy operace může také způsobit nasazení záměna selhání. V tématu [Poradce při potížích se nasazení služby cloud](cloud-services-troubleshoot-deployment-problems.md) další podrobnosti.

**Nesnižuje prohození výpadek své aplikaci? Jak by měly zpracovávat ho?**

Jak je popsáno v poslední části, prohození nasazení je obvykle velmi rychlé vzhledem k tomu, že je právě změnu konfigurace pro vyrovnávání zatížení Azure. V některých případech ale ho můžete trvat deset nebo víc sekund a způsobit selhání přechodný připojení. Chcete-li omezit vliv na vaše zákazníky, zvažte implementaci [logika opakovaných pokusů klienta](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Postupy: odkaz prostředek cloudové služby
Abyste označili, na jakých dalších prostředcích vaše cloudová služba závisí, můžete s ní propojit nějakou instanci Azure SQL Database nebo účet úložiště v systému . Můžete propojit a odpojit prostředky na **propojené prostředky** stránky a následně monitorovat jejich využití na řídicím panelu cloudové služby. Pokud propojený účet úložiště monitorování zapnutý, můžete sledovat celkový počet požadavků na řídicím panelu cloudové služby.

Použití **odkaz** propojit nový nebo existující databázi SQL instance nebo úložiště účet do cloudové služby. Potom můžete škálovat databázi společně s roli služby cloud, který se používá na **škálování** stránky. (Účet úložiště škáluje automaticky jako zvýšení využití.) Další informace najdete v tématu [postup škálování cloudové služby a propojené prostředky](cloud-services-how-to-scale.md).

Také můžete monitorovat, spravovat a škálovat v databázi **databáze** uzlu portálu Azure classic.

"Propojování" prostředků v tomto smyslu nepodporuje připojení aplikace k prostředku. Pokud vytvoříte novou databázi pomocí **odkaz**, budete muset přidat připojovací řetězce do kódu aplikace a pak upgradujte cloudové služby. Také budete muset přidat připojovací řetězce, pokud vaše aplikace používá prostředky v propojený účet úložiště.

Následující postup popisuje, jak propojit novou instanci databáze SQL, nasazeny na nový server databáze SQL, do cloudové služby.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Propojení instanci SQL databáze do cloudové služby
1. V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**. Pak klikněte na název cloudové služby za účelem otevře řídicí panel.
2. Klikněte na tlačítko **propojené prostředky**.

    **Propojené prostředky** otevře se stránka.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Klikněte na možnost **odkaz prostředek** nebo **odkaz**.

    **Prostředků odkaz** spustí se průvodce.

    ![Odkaz Page1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Klikněte na tlačítko **vytvořte nový prostředek** nebo **propojení na existující prostředek**.
5. Vyberte typ prostředku propojení. V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **SQL Database**. (Jenom portál Azure classic podporuje propojení účtu úložiště v cloudové službě.)
6. Chcete-li dokončit konfiguraci databáze, postupujte podle pokynů v nápovědě pro **databází SQL** oblasti portálu Azure classic.

    Můžete sledovat průběh operace propojení v oblasti zpráv.

    Po dokončení propojení můžete monitorovat stav propojeného prostředku na řídicím panelu cloudové služby. Informace o škálování propojené databáze SQL najdete v tématu [postup škálování cloudové služby a propojené prostředky](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Zrušení propojení propojeného prostředku
1. V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**. Pak klikněte na název cloudové služby za účelem otevře řídicí panel.
2. Klikněte na tlačítko **propojené prostředky**a pak vyberte prostředek.
3. Klikněte na tlačítko **zrušit propojení**. Pak klikněte na tlačítko **Ano** při potvrzení.

    Databáze SQL se odpojuje nemá žádný vliv na databázi nebo aplikace připojení k databázi. Stále můžete spravovat v databázi **databází SQL** oblasti portálu Azure classic.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Postupy: odstranění nasazení a cloudové služby
Před odstraněním cloudové služby, je nutné odstranit každé existující nasazení.

Pokud chcete uložit výpočetní náklady, můžete odstranit pracovní nasazení po ověření, že produkčního nasazení funguje podle očekávání. Jste fakturovaná výpočetní náklady pro instance rolí i v případě, že Služba cloudu není spuštěna.

Pomocí následujícího postupu můžete odstranit nasazení nebo cloudové služby.

1. V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**.
2. Vyberte cloudovou službu a pak klikněte na **odstranit**. (Pokud chcete vybrat cloudové služby bez otevření řídicí panel, klikněte na libovolné místo kromě názvu v položce cloudové služby.)

    Pokud máte nasazení v pracovním nebo produkčním, zobrazí se nabídky možností podobné následující v dolní části okna. Před odstraněním cloudové služby, je nutné odstranit všechna existující nasazení.

    ![Odstranění nabídky](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)

3. Pokud chcete odstranit nasazení, klikněte na tlačítko **odstranit produkční nasazení** nebo **odstranit pracovní nasazení**. Potom v potvrzovací výzvu, klikněte na **Ano**.
4. Pokud budete chtít odstranit cloudové služby, opakujte krok 3, v případě potřeby odstranit další nasazení.
5. Chcete-li odstranit cloudovou službu, klikněte na tlačítko **odstranění Cloudová služba**. Potom v potvrzovací výzvu, klikněte na **Ano**.

> [!NOTE]
> Pokud podrobné monitorování je nakonfigurovaný pro cloudové služby, Azure neodstraní data sledování z vašeho účtu úložiště při odstraňování cloudové služby. Musíte ručně odstranit data. Informace o tom, kde najít metriky tabulky najdete v tématu "postup: přístup podrobné monitorování data mimo portál Azure classic" v [postup monitorování cloudové služby](cloud-services-how-to-monitor.md).
>
>

## <a name="next-steps"></a>Další kroky
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).
* Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).
