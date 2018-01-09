---
title: "Azure Security Center rychlý start - zaváděním vašeho počítače se systémem Windows do Security Center | Microsoft Docs"
description: "Tento rychlý start se dozvíte, jak zřídit agenta Microsoft Monitoring Agent na počítači se systémem Windows."
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
ms.date: 01/07/2018
ms.author: terrylan
ms.openlocfilehash: 50cbbca9181d67bc41632a4650c76b9636a72356
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-windows-computers-to-azure-security-center"></a>Rychlý úvod: Počítače se systémem Windows registrovat do Azure Security Center
Po můžete zaváděním předplatné Azure, můžete povolit Security Center pro prostředky, které jsou spuštěné mimo Azure, pro příklad místní nebo v ostatních cloudů, služba Microsoft Monitoring Agent.

Tento rychlý start se dozvíte, jak nainstalovat službu Microsoft Monitoring Agent na počítači se systémem Windows.

## <a name="prerequisites"></a>Požadavky
Pokud chcete začít využívat Security Center, musíte mít předplatné pro Microsoft Azure. Pokud nemáte předplatné, můžete si vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

Před zahájením tento rychlý start, musí být na standardní cenovou úroveň Security Center. V tématu [Onboard předplatného Azure Security Center standardní](security-center-get-started.md) pro pokyny k upgradu. Můžete zkusit Security Center Standard bez nákladů pro prvních 60 dní.

## <a name="add-new-windows-computer"></a>Přidat nový počítač se systémem Windows

1. Přihlaste se k webu [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. V nabídce **Microsoft Azure** vyberte **Security Center**. **Security Center – přehled** otevře.

 ![Security Center – přehled][2]

3. V části v hlavní nabídce Security Center, vyberte **registrace k pokročilým zabezpečením**.
4. Vyberte **chcete přidat počítače mimo Azure**.

   ![Připojit k rozšířené zabezpečení][3]

5. Na **přidat nové počítače mimo Azure**, se zobrazí seznam vašich pracovních prostorů analýzy protokolů. Seznam obsahuje výchozí pracovní prostor pomocí služby Security Center při vytvořena automatické zřizování byl povolen, pokud tyto produkty. Vyberte tento pracovní prostor nebo jiné pracovní prostor, který chcete použít.

    ![Přidat počítač mimo Azure][4]

  **Přímé agenta** okno otevře s odkazem pro stažení agenta Windows a klíčů pro ID vašeho pracovního prostoru pro použití v konfiguraci agenta.

6.  Vyberte **stáhnout agenta Windows** odkaz pro typ procesoru počítače stáhnout instalační soubor.

7.  Na pravé straně **ID pracovního prostoru**, vyberte ikonu kopírování a vložení ID do poznámkového bloku.

8.  Na pravé straně **primární klíč**, vyberte ikonu kopírování a vložení klíč do poznámkového bloku.

## <a name="install-the-agent"></a>Instalace agenta
Nyní je nutné nainstalovat stažený soubor v cílovém počítači.

1. Zkopírujte soubor do cílového počítače a spusťte instalační program.
2. Na **úvodní** vyberte **Další**.
3. Na **licenční podmínky** , přečtěte si licenční a pak vyberte **souhlasím**.
4. Na **cílovou složku** stránky, změnit nebo ponechat výchozí instalační složku a potom vyberte **Další**.
5. Na **možnosti instalace agenta** vyberte připojit agenta k Azure Log Analytics (OMS) a potom vyberte **Další**.
6. Na **Azure Log Analytics** stránky, vložte **ID pracovního prostoru** a **klíč pracovního prostoru (primární klíč)** kterou jste zkopírovali do poznámkového bloku v předchozím postupu.
7. Pokud počítač by měl nahlásit pracovní prostor analýzy protokolů v cloudu Azure Government, vyberte **Azure US Government** formuláře **cloudu Azure** rozevíracího seznamu.  Pokud počítač musí komunikovat přes proxy server ke službě Analýza protokolů, vyberte **Upřesnit** a zadejte adresu URL a číslo portu proxy serveru.
8. Vyberte **Další** po dokončení poskytuje nezbytné nastavení konfigurace.

  ![Instalace agenta][5]

9. Na **připraveno k instalaci** stránka, zkontrolujte vybrané možnosti a pak vyberte **nainstalovat**.
10. Na **konfigurace byla úspěšně dokončena** vyberte **dokončit**

Po dokončení se **Microsoft Monitoring Agent** zobrazí v **Ovládacích panelech**. Můžete zkontrolovat konfiguraci existuje a ověřte, zda je agent připojen.

Další informace o instalaci a konfiguraci agenta najdete v tématu [počítače se systémem Windows připojit](../log-analytics/log-analytics-agent-windows.md#install-the-agent-using-setup).

Teď můžete monitorovat počítače mimo Azure na jednom místě a virtuální počítače Azure. V části **výpočetní**, budete mít přehled o všech virtuálních počítačů a počítačů spolu s doporučení. Každý sloupec představuje jednu sadu doporučení. Barva představuje Virtuálního počítače nebo počítače aktuální stav zabezpečení pro tohoto doporučení. Security Center navíc zobrazí všechny detekce pro tyto počítače v výstrahy zabezpečení.

  ![Výpočetní okno][6]

Existují dva typy ikon reprezentované na **výpočetní** okno:

![icon1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Počítače bez Azure

![icon2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Virtuální počítač Azure

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud již nepotřebujete, můžete odebrat agenta z počítače se systémem Windows.

Postup odebrání agenta:

1. Otevřete **Ovládací panely**.
2. Otevřete **Programy a funkce**.
3. V části **Programy a funkce** vyberte **Microsoft Monitoring Agent** a klikněte na **Odinstalovat**.

## <a name="next-steps"></a>Další postup
V tento rychlý start zřídí agenta Microsoft Monitoring Agent na počítači se systémem Windows. Další informace o tom, jak používat Security Center, i nadále kurz pro konfiguraci zásad zabezpečení a vyhodnocení zabezpečení vašich prostředků.

> [!div class="nextstepaction"]
> [Kurz: Definování a vyhodnocení zásad zabezpečení](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png
