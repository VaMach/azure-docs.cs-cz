---
title: "Azure Security Center rychlý start - zaváděním předplatného Azure Security Center standardní | Microsoft Docs"
description: "Tento rychlý start se dozvíte, jak upgradovat standardní cenovou úroveň pro dodatečné zabezpečení Security Center."
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
ms.openlocfilehash: ac4e3b36b08223f7e3b54850ed53a8185e85f0d2
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Rychlý úvod: Zařadit předplatného Azure Security Center standardní
Azure Security Center poskytuje ochranu jednotná zabezpečení správy a hrozeb v rámci úlohy hybridní cloud. Při úroveň Free nabízí omezenou zabezpečení vašich prostředků Azure pouze, úroveň Standard rozšiřuje tyto možnosti místní a ostatních cloudů. Standard Security Center vám pomůže najít a opravit chyby zabezpečení, použijete řízení přístupu a aplikace chcete blokovat škodlivé aktivity, detekovat hrozby pomocí analýzy a intelligence a rychle reagovat v případě útoku. Můžete zkusit Security Center standardní zdarma pro prvních 60 dní.

V tomto článku upgradujte na plán úrovně Standard pro zvýšení zabezpečení a nainstalujte agenta Microsoft Monitoring Agent na virtuální počítače pro monitorování ohrožení zabezpečení a hrozbami.

## <a name="prerequisites"></a>Požadavky
Pokud chcete začít využívat Security Center, musíte mít předplatné pro Microsoft Azure. Pokud nemáte předplatné, můžete si vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

Pokud chcete upgradovat předplatné na úroveň Standard, musí být přiřazena role vlastník předplatného, Přispěvatel předplatné nebo správce zabezpečení.

## <a name="enable-your-azure-subscription"></a>Aktivujte předplatné Azure

1. Přihlaste se k webu [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. V nabídce **Microsoft Azure** vyberte **Security Center**. **Security Center – přehled** otevře.

 ![Security Center – přehled][2]

**Security Center – přehled** poskytuje jednotný pohled na postavení zabezpečení vašich úloh hybridní cloud, umožňuje zjistit a vyhodnotit zabezpečení vašich úloh a identifikovat a zmírnění rizik. Některé z vašich předplatných Azure Security Center automaticky povolí dříve zařazený, nemá vy nebo jiný uživatel předplatné úroveň Free.

Můžete zobrazit a filtrovat seznam předplatných kliknutím **odběry** položku nabídky. Security Center nyní zahájí vyhodnocování zabezpečení tyto odběry k identifikaci ohrožení zabezpečení. Chcete-li přizpůsobit typy hodnocení, můžete změnit zásady zabezpečení. Zásady zabezpečení definují požadovanou konfiguraci úloh a pomáhají zajišťovat dodržování předpisů společnosti nebo soulad se zákonnými požadavky na zabezpečení.

V rámci minut spuštění Security Center poprvé mohou se zobrazit:

- **Doporučení** pro způsobů, jak zlepšit zabezpečení vašich předplatných Azure. Kliknutím **doporučení** dlaždici spustíte seznam seřazený podle priority.
- Inventář **výpočetní**, **sítě**, **& úložiště dat**, a **aplikace** prostředky, které teď hodnotí podle Security Center spolu s postavení zabezpečení jednotlivých.

Chcete-li plně využívat Security Center, proveďte následující postup pro upgrade na vrstvě Standard a instalaci aplikace Microsoft Monitoring Agent.

## <a name="upgrade-to-the-standard-tier"></a>Upgradujte na plán úrovně Standard
Pro účely kurzy a Security Center – elementy QuickStart musíte provést upgrade na úroveň Standard. Prvních 60 dní jsou zdarma a můžete se vrátit úroveň Free kdykoli.

1. V části v hlavní nabídce Security Center, vyberte **registrace k pokročilým zabezpečením**.

2. V části **registrace k pokročilým zabezpečením**, Security Center obsahuje seznam předplatných a pracovní prostory, které jsou způsobilé pro registraci. Zvolte předplatné ze seznamu.

  ![Vyberte předplatné][4]

3. **Zásady zabezpečení** poskytuje informace o skupinách prostředků obsažené v rámci předplatného. **Ceny** se rovněž otevře.
4. V části **cenová**, vyberte **standardní** k upgradu z volné Standard a klikněte na tlačítko **Uložit**.

  ![Vyberte možnost Standard.][5]

Teď, když jste jste upgradovali na úroveň Standard, máte přístup k dalším funkcím Security Center, včetně **ovládací prvky adaptivní aplikace**, **právě v čas virtuálních počítačů přístup**, **zabezpečení výstrahy**, **hrozby intelligence**, **automatizace playbooks**a další. Všimněte si, že výstrahy zabezpečení se zobrazí jenom při Security Center detekuje podezřelé aktivity.

  ![Výstrahy zabezpečení][7]

## <a name="automate-data-collection"></a>Automatizovat shromažďování dat
Security Center shromažďuje data z virtuálních počítačích Azure a počítače mimo Azure pro monitorování ohrožení zabezpečení a hrozbami. Data jsou shromažďována pomocí Microsoft Monitoring Agent, který čte různé konfigurace související se zabezpečením a protokoly událostí z počítače a zkopíruje data do pracovního prostoru pro analýzu. Ve výchozím nastavení Security Center pro vytvoření nového pracovního prostoru.

Pokud je povoleno automatické zřizování, Security Center nainstaluje agenta Microsoft Monitoring Agent na všech podporovaných virtuálních počítačích Azure a všechny nové, které jsou vytvořeny. Automatické zřizování důrazně doporučujeme.

Zapněte automatické zřizování služby Microsoft Monitoring Agent:

1. V části v hlavní nabídce Security Center, vyberte **zásady zabezpečení**.
2. Vyberte předplatné.
3. V části **zásady zabezpečení**, vyberte **shromažďování dat**.
4. V části **shromažďování dat**, vyberte **na** zapněte automatické zřizování.
5. Vyberte **Uložit**.

  ![Zapněte automatické zřizování][6]

Tento nový náhled na virtuální počítače Azure Security Center zobrazit další doporučení týkající se systému aktualizovat stav konfigurace zabezpečení operačního systému, aplikace endpoint protection, stejně jako další výstrahu zabezpečení.

  ![Doporučení][8]

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
V tento rychlý start upgradovat na plán úrovně Standard a zřízení Microsoft Monitoring Agent pro správu jednotná zabezpečení a ochrana před internetovými útoky v hybridní cloudové úlohy. Další informace o tom, jak používat Security Center, nadále rychlý start pro připojování počítačů používajících systém Windows místně a v ostatních cloudů.

> [!div class="nextstepaction"]
> [Rychlý úvod: Počítače se systémem Windows registrovat do Azure Security Center](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/onboarding.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
