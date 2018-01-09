---
title: "Azure Security Center rychlý start - zaváděním počítačů Linux Security Center | Microsoft Docs"
description: "Tento rychlý start se dozvíte, jak se budou registrovat počítačů Linux Security Center."
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
ms.openlocfilehash: 365afd2199b1b8b2e70d882f6a729384edbdffbc
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-linux-computers-to-azure-security-center"></a>Rychlý úvod: Zařadit Linux počítačů do Azure Security Center
Po můžete zaváděním předplatné Azure, můžete povolit Security Center pro prostředky Linux spuštěné mimo Azure, pro příklad místní nebo v ostatních cloudů, Služba agenta systému Linux.

Tento rychlý start ukazuje, jak instalovat agenta systému Linux na počítač se systémem Linux.

## <a name="prerequisites"></a>Požadavky
Pokud chcete začít využívat Security Center, musíte mít předplatné pro Microsoft Azure. Pokud nemáte předplatné, můžete si vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

Před zahájením tento rychlý start, musí být na standardní cenovou úroveň Security Center. V tématu [Onboard předplatného Azure Security Center standardní](security-center-get-started.md) pro pokyny k upgradu. Můžete zkusit Security Center Standard bez nákladů pro prvních 60 dní.

## <a name="add-new-linux-computer"></a>Přidat nový počítač se systémem Linux

1. Přihlaste se k webu [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. V nabídce **Microsoft Azure** vyberte **Security Center**. **Security Center – přehled** otevře.

 ![Security Center – přehled][2]

3. V části v hlavní nabídce Security Center, vyberte **registrace k pokročilým zabezpečením**.
4. Vyberte **chcete přidat počítače mimo Azure**.
   ![Připojit k rozšířené zabezpečení][3]

5. Na **přidat nové počítače mimo Azure**, se zobrazí seznam vašich pracovních prostorů analýzy protokolů. Seznam obsahuje výchozí pracovní prostor pomocí služby Security Center při vytvořena automatické zřizování byl povolen, pokud tyto produkty. Vyberte tento pracovní prostor nebo jiné pracovní prostor, který chcete použít.

    ![Přidat počítač mimo Azure][4]

6.  Na **přímé agenta** v části **stažení a TAKY agenta pro LINUX**, vyberte **kopie** tlačítko Kopírovat *wget* příkaz.

7.  Otevřete Poznámkový blok a vložte tento příkaz. Uložte tento soubor do umístění, které může být přístupné z počítače systému Linux.

## <a name="install-the-agent"></a>Instalace agenta

1.  V počítači Linux otevřete soubor, který byl dříve uložili. Vyberte celý obsah, kopírovat, otevřete konzolu terminálu a vložte příkaz.
2.  Po dokončení instalace můžete ověřit, zda *omsagent* nainstalovaná *pgrep* příkaz. Vrátí tento příkaz *omsagent* PID (ID procesu) jak je uvedeno níže:

  ![Instalace agenta][5]

Protokoly pro Security Center agenta pro Linux najdete na: */var/opt/microsoft/omsagent/<workspace id>/log/*

  ![Protokoly pro agenta][6]

Po určité době může trvat až 30 minut, zobrazí se nové počítače se systémem Linux v Centru zabezpečení.

Teď můžete monitorovat počítače mimo Azure na jednom místě a virtuální počítače Azure. V části **výpočetní**, budete mít přehled o všech virtuálních počítačů a počítačů spolu s doporučení. Každý sloupec představuje jednu sadu doporučení. Barva představuje Virtuálního počítače nebo počítače aktuální stav zabezpečení pro tohoto doporučení. Security Center navíc zobrazí všechny detekce pro tyto počítače v výstrahy zabezpečení.

  ![Výpočetní okno][7] existují dva typy ikon reprezentované na **výpočetní** okno:

  ![icon1](./media/quick-onboard-linux-computer/security-center-monitoring-icon1.png) Počítače bez Azure

  ![icon2](./media/quick-onboard-linux-computer/security-center-monitoring-icon2.png) Virtuální počítač Azure

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud již nepotřebujete, můžete odebrat agenta z počítače se systémem Linux.

Postup odebrání agenta:

1. Stáhnout agenta pro Linux [universal skriptu](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) do počítače.
2. Spusťte soubor .sh sady s *– mazání* argument v počítači, který kompletně odebere agent a jeho konfigurace.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

## <a name="next-steps"></a>Další postup
V této úvodní zřízený agent na počítač se systémem Linux. Další informace o tom, jak používat Security Center, i nadále kurz pro konfiguraci zásad zabezpečení a vyhodnocení zabezpečení vašich prostředků.

> [!div class="nextstepaction"]
> [Kurz: Definování a vyhodnocení zásad zabezpečení](tutorial-security-policy.md)

<!--Image references-->
[1]: ./media/quick-onboard-linux-computer/portal.png
[2]: ./media/quick-onboard-linux-computer/overview.png
[3]: ./media/quick-onboard-linux-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-linux-computer/add-computer.png
[5]: ./media/quick-onboard-linux-computer/pgrep-command.png
[6]: ./media/quick-onboard-linux-computer/logs-for-agent.png
[7]: ./media/quick-onboard-linux-computer/compute.png
