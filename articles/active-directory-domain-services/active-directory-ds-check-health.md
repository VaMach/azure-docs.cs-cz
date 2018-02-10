---
title: "Služba Azure AD Domain Services – zkontrolujte stav vaší spravované domény | Microsoft Docs"
description: "Zkontrolujte stav vaší spravované domény pomocí stránky stavu na portálu Azure."
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: ergreenl
ms.openlocfilehash: 4adbce0305bdc1a9b261f5cf644fad876d101bc6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="azure-ad-domain-services---check-the-health-of-your-managed-domain"></a>Služba Azure AD Domain Services – zkontrolujte stav vaší spravované domény

## <a name="your-domains-health"></a>Vaše doména stavu

Pomocí stránky stavu na portálu Azure, budete moci zachovat aktuální na co se děje na vaší spravované domény. V tomto článku kroky prostřednictvím všechny prvky stav stránky a můžete naučit, jak zajistit, vaše doména je v tip-top tvaru.

### <a name="view-your-health-page"></a>Zobrazit stránku stavu

1. Přejděte na stránku služby Azure AD Domain Services [portálu Azure](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices)
2. Klikněte na domény, které chcete zobrazit stav.
3. V levém navigačním panelu klikněte na tlačítko "Stavu".

Na následujícím obrázku je příklad stránky stavu.

![Příklad stavu stránky](.\media\active-directory-domain-services-alerts\health-page.png)

>[!NOTE]
> Vaše doména stavu je vyhodnocován kolem každou hodinu. Po provedení změn vaší spravované domény, musíte počkat, až po aktualizaci stavu na další cyklus vyhodnocení zobrazení vaší doméně. Můžete zkontrolovat, kdy bylo naposled vyhodnoceno doménu pomocí "Posledního vyhodnocení" časového razítka umístěný v pravém horním rohu.
>

### <a name="status-of-your-managed-domain"></a>Stav vaší spravované domény

Tento stav v horní části stránky napravo od vaší stavu označuje, celkový stav vaší domény. Stav faktory všechny existující výstrahy ve vaší doméně. Stav vaší domény můžete také zobrazit na stránce Přehled služby Azure AD Domain Services.

Stavy, které jsou spravované domény:

| Status | Ikona | Vysvětlení |
| --- | :----: | --- |
| Běží | <img src= ".\media\active-directory-domain-services-alerts\running-icon.png" width = "15"> | Spravované domény běží bez problémů a nemá žádné výstrahy kritický nebo upozornění. Tato doména je pravděpodobně informační výstrahy. |
| Vyžaduje pozornost (upozornění) | <img src= ".\media\active-directory-domain-services-alerts\warning-icon.png" width = "15"> | Neexistují žádné kritické výstrahy na vaší spravované domény, ale existují jedné nebo několika výstrah upozornění, které je třeba vzít v úvahu. |
| Vyžaduje pozornost (kritická) | <img src= ".\media\active-directory-domain-services-alerts\critical-icon.png" width = "15"> | Na vaší spravované domény je jeden nebo více kritické výstrahy. Můžete mít rovněž varování nebo informační výstrahy. |
| Nasazování | <img src= ".\media\active-directory-domain-services-alerts\deploying-icon.png" width = "15"> | Vaše doména je právě nasazované. |

## <a name="monitors"></a>Monitory

Monitorování podrobnosti některé aspekty o vaší spravované domény, který dohlížel Azure AD Domain Services. Vyřešte všechny výstrahy na vaší spravované domény je nejlepší způsob, jak udržovat vaše monitorování v dobrém stavu.

Jsou k dispozici monitorování:
 - Backup
 - Synchronizace v AAD

### <a name="backup"></a>Backup

Tato funkce sleduje jak často jsme provést zálohování vaší spravované domény. Následuje tabulka, která vysvětluje podrobnosti sloupec zálohování monitorování a co hodnoty by měla být očekávána.

| Hodnota podrobností | Vysvětlení |
| --- | --- |
|"Dosud nezálohovali" | Tento stav je normální pro nově vytvořené domény. První zálohování se vytvoří obvykle po 24 hodinách. Pokud není nově vytvořená vaší spravované domény nebo jsou v tomto stavu pro neobvyklé množství času, [obraťte se na podporu](active-directory-ds-contact-us.md). |
| Poslední zálohy před 1 až 14 dny. | Obecně platí to je očekávané hodnotě zálohování monitorování. |
| Poslední zálohy před více než 14 dny. | Kdykoli delší než dva týdny je neobvykle dlouhou dobu od poslední zálohy. První, vyřešte všechny výstrahy, které se zobrazí na vaší spravované domény a pak v případě problém pořád opakuje, [obraťte se na podporu](active-directory-ds-contact-us.md). |


### <a name="synchronization-with-azure-ad"></a>Synchronizace se službou Azure AD

Microsoft uchovává informace o tom, jak často je vaší spravované domény synchronizovat se službou Azure Active Directory. Množství uživatelů ve vaší spravované domény a také množství změn provedených od poslední synchronizace může obě ovlivnit jak dlouho může trvat interval synchronizace. Obecně platí, pokud byla delší než tří dnů od poslední synchronizace, se doporučuje [obraťte se na podporu](active-directory-ds-contact-us.md).

## <a name="alerts"></a>Výstrahy

Výstrahy jsou problémy s u vaší spravované domény, který třeba se zabývat v pořadí pro Azure AD Domain Services ke spuštění. Každá výstraha popisuje problém a obsahuje adresu URL, která popisuje konkrétní kroky pro řešení problému. Chcete-li zobrazit všechny výstrahy a jejich řešení, navštivte [řešení výstrahy](active-directory-ds-troubleshoot-alerts.md) článku.

### <a name="severity"></a>Závažnost
Výstrahy jsou rozdělené do tří různé úrovně závažnosti: kritické, upozornění a informativní.

 * **Kritické výstrahy** jsou problémy, které vážně ovlivnit vaší spravované domény. Tyto výstrahy by se řešit okamžitě, jak Microsoft nelze monitorovat, spravovat, opravy a synchronizovat vaší spravované domény.
 * **Upozorňující výstrahy** může být problémy, které může mít vliv na vaši doménu v budoucnu, ale nejsou "nejnovější nutně" služby. Tyto výstrahy popisují osvědčené postupy a poskytněte návrhy k ochraně vaší spravované domény.
 * **Informační výstrahy** jsou oznámení, které nejsou, které mají vliv negativně vaší domény. Informační výstrahy jsou navržené tak, aby je obeznámen s co se děje v doméně a Azure AD Domain Services.

## <a name="next-steps"></a>Další postup
- [Vyřešení výstrahy na vaší spravované domény](active-directory-ds-troubleshoot-alerts.md)
- [Další informace o Azure AD Domain Services](active-directory-ds-features.md)
- [Kontaktujte nás](active-directory-ds-contact-us.md)
