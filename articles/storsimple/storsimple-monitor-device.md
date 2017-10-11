---
title: "Monitorování zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak používat službu StorSimple Manager k monitorování výkonu, využití kapacity, propustnost sítě a zařízení výkon vstupně-výstupní operace."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: bd4f7704-4f6f-47d0-927a-b1c91eabc453
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: d45bb37c8417785db0ea38be4375a998b6d9f109
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>Použít službu StorSimple Manager k monitorování zařízení StorSimple
## <a name="overview"></a>Přehled
Monitorování konkrétní zařízení v rámci vašeho řešení StorSimple můžete použít službu StorSimple Manager. Můžete vytvořit vlastní grafy na základě vstupně-výstupní výkon, využití kapacity, propustnost sítě a metriky výkonu zařízení. 

K zobrazení informací o monitorování pro určité zařízení, na portálu Azure classic, vyberte službu StorSimple Manager. Klikněte **monitorování** a pak vyberte ze seznamu zařízení. **Monitorování** stránka obsahuje následující informace.

## <a name="io-performance"></a>Vstupně-výstupní výkon
**Vstupně-výstupní výkon** sleduje metriky týkající se počet čtení a zápisu operace mezi serverem buď rozhraní iniciátor iSCSI na hostitelském serveru a zařízení nebo zařízení a cloudem. Tento výkon lze měřit pro konkrétní svazku, kontejner svazků konkrétní nebo všechny kontejnery svazků.

Následující graf zobrazuje vstupně-výstupních operací pro iniciátory pro vaše zařízení pro všechny svazky pro produkční zařízení. Metriky vykreslí čtení a zápisu bajtů za sekundu, čtení a vstupně-výstupní operace za sekundu, zápisu a čtení a zápisu latenci.

![Výkon vstupně-výstupní operace z iniciátor a zařízení](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Pro stejného zařízení jsou zobrazeny vstupně-výstupních operací pro data ze zařízení do cloudu pro všechny kontejnery svazků. Na tomto zařízení data se pouze ve vrstvě lineární a nic se uniknout do cloudu. Neprobíhají žádné operace čtení a zápis, ke kterým dochází ze zařízení do cloudu. Proto jsou vrcholů v grafu v intervalu 5 minut, který odpovídá frekvenci, kdy prezenční signál je zaškrtnuta možnost mezi zařízením a služby. 

![Výkon vstupně-výstupní operace ze zařízení do cloudu](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)

Pro stejné zařízení pořízení snímku cloudu pro svazek data začínající na 2:00 pm. Výsledkem dat odesílaných ze zařízení do cloudu. Čtení zápisy byly zpracovat do cloudu v této hodnotě duration. Vstupně-výstupní operace graf zobrazuje ve špičce různé metriky odpovídající čas při pořízení snímku. 

![Výkon vstupně-výstupní operace pro zařízení do cloudu po cloudový snímek](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)

## <a name="capacity-utilization"></a>Využití kapacity
**Využití kapacity** sleduje metriky týkající se množství prostoru úložiště dat, který je používán svazky, kontejnery svazků nebo zařízení. Můžete vytvořit sestavy na základě využití kapacity primárního úložiště, cloudového úložiště nebo úložiště zařízení. Využití kapacity lze měřit na konkrétním svazku, kontejner svazků konkrétní nebo všechny kontejnery svazků.

Primární, cloudu, a kapacitu úložiště zařízení lze popsat následujícím způsobem:

### <a name="primary-storage-capacity-utilization"></a>Využití kapacity primárního úložiště
Tyto grafy zobrazují množství dat zapsaných na svazky zařízení StorSimple, než je s odstraněním duplicitních dat a komprimované data. Využití primárního úložiště můžete zobrazit všechny svazky nebo pro samostatný svazek.

Při zobrazení grafů využití kapacity svazku primárního úložiště pro všechny svazky a každé jednotlivé svazky a sečtěte primární data v obou těchto případech může být Neshoda mezi dvěma čísly. Celkový počet primární data na všech svazcích nemusí dohromady celkový součet primární data jednotlivé svazky. To může být způsobeno jedním z následujících akcí:

* **Snímek dat, které jsou zahrnuté pro všechny svazky**: Toto chování je vidět jenom v případě, že používáte verzi starší než Update 3. Primární data zobrazená pro všechny svazky, je součet hodnot primární data pro jednotlivé svazky a data snímku. Primární data zobrazená pro daný svazek odpovídá pouze množství dat, které jsou přiděleny na svazek (a neobsahuje odpovídající data snímku svazku).
  
    Můžete to taky vysvětluje podle vzorce:
  
    *Primární data (všech svazků) = součet (primární data (svazku i) + velikost dat snímku (svazku i))*
  
    *WHERE, primární data (svazku i) = velikost přidělené do svazku i primární dat*
  
    Pokud jsou snímky odstranit prostřednictvím služby, se odstranění asynchronně provede na pozadí. Může trvat nějakou dobu velikost dat svazku aktualizovat po odstranění snímku. 
  
    Pokud verzi Update 3 nebo novější, data snímku není zahrnutý v datech svazku. A primární využití se vypočítává takto:
  
    * Primární data (všech svazků) = součet (primární dat (svazku i)
  
    *WHERE, primární data (svazku i) = velikost přidělené do svazku i primární dat*
* **Svazky s monitorováním zakázáno součástí všech svazků**: Pokud máte svazky na vašem zařízení, pro které monitorování je vypnutý, data monitorování pro tyto jednotlivé svazky nebudete mít k dispozici v grafech. Data pro všechny svazky v grafu však obsahuje svazky, pro které monitorování je vypnutý. 
* **Odstranit svazky s za provozu přidružených záloh, které jsou zahrnuté pro všechny svazky**: Pokud svazky obsahující data snímku se odstraní, ale přidružené snímky stále existují, pak mohou se zobrazit neshody.
* **Odstranit svazky, které jsou zahrnuté pro všechny svazky**: V některých případech může existovat starých svazků, i když tyto byly odstraněny. Není-li zobrazit důsledky odstranění a zařízení může zobrazovat nižší dostupné kapacity. Budete muset kontaktovat Microsoft Support odebrat tyto svazky.

Následující graf zobrazuje využití kapacity úložiště primární zařízení StorSimple, před a po pořízení snímku cloudu. Protože se právě objem dat, cloudový snímek neměli měnit primárního úložiště. Jak vidíte, graf zobrazuje žádný rozdíl ve využití primární kapacity v důsledku pořizování snímku cloudu. Cloudový snímek zahájená přibližně 2:00 pm na daném zařízení.

![Využití kapacity primární před cloudový snímek](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Využití kapacity primární po cloudový snímek](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Pokud používáte Update 2 nebo vyšší, lze rozčlenit využití kapacity primárního úložiště ve svazku, všechny svazky, všechny vrstvené svazky a všechny místně vázaných svazků jak je uvedeno níže. Rozdělení podle všechny místně vázaných svazků vám umožní rychle zjistit, kolik místní vrstvy se používá.

![Využití primární kapacity pro všechny místně vázaných svazků](./media/storsimple-monitor-device/localvolumes.png)

### <a name="cloud-storage-capacity-utilization"></a>Využití kapacity úložiště cloudu
Tyto grafy zobrazují množství používá úložiště v cloudu. Tato data se odstraňují a komprimované. Toto množství zahrnuje cloudových snímků, které může obsahovat data, která se nereflektují v jakýkoli svazek, primární a je uložen pro účely starší verze nebo požadované uchování. Můžete porovnat primární a cloudu údajů spotřeba úložiště, kde získáte představu snížení míry data, i když počet nesmí být přesná. Následující graf zobrazuje zařízení StorSimple před a po pořízení snímku cloudu využití kapacity úložiště cloudu. Cloudový snímek zahájená přibližně 2:00 pm na daném zařízení a zobrazí se využití kapacity cloudu snímek ve stejnou dobu, zvýšení z 5.73 MB na 4.04 GB.

![Využití kapacity cloudu před cloudový snímek](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Využití kapacity cloudu po cloudový snímek](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)

### <a name="device-storage-capacity-utilization"></a>Využití kapacity úložiště zařízení
Tyto grafy zobrazují celkové využití pro zařízení, který může mít více než primárního úložiště využití, protože obsahuje lineární vrstvy SSD. Tato úroveň obsahuje množství dat, která také existuje na zařízení je jiné úrovně. Kapacity ve vrstvě SSD lineární je procházení tak, aby nová data se dodává, stará data bude přesunuta do vrstvy HDD (kdy je s odstraněním duplicitních dat a komprimované) a následně do cloudu.

V průběhu času využití primární kapacitu a využití kapacity zařízení pravděpodobně zvýší společně dokud data začne být rozvrstvena do cloudu. V tomto bodě využití kapacity zařízení pravděpodobně začnou stagnovat, ale jako další data se zapisují, zvýší se využití primární kapacity.

Následující graf zobrazuje využití kapacity úložiště primární zařízení StorSimple, před a po pořízení snímku cloudu. Cloudový snímek zahájená 14:00:00 a využití kapacity zařízení spuštění snížení v daném čase. Využití kapacity úložiště zařízení snížil z 11.58 GB na 7.48 GB. To označuje, že s největší pravděpodobností nekomprimovaných dat ve vrstvě SSD lineární bylo odstranění duplicit, komprimované a přesunout do vrstvy HDD. Všimněte si, že pokud zařízení už má velké množství dat SSD a HDD vrstev, nemusíte vidět tento pokles. V tomto příkladu zařízení má malou část data.

![Využití kapacity zařízení před cloudový snímek](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Využití kapacity zařízení po cloudový snímek](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)

## <a name="network-throughput"></a>Propustnost sítě
**Propustnost sítě** sleduje metriky týkající se množství dat přenesených z rozhraní sítě iniciátor iSCSI na hostitelském serveru a zařízení a mezi zařízením a cloudem. Tato metrika pro každou síťová rozhraní iSCSI můžete sledovat na vašem zařízení.

Následující graf zobrazuje propustnost sítě pro Data 0 a Data 4 síťových rozhraní 1 GbE i na vašem zařízení. V tomto případě Data 0 byl povolenou podporu cloudu zatímco 4 dat byla iSCSI povolený. Pro zařízení StorSimple uvidíte příchozí i odchozí provoz. Ploché řádek v tabulce od 15:24:00 je díky tomu, že jsme shromažďování dat pouze každých 5 minut a třeba ji ignorovat. 

![Propustnost sítě pro Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Propustnost sítě pro Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)

## <a name="device-performance"></a>Výkon zařízení
**Výkon zařízení** sleduje metriky týkající se výkonu vašeho zařízení. Následující graf zobrazuje statistik využití procesoru pro zařízení v provozním prostředí.

![Využití procesoru pro zařízení](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [pomocí řídicího panelu zařízení služby StorSimple Manager](storsimple-device-dashboard.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

