---
title: "Nastavit zásadu replikace pro replikaci technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak si nastavte zásady pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokalita VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5d9b79cf-89f2-4af9-ac8e-3a32ad8c6c4d
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: a3ecc555e049914d7bffdddce7b0522f09362d24
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-8-set-up-a-replication-policy"></a>Krok 8: Nastavení zásad replikace

Po dokončení konfigurace [mapování sítě](vmm-to-vmm-walkthrough-network-mapping.md), použijte tento článek nastavit zásada replikace pro virtuální počítač (VM) replikace Hyper-V do sekundární lokality pomocí [Azure Site Recovery](site-recovery-overview.md).

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

- Když vytvoříte zásadu replikace, všechny hostitele pomocí zásad, musí mít stejný operační systém. Cloudu VMM může obsahovat hostitelů Hyper-V s různými verzemi systému Windows Server, ale v takovém případě musíte víc zásad replikace.
- Můžete provádět počáteční replikaci v režimu offline.

## <a name="configure-replication-settings"></a>Konfigurace nastavení replikace

1. Pokud chcete vytvořit novou zásadu replikace, klikněte na **Připravit infrastrukturu** > **Nastavení replikace** > **+Vytvořit a přidružit**.

    ![Síť](./media/vmm-to-vmm-walkthrough-replication/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady. Typ zdroje a cíle musí být **technologie Hyper-V**.
3. V **verze hostitele technologie Hyper-V**, vyberte, jaký operační systém běží na hostiteli.
4. V **typ ověřování** a **port ověřování**, zadejte, jak ověření přenosů mezi primárními a obnovovacími hostitelské servery technologie Hyper-V. Vyberte **certifikát** pouze tehdy, je pracovní prostředí protokolu Kerberos. Azure Site Recovery automaticky nakonfiguruje certifikáty pro ověřování pomocí protokolu HTTPS. Nemusíte dělat nic ručně. Ve výchozím nastavení bude otevřen port 8083 a 8084 (pro certifikáty) v bráně Windows Firewall na hostitelských serverech technologie Hyper-V. Pokud vyberete **Kerberos**, lístek protokolu Kerberos se použije pro vzájemné ověřování serverů hostitele. Všimněte si, že toto nastavení platí jen pro hostitelské servery technologie Hyper-V v systému Windows Server 2012 R2.
5. V části **Frekvence kopírování** určete, jak často chcete replikovat rozdílová data po počáteční replikaci (každých 30 sekund, 5 minut nebo 15 minut).
6. V části **Uchování bodu obnovení** zadejte (v hodinách), jak dlouhý bude interval uchovávání dat pro jednotlivé body obnovení. Chráněné počítače je možné obnovit do libovolného bodu v rámci tohoto intervalu.
7. V **frekvence snímkování konzistentní aplikace vzhledem**, zadejte, jak často (1 – 12 hodin) body obnovení obsahující snímky konzistentní s aplikacemi jsou vytvořeny. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače, a snímek konzistentní vzhledem k aplikacím, který vytvoří snímek dat aplikací ve virtuálním počítači v daném okamžiku. Snímky konzistentní vzhledem k aplikacím využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu. Pokud povolíte snímky konzistentní s aplikacemi, bude mít vliv výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byla hodnota, kterou nastavíte, menší než počet dalších bodů obnovení, které nakonfigurujete.
8. V **kompresi přenosu dat**, zadejte, zda by měl být komprimované replikovaná data, která se přenáší.
9. Vyberte **odstranit repliku virtuálního počítače**, chcete-li určit, že virtuální počítač repliky měla by být odstraněna, pokud zakažte ochranu pro zdrojový virtuální počítač. Pokud povolíte toto nastavení, když zakažte ochranu pro zdrojový virtuální počítač se odebere z konzole Site Recovery, nastavení obnovení lokality pro nástroj VMM se odeberou z konzoly nástroje VMM a replika odstraněn.
10. V **počáteční metodu replikace**, pokud replikujete přes síť, určete, zda chcete spustit počáteční replikaci nebo ji můžete naplánovat. Pokud chcete uložit šířku pásma sítě, můžete chtít naplánovat dobu mimo špičky. Pak klikněte na **OK**.

     ![Zásady replikace](./media/vmm-to-vmm-walkthrough-replication/gs-replication2.png)
11. Když vytvoříte novou zásadu, automaticky se přidruží ke cloudu VMM. V **zásady replikace**, klikněte na tlačítko **OK**. K této zásadě replikace můžete přidružit další Cloudy VMM (a virtuální počítače v nich) **replikace** > název zásady > **přidružit Cloud VMM**.

     ![Zásady replikace](./media/vmm-to-vmm-walkthrough-replication/policy-associate.png)



## <a name="prepare-for-offline-initial-replication"></a>Příprava pro počáteční replikaci prováděnou offline

Můžete provést offline replikaci dat prvotní kopie. Můžete to následujícím způsobem připravit:

* Na zdrojovém serveru zadejte cestu umístění, ze kterého bude export dat probíhat. Přiřaďte úplné řízení pro systém souborů NTFS a sdílené složky oprávnění ke službě VMM na cestu pro export. Na cílovém serveru zadejte cestu umístění, ze kterého dojde k importu. Přiřaďte stejné oprávnění pro tuto cestu importu.
* Pokud je import nebo export cesta sdílená, přiřaďte správce, Power Users, Print Operator nebo operátor serveru členství ve skupině pro účet služby VMM na vzdáleném počítači, na kterém sdílený se nachází.
* Pokud používáte všechny účty spustit jako přidejte hostitele, na cestám pro import a export, přiřaďte pro čtení a oprávnění k zápisu do účty spustit jako v nástroji VMM.
* Import a export sdílené složky nesmí nacházet na libovolném počítači se používá jako server hostitele technologie Hyper-V, protože konfigurace zpětné smyčky není podporována technologií Hyper-V.
* Ve službě Active Directory na každém Hyper-V serveru hostitele, který obsahuje virtuální počítače, který chcete chránit, povolit a nakonfigurovat omezené delegování důvěřovat vzdálených počítačích, na kterých cestám pro import a export nacházejí, následujícím způsobem:
  1. Na řadiči domény otevřete **Active Directory Users and Computers**.
  2. Ve stromu konzoly klikněte na tlačítko **DomainName** > **počítače**.
  3. Klikněte pravým tlačítkem na název serveru hostitele technologie Hyper-V > **vlastnosti**.
  4. Na **delegování** , klikněte na **důvěřovat tomuto počítači pro delegování pouze určeným službám**.
  5. Klikněte na tlačítko **používající libovolný protokol pro ověřování**.
  6. Klikněte na tlačítko **přidat** > **uživatelé a počítače**.
  7. Zadejte název počítače, který je hostitelem cestu pro export > **OK**. Ze seznamu dostupných služeb, podržte stisknutou klávesu CTRL a klikněte na tlačítko **cifs** > **OK**. Opakujte pro název počítače, který je hostitelem cestu importu. Opakujte podle potřeby pro další hostitelské servery technologie Hyper-V.



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 9: povolení replikace](vmm-to-vmm-walkthrough-enable-replication.md).
