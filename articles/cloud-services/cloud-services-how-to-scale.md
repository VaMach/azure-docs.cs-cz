---
title: "Automatické škálování cloudové služby na portálu classic | Microsoft Docs"
description: "(klasické) Další informace o použití portálu classic konfigurace pravidel automatického škálování pro cloudové služby webovou roli nebo role pracovního procesu v Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: eb167d70-4eba-42a4-b157-d8d0688abf4b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 90d55bbac6e113d6add848ace67cf0749e26342b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-classic-portal"></a>Postup konfigurace automatického škálování pro cloudové služby na portálu classic
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-scale-portal.md)
> * [Portál Azure Classic](cloud-services-how-to-scale.md)

Na stránce škálování portálu Azure classic můžete nakonfigurovat nastavení automatického škálování pro vaši webovou roli nebo role pracovního procesu. Alternativně můžete nakonfigurovat, ruční škálování místo založeného na pravidlech automatické škálování.

> [!NOTE]
> Tento článek se zaměřuje na webových a pracovních rolí cloudové služby. Když vytvoříte virtuální počítač (klasický) přímo, je hostovaná v cloudové službě. Některé z těchto informací se vztahuje na tyto typy virtuálních počítačů. Škálování skupinu dostupnosti virtuálních počítačů se právě vypíná je zapnout a vypnout na základě pravidel škálování, které nakonfigurujete. Další informace o virtuální počítače a skupiny dostupnosti najdete v tématu [Správa dostupnosti virtuálních počítačů](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

Před konfigurací škálování pro vaši aplikaci je třeba zvážit následující informace:

* Škálování má vliv základní použití.

    Větší instance rolí pomocí více jader. Je možné škálovat aplikaci pouze v rámci maximální počet jader pro předplatné. Řekněme například, že vaše předplatné může mít 20 jader. Pokud spouštíte aplikaci pomocí dvou středních cloudové služby (celkem 4 jádra), můžete pouze postupně škálovat ostatních nasazení cloudové služby ve vašem předplatném ve zbývajících 16 jader. Další informace o velikosti najdete v tématu [cloudové služby velikosti](cloud-services-sizes-specs.md).

* Musíte vytvořit frontu a přidružte ji k roli předtím, než je možné škálovat aplikaci pro zprávu prahovou hodnotu. Další informace najdete v tématu [jak používat fronty služby úložiště](../storage/queues/storage-dotnet-how-to-use-queues.md).

* Je možné škálovat prostředky, které jsou propojeny s cloudové služby. Další informace o propojení prostředků, najdete v tématu [postupy: odkaz prostředek cloudové služby](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

* Povolit vysokou dostupnost vaší aplikace, se ujistěte, že je nasazený s dvěma nebo více instancí role. Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

## <a name="schedule-scaling"></a>Plánování škálování
Ve výchozím nastavení všechny role neprovádějte určitého plánu. Proto žádné nastavení změnit platit pro všechny časy a všechny dny po celý rok. Pokud chcete, můžete nastavit ruční nebo automatické škálování pro jednu z následujících režimů:

* Dny v týdnu
* Víkendů
* Týden noci
* Ráno týden
* Konkrétní kalendářní data
* Konkrétní rozsahy dat

Nastavení plánu je nakonfigurované [portál Azure classic](https://manage.windowsazure.com/) na  
**Cloudové služby** > **\[cloudové služby\]** > **škálování**  >   **\[Produkční nebo pracovní\]**  stránky.

Klikněte **časů plánu** tlačítko pro každou roli, kterou chcete změnit.

![Cloudové služby Automatické škálování podle plánu][scale_schedules]

## <a name="manual-scale"></a>Ruční škálování
Na **škálování** stránky, můžete ručně zvýšit nebo snížit počet spuštěných instancí v cloudové službě. Toto nastavení je nakonfigurovat pro každý plán, který jste vytvořili nebo celou dobu, pokud jste dosud nevytvořili plánu.

1. V [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a pak klikněte na název cloudové služby za účelem otevře řídicí panel.
   
   > [!TIP]
   > Pokud nevidíte cloudové služby, budete muset změnit z **produkční** k **pracovní** nebo naopak.

2. Klikněte na tlačítko **škálování**.
3. Vyberte plán, kterou chcete změnit možnosti škálování. *Ne naplánovaném čase* je výchozí, pokud máte žádné plány definované.
4. Najít **škálování podle metriky** a vyberte **NONE**. Toto nastavení je výchozí nastavení pro všechny role.
5. Každá role v rámci cloudové služby má jezdce pro změnu počtu instancí používat.
   
    ![Ručně škálovat, roli cloudové služby][manual_scale]
   
    Pokud potřebujete více instancí, budete muset změnit [cloudu velikost virtuálního počítače služby](cloud-services-sizes-specs.md).
6. Klikněte na **Uložit**.  
   Instance role jsou přidat nebo odebrat závislosti na vybrané položky.

> [!TIP]
> Vždy, když se zobrazí ![][tip_icon] umožňující ho a můžete získat nápovědu ke konkrétní nastavení nepodporuje.

## <a name="automatic-scale---cpu"></a>Automatické škálování - procesoru
Tento režim škáluje, pokud průměrná procento využití procesoru přejde nad nebo pod zadaných prahových hodnot. Instance role jsou vytvořené nebo odstraněné v takovém případě.

1. V [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a pak klikněte na název cloudové služby za účelem otevře řídicí panel.
   
   > [!TIP]
   > Pokud nevidíte cloudové služby, budete muset změnit z **produkční** k **pracovní** nebo naopak.

2. Klikněte na tlačítko **škálování**.
3. Vyberte plán, kterou chcete změnit možnosti škálování. *Ne naplánovaném čase* je výchozí, pokud máte žádné plány definované.
4. Najít **škálování podle metriky** a vyberte **procesoru**.
5. Teď můžete nakonfigurovat minimální a maximální rozsah instancí role, cíl využití procesoru (pro aktivaci škálování nahoru) a jak velký počet instancí škálovat nahoru a dolů pomocí.

![Škálování role cloudové služby podle zatížení procesoru][cpu_scale]

> [!TIP]
> Vždy, když se zobrazí ![][tip_icon] umožňující ho a můžete získat nápovědu ke konkrétní nastavení nepodporuje.

## <a name="automatic-scale---queue"></a>Automatické škálování - fronty
Tento režim automaticky přizpůsobí, pokud počet zpráv ve frontě přejde nad nebo pod zadanou prahovou hodnotu. Instance role jsou vytvořené nebo odstraněné v takovém případě.

1. V [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a pak klikněte na název cloudové služby za účelem otevře řídicí panel.
   
   > [!TIP]
   > Pokud nevidíte cloudové služby, budete muset změnit z **produkční** k **pracovní** nebo naopak.

2. Klikněte na tlačítko **škálování**.
3. Najít **škálování podle metriky** a vyberte **fronty**.
4. Teď můžete nakonfigurovat minimální a maximální rozsah instancí role, fronty a počet zpráv fronty pro zpracování pro každou instanci a kolik instancí škálovat nahoru a dolů pomocí.

![Škálovat roli cloudové služby, protože fronta zpráv][queue_scale]

> [!TIP]
> Vždy, když se zobrazí ![][tip_icon] umožňující ho a můžete získat nápovědu ke konkrétní nastavení nepodporuje.

## <a name="scale-linked-resources"></a>Škálování odkazované zdroje.
Často při změně měřítka roli, je výhodné se škálovat databázi, který také používá aplikace. Pokud jste databáze ke cloudové službě, můžete kliknutím na příslušný odkaz přístup nastavení škálování pro tento prostředek.

1. V [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a pak klikněte na název cloudové služby za účelem otevře řídicí panel.
   
   > [!TIP]
   > Pokud nevidíte cloudové služby, budete muset změnit z **produkční** k **pracovní** nebo naopak.

2. Klikněte na tlačítko **škálování**.
3. Najít **propojené prostředky** části a klikněte na tlačítko **spravovat škálování pro tuto databázi**.
   
   > [!NOTE]
   > Pokud se nezobrazí **propojené prostředky** části pravděpodobně nemáte žádné propojené prostředky.

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
