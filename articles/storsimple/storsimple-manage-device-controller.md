---
title: "Správa řadiče zařízení StorSimple | Microsoft Docs"
description: "Naučte se zastavit, vypnout, restartovat nebo obnovit řadičů zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4ee989d0-956f-4c14-951e-fd4e490ea09d
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 8182d0ccf9fb57a9dba383d376d1c557845bcc8a
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>Spravovat vaše řadiče zařízení StorSimple
> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Pokud chcete zobrazit verzi tohoto článku na nový portál Azure, přejděte na [spravovat vaše řadiče zařízení StorSimple](storsimple-8000-manage-device-controller.md). Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Přehled
Tento kurz popisuje různé operace, které lze provést na řadiče zařízení StorSimple. Řadiče v zařízení StorSimple jsou řadiče redundantní (sdílené) v konfiguraci aktivní pasivní. V daném okamžiku jenom jeden řadič je aktivní a zpracovává všechny diskových a síťových operací. Jiný řadič je v pasivním režimu. Pokud řadič active selže, stane řadičem pasivní aktivní automaticky.

Tento kurz zahrnuje podrobné pokyny ke správě řadiče zařízení pomocí:

* **Řadiče** části **údržby** stránky ve službě StorSimple Manager
* Prostředí Windows PowerShell pro StorSimple.

Doporučujeme vám, že spravujete řadiče zařízení prostřednictvím služby StorSimple Manager. Pokud se akce lze provést pouze pomocí prostředí Windows PowerShell pro StorSimple, tento kurz provede poznamenejte si ho.

Po přečtení tohoto kurzu, budete moci:

* Restartování nebo vypnutí řadič zařízení StorSimple
* Vypněte zařízení StorSimple
* Obnovit výchozí tovární nastavení v zařízení StorSimple

## <a name="restart-or-shut-down-a-single-controller"></a>Restartování nebo vypnutí jediného kontroleru
Řadič restartování nebo vypnutí není vyžadován jako součást operace normální systému. Operace vypnutí pro jedno zařízení řadiče jsou běžné pouze v případech, ve kterých hardwarová komponenta selhání zařízení vyžaduje nahrazení. V situaci, ve kterém ovlivňuje výkon nadměrného využití paměti nebo chybně fungující řadič může také vyžadovat restartování řadiče. Může také musíte restartovat řadič po úspěšné řadiče náhradní, pokud chcete povolit a testovací nahrazené kontroler.

Restartování zařízení není rušivé iniciátorům připojené, za předpokladu, že je k dispozici pasivní řadič. Pokud není pasivní řadič k dispozici nebo zapnuté vypnout a potom restartování řadič active může vést k přerušení služby a výpadky.

> [!IMPORTANT]
> * **Běžícího řadiče nikdy odeberte fyzicky jako to může vést ke ztrátě redundance a k vyššímu riziku výpadku.**
> * Následující postup platí jenom pro fyzického zařízení StorSimple. Informace o tom, jak spuštění, zastavení a restartování virtuálního zařízení najdete v tématu [pracovat s virtuálním zařízením](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).
> 
> 

Můžete restartovat nebo vypnout řadič jedno zařízení pomocí portálu Azure classic služby StorSimple Manager nebo prostředí Windows PowerShell pro StorSimple

Ke správě řadičů zařízení z portálu Azure classic, proveďte následující kroky.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Chcete-li restartovat nebo vypnout řadič portálu classic
1. Přejděte na **zařízení > údržby**.
2. Přejděte na **stavu hardwaru** a ověřte, zda je stav řadičů na vašem zařízení **stavu v pořádku**.
   
    ![Ověřte, zda jsou v pořádku řadiče zařízení StorSimple](./media/storsimple-manage-device-controller/IC766017.png)
3. V dolní části **údržby** klikněte na tlačítko **Správa řadičů**.
   
    ![Správa řadiče zařízení StorSimple](./media/storsimple-manage-device-controller/IC766018.png)</br>
   
   > [!NOTE]
   > Pokud nevidíte **Správa řadičů**, pak je potřeba nainstalovat aktualizace. Další informace najdete v tématu [aktualizace zařízení StorSimple](storsimple-update-device.md).
   > 
   > 
4. V **změnit nastavení řadiče** dialogové okno pole, postupujte takto:
   
   1. Z **vyberte řadiče** rozevíracího seznamu, vyberte řadič, kterou chcete spravovat. Možnosti jsou řadič 0 a řadič 1. Tyto řadiče se také identifikují aktivní nebo pasivní.
      
      > [!NOTE]
      > Řadič nelze spravovat, pokud je k dispozici nebo je zapnuté vypnout, a nebude se zobrazovat v rozevíracím seznamu.
      > 
      > 
   2. Z **vybrat akci** rozevíracího seznamu vyberte **restartování řadiče** nebo **vypněte řadič**.
      
       ![Restartujte řadič pasivní zařízení StorSimple](./media/storsimple-manage-device-controller/IC766020.png)
   3. Klikněte na ikonu zaškrtnutí ![Ikona zaškrtnutí](./media/storsimple-manage-device-controller/IC740895.png).

To bude restartování nebo vypnutí kontroleru. Následující tabulka shrnuje podrobnosti co se stane, že v závislosti na vybrané možnosti provedení v **změnit nastavení řadiče** dialogové okno.  

| Výběr # | Pokud zvolíte možnost... | To se stane. |
| --- | --- | --- |
| 1. |Restartujte řadič pasivní. |Pro restartujte řadič se vytvoří úlohu, a po úspěšném vytvoření úlohy se zobrazí oznámení. Tím zahájíte restartování řadiče. Můžete monitorovat proces restartování díky přístupu k **služby > řídicí panel > Zobrazit protokoly operací** a pak filtrování podle parametrů, které jsou specifické pro vaši službu. |
| 2. |Restartujte řadič active. |Zobrazí se následující upozornění: "Pokud restartujete řadič active, zařízení bude převzetí služeb při selhání řadičem pasivní. Opravdu chcete pokračovat?" </br>Pokud se rozhodnete pokračovat v provádění této operace, další kroky budou stejné jako ty používané k restartování řadičem pasivní (viz výběr 1). |
| 3. |Vypněte pasivní řadič. |Zobrazí se následující zpráva: "po dokončení vypnutí musíte stisknout tlačítko power na vašem řadiči ji zapnout. Opravdu že chcete vypnout tento řadič?" </br>Pokud se rozhodnete pokračovat v provádění této operace, další kroky budou stejné jako ty používané k restartování řadičem pasivní (viz výběr 1). |
| 4. |Vypněte aktivní řadiče. |Zobrazí se následující zpráva: "po dokončení vypnutí musíte stisknout tlačítko power na vašem řadiči ji zapnout. Opravdu že chcete vypnout tento řadič?" </br>Pokud se rozhodnete pokračovat v provádění této operace, další kroky budou stejné jako ty používané k restartování řadičem pasivní (viz výběr 1). |

#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Chcete-li restartovat nebo vypnout řadič ve Windows Powershellu pro StorSimple
Proveďte následující postup vypnout nebo restartovat jedním řadičem zařízení StorSimple z portálu Azure classic.

1. Přístup k zařízení pomocí konzoly sériového portu nebo relace Telnetu ze vzdáleného počítače. Připojit k řadič 0 nebo 1 řadiče pomocí kroků v [použití klienta PuTTY k připojení ke konzole sériového portu zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup**.
3. Zpráva hlavičky, poznamenejte si řadiče jste připojeni k (řadič 0 nebo 1 řadič) a jestli je aktivní nebo pasivní řadič (pohotovostní).
   
   * Chcete-li vypnout jeden řadič, v příkazovém řádku zadejte:
     
       `Stop-HcsController`
     
       To se zastaví kontroler, který jste připojeni k. Pokud zastavíte řadič active, pak ji bude převzetí služeb při selhání řadičem pasivní než vypne.
   * Restartujte řadič do příkazového řádku zadejte:
     
       `Restart-HcsController`
     
       Restartuje tento kontroler, který jste připojeni k. Pokud restartujete řadič active, ji budou převzetí služeb při selhání řadičem pasivní před restartováním počítače.

## <a name="shut-down-a-storsimple-device"></a>Vypněte zařízení StorSimple
Tato část vysvětluje, jak vypnout spuštěný nebo selhání zařízení StorSimple ze vzdáleného počítače. Zařízení je vypnutý po vypnutí oběma řadičům zařízení. Vypnutí zařízení se provádí, když zařízení, fyzicky přesunete nebo je vyřazeno z provozu.

> [!IMPORTANT]
> Před vypnutím zařízení, zkontrolujte stav součásti zařízení. Přejděte na **zařízení > údržby > stavu hardwaru** a ověřte, zda je stav DIODU všechny součásti zelená. Zelený stav bude mít jenom zařízení v pořádku. Pokud vaše zařízení je vypínán dolů k nahrazení chybně fungující komponenty, zobrazí se sníženou (žlutý) stav pro příslušné součásti nebo se nezdařilo (červený).
> 
> 

#### <a name="to-shut-down-a-storsimple-device"></a>Vypnutí zařízení StorSimple
1. Použití [restartování nebo vypnutí řadič](#restart-or-shut-down-a-single-controller) postup identifikovat a vypněte pasivní řadič na vašem zařízení. Tuto operaci může provést na portálu Azure classic nebo v prostředí Windows PowerShell pro StorSimple.
2. Opakujte předchozí krok vypnout aktivní řadiče.
3. Teď bude potřeba podívejte se na pozadí roviny tohoto zařízení. Po dva řadiče jsou zcela vypnout, by měl být stav LED na obou řadičích blikat red. Pokud potřebujete vypnout zařízení zcela v tuto chvíli překlopit hlavní přepínače na napájení a chlazení moduly (PCMs) na pozici OFF. To by měl vypnout zařízení.

<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>V zařízení resetovat výchozí tovární nastavení
> [!IMPORTANT]
> Pokud potřebujete obnovit v zařízení výchozí tovární nastavení, obraťte se na Microsoft Support. Níže popsaný postup by měl použít pouze ve spojení s Microsoft Support.
> 
> 

Tento postup popisuje postup resetování zařízení s Microsoft Azure StorSimple výchozí tovární nastavení pomocí Windows Powershellu pro StorSimple.
Když zařízení resetujete, odeberete všechna data a nastavení z celý cluster ve výchozím nastavení.

Proveďte následující postup resetování zařízení s Microsoft Azure StorSimple výchozí tovární nastavení:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Chcete-li obnovit zařízení do výchozího nastavení v prostředí Windows PowerShell pro StorSimple
1. Přístup k zařízení pomocí jeho konzoly sériového portu. Zkontrolujte zprávy a informační zpráva k zajištění, že jste připojeni k aktivnímu řadiči.
2. V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup**.
3. Do příkazového řádku zadejte následující příkaz k resetování celý cluster, odebrání všech nastavení řadiče, metadat a dat:
   
    `Reset-HcsFactoryDefault`
   
    Chcete-li místo toho resetovat jediného kontroleru, použijte [resetování HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) rutiny s `-scope` parametr.)
   
    Systém se restartuje vícekrát. Při obnovení byla úspěšně dokončena, budete upozorněni. V závislosti na modelu systému může trvat 45 – 60 minut pro zařízení s 8100 a 60 až 90 minut pro 8600 k dokončení tohoto procesu.
   
   > [!TIP]
   > * Pokud používáte aktualizace 1.2 nebo dříve použít `–SkipFirmwareVersionCheck` parametr tak, aby přeskočil kontrolu verze firmwaru (v opačném případě se zobrazí chybová zpráva o neshodě firmware: Obnovení továrního nastavení nemůže pokračovat z důvodu neshody v verzí firmwaru. ).
   > * Postup resetování factory mohou být neúspěšné pro zařízení StorSimple, které běží na portálu Government Update 1 nebo 1.1 a provedli úspěšné jednoduchá nebo duální řadiče nahrazení (s nahrazení řadiče, které byly součástí softwaru před aktualizací 1). To se stane, když objekt factory resetovat, že obrázek je ověřit přítomnost souboru SHA1 na kontroler, který neexistuje před aktualizací softwaru 1. Pokud je v že tomto objektu pro vytváření resetovat selhání, obraťte se na Microsoft Support a pomůže vám při další kroky. Tento problém není seznámili s nahrazení řadiče, které byly odeslány z objektu pro vytváření Update 1 nebo novější softwaru.
   > 
   > 

## <a name="questions-and-answers-about-managing-device-controllers"></a>Otázky a odpovědi týkající se správy řadiče zařízení
V této části jsme vytvořili souhrn některé nejčastější dotazy ohledně správy řadiče zařízení StorSimple.

**Otázka:** Co se stane, když oběma řadičům na zařízení jsou v pořádku a zapnuté na a I restartování nebo vypnutí řadič active?

**Odpověď:** Pokud jsou oba řadiče v zařízení v pořádku a zapnuté, zobrazí se výzva k potvrzení. Můžete se rozhodnout pro:

* **Restartujte řadič active** – budete informováni, že restartování řadič služby active způsobí převzetí služeb při selhání pasivní řadiče zařízení. Kontroler se restartuje.
* **Vypnout aktivní řadiče** – budete informováni, že vypíná řadič služby active dojde k výpadkům. Musíte se také stisknout tlačítko napájení v zařízení zapnout na řadiči.

**Otázka:** Co se stane, pokud je k dispozici nebo je zapnuté pasivní řadič na zařízení vypnout a I restartování nebo vypnutí řadič active?

**Odpověď:** Pokud je pasivní řadič na vašem zařízení není k dispozici nebo zapnuté vypnout a zvolíte možnost:

* **Restartujte řadič active** – budete informováni, že budete pokračovat operaci bude mít za následek dočasné přerušení služby, a zobrazí se výzva k potvrzení.
* **Vypnout aktivní řadiče** – budete informováni, že budete pokračovat operaci, dojde k výpadkům a že budete muset push tlačítka napájení na jeden nebo oba řadiče zapnutí zařízení. Zobrazí se výzva k potvrzení.

**Otázka:** Pokud nemá řadič restartování nebo vypnutí nepodaří průběhu?

**Odpověď:** Restartování nebo vypnutí řadič může selhat, pokud:

* Probíhá aktualizace zařízení.
* Již probíhá restartování řadiče.
* Vypnutí řadiče již probíhá.

**Otázka:** Jak můžete zjistit, pokud byl restartován nebo vypnut řadič?

**Odpověď:** Můžete zkontrolovat stav řadiče na stránce údržby. Stav řadiče označí, zda řadič byl restartován nebo vypnut. Kromě toho stránky výstrah bude obsahovat vyvolání informační výstrahy, pokud řadičem byl restartován nebo vypnut. Operace restart a vypnutí řadiče se zaznamenávají také protokoly operací. Další informace o protokoly operací, přejděte na [zobrazit protokoly operací](storsimple-service-dashboard.md#view-the-operations-logs).

**Otázka:** Je k dispozici žádný vliv na vstupně-výstupních operací v důsledku řadiče převzetí služeb při selhání?

**Odpověď:** Připojení TCP mezi iniciátorů a aktivní řadiče se resetuje v důsledku řadiče převzetí služeb při selhání, ale se znovu vytvoří, když je pasivní řadič předpokládá operace. Během této operace může být pozastavení dočasných (méně než 30 sekund) v vstupně-výstupní aktivitu mezi iniciátorů a zařízení.

**Otázka:** Jak se vrátit Moje řadič služby po byla vypnout a odebrat?

**Odpověď:** Pokud chcete vrátit řadič služby, je nutné vložit ji do skříň jak je popsáno v [nahradit modul řadiče zařízení StorSimple](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Další kroky
* Pokud narazíte na potíže s řadičů zařízení StorSimple, které nelze vyřešit pomocí postupů uvedených v tomto kurzu [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).
* Další informace o používání služby StorSimple Manager, přejděte na [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

