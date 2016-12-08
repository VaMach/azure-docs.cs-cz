---
title: "Virtuální zařízení StorSimple Update 2| Dokumentace Microsoftu"
description: "Naučte se vytvářet, nasazovat a spravovat virtuální zařízení StorSimple ve službě Microsoft Azure Virtual Network. (Platí pro StorSimple Update 2)."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: f37752a5-cd0c-479b-bef2-ac2c724bcc37
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 35b0d0e7dd73852900384c34b8b842754434cc93
ms.openlocfilehash: 4fbdb64918a642dbe899ab8b606fbf58e1fb94d3


---
# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Nasazení a správa virtuálního zařízení StorSimple v Azure
## <a name="overview"></a>Přehled
Virtuální zařízení řady StorSimple 8000 je další schopností, která se dodává s vaším řešením Microsoft Azure StorSimple. Virtuální zařízení StorSimple běží na virtuálním počítači ve službě Microsoft Azure Virtual Network a slouží k zálohování a klonování dat z hostitelů. Tento návod popisuje, jak nasadit a spravovat virtuální zařízení v Azure, a vztahuje se na všechna virtuální zařízení používající verzi softwaru Update 2 nebo nižší.

#### <a name="virtual-device-model-comparison"></a>Porovnání modelů virtuálního zařízení
Virtuální zařízení StorSimple je k dispozici ve dvou modelech – standardní 8010 (dříve označovaný jako 1100) a prémiový 8020 (zaveden v Update 2). Porovnání obou modelů je v následující tabulce.

| Model zařízení | 8010<sup>1</sup> | 8020 |
| --- | --- | --- |
| **Maximální kapacita** |30 TB |64 TB |
| **Virtuální počítač Azure** |Standard_A3 (4 jádra, 7 GB paměti) |Standard_DS3 (4 jádra, 14 GB paměti) |
| **Kompatibilita verzí** |Verze používající software před Update 2 nebo novější |Verze používající software Update 2 nebo novější |
| **Dostupnost v oblastech** |Všechny oblasti Azure |Oblasti Azure, které podporují službu Premium Storage<br></br>Seznam oblastí naleznete v tématu [podporované oblasti pro 8020](#supported-regions-for-8020) |
| **Typ úložiště** |Pro místní disky používá službu Azure Standard Storage<br></br> Zjistěte, jak [vytvořit účet služby Standard Storage](../storage/storage-create-storage-account.md) |Pro místní disky používá Azure Premium Storage.<sup>2</sup> <br></br>Zjistěte, jak [vytvořit účet služby Premium Storage](../storage/storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk) |
| **Pokyny týkající se úloh** |Načítání souborů ze zálohy na úrovni položek |Scénáře vývoje a testování v cloudu, nízká latence, náročnější úlohy <br></br>Sekundární zařízení pro zotavení po havárii |

<sup>1</sup> *Dříve označované jako 1100*.

<sup>2</sup> *Pro cloudovou vrstvu používají zařízení 8010 i 8020 službu Azure Standard Storage. Rozdíl je pouze u místní vrstvy v rámci zařízení*.

#### <a name="supported-regions-for-8020"></a>Podporované oblasti pro 8020
Oblasti služby Premium Storage, které jsou aktuálně podporovány pro zařízení 8020, jsou uvedeny v následující tabulce. Tento seznam bude průběžně aktualizován s tím, jak bude služba Premium Storage k dispozici v dalších oblastech. 

| Sér. č. | Aktuálně podporované v oblastech |
| --- | --- |
| 1 |Střed USA |
| 2 |Východ USA |
| 3 |Východ USA 2 |
| 4 |Západ USA |
| 5 |Severní Evropa |
| 6 |Západní Evropa |
| 7 |Jihovýchodní Asie |
| 8 |Japonsko – východ |
| 9 |Japonsko – západ |
| 10 |Austrálie – východ |
| 11 |Austrálie – jihovýchod* |
| 12 |Východní Asie* |
| 13 |Střed USA – jih* |

*V těchto oblastech byla služba Premium Storage uvedena nedávno.

Tento článek popisuje podrobný postup nasazení virtuálního zařízení StorSimple v Azure. Po přečtení tohoto článku:

* Budete chápat, jak se liší virtuální zařízení od fyzického zařízení.
* Budete schopni vytvořit a nakonfigurovat virtuální zařízení.
* Připojíte se k virtuálnímu zařízení.
* Naučíte se pracovat s virtuálním zařízením.

Tento kurz se vztahuje na všechna virtuální zařízení StorSimple používající software verze Update 2 nebo vyšší. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Jak se liší virtuální zařízení od fyzického zařízení
Virtuální zařízení StorSimple je čistě softwarová verze zařízení StorSimple, která běží na jednom uzlu virtuálního počítače Microsoft Azure. Virtuální zařízení podporuje scénáře zotavení po havárii, ve kterých fyzické zařízení není k dispozici, a je vhodné pro použití v načítání na úrovni položek ze zálohy, místní zotavení po havárii a scénáře vývoje a testování v cloudu.

#### <a name="differences-from-the-physical-device"></a>Rozdíl oproti fyzickému zařízení
V následující tabulce jsou uvedeny některé hlavní rozdíly mezi virtuálním zařízením StorSimple a fyzickým zařízením StorSimple.

|  | Fyzické zařízení | Virtuální zařízení |
| --- | --- | --- |
| **Umístění** |Nachází se v datovém centru. |Běží v Azure. |
| **Síťová rozhraní** |Má šest síťových rozhraní: DATA 0 až DATA 5. |Má pouze jedno síťové rozhraní: DATA 0. |
| **Registrace** |Registruje se během konfigurace. |Registrace je samostatná úloha. |
| **Šifrovací klíč dat služby** |Obnovte na fyzickém zařízení a potom aktualizujte virtuální zařízení pomocí nového klíče. |Nelze znovu vygenerovat z virtuálního zařízení. |

## <a name="prerequisites-for-the-virtual-device"></a>Požadavky na virtuální zařízení
Následující části popisují požadavky na konfiguraci pro virtuální zařízení StorSimple. Před nasazením virtuálního zařízení si přečtěte [požadavky na zabezpečení pro použití virtuálních zařízení](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Požadavky na Azure
Než zřídíte virtuální zařízení, je třeba provést následující přípravy v prostředí Azure:

* Pro virtuální zařízení [nakonfigurujte virtuální síť na Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Používáte-li službu Premium Storage, musíte vytvořit virtuální síť v oblasti Azure, která podporuje službu Premium Storage. Další informace o [oblastech, které jsou aktuálně podporovány pro 8020](#supported-regions-for-8020).
* Je vhodné použít výchozí server DNS poskytovaný platformou Azure místo zadávání vlastního názvu serveru DNS. Pokud název serveru DNS není platný nebo pokud server DNS není schopen správně přeložit IP adresy, vytvoření virtuálního zařízení se nezdaří.
* Připojení point-to-site a site-to-site jsou volitelná, ale nejsou vyžadována. Pokud chcete, můžete nastavit tyto možnosti pro pokročilejší scénáře. 
* Můžete vytvořit [virtuální počítače Azure Virtual Machines](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (hostitelské servery) ve virtuální síti, které mohou používat svazky vystavené virtuálním zařízením. Tyto servery musí splňovat následující požadavky:                             
  
  * Být virtuální počítače Windows nebo Linux s nainstalovaným softwarem iniciátoru iSCSI.
  * Běžet ve stejné virtuální síti jako virtuální zařízení.
  * Moci se připojit k cíli iSCSI virtuálního zařízení prostřednictvím interní IP adresy virtuálního zařízení.
* Ujistěte se, že jste nakonfigurovali podporu pro přenosy iSCSI a cloudové přenosy ve stejné virtuální síti.

#### <a name="storsimple-requirements"></a>Požadavky na StorSimple
Před vytvořením virtuálního zařízení aktualizujte svoji službu Azure StorSimple následujícím způsobem:

* Přidejte [záznamy řízení přístupu](storsimple-manage-acrs.md) pro virtuální počítače, které budou představovat hostitelské servery pro virtuální zařízení.
* Nastavte používání [účtu úložiště](storsimple-manage-storage-accounts.md#add-a-storage-account) ve stejné oblasti jako virtuální zařízení. Účty úložiště v jiných oblastech mohou vést k nižšímu výkonu. U virtuálního zařízení můžete použít účet služby Standard Storage nebo Premium Storage. Další informace o tom, jak vytvořit [účet služby Storage úrovně Standard]((../storage/storage-create-storage-account.md) nebo [účet služby Storage úrovně Premium](../storage/storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)
* Pro vytvoření virtuálního zařízení použijte jiný účet úložiště, než jaký se používá pro vaše data. Použití stejného účtu úložiště může vést k nižšímu výkonu.

Před zahájením se ujistěte, že máte k dispozici následující informace:

* Účet portálu Azure Classic s přihlašovacími údaji.
* Kopie šifrovacího klíče dat služby z fyzického zařízení.

## <a name="create-and-configure-the-virtual-device"></a>Vytvoření a konfigurace virtuálního zařízení
Před provedením těchto postupů se ujistěte, že jste splnili [požadavky na virtuální zařízení](#prerequisites-for-the-virtual-device). 

Po vytvoření virtuální sítě, konfiguraci služby StorSimple Manager a registraci fyzického zařízení StorSimple u služby můžete pomocí následujících kroků vytvořit a nakonfigurovat virtuální zařízení StorSimple. 

### <a name="step-1-create-a-virtual-device"></a>Krok 1: Vytvoření virtuálního zařízení
Pomocí následujících kroků vytvořte virtuální zařízení StorSimple.

[!INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Pokud se vytváření virtuálního zařízení v tomto kroku nezdaří, je možné, že nemáte připojení k internetu. Další informace najdete v oddílu [řešení potíží při selhání připojení k internetu](#troubleshoot-internet-connectivity-errors) během vytváření virtuálního zařízení.

### <a name="step-2-configure-and-register-the-virtual-device"></a>Krok 2: Konfigurace a registrace virtuálního zařízení
Před zahájením tohoto postupu se ujistěte, že máte kopii šifrovacího klíče dat služby. Šifrovací klíč dat služby byl vytvořen při konfiguraci prvního zařízení StorSimple, přičemž jste dostali pokyn uložit jej na bezpečném místě. Pokud nemáte kopii šifrovacího klíče dat služby, obraťte se o pomoc na podporu společnosti Microsoft.

Pomocí následujících kroků proveďte konfiguraci a registraci virtuálního zařízení StorSimple.

[!INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Krok 3 (nepovinný): Úprava konfiguračních nastavení zařízení
Následující část popisuje konfigurační nastavení zařízení potřebná pro virtuální zařízení StorSimple, pokud chcete použít CHAP nebo Snapshot Manager zařízení StorSimple nebo změnit heslo správce zařízení.

#### <a name="configure-the-chap-initiator"></a>Konfigurace iniciátoru CHAP
Tento parametr obsahuje přihlašovací údaje, které virtuální zařízení (cíl) očekává z iniciátorů (serverů), které se pokoušejí o přístup ke svazkům. Iniciátory se při ověření identifikují vašemu zařízení uvedením uživatelského jména a hesla CHAP. Podrobný postup je uveden v části [Konfigurace CHAP pro vaše zařízení](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>Konfigurace cíle CHAP
Tento parametr obsahuje přihlašovací údaje, které virtuální zařízení používá, když iniciátor s podporou CHAP žádá o vzájemné nebo obousměrné ověření. Virtuální zařízení se při ověření identifikuje iniciátoru pomocí uživatelského jména a hesla Reverse CHAP. Upozorňujeme, že nastavení cíle CHAP jsou globální nastavení. Při jejich použití budou všechny svazky připojené k virtuálnímu zařízení úložiště používat ověřování CHAP. Podrobný postup je uveden v části [Konfigurace CHAP pro vaše zařízení](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Konfigurace hesla Snapshot Manageru zařízení StorSimple
Software Snapshot Manager zařízení StorSimple se nachází na hostiteli systému Windows a umožňuje správcům spravovat zálohy zařízení StorSimple ve formě místních a cloudových snímků.

> [!NOTE]
> Pro virtuální zařízení je hostitelem systému Windows virtuální počítač Azure.
> 
> 

Při konfiguraci zařízení ve Snapshot Manageru zařízení StorSimple budete vyzváni k zadání IP adresy a hesla zařízení StorSimple pro ověření zařízení úložiště. Podrobný postup je uveden v části [Konfigurace hesla Snapshot Manageru zařízení StorSimple](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-the-device-administrator-password"></a>Změna hesla správce zařízení
Pokud pro přístup k virtuálnímu zařízení použijete rozhraní Windows PowerShell, budete požádáni o zadání hesla správce zařízení. Pro zabezpečení vašich dat je před použitím virtuálního zařízení nutné toto heslo změnit. Podrobný postup je uveden v části [Konfigurace hesla správce zařízení](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-to-the-virtual-device"></a>Vzdálené připojení k virtuálnímu zařízení
Vzdálený přístup k virtuálnímu zařízení pomocí rozhraní Windows PowerShell není ve výchozím nastavení povolen. Nejprve musíte povolit vzdálenou správu virtuálních zařízení a pak ji povolit na straně klienta, který se použije pro přístup k virtuálnímu zařízení.

Dvoustupňový proces vzdáleného připojení je podrobně popsán níže.

### <a name="step-1-configure-remote-management"></a>Krok 1: Konfigurace vzdálené správy
Proveďte následující kroky konfigurace vzdálené správy svého virtuálního zařízení StorSimple.

[!INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Krok 2: Vzdálený přístup k virtuálnímu zařízení
Po povolení vzdálené správy na stránce konfigurace zařízení StorSimple můžete použít vzdálenou komunikaci prostředí Windows PowerShell pro připojení k virtuálnímu zařízení z jiného virtuálního počítače ve stejné virtuální síti; například se můžete připojit z hostitelského virtuálního počítače, který jste nakonfigurovali a používali pro připojení k iSCSI. Ve většině nasazení již budete mít otevřen veřejný koncový bod pro přístup k hostitelskému virtuálnímu počítači, který můžete použít pro přístup k virtuálnímu zařízení.

> [!WARNING]
> **Pro zvýšení zabezpečení důrazně doporučujeme používat pro připojování ke koncovým bodům protokol HTTPS a po dokončení vzdálené relace PowerShellu koncové body odstranit.**
> 
> 

Při nastavování vzdálené komunikace pro vaše virtuální zařízení je vhodné dodržovat postupy uvedené v části [Vzdálené připojení k zařízení StorSimple](storsimple-remote-connect.md).

## <a name="connect-directly-to-the-virtual-device"></a>Přímé připojení k virtuálnímu zařízení
K virtuálnímu zařízení se lze také připojit přímo. Pokud se chcete k virtuálnímu zařízení připojit přímo z jiného počítače mimo virtuální síť nebo mimo prostředí Microsoft Azure, musíte vytvořit další koncové body níže popsaným postupem. 

Prostřednictvím následujících kroků vytvořte na virtuálním zařízení veřejný koncový bod.

[!INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Doporučujeme připojit se z jiného virtuálního počítače ve stejné virtuální síti, protože tento postup minimalizuje počet veřejných koncových bodů ve virtuální síti. Při použití této metody se jednoduše připojíte k virtuálnímu počítači prostřednictvím relace vzdálené plochy a potom můžete provést konfiguraci tohoto virtuálního počítače pro použití stejně jako u jiných klientů systému Windows v místní síti. Nemusíte připojit číslo veřejného portu, protože port již bude znám.

## <a name="work-with-the-storsimple-virtual-device"></a>Práce s virtuálním zařízením StorSimple
Nyní, po vytvoření a konfiguraci virtuálního zařízení StorSimple, s ním můžete začít pracovat. S kontejnery svazků, svazky a zásadami zálohování na virtuální zařízení můžete pracovat stejně jako na fyzickém zařízení StorSimple; jediným rozdílem je, že nesmíte zapomenout vybrat virtuální zařízení ze seznamu zařízení. Podrobné postupy různých úloh správy pro virtuální zařízení jsou popsány v části [Použití služby StorSimple Manager pro správu virtuálních zařízení](storsimple-manager-service-administration.md) .

Následující části popisují některé rozdíly, na které můžete narazit při práci s virtuálním zařízením.

### <a name="maintain-a-storsimple-virtual-device"></a>Údržba virtuálního zařízení StorSimple
Protože jde čistě o softwarové zařízení, údržba je ve srovnání s údržbou fyzického zařízení minimální. Máte následující možnosti:

* **Aktualizace softwaru** – můžete zobrazit datum poslední aktualizace softwaru spolu s případnými stavovými zprávami aktualizace. Kontrolu nových aktualizací můžete provést ručním vyhledáním pomocí tlačítka **Scan updates** (Vyhledat aktualizace) v dolní části stránky. Pokud jsou k dispozici aktualizace, nainstalujte je kliknutím na tlačítko **Install Updates** (Instalovat aktualizace). Vzhledem k tomu, že na virtuálním zařízení je pouze jedno rozhraní, dojde při použití aktualizací k mírnému přerušení služby. Virtuální zařízení se pro použití případných vydaných aktualizací vypne a restartuje (v případě potřeby). Podrobný postup je popsán v části [Aktualizace zařízení](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
* **Balíček pro podporu** – můžete vytvořit a odeslat balíček pro podporu, který podpoře společnosti Microsoft pomůže vyřešit vaše potíže s virtuálním zařízením. Podrobný postup je popsán v části [Vytvoření a správa balíčku pro podporu](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Účty služby Storage pro virtuální zařízení
Účty služby Storage jsou vytvořeny pro použití službou StorSimple Manager, virtuálním zařízením a fyzickým zařízením. Při vytváření účtů úložiště vám doporučujeme použít identifikátor oblasti s popisným názvem, který pomůže zajistit konzistentnost oblasti ve všech součástech systému. Pro virtuální zařízení je důležité mít všechny součásti ve stejné oblasti, aby se zabránilo problémům s výkonem.

Podrobný postup je popsán v části [Přidání účtu úložiště](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Deaktivace virtuálního zařízení StorSimple
Deaktivace virtuálního zařízení odstraní virtuální počítač a prostředky vytvořené při jeho zřizování. Po deaktivaci virtuálního zařízení je nelze obnovit do předchozího stavu. Před deaktivací virtuálního zařízení nezapomeňte zastavit nebo odstranit klienty a hostitele, kteří na něm závisí.

Deaktivace virtuálního zařízení má za následek následující akce:

* Virtuální zařízení je odebráno.
* Je odebrán disk operačního systému a datové disky, které jsou vytvořeny pro virtuální zařízení.
* Hostované služby a virtuální sítě vytvořené při zřizování zůstanou zachovány. Pokud je nepoužíváte, je nutné je odstranit ručně.
* Cloudové snímky vytvořené pro virtuální zařízení zůstanou zachovány.

Podrobný postup je popsán v části [Deaktivace a odstranění zařízení StorSimple](storsimple-deactivate-and-delete-device.md).

Jakmile je virtuální zařízení zobrazeno na stránce služby StorSimple Manager jako deaktivované, můžete je odstranit ze seznamu zařízení na stránce **Zařízení**.

### <a name="start-stop-and-restart-a-virtual-device"></a>Spuštění, zastavení a restartování virtuálního zařízení
Na rozdíl od fyzického zařízení StorSimple nemá virtuální zařízení StorSimple žádné tlačítko pro zapnutí nebo vypnutí. Mohou však nastat situace, kdy potřebujete virtuální zařízení zastavit a restartovat. Některé aktualizace například mohou vyžadovat restart virtuálního počítače pro dokončení procesu aktualizace. Nejjednodušším způsobem spuštění, zastavení a restartování virtuálního zařízení je použití konzoly pro správu služby Virtual Machines.

Když se podíváte na konzolu pro správu, je u virtuálního zařízení uveden stav **Spuštěno**, protože je po vytvoření spuštěno ve výchozím nastavení. Virtuální počítač můžete kdykoli spustit, zastavit nebo restartovat.

[!INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Obnovení na výchozí hodnoty z výroby
Pokud se rozhodnete, že chcete s virtuálním zařízením začít znovu, můžete je jednoduše deaktivovat a odstranit a potom vytvořit nové. Stejně jako při obnovení fyzického zařízení nebude mít nové virtuální zařízení nainstalovány žádné aktualizace; proto nezapomeňte před jeho použitím provést kontrolu aktualizací.

## <a name="fail-over-to-the-virtual-device"></a>Převzetí služeb při selhání virtuálního zařízení
Zotavení po havárii je jedním z klíčových scénářů, pro něž je virtuální zařízení StorSimple určeno. Tento scénář předpokládá možnou nedostupnost fyzického zařízení StorSimple nebo celého datového centra. Naštěstí můžete použít virtuální zařízení k obnovení operací v alternativním umístění. Kontejnery svazků ze zdrojového zařízení během zotavení po havárii změní vlastnictví a jsou přeneseny na virtuální zařízení. Předpokladem pro zotavení po havárii je, že virtuální zařízení bylo vytvořeno a nakonfigurováno, všechny svazky v kontejneru svazků jsou v režimu offline a kontejner svazků má přiřazený cloudový snímek.

> [!NOTE]
> * Při použití virtuálního zařízení jako sekundárního zařízení pro zotavení po havárii mějte na paměti, že zařízení 8010 má k dispozici úložiště Standard Storage 30 TB a zařízení 8020 úložiště Premium Storage 64 TB. Vyšší kapacita virtuálního zařízení 8020 může být pro scénář zotavení po havárii vhodnější.
> * Ze zařízení se softwarem Update 2 nelze převzít služby při selhání nebo klonovat na zařízení se softwarem starším než Update 1. Můžete však převzít služby při selhání ze zařízení se softwarem Update 2 na zařízení se softwarem Update 1 (1.1 nebo 1.2)
> 
> 

Podrobný postup je popsán v části [Převzetí služeb při selhání na virtuální zařízení](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).

## <a name="shut-down-or-delete-the-virtual-device"></a>Vypnutí nebo odstranění virtuálního zařízení
Pokud jste již nakonfigurovali a používali virtuální zařízení StorSimple, ale nyní chcete zastavit nabíhání poplatků za jeho použití, můžete virtuální zařízení vypnout. Vypnutí virtuálního zařízení neodstraní jeho operační systém nebo datové disky v úložišti. Zastaví nabíhání poplatků za odběr, ale poplatky za úložiště pro disky operačního systému a dat budou pokračovat.

Pokud virtuální zařízení odstraníte nebo vypnete, zobrazí se na stránce Zařízení služby StorSimple Manager jako **Offline**. Zařízení také můžete deaktivovat nebo odstranit, pokud chcete odstranit zálohy vytvořené virtuálním zařízením. Další informace naleznete v tématu [Deaktivace a odstranění zařízení StorSimple](storsimple-deactivate-and-delete-device.md).

[!INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[!INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

## <a name="troubleshoot-internet-connectivity-errors"></a>Řešení potíží s připojením k internetu
Pokud během vytváření virtuálního zařízení není k dispozici připojení k internetu, daný krok vytváření se nezdaří. Chcete-li zjistit, zda za selhání může připojení k internetu, proveďte na klasickém webu Azure Portal následující kroky:

1. Vytvořte v Azure virtuální počítač s Windows Serverem 2012. Tento virtuální počítač by měl používat stejný účet úložiště, virtuální síť a podsíť jako vaše virtuální zařízení. Pokud již máte v Azure hostitele s Windows Serverem, který používá stejný účet úložiště, virtuální síť a podsíť, můžete jej také použít pro řešení potíží s připojením k internetu.
2. Vzdáleně se přihlaste k virtuálnímu počítači, který jste vytvořili v předchozím kroku. 
3. Na virtuálním počítači otevřete okno příkazového řádku (stiskněte Win + R a zadejte `cmd`).
4. V příkazovém řádku spusťte následující příkaz.
   
    `nslookup windows.net`
5. Pokud se `nslookup` nezdaří, pak problém s připojením k internetu brání virtuálnímu zařízení v registraci do služby StorSimple Manager. 
6. Proveďte požadované změny své virtuální sítě a ujistěte se, že virtuální zařízení může přistupovat k webům Azure, například „windows.net“.

## <a name="next-steps"></a>Další kroky
* Podívejte se, jak [použít službu StorSimple Manager pro správu virtuálního zařízení](storsimple-manager-service-administration.md).
* Naučte se [obnovit svazek StorSimple ze zálohovacího skladu](storsimple-restore-from-backup-set.md). 




<!--HONumber=Nov16_HO3-->


