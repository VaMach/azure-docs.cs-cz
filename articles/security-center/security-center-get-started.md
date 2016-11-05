---
title: Úvodní příručka ke službě Azure Security Center | Microsoft Docs
description: Tento dokument vám pomůže rychle začít používat Azure Security Center. Provede vás komponentami pro správu zásad a monitorování zabezpečení a propojí k dalším krokům.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: ''

ms.service: security-center
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2016
ms.author: terrylan

---
# Úvodní příručka ke službě Azure Security Center
Tento dokument vám pomůže rychle začít používat Azure Security Center. Provede vás komponentami pro správu zásad a monitorování zabezpečení a propojí k dalším krokům.

> [!NOTE]
> Informace v tomto dokumentu se týkají verze Preview služby Azure Security Center. Tento dokument vám tuto službu představí formou ukázkového nasazení. Není to podrobný průvodce.
> 
> 

## Co je Azure Security Center?
 Security Center pomáhá předcházet hrozbám, zjišťovat je a reagovat na ně a nabízí lepší přehled o zabezpečení prostředků Azure a kontrolu nad nimi. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

## Požadavky
Pokud chcete začít využívat Security Center, musíte mít předplatné pro Microsoft Azure. Služba Security Center je povolená s vaším předplatným. Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

 Služba Security Center je přístupná prostřednictvím [portálu Azure](https://azure.microsoft.com/features/azure-portal/). Další informace najdete v [dokumentaci k portálu](https://azure.microsoft.com/documentation/services/azure-portal/).

## Přístup ke službě Security Center
Na portálu použijte pro přístup ke službě Security Center následující kroky:

1. Vyberte **Procházet** a přejděte k možnosti **Security Center**.
   ![Přístup ke službě Azure Security Center na portálu][1]
2. Vyberte **Security Center**. Otevře se okno **Security Center**.
3. Pro zajištění snadného přístupu k oknu **Security Center** v budoucnu vyberte možnost **Připnout okno na řídicí panel** (vpravo nahoře).
   ![Možnost Připnout okno na řídicí panel][2]

## Použití služby Security Center
Můžete nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure. Nakonfigurujme **zásady** zabezpečení pro vaše předplatné:

1. Vyberte dlaždici **Zásady** v okně **Security Center**.
   ![Security Center][3]
2. V okně **Zásady zabezpečení – Define policy per subscription or resource group** (Definovat zásady pro předplatné nebo skupinu prostředků) vyberte předplatné.
   ![Okno Zásady zabezpečení v Azure Security Center][4]
3. V okně **Zásady zabezpečení** zapněte **Shromažďování dat**, aby se automaticky shromažďovaly protokoly. Zapnutí **Shromažďování dat** také zajistí rozšíření monitorování na všech aktuálních a nových virtuálních počítačích v rámci příslušného předplatného.
4. Vyberte **Choose a storage account per region** (Zvolit účet úložiště pro oblast). Pro každou oblast, ve které máte spuštěné virtuální počítače, si zvolíte účet úložiště, na který se mají ukládat data shromážděná z daných virtuálních počítačů. Pokud pro každou oblast nevyberete účet úložiště, vytvoří se vám automaticky. Shromážděná data budou z důvodů zabezpečení logicky oddělená od ostatních zákaznických dat.
   
   > [!NOTE]
   > Doporučujeme, abyste nejdřív zapnuli shromažďování dat a zvolili účet úložiště na úrovni předplatného.  Zásady zabezpečení je možné nastavit na úrovni předplatného Azure nebo na úrovni skupiny prostředků, ale ke konfiguraci shromažďování dat a účtů úložiště dochází jenom na úrovni předplatného.
   > 
   > 
5. Zapněte **doporučení**, která chcete v rámci svých zásad zabezpečení využít. Příklady:
   
   * Pokud zapnete **Aktualizace systému**, ve všech podporovaných virtuálních počítačích se vyhledají chybějící aktualizace operačního systému.
   * Pokud zapnete **Baseline rules** (Standardní pravidla), ve všech podporovaných virtuálních počítačích se vyhledají konfigurace operačního systému, které by mohly zvýšit jejich zranitelnost vůči útokům.

Zpracování **doporučení**:

1. Vraťte se do okna **Security Center** a vyberte dlaždici **Doporučení**. Security Center pravidelně analyzuje stav zabezpečení vašich prostředků Azure. Pokud se naleznou potenciální chyby zabezpečení, zobrazí se zde doporučení.
2. Kliknutím na jednotlivá doporučení zobrazíte další informace nebo provedete akce vedoucí k vyřešení daného problému.
   ![Doporučení ve službě Azure Security Center][5]

K zobrazení celkového stavu a stavu zabezpečení prostředků využijte **Resource security health** (Stav zabezpečení prostředků):

1. Vraťte se do okna **Security Center**.
2. Dlaždice **Resources security health** (Stav zabezpečení prostředků) obsahuje indikátory stavu zabezpečení pro **virtuální počítače**, **síť**, **SQL** a **aplikace**.
3. Další informace zobrazíte výběrem možnosti **Virtuální počítače**.
4. V okně **Virtuální počítače** se zobrazí souhrn stavů, který zahrnuje stav antimalwarových programů, aktualizací systému, restartování a standardních pravidel virtuálních počítačů.
5. Výběrem položky v části **DOPORUČENÍ PRO VIRTUÁLNÍ POČÍTAČE** zobrazíte další informace nebo provedete akce vedoucí ke konfiguraci příslušných ovládacích prvků.
6. Přejděte k podrobnostem pro konkrétní virtuální počítače.
   ![Dlaždice stavu prostředků ve službě Azure Security Center][6]

Zpracování **výstrah zabezpečení**:

1. Vraťte se do okna **Security Center** a vyberte dlaždici **Výstrahy zabezpečení**. V okně **Výstrahy zabezpečení** se zobrazí seznam výstrah. Tyto výstrahy generuje služba Security Center při analýze protokolů zabezpečení a síťové aktivity. Zobrazí se i výstrahy z integrovaných partnerských řešení.
   ![Výstrahy zabezpečení ve službě Azure Security Center][7]
2. Výběrem výstrahy zobrazíte další informace.
   ![Podrobnosti výstrah zabezpečení ve službě Azure Security Center][8]

Zobrazení stavu **partnerských řešení**:

1. Vraťte se do okna **Security Center**. Dlaždice **Partner solutions** (Partnerská řešení) umožňuje přehledně sledovat stav partnerských řešení integrovaných ve vašem předplatném Azure.
2. Vyberte dlaždici **Partner solutions** (Partnerská řešení). Otevře se okno se seznamem partnerských řešení připojených k Security Center.
   ![Partnerská řešení][9]
3. Vyberte jedno partnerské řešení. V tomto příkladu vybereme řešení **F5-WAF2**.  Otevře se okno, které obsahuje stav tohoto partnerského řešení a jeho přidružené prostředky. Výběrem možnosti **Solution console** (Konzola řešení) otevřete prostředí pro správu tohoto partnerského řešení.
   ![Podrobné zobrazení partnerského řešení][10]

## Další kroky
V tomto dokumentu jste se seznámili s komponentami správy zásad a monitorování zabezpečení ve službě Security Center. Další informace najdete v těchto článcích:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak se konfigurují zásady zabezpečení pro vaše předplatné Azure a skupiny prostředků.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) – Získejte nejnovější informace o zabezpečení Azure.

<!--Image references-->
[1]: ./media/security-center-get-started/security-tile.png
[2]: ./media/security-center-get-started/pin-blade.png
[3]: ./media/security-center-get-started/security-center.png
[4]: ./media/security-center-get-started/security-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/partner-solutions-detail.png



<!--HONumber=Jun16_HO2-->


