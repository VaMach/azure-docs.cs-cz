---
title: "Kurz pro Azure Security Center – chránit prostředky s Azure Security Center | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak nakonfigurovat jenom v čase virtuální počítač přístup k zásady a zásady řízení aplikací."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: f0a32f90e68101f805a52427fab2d5bb29b94939
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="tutorial-protect-your-resources-with-azure-security-center"></a>Kurz: Chránit prostředky s Azure Security Center
Security Center omezuje vaše ohrožení pomocí ovládacích prvků přístupu a aplikace pro blokování podezřelé aktivity. Právě v čas virtuální počítač (VM) omezuje přístup ohrožení útokem tím, že vám tak, aby odepřel trvalý přístup k virtuálním počítačům. Místo toho zadejte řízené a auditování přístupu k virtuálním počítačům pouze v případě potřeby. Ovládací prvky adaptivní aplikace pomůže posílení virtuálních počítačů před malwarem kontrolou, které aplikace můžete spustit na virtuální počítače. Služba Security Center pomocí strojového učení analyzuje procesy spuštěné na virtuálním počítači a pomáhá s aplikováním pravidel přidávání na seznam povolených na základě těchto informací.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Konfigurovat jenom v čase virtuálních počítačů zásada přístupu
> * Nakonfigurujte zásady řízení aplikace

Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) před zahájením.

## <a name="prerequisites"></a>Požadavky
Chcete-li krok prostřednictvím funkcí zahrnuté v tomto kurzu, musí být na standardní cenovou úroveň Security Center. Můžete zkusit Security Center standardní zdarma pro prvních 60 dní. Rychlý Start [Onboard předplatného Azure Security Center standardní](security-center-get-started.md) vás provede postup upgradu na Standard.

## <a name="manage-vm-access"></a>Spravovat přístup virtuálních počítačů
Právě v čase přístup virtuálních počítačů lze zamknout příchozí přenosy na virtuální počítače Azure, snižuje riziko napadení a snadného přístupu pro připojení k virtuálním počítačům v případě potřeby.

Právě v čase přístup virtuální počítač je ve verzi preview.

Porty pro správu se nemusíte být otevřený za všech okolností. Pouze musí být otevřený, pokud jsou připojeny k virtuálnímu počítači, například k provádění úkolů údržby nebo správy. Pokud právě v čase je povoleno, Security Center používá skupina zabezpečení sítě (NSG) pravidla, která omezit přístup k portům správy, takže nemůžou být cílem útočníků.

1. Vyberte z hlavní nabídky Security Center **těsně v čas virtuálních počítačů přístup** v části **ADVANCED OBRANY cloudu**.

  ![Přístup k virtuálnímu počítači podle potřeby][1]

  **Právě v čas virtuálních počítačů přístup** poskytuje informace o stavu virtuálních počítačů:

  - **Nakonfigurované** -virtuálních počítačů, které jsou nakonfigurované pro podporu právě v přístup k časovému virtuálních počítačů.
  - **Doporučená** -virtuálních počítačů, které může podporovat jenom v přístup k časovému virtuálních počítačů, ale nebyly nakonfigurovány na.
  - **Žádná doporučení** -důvody, které můžou způsobit, že virtuální počítač tak, aby se doporučuje jsou:

    - Chybí NSG - právě v čase řešení vyžaduje skupinu NSG se.
    - Classic virtuálního počítače – Security Center, které jsou právě čas virtuálních počítačů přístup aktuálně podporuje pouze virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager.
    - Druhá - virtuální počítač je v této kategorii Pokud právě v čase řešení vypnutý v zásadách zabezpečení předplatné nebo skupinu prostředků nebo že virtuálního počítače chybí veřejnou IP adresu a nemá skupinu NSG na místě.

2. Vyberte doporučené virtuální počítač a klikněte na tlačítko **povolení JIT na 1 virtuální počítač** nakonfigurovat jenom v zásadách čas pro tento virtuální počítač:

  Můžete uložit výchozí porty, které doporučuje Security Center nebo můžete přidávat a konfigurovat nový port, na kterém chcete povolit právě v době řešení. V tomto kurzu budeme přidat na port výběrem **přidat**.

  ![Přidat konfiguraci portů][2]

3. V části **konfiguraci portů přidat**, můžete identifikovat:

  - Port
  - Typ protokolu
  - Povolené zdrojové IP adresy - rozsahy IP adres povolené získat přístup na schválené žádost
  - Doba požadavku maximální - maximální časový interval, může být otevřena specifického portu

4. Vyberte **OK** uložit.

## <a name="harden-vms-against-malware"></a>Posílení zabezpečení virtuálních počítačů před malwarem
Ovládací prvky adaptivní aplikace můžete definovat sadu aplikací, které je povoleno spustit na skupiny nakonfigurované prostředků, která mezi další výhody pomáhá posílení virtuální počítače před malwarem. Služba Security Center pomocí strojového učení analyzuje procesy spuštěné na virtuálním počítači a pomáhá s aplikováním pravidel přidávání na seznam povolených na základě těchto informací.

Ovládací prvky adaptivní aplikace je ve verzi preview. Tato funkce je jenom pro počítače s Windows k dispozici.

1. Vrátit do hlavní nabídky Security Center. V části **ADVANCED OBRANY cloudu**, vyberte **ovládací prvky adaptivní aplikace**.

   ![Adaptivní řízení aplikací][3]

  **Skupiny prostředků** část obsahuje tři karty:

  - **Nakonfigurované**: seznam prostředků skupiny obsahující virtuální počítače, které byly nakonfigurovány s řízení aplikace.
  - **Doporučená**: seznam skupin zdrojů pro aplikaci, pro kterou se doporučuje ovládacího prvku.
  - **Žádná doporučení**: seznam prostředků skupiny obsahující virtuální počítače bez žádná doporučení řízení aplikací. Například virtuální počítače, na kterých se neustále mění aplikace a které se ještě nedostaly do stabilního stavu.

2. Vyberte **doporučeno** kartě seznam skupiny prostředků s doporučeními řízení aplikací.

  ![Doporučení řízení aplikace][4]

3. Vyberte skupinu prostředků, otevřete **vytvoření pravidel aplikace řízení** možnost. V části **Vybrat virtuální počítače** zkontrolujte seznam doporučených virtuálních počítačů a zrušte zaškrtnutí těch, na které nechcete použít řízení aplikací. V části **Vybrat procesy pro pravidla přidávání na seznam povolených** zkontrolujte seznam doporučených aplikací a zrušte zaškrtnutí těch, které nechcete použít. Seznam obsahuje:

  - **NÁZEV**: úplná cesta k aplikaci
  - **PROCESY**: kolik aplikací jsou umístěny v rámci každé cesty
  - **BĚŽNÉ**: "Ano" označuje, že tyto procesy byly provedeny na nejvíc virtuálních počítačů v této skupině prostředků
  - **VYUŽITELNÝCH**: označuje ikona upozornění, pokud aplikace může útočník zneužít vynechat vytvoření seznamu povolených aplikací. Tyto aplikace doporučujeme před schválením zkontrolovat.

4. Jakmile dokončíte váš výběr, vyberte **vytvořit**.

## <a name="clean-up-resources"></a>Vyčištění prostředků
Další kurzy v této kolekci a – elementy QuickStart stavět na tento rychlý start. Pokud budete chtít pokračovat v práci s kurzy a následné – elementy QuickStart, pokračovat v provozu na plán úrovně Standard a udržovat povoleno automatické zřizování. Pokud neplánujete pokračujte nebo chcete vrátit do úroveň Free:

1. Vrátit do hlavní nabídky Security Center a vyberte **zásady zabezpečení**.
2. Vyberte předplatné nebo zásad, který chcete vrátit do volné. **Zásady zabezpečení** otevře.
3. V části **součásti zásad**, vyberte **cenová úroveň**.
4. Vyberte **volné** změna odběru z Standard úroveň na úroveň Free.
5. Vyberte **Uložit**.

Pokud chcete vypnout automatické zřizování:

1. Vrátit do hlavní nabídky Security Center a vyberte **zásady zabezpečení**.
2. Vyberte předplatné, které chcete vypnout automatické zřizování.
3. V části **zásady zabezpečení – shromažďování dat**, vyberte **vypnout** pod **registrace** zakázat automatické zřizování.
4. Vyberte **Uložit**.

>[!NOTE]
> Zakázání automatické zřizování neodebere agenta Microsoft Monitoring Agent z virtuálních počítačů Azure, kde byla vytvořena na agenta. Zakázání automatické zřizování omezení sledování zabezpečení pro vaše prostředky.
>

## <a name="next-steps"></a>Další postup
V tomto kurzu jste zjistili, jak omezit vaše ohrožení podle:

> [!div class="checklist"]
> * Konfigurace jenom v čas virtuálních počítačů přístup zásady zajistit řídit a auditovat přístup k virtuálním počítačům pouze v případě potřeby
> * Konfigurace zásady adaptivní použití ovládacích prvků řídit, která aplikace můžete spustit na virtuální počítače

Přechodu na další informace o reakce na bezpečnostní incidenty v dalším kurzu.

> [!div class="nextstepaction"]
> [Kurz: Reakce na bezpečnostní incidenty v oblasti](tutorial-security-incident.md)

<!--Image references-->
[1]: ./media/tutorial-protect-resources/just-in-time-vm-access.png
[2]: ./media/tutorial-protect-resources/add-port.png
[3]: ./media/tutorial-protect-resources/adaptive-application-control-options.png
[4]: ./media/tutorial-protect-resources/recommended-resource-groups.png
