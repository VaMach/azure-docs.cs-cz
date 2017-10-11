---
title: "Konfigurace funkce MPIO na hostiteli připojen k poli virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje postup konfigurace funkce Multipath I/O (MPIO) pro pole virtuální zařízení StorSimple připojené k hostitele se systémem Windows Server 2012 R2."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5b7a7f99-ee5b-4b7d-ab32-483a5a1fa504
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/01/2017
ms.author: alkohli
ms.openlocfilehash: c75c6ed40754aee964e2b68f4f569dc1422507f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Konfigurace funkce Multipath I/O v hostiteli systému Windows Server pro pole virtuální zařízení StorSimple
## <a name="overview"></a>Přehled
Tento článek popisuje, jak nainstalovat funkci Multipath I/O (MPIO) na hostiteli s Windows Server, použít specifické nastavení pro svazky jen StorSimple, a potom ověřit funkce MPIO pro svazky zařízení StorSimple. Postupu se předpokládá, že vaše virtuální zařízení StorSimple 1200 pole se dvěma síťovými rozhraními je připojený k hostiteli systému Windows Server se dvěma síťovými rozhraními. Informace obsažené v tomto článku se vztahuje pouze na pole virtuální. Informace o řadu zařízení StorSimple 8000, přejděte na [konfigurace funkce MPIO pro hostitele zařízení StorSimple](storsimple-configure-mpio-windows-server.md). 

Funkci MPIO v systému Windows Server pomáhá sestavení vysoce dostupné a odolné proti chybám úložiště konfigurace. Funkce MPIO používá redundantní komponenty fyzických cesty – adaptéry, kabely a přepínače – k vytvoření logických cest mezi serverem a paměťovým zařízením. Pokud dojde k selhání součásti způsobuje logickou cestu k selhání, logika více cest použije pro vstupně-výstupní operace alternativní cestu, tak, aby aplikace můžete stále přístup k datům. Kromě toho v závislosti na vaší konfiguraci funkce MPIO lze také vylepšit výkon znovu Vyrovnávání zatížení mezi tyto cesty. Další informace najdete v tématu [Přehled funkce MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO přehled a funkce").

Vysoká-dostupnosti vašeho řešení StorSimple konfigurujte funkci MPIO na hostitelích systému Windows Server připojený k virtuální zařízení StorSimple pole (model 1200). Hostitelské servery tolerovat klikněte na odkaz, sítě nebo selhání rozhraní. 

Budete muset postupovat podle těchto kroků konfigurace funkce MPIO: 

* Předpoklady konfigurace
* Krok 1: Instalace funkce MPIO na hostitele Windows serveru
* Krok 2: Konfigurace funkce MPIO pro svazky zařízení StorSimple
* Krok 3: Připojení StorSimple svazky na hostiteli

Každou z výše uvedené kroky popsané v následujících částech.

## <a name="prerequisites"></a>Požadavky
Tato část podrobně popisuje požadavky konfigurace pro Windows Server hostitele a virtuální pole.

### <a name="on-windows-server-host"></a>Na hostitelském Windows serveru
* Ujistěte se, že váš hostitel systému Windows Server má 2 síťových rozhraní, které jsou povolené.

### <a name="on-storsimple-virtual-array"></a>V poli virtuální zařízení StorSimple
* Virtuální pole by měl nakonfigurovaný jako server se službou iSCSI. Další informace najdete v tématu [nastavit virtuální pole jako server se službou iSCSI](storsimple-virtual-array-deploy3-iscsi-setup.md). Jeden nebo víc síťových rozhraní by měla být povolená na pole.   
* Rozhraní sítě na vaše virtuální pole by měly být dostupné z hostitele Windows serveru.
* Na virtuální zařízení StorSimple pole by se vytvořit jeden nebo více svazků. Další informace najdete v tématu [přidat svazek](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume) na pole virtuální zařízení StorSimple. V tomto postupu jsme vytvořili svazky 3 (1 místně vázaný a 2 vrstvené svazky, jak je uvedené níže) v poli virtuální.
  
    ![mpio0](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Konfigurace hardwaru pro pole virtuální zařízení StorSimple
Následující obrázek znázorňuje konfigurace hardwaru pro vysokou dostupnost a vyrovnávání zatížení více cest pro hostitele Windows serveru a pole virtuální zařízení StorSimple použít v tomto postupu.

![Konfigurace funkce MPIO hardwaru](./media/storsimple-virtual-array-configure-mpio-windows-server/1200hardwareconfig.png)

Jak je vidět na předchozím obrázku:

* Pole virtuální zařízení StorSimple zřídit na Hyper-V je zařízením active jeden uzel, který je nakonfigurovaný jako server se službou iSCSI.
* Na pole jsou povolené dvě rozhraní virtuální sítě. V místní webové uživatelské rozhraní vaší virtuální pole, ověřte, zda jsou dvě síťová rozhraní zapnutá přechodem na **nastavení sítě** jak je uvedeno níže:
  
    ![Síťová rozhraní zapnuta 1200](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio9.png)
  
    Poznámka: adresy IPv4 povolené síťové rozhraní (Ethernet, Ethernet 2 ve výchozím nastavení) a uložit pro pozdější použití na hostiteli.
* Dvě síťová rozhraní jsou povolené na hostiteli s Windows Server. Pokud připojené rozhraní pro hostitele a pole jsou ve stejné podsíti, pak bude 4 cesty k dispozici. To se velká písmena v tomto postupu. Ale pokud každé síťové rozhraní na rozhraní pole a hostitele v jiné podsíti protokolu IP (a ne směrovatelné), pak pouze 2 cesty bude k dispozici.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Krok 1: Instalace funkce MPIO na hostitele Windows serveru
Funkce MPIO je volitelná funkce v systému Windows Server a není nainstalována ve výchozím nastavení. Je nutné ji nainstalovat jako funkci pomocí Správce serveru. K instalaci této funkce na hostiteli s Windows Server, proveďte následující postup.

[!INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Krok 2: Konfigurace funkce MPIO pro svazky zařízení StorSimple
Funkce MPIO je potřeba nakonfigurovat tak, aby identifikovat svazky zařízení StorSimple. Konfigurace funkce MPIO rozpoznat svazky zařízení StorSimple, proveďte následující kroky.

[!INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Krok 3: Připojení StorSimple svazky na hostiteli
Po dokončení konfigurace funkce MPIO v systému Windows Server, svazky, které jsou vytvořené na poli StorSimple může být připojen a pak můžete využít výhod funkce MPIO pro redundanci. Připojení svazku, proveďte následující kroky.

#### <a name="to-mount-volumes-on-the-host"></a>Chcete-li připojit svazky na hostiteli
1. Otevřete **vlastnosti iniciátoru iSCSI** okno na hostitele Windows serveru. Přejděte na **správce serveru > řídicí panel > Nástroje > iniciátor iSCSI**.
2. V **vlastnosti iniciátoru iSCSI** dialogové okno, klikněte na tlačítko **zjišťování**a potom klikněte na **zjistit cílový portál**.
3. V **zjistit cílový portál** dialogové okno pole, postupujte takto:
   
    1. Zadejte IP adresu prvního povoleno síťového rozhraní na pole virtuální zařízení StorSimple. Ve výchozím nastavení, bude **Ethernet**. 
    2. Klikněte na tlačítko **OK** se vrátíte do **vlastnosti iniciátoru iSCSI** dialogové okno.
     
    > [!IMPORTANT]
    > Pokud používáte privátní sítě pro připojení iSCSI, zadejte IP adresu portu DATA, která je připojena k privátní síti.
     
4. Opakujte kroky 2 až 3 pro druhý rozhraní sítě (například Ethernet 2) na pole. 
5. Vyberte **cíle** ve **vlastnosti iniciátoru iSCSI** dialogové okno. Pro vaše virtuální pole, měli byste vidět prostor pro každý svazek jako cíl v části **zjištěné cíle**. V takovém případě by být zjištěny tři cíle (odpovídající tři svazky).
   
    ![mpio1](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio1.png)
6. Klikněte na tlačítko **Connect** k vytvoření relace iSCSI s pole virtuální zařízení StorSimple. A **připojit k cíli** se zobrazí dialogové okno. Vyberte **povolit více cestami** zaškrtávací políčko. Klikněte na tlačítko **rozšířené**.
   
    ![mpio2](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio2.png)
7. V **Upřesnit nastavení** dialogové okno pole, postupujte takto:                                        
   
    1. Na **místní adaptér** rozevíracího seznamu vyberte **iniciátor iSCSI společnosti Microsoft**.
    2. Na **iniciátor IP** rozevíracího seznamu vyberte IP adresu hostitele.
    3. Na **cílový portál** IP rozevíracího seznamu vyberte IP rozhraní pole.
    4. Klikněte na tlačítko **OK** se vrátíte do **vlastnosti iniciátoru iSCSI** dialogové okno.
     
        ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

8. Klikněte na **Vlastnosti**. 
   
    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)

9. V **vlastnosti** dialogové okno, klikněte na tlačítko **Přidat relaci**.
   
   ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)
10. V **připojit k cíli** dialogové okno, vyberte **povolit více cestami** zaškrtávací políčko. Klikněte na tlačítko **rozšířené**.
11. V **Upřesnit nastavení** dialogové okno:                                        
    
    1. Na **místní adaptér** rozevíracího seznamu vyberte iniciátor iSCSI společnosti Microsoft.

    2. Na **iniciátor IP** rozevíracího seznamu vyberte IP adresu odpovídající hostitele. V takovém případě se připojujete dvě rozhraní sítě v poli jedno síťové rozhraní na hostiteli. Proto toto rozhraní je stejný jako zadaná pro první relaci.

    3. Na **portál cílová IP adresa** rozevíracího seznamu, vyberte možnost IP adresa pro druhý rozhraní dat, které jsou povolené v poli.

    4. Klikněte na tlačítko **OK** se vraťte do dialogového okna Vlastnosti iniciátoru iSCSI. Druhý relaci jste přidali k cíli.
      
       ![mpio11](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio11.png)
    
    5. Po přidání požadovaných relací (cest), v **vlastnosti iniciátoru iSCSI** dialogovém okně vyberte cíl a klikněte na tlačítko **vlastnosti**. Na kartě relací **vlastnosti** dialogové okno, Poznámka čtyři relace identifikátory, které odpovídají permutací možné cesta. Zrušit relaci, zaškrtněte políčko vedle identifikátor relace a potom klikněte na **odpojení**.

    6. Chcete-li zobrazit zařízení uvedené v rámci relací, vyberte **zařízení** kartě. Ke konfiguraci funkce MPIO zásady pro vybrané zařízení, klikněte na tlačítko **MPIO**.

    7. **Podrobnosti** se zobrazí dialogové okno. Na **MPIO** kartě můžete vybrat odpovídající **zásad vyrovnávání zatížení** nastavení. Můžete také zobrazit **Active** nebo **pohotovostní** typ cesty.
12. Opakujte kroky 8 až 11 přidat další relace (cest) k cíli. Dvě rozhraní na hostiteli a dva na virtuální pole můžete přidat celkem čtyři relace pro každý cíl. 
    
    ![mpio14](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio14.png)
13. Bude potřeba tento postup opakujte pro každý svazek (povrchy jako cíl).
    
    ![mpio15](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio15.png)
14. Otevřete **Správa počítače** přechodem na **správce serveru > řídicí panel > Správa počítače**. V levém podokně klikněte na **úložiště > Správa disků**. Svazky vytvořené v poli virtuální zařízení StorSimple, které jsou viditelné pro tohoto hostitele se zobrazí v části **Správa disků** jako nové disky.
15. Inicializujte disk a vytvořte nový svazek. Během procesu formátu vyberte velikost alokační jednotky (Austrálie) 64 kB. Opakujte postup pro všech dostupných svazků.
    
    ![Správa disků](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio20.png)
16. V části **Správa disků**, klikněte pravým tlačítkem myši **disku** a vyberte **vlastnosti**.
17. V **vlastnosti zařízení s více cestami disku** dialogové okno, klikněte na tlačítko **MPIO** kartě.
    
    ![Vlastnosti disku](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio21.png)
18. V **DSM název** klikněte na tlačítko **podrobnosti** a ověřte, zda jsou parametry nastavena na výchozí parametry. Výchozí parametry jsou:
    
    * Cestu ověřte období = 30
    * Počet opakování = 3
    * PDO odebrat období = 20
    * Interval opakování = 1
    * Ověřte cestu povolené = není zaškrtnuto.
    
    > [!NOTE]
    > **Neměňte výchozí parametry.**
   
## <a name="next-steps"></a>Další kroky
Další informace o [pomocí služby StorSimple Manager zařízení ke správě pole virtuální zařízení StorSimple](storsimple-virtual-array-manager-service-administration.md).

