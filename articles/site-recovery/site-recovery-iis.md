---
title: "Replikovat vícevrstvé IIS na základě webové aplikace pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak k replikaci adresáře služby IIS webové farmy virtuálních počítačů pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.openlocfilehash: 00d5c1fa8c0c16daef5d928147e169553672e1f6
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="replicate-a-multi-tier-iis-based-web-application-using-azure-site-recovery"></a>Replikovat vícevrstvé aplikace webové služby IIS na základě pomocí Azure Site Recovery

## <a name="overview"></a>Přehled


Software, aplikace je modul produktivitu v organizaci. Různé webových aplikací můžete v organizaci slouží k jiným účelům. Některé z těchto jako mzdy zpracování, finanční aplikace a zákazníků, kterým čelí weby může být co nejvíce kritické pro organizaci. Je důležité pro organizaci je a ve spuštění časy, aby se zabránilo ztrátě produktivity a důležitější zabránit škody do bitové kopie brand organizace.

Kritické webové aplikace jsou obvykle nastavit jako vícevrstvé aplikace s web, databázi a aplikace na různých vrstev. Kromě se šíří přes různých úrovní, aplikace může také používat více serverů v jednotlivých vrstvách provoz vyrovnávat zatížení. Kromě toho mapování mezi různými úrovněmi a na webovém serveru může být založené na statických IP adres. Na převzetí služeb při selhání některé z těchto mapování potřeba aktualizovat, zejména, pokud máte více webů, které jsou nakonfigurované na webovém serveru. Pokud webových aplikací používat protokol SSL, je potřeba aktualizovat vazby certifikátu.

Tradiční bez replikace na základě obnovení metody zahrnují zálohování různé konfigurační soubory, nastavení registru, vazby, vlastní komponent (COM nebo rozhraní .NET), obsahu, a také certifikáty a obnovit soubory pomocí sady ruční kroky. Tyto postupy jsou jasně náročnější, chyba náchylné k chybám a není škálovatelné. Je například možné snadno pro vás být ponecháno s žádný výběr a zapomněli zálohování certifikátů, ale koupit nové certifikáty pro server po převzetí služeb při selhání.

Řešení pro zotavení po havárii funkční, by měla umožnit modelování plány obnovení kolem architekturách složitých aplikací. Musí být také možnost přidat vlastní postup zpracování aplikace mapování mezi různými úrovněmi. Pokud dojde k havárii, to poskytuje řešení opravdu snímek jedním kliknutím vedoucí k nižší RTO.


Tento článek popisuje, jak chránit, webové aplikace služby IIS na základě pomocí [Azure Site Recovery](site-recovery-overview.md). Tento článek popisuje osvědčené postupy pro replikaci tři vrstvy webové aplikace IIS na základě Azure, jak můžete provést postupu zotavení po havárii a jak můžete převzetí služeb při selhání aplikaci do Azure.


## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že rozumíte následující požadavky:

1. [Replikaci virtuálního počítače do Azure](site-recovery-vmware-to-azure.md)
1. Postup [návrh k síti pro obnovení](site-recovery-network-design.md)
1. [Provádění převzetí služeb při selhání do Azure](./site-recovery-test-failover-to-azure.md)
1. [Provádění převzetí služeb při selhání do Azure](site-recovery-failover.md)
1. Postup [replikace řadiče domény](site-recovery-active-directory.md)
1. Postup [replikaci systému SQL Server](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Vzory pro nasazení
Webové aplikace služby IIS na základě obvykle zahrnuje jednu z následujících vzory nasazení:

**Vzor nasazení 1** služby IIS na základě webové farmy se Routing(ARR) požádat o aplikaci, Server služby IIS a Microsoft SQL Server.

![Vzor nasazení](./media/site-recovery-iis/deployment-pattern1.png)

**Vzor nasazení 2** služby IIS na základě webové farmy se Routing(ARR) požádat o aplikaci, Server služby IIS, aplikační Server a Microsoft SQL Server.


![Vzor nasazení](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Podpora pro obnovení lokality

Pro účely vytváření tohoto článku, se používají virtuální počítače VMware s serveru IIS verze 7.5 na Windows Server 2012 R2 Enterprise. Replikace obnovení lokality je nezávislá na aplikace, doporučení uvedená tady se očekává počkejte následující scénáře také a pro různé verze služby IIS.

### <a name="source-and-target"></a>Zdroj a cíl

**Scénář** | **Sekundární lokality** | **To Azure**
--- | --- | ---
**Hyper-V** | Ano | Ano
**VMware** | Ano | Ano
**Fyzický server** | Ne | Ano
**Azure**|Není k dispozici|Ano

## <a name="replicate-virtual-machines"></a>Replikace virtuálních počítačů

Postupujte podle [v tomto návodu](site-recovery-vmware-to-azure.md) k zahájení replikace všech IIS webové farmy virtuálních počítačů do Azure.

Pokud používáte statickou IP adresu, zadejte IP adresa, která má virtuální počítač přijmout [ **cílová IP adresa** ](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties) nastavení v nastavení výpočtů a sítě.

![Cílové IP](./media/site-recovery-active-directory/dns-target-ip.png)


## <a name="creating-a-recovery-plan"></a>Vytvoření plánu obnovení

Plán obnovení umožňuje pořadí převzetí služeb při selhání v různých vrstvách vícevrstvé aplikace, proto zachování konzistence aplikace. Tady jsou kroky k vytvoření plánu obnovení vícevrstvých webových aplikací.  [Další informace o vytváření plánu obnovení](./site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>Probíhá přidávání virtuálních počítačů do skupin převzetí služeb při selhání
Typické vícevrstvé aplikace webové služby IIS se skládá z vrstvy databáze SQL virtuálních počítačů, se webová úroveň zřízená serveru IIS a aplikační vrstvy. Přidejte všechny tyto virtuální počítače do jiné skupiny založené na vrstvě podle údaje v následující kroky. [Další informace o přizpůsobení plán obnovení](site-recovery-runbook-automation.md#customize-the-recovery-plan).

1. Vytvoření plánu obnovení. Přidáte virtuální počítače vrstvy databáze v 1. skupina zajistit, že jsou vypnutí poslední a první zapínají.

1. Přidáte virtuální počítače vrstvy aplikace ve skupině 2 tak, aby se zapínají po byla zapínají databázové vrstvy.

1. Přidáte virtuální počítače vrstvy webové skupiny 3 tak, aby se zapínají po byla zapínají aplikační vrstvě.

1. Přidejte virtuální počítače Vyrovnávání zatížení do skupiny 4 tak, aby se zapínají po byla zapínají webovou vrstvu.


### <a name="adding-scripts-to-the-recovery-plan"></a>Probíhá přidávání skriptů do plánu obnovení
Musíte udělat některé operace na virtuálních počítačích Azure post převzetí služeb při selhání a testovací převzetí služeb při selhání aby IIS webové farmy funkce správně. Můžete automatizovat převzetí služeb při selhání operaci post jako aktualizaci položky DNS, změna vazbu webu, můžete změnit v připojovacím řetězci přidáním příslušných skriptů v plánu obnovení, jak je uvedeno níže. [Další informace o přidání skript plánu obnovení](./site-recovery-how-to-add-vmmscript.md).

#### <a name="dns-update"></a>DNS update
Pokud DNS je nakonfigurován pro dynamické aktualizace DNS, pak po spuštění virtuálních počítačů obvykle aktualizovat DNS s novou IP Adresou. Pokud chcete přidat na explicitní krok Aktualizovat DNS novou IP adresy virtuálních počítačů, přidejte to [skript pro aktualizaci IP ve službě DNS](https://aka.ms/asr-dns-update) jako akce post na skupiny plánu obnovení.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Připojovací řetězec v souboru web.config aplikace
Připojovací řetězec Určuje databázi, která komunikuje webu.

Pokud připojovací řetězec, používá název virtuálního počítače databáze, nejsou žádné další kroky potřebné post převzetí služeb při selhání. Aplikace může automaticky komunikovat k databázi. Navíc pokud se uchovávají IP adresu pro virtuální počítač databáze, se nebude potřeba aktualizovat připojovací řetězec. Pokud připojovací řetězec odkazuje na virtuální počítač databáze pomocí IP adresy, musí se jednat o aktualizované post převzetí služeb při selhání. Například následující připojovací řetězec odkazuje na databázi s IP Adresou 127.0.1.2

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Připojovací řetězec v webovou vrstvu můžete aktualizovat přidáním [skript aktualizace připojení služby IIS](https://aka.ms/asr-update-webtier-script-classic) po 3. skupina v plánu obnovení.

#### <a name="site-bindings-for-the-application"></a>Vazby webu pro aplikaci
Každé lokalitě se skládá z vazby informace, které zahrnují typ vazby, IP adresu, kdy server služby IIS čeká na požadavky na lokalitu, číslo portu a názvy hostitelů pro tuto lokalitu. Během převzetí služeb při selhání může tyto vazby potřeba aktualizovat, pokud dojde ke změně ve sloupci IP adresa s nimi spojených.

> [!NOTE]
>
> Pokud jste označili 'všechny nepřiřazené' pro vazbu webu jako v příkladu níže, nemusíte aktualizovat post převzetí této vazby. Navíc pokud IP adresa spojená s lokalitou nedojde ke změně po převzetí služeb při selhání, vazby webu nemusí být aktualizována (uchování IP adresy závisí na architektuře sítě a podsítě přidělené k lokalitám primárními a obnovovacími a proto může nebo nemusí být v rámci výpočetních procesů pro vaši organizaci.)

![Vazba SSL](./media/site-recovery-iis/sslbinding.png)

Pokud IP adresa byla přidružena lokalitu, musíte aktualizovat všechny vazby webu s novou IP adresu. Můžete přidat [webové vrstvy aktualizační skript](https://aka.ms/asr-web-tier-update-runbook-classic) po 3. skupina v plánu obnovení ke změně vazeb webu.


#### <a name="update-load-balancer-ip-address"></a>Aktualizujte IP adresa služby Vyrovnávání zatížení
Pokud máte virtuální počítač směrování žádostí na aplikace, přidejte [směrování žádostí na aplikace služby IIS převzetí služeb při selhání skriptu](https://aka.ms/asr-iis-arrtier-failover-script-classic) po 4 skupiny aktualizovat IP adresu.

#### <a name="the-ssl-cert-binding-for-an-https-connection"></a>Vazby certifikátu SSL pro připojení https
Weby může mít přidružený certifikát SSL, který pomáhá zajistit zabezpečenou komunikaci mezi webovém serveru a prohlížeče uživatele. Pokud má web přiřazeno připojení https a vazbu https přidružené lokality na IP adresu serveru služby IIS s vazbou certifikátu SSL, novou vazbu webu musí být přidán pro certifikát s IP Adresou převzetí služeb při selhání post v IIS virtuálního počítače.

Certifikát SSL se dá vydat proti-

(a) na plně kvalifikovaný název domény webu<br>
b) název serveru<br>
c) certifikát se zástupným znakem pro název domény<br>
d) IP adresu – Pokud je certifikát SSL vydaný pro IP serveru IIS, jiný certifikát SSL musí být vydaný pro IP adresu serveru služby IIS na webu Azure a další vazby SSL pro tento certifikát musí být vytvořen. Proto se doporučuje nepoužívat certifikát SSL vydaný pro IP. Tato možnost je méně používaných a bude brzy zastaralá podle nové změny fórum certifikační Autority nebo prohlížeče.

#### <a name="update-the-dependency-between-the-web-and-the-application-tier"></a>Aktualizace závislosti mezi webové a aplikační vrstvě
Pokud máte závislosti konkrétní aplikace na základě IP adresy virtuálních počítačů, musíte aktualizovat tuto závislost převzetí post.

## <a name="doing-a-test-failover"></a>Provádění testovací převzetí služeb
Postupujte podle [v tomto návodu](site-recovery-test-failover-to-azure.md) provedete testovací převzetí služeb.

1.  Přejděte na portál Azure a vyberte trezoru služby zotavení.
1.  Klikněte na plán obnovení pro webové farmy IIS vytvořit.
1.  Klikněte na "Testovací převzetí služeb".
1.  Vyberte bod obnovení a virtuální síť Azure ke spuštění procesu testovací převzetí služeb při selhání.
1.  Jakmile sekundární prostředí zapnutý, můžete provést vaše ověření.
1.  Po dokončení se k ověření, můžete vybrat ověření dokončení a vyčištění prostředí testovacího převzetí služeb při selhání.

## <a name="doing-a-failover"></a>Převzetím služeb
Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.

1.  Přejděte na portál Azure a vyberte trezoru služby zotavení.
1.  Klikněte na plán obnovení pro webové farmy IIS vytvořit.
1.  Klikněte na 'Převzetí služeb při selhání'.
1.  Vyberte bod obnovení se spustit proces převzetí služeb při selhání.

## <a name="next-steps"></a>Další postup
Další informace o [replikovat jiné aplikace](site-recovery-workload.md) pomocí Site Recovery.
