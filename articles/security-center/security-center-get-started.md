---
title: "Úvodní příručka Azure Security Center | Microsoft Docs"
description: "Tento článek vám pomůže rychle začít používat službu Azure Security Center. Provede vás komponentami pro správu zásad a monitorování zabezpečení a poskytne vám odkazy na další kroky."
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
ms.date: 09/14/2017
ms.author: terrylan
ms.openlocfilehash: c28f92af96f31d1c386cf072f83fc142b9a7f588
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-center-quick-start-guide"></a>Úvodní příručka ke službě Azure Security Center
Tento článek vám pomůže rychle začít používat Azure Security Center. Provede vás komponentami pro správu zásad a monitorování zabezpečení služby Security Center.

## <a name="prerequisites"></a>Požadavky
Pokud chcete začít využívat Security Center, musíte mít předplatné pro Microsoft Azure. Pokud nemáte předplatné, můžete si vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

Úroveň Free služby Security Center je automaticky povolené na všechny odběry služby Azure a poskytuje zásady zabezpečení, vyhodnocení průběžné zabezpečení a doporučení zabezpečení možné použít k ochraně vašich prostředků Azure.

Služba Security Center je přístupná prostřednictvím [portálu Azure](https://azure.microsoft.com/features/azure-portal/). Další informace o portálu Azure Portal najdete v [dokumentaci k portálu](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="permissions"></a>Oprávnění
V Centru zabezpečení zobrazí jenom informace týkající se prostředek v případě jsou přiřazenou roli vlastník, Přispěvatel nebo Čtenář pro předplatné nebo skupinu prostředků, které daný prostředek patří. V tématu [oprávnění v Azure Security Center](security-center-permissions.md) Další informace o rolích a povolených akcí v Centru zabezpečení.

## <a name="data-collection"></a>Shromažďování dat
Security Center shromažďuje data z Azure virtuální počítače (VM) a počítače mimo Azure pro monitorování ohrožení zabezpečení a hrozbami. Data jsou shromažďována pomocí Microsoft Monitoring Agent, který čte různé konfigurace související se zabezpečením a protokoly událostí z počítače a zkopíruje data do pracovního prostoru pro analýzu. Security Center zřizuje agenta Microsoft Monitoring Agent do všech existujících podporované virtuální počítače Azure a všechny nové, které jsou vytvořeny. V tématu [povolení shromažďování dat](security-center-enable-data-collection.md) Další informace o tom, jak shromažďování dat funguje.

Automatické zřizování se důrazně doporučuje a je vyžadovaný pro odběry ve standardní vrstvě služby Security Center. Zakázání automatické zřizování omezení sledování zabezpečení pro vaše prostředky.

V tématu [Security Center ceny](security-center-pricing.md) Další informace o volné a cenové úrovně Standard.

Následující kroky popisují, jak získat přístup ke komponentám služby Security Center a jak je používat.

> [!NOTE]
> Tento článek vám tuto službu představí formou ukázkového nasazení. Tento článek není podrobný průvodce.
>
>

## <a name="access-security-center"></a>Přístup ke službě Security Center
Na portálu použijte pro přístup ke službě Security Center následující kroky:

1. V nabídce **Microsoft Azure** vyberte **Security Center**.

   ![Nabídky Azure][1]
2. Pokud se službou Security Center pracujete poprvé, otevře se okno **Vítejte**. Vyberte **spuštění Security Center** otevřete **Security Center**.
   ![Obrazovka Vítejte][10]
3. Po spuštění Security Center z okna úvodní nebo vyberte v nabídce Microsoft Azure Security Center **Security Center** otevře. Pro zajištění snadného přístupu k oknu **Security Center** v budoucnu vyberte možnost **Připnout okno na řídicí panel** (vpravo nahoře).
   ![Možnost Připnout okno na řídicí panel][2]

## <a name="use-security-center"></a>Použití služby Security Center
Můžete nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure. Nakonfigurujme zásady zabezpečení pro vaše předplatné:

1. V hlavní nabídce Security Center, vyberte **zásady zabezpečení**.
2. V části **Security Center – zásady zabezpečení**, vyberte předplatné.
3. V části **zásady zabezpečení – shromažďování dat**, **automatické zřizování** je zapnutý. Security Center zřizuje agenta Microsoft Monitoring Agent do všech existujících podporované virtuální počítače Azure a všechny nové, které jsou vytvořeny.

    ![Zásady zabezpečení][12]

4. Na **zásady zabezpečení** okně vyberte komponentu zásad **zásady zabezpečení**.

     ![Zásady zabezpečení][11]

5. V části **zobrazit doporučení pro**, zapnout doporučení, které chcete zobrazit jako součást vaše zásady zabezpečení. Příklady:

   * Nastavení **aktualizací systému** k **na** kontroly všechny podporované virtuální počítače pro chybějící aktualizace operačního systému.
   * Nastavení **ohrožení zabezpečení operačního systému** k **na** kontroly všechny podporované virtuální počítače vyhledají konfigurace operačního systému, které může zvýšit virtuálního počítače vůči útokům.

6. Vyberte **Uložit**.

### <a name="view-recommendations"></a>Zobrazení doporučení
1. Vraťte se do okna **Security Center** a vyberte dlaždici **Doporučení**. Security Center pravidelně analyzuje stav zabezpečení vašich prostředků Azure. Když služba Security Center identifikuje potenciální ohrožení zabezpečení, zobrazí doporučení v okně **Doporučení**.
   ![Doporučení ve službě Azure Security Center][5]
2. Po výběru doporučení v okně **Doporučení** se zobrazí další informace nebo budete moci provést akci vedoucí k vyřešení daného problému.

### <a name="view-the-security-state-of-your-resources"></a>Zobrazit stav zabezpečení vašich prostředků
1. Vraťte se do okna **Security Center**. **Prevence** část řídicího panelu obsahuje indikátory stavu zabezpečení pro virtuální počítače, sítě, datům a aplikacím.
2. Vyberte **výpočetní** zobrazíte další informace. **Výpočetní** otevře se okno zobrazuje tři karty:

  - **Přehled** – obsahuje, monitorování a doporučení pro virtuální počítače.
  - **Virtuální počítače a počítače** -uvádí aktuální stav zabezpečení všech virtuálních počítačů a počítačů.
  - **Cloudové služby** -obsahuje seznam webových a pracovních rolí, které jsou monitorovány pomocí služby Security Center.

    ![Výpočetní stav zabezpečení][6]

3. Na **přehled** vyberte doporučení v části můžete zobrazit další informace nebo akce vedoucí ke konfiguraci příslušných ovládacích prvků.
4. Na **virtuálních počítačů a počítačů** , vyberte prostředek, k zobrazení dalších podrobností.

### <a name="view-security-alerts"></a>Zobrazení výstrah zabezpečení
1. Vraťte se do okna **Security Center** a vyberte dlaždici **Výstrahy zabezpečení**. Otevře se okno **Výstrahy zabezpečení** se seznamem výstrah. Tyto výstrahy generuje služba Security Center při analýze protokolů zabezpečení a síťové aktivity. Součástí jsou i výstrahy z integrovaných partnerských řešení.
   ![Výstrahy zabezpečení ve službě Azure Security Center][7]

2. Výběrem výstrahy zobrazíte další informace. V tomto příkladu budeme vyberte **upravil systému binární zjištěných ve výpisu filtru**. Otevřou se okna s dalšími podrobnostmi o příslušné výstraze.
   ![Podrobnosti výstrah zabezpečení ve službě Azure Security Center][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Zobrazení stavu partnerských řešení
1. Vraťte se do okna **Security Center**. **Řešení zabezpečení** dlaždice umožňuje přehledně, sledovat stav vašich partnerských řešení integrovaných ve vašem předplatném Azure.
2. Vyberte **řešení zabezpečení** dlaždici. Otevře se okno se seznamem partnerských řešení připojených k Security Center.
   ![Partnerská řešení][9]
3. Vyberte jedno partnerské řešení. Otevře se okno se stavem tohoto partnerského řešení a jeho přidruženými prostředky. Výběrem možnosti **Solution console** (Konzola řešení) otevřete prostředí pro správu tohoto partnerského řešení.

   ![Partnerská řešení][13]

## <a name="next-steps"></a>Další kroky
Tento článek vám poskytl úvod do komponent správy zásad a monitorování zabezpečení služby Security Center. Teď už jste se se službou Security Center seznámili a můžete vyzkoušet následující kroky:

* Konfigurace zásad zabezpečení pro vaše předplatné Azure, najdete v části [nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md).
* Pomocí doporučení v Centru zabezpečení můžete chránit prostředky v Azure najdete v tématu [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md).
* Zkontrolujte a aktuální výstrahy zabezpečení, najdete v části [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md).
- Další informace o integraci s partnery zvýšení celkové zabezpečení, najdete v článku [partnera a integrace řešení](security-center-partner-integration.md).
- Zjistěte, jak se spravuje dat a zabezpečení ve službě Security Center, najdete v části [zabezpečení dat v Azure Security Center](security-center-data-security.md).
* Přečtěte si další informace o [funkcích pokročilé detekce hrozeb](security-center-detection-capabilities.md) poskytovaných spolu s [úrovní Standard](security-center-pricing.md) ve službě Security Center. Úroveň Standard se bezplatně nabízí pro dobu prvních 60 dní.
* Pokud máte dotazy týkající se použití služby Security Center, přečtěte si článek [Azure Security Center – nejčastější dotazy](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
[11]: ./media/security-center-get-started/show-recommendations-for.png
[12]: ./media/security-center-get-started/automatic-provisioning.png
[13]: ./media/security-center-get-started/partner-solutions-detail.png
