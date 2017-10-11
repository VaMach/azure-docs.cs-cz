---
title: "Řídicí panel služby StorSimple Manager | Microsoft Docs"
description: "Popisuje řídicí panel služby StorSimple Manager a vysvětluje, jak použít jej k monitorování stavu vašeho řešení StorSimple."
services: storsimple
documentationcenter: 
author: SharS
manager: carmonm
editor: 
ms.assetid: fb0f131d-d60b-45d7-ace2-56d0502e6627
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: 596431b7279b753ca4da838eb028cdde2022ce02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-dashboard"></a>Pomocí řídicího panelu služby StorSimple Manager
## <a name="overview"></a>Přehled
Stránka řídicího panelu služby StorSimple Manager poskytuje přehled o všech zařízení, které jsou připojené ke službě StorSimple Manager zvýraznění ty, které vyžadují pozornost. správce systému. Tento kurz představuje stránka řídicího panelu, popisuje obsah řídicího panelu a funkce a popisuje úlohy, které můžete provést z této stránky.

![Řídicí panel služby](./media/storsimple-service-dashboard/HCS_ServiceDashboard.png)

Řídicí panel služby StorSimple Manager zobrazí následující informace:

* V **grafu** oblasti, uvidíte graf relevantní metriky pro vaše zařízení. Můžete zobrazit primárního úložiště (místně ukotvena a víceúrovňová) používané v rámci celé všechna zařízení, jakož i cloudového úložiště využívat zařízení v časovém intervalu. Zadejte 1 týden, měsíc 1, 3 měsíců nebo 1 rok časové rozmezí pomocí ovládacích prvků v pravém horním rohu grafu.
* **Přehled využití** zobrazuje primární úložiště, které je zřízený a využívat všech zařízení relativně k celkové úložiště k dispozici ve všech zařízeních. **Zřízení** odkazuje na velikost úložiště, která je připravená a přidělení k použití, při **používá** odkazuje na využití svazků, jak je vidět iniciátory, které jsou připojené k zařízení.
* **Výstrahy** oblasti obsahuje snímek všech aktivních výstrah ve všech zařízeních, seskupené podle závažnosti výstrah. Kliknutím na úroveň závažnosti otevře **výstrahy** stránky, rozsah zobrazit tyto výstrahy. Na **výstrahy** stránky, můžete kliknout na jednotlivé výstrahy zobrazíte další podrobnosti o této výstraze, včetně všechny doporučené akce. Pokud byl problém vyřešen, zrušte výstrahy.
* **Úlohy** oblasti poskytuje snímek posledních úloh na všech zařízeních, které jsou připojené k vaší službě. Existují odkazy, které můžete se podívat na úlohy, které jsou aktuálně probíhá, zařízení, která se nezdařila v posledních 24 hodin nebo ty, které jsou naplánované spuštění v dalších 24 hodin.
* **Rychlého přehledu** oblasti poskytuje užitečné informace jako je například stav služby, počet zařízení připojených k service, umístění služby a podrobnosti předplatného, která souvisí se službou. Je také odkaz na protokol operací. Kliknutím na odkaz zobrazíte seznam všech dokončených operací služby StorSimple Manager.

Stránka řídicího panelu služby StorSimple Manager můžete spustit následující úlohy:

* Zobrazení nebo znovu vygenerovat registrační klíč služby.
* Změna šifrovacího klíče dat služby.
* Zobrazte protokoly operací.

## <a name="view-or-regenerate-the-service-registration-key"></a>Zobrazení nebo znovu vygenerovat registrační klíč služby
Registrační klíč služby slouží k registraci zařízení s Microsoft Azure StorSimple u služby StorSimple Manager tak, aby zařízení se zobrazí na portálu Azure classic další akce správy. Klíč je vytvořen na první zařízení a sdílet s ostatními zařízení.

Kliknutím na tlačítko **registrační klíč** (v dolní části stránky) otevře **registrační klíč služby** dialogové okno, kde můžete kopírovat aktuální registrační klíč služby do schránky nebo znovu vygenerovat registrační klíč služby.

Obnovuje se klíč neovlivní dříve zaregistrovaný zařízení: ovlivňuje pouze zařízení, která jsou zaregistrovaná ve službě service po znovu vygeneruje klíč.

Další informace o prohlížení a generování registrační klíč služby, přejděte na [získat registrační klíč služby](storsimple-manage-service.md#get-the-service-registration-key).

## <a name="change-the-service-data-encryption-key"></a>Změna šifrovacího klíče dat služby
Šifrovací klíče dat služby slouží k šifrování dat důvěrné zákazníka, jako je například přihlašovací údaje účtu úložiště, které se odesílají ze služby StorSimple Manager zařízení StorSimple. Je potřeba pravidelně měnit tyto klíče, pokud má vaše organizace IT zásad střídání klíče na zařízení úložiště. Proces změny klíče může být mírně lišit podle toho, jestli je jednoho zařízení nebo více zařízení spravovaná pomocí služby StorSimple Manager.

Změna šifrovacího klíče dat služby je proces, krok 3:

1. Zařízení, které chcete změnit šifrovacího klíče dat služby pomocí portálu Azure classic, autorizovat.
2. Pomocí Windows Powershellu pro StorSimple, zahajte změnu klíče šifrování dat služby.
3. Pokud máte více než jedno zařízení StorSimple, aktualizujte šifrovacího klíče dat služby na jiných zařízení.

Následující kroky popisují proces výměna šifrovacího klíče dat služby.

[!INCLUDE [storsimple-change-data-encryption-key](../../includes/storsimple-change-data-encryption-key.md)]

## <a name="view-the-operations-logs"></a>Zobrazit protokoly operací
Protokoly operací můžete zobrazit kliknutím na odkaz protokoly operaci, která je k dispozici v **rychlého přehledu** na řídicím panelu. Tím přejdete na stránku služby správy, kde můžete filtrovat a najdete v protokolech určité k vaší službě StorSimple Manager.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [řešení zařízení StorSimple](storsimple-troubleshoot-operational-device.md).
* Další informace o tom, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

