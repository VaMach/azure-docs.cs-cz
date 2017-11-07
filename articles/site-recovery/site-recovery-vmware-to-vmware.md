---
title: "Nastavit zotavení po havárii virtuálních počítačů VMware nebo fyzických serverů do sekundární lokality | Microsoft Docs"
description: "Tento článek vysvětluje, jak replikovat místní virtuální počítače VMware nebo Windows nebo Linuxem fyzických serverů do sekundární lokality, se službou Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: b2cba944-d3b4-473c-8d97-9945c7eabf63
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2017
ms.author: raynew
ms.openlocfilehash: 8cfaa56735c1f4e2e01b58fdde2ad0e77b388762
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="set-up-disaster-recovery-of-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Nastavit zotavení po havárii virtuálních počítačů VMware nebo fyzických serverů do sekundární lokality


InMage Scout v Azure Site Recovery poskytuje v reálném čase replikaci mezi místními servery VMware. InMage Scout je součástí předplatného služby Azure Site Recovery.

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) před zahájením.


## <a name="create-a-vault"></a>Vytvoření trezoru
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/) > **Recovery Services**.
2. Klikněte na tlačítko Nový > Správa > Zálohování a obnovení lokality (OMS). Alternativně můžete klikněte na tlačítko Procházet > trezoru služeb zotavení > Přidat.
3. Do pole **Název** zadejte popisný název pro identifikaci trezoru. Máte-li více předplatných, vyberte jedno z nich.
4. V **skupiny prostředků** vytvořte novou skupinu prostředků nebo vyberte nějaký existující. Zadejte oblast Azure k dokončení povinná pole.
5. V **umístění**, vyberte zeměpisnou oblast trezoru. Pokud chcete zkontrolovat oblasti jsou podporované, najdete v části [ceník služby Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Pokud chcete rychle přístup k úložišti z řídicího panelu klikněte na připnout na řídicí panel a pak klikněte na tlačítko vytvořit.
7. Nový trezor se zobrazí na řídicím panelu > všechny prostředky a na hlavní služeb zotavení trezory stránky.

## <a name="configure-the-vault-and-download-inmage-scout-components"></a>Konfigurace trezoru a InMage Scout součásti stáhnout
1. V služeb zotavení trezorů vyberte trezor a klikněte na tlačítko **nastavení**.
2. V **nastavení** > **Začínáme** klikněte na tlačítko **Site Recovery** > Krok 1: **Příprava infrastruktury**  >  **Cíl ochrany**.
3. V **cíl ochrany** vyberte k obnovení lokality a vyberte možnost Ano, s VMware vSphere hypervisoru. Pak klikněte na OK.
4. V **Scout instalace**, klikněte na tlačítko Stáhnout na stáhnout InMage Scout 8.0.1 GA softwaru a registrační klíč. Instalační soubory pro všechny požadované součásti jsou v souboru ZIP staženého.

## <a name="step-3-install-component-updates"></a>Krok 3: Instalace aktualizace součástí
Přečtěte si informace o nejnovější [aktualizace](#updates). Nainstalujete soubory aktualizací na serverech v následujícím pořadí:

1. RX serveru podle potřeby
2. Konfigurace serverů
3. Proces servery
4. Hlavních cílových serverů
5. vContinuum servery
6. Zdrojový server (Windows a Linux Server)

Nainstalujte aktualizace následujícím způsobem:

1. Stažení [aktualizace](https://aka.ms/asr-scout-update5) soubor .zip. Tento soubor ZIP obsahuje následující soubory:

   * RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz
   * CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
   * UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
   * UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
   * vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe
   * Uživatelský Agent update4 bits pro RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
2. Rozbalte soubory .zip.<br>
3. **Pro RX server**: kopírování **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** RX serveru a rozbalte ho. V rozbalené složce spustit **/Install**.<br>
4. **Pro konfiguraci serveru nebo procesový server**: kopírování **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** konfigurační server a procesový server. Dvakrát klikněte na spustit.<br>
5. **Pro hlavní cílový server Windows**: aktualizace jednotná agenta, zkopírujte **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** na hlavním cílovém serveru. Dvojím kliknutím ji spustit. Všimněte si, unified agent je také použít na zdrojovém serveru a pokud zdroj není aktualizován do Update4. Měli byste jej nainstalovat na zdrojovém serveru i, jak je uvedeno dále v tomto seznamu.<br>
6. **Pro vContinuum server**: kopírování **vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe** server vContinuum.  Ujistěte se, že jste zavřeli Průvodce vContinuum. Poklikejte na soubor ji spustit.<br>
7. **Pro hlavní cílový server Linux**: aktualizace jednotná agenta, zkopírujte **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** na hlavní server cílového serveru a rozbalte ho. V rozbalené složce spustit **/Install**.<br>
8. **Pro zdrojový server Windows**: není nutné pro instalaci agenta aktualizací 5 ve zdroji, pokud je již spuštěn aktualizací 4. Pokud je spuštěn menší než aktualizace 4, použít agenta aktualizací 5.
Aktualizace jednotná agenta, zkopírujte **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** na zdrojovém serveru. Dvojím kliknutím ji spustit. <br>
9. **Pro zdrojový server Linux**: aktualizace jednotná agenta, zkopírujte odpovídající verzi souboru uživatelský Agent na Linux server a rozbalte ho. V rozbalené složce spustit **/Install**.  Příklad: Pro RHEL 6,7 64bitový server, zkopírujte **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** k serveru a rozbalte ho. V rozbalené složce spustit **/Install**.

## <a name="step-4-set-up-replication"></a>Krok 4: Nastavení replikace
1. Nastavení replikace mezi zdrojové a cílové lokality VMware.
2. Pokyny použijte dokumentaci InMage Scout, který byl stažen s tímto produktem. Alternativně můžete přístupu k dokumentaci následujícím způsobem:

   * [Poznámky k verzi](https://aka.ms/asr-scout-release-notes)
   * [Matice kompatibility](https://aka.ms/asr-scout-cm)
   * [Uživatelská příručka](https://aka.ms/asr-scout-user-guide)
   * [RX uživatelská příručka](https://aka.ms/asr-scout-rx-user-guide)
   * [Příručka pro rychlou instalaci](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Aktualizace
### <a name="azure-site-recovery-scout-801-update-5"></a>Azure Site Recovery Scout 8.0.1 aktualizace 5
Kumulativní aktualizace je Scout aktualizace 5. Obsahuje všechny opravy aktualizaci1 do update4 a následující nové oprav chyb a vylepšení.
Opravy, které se přidají update4 Scout obnovení lokality k update5 jsou specifické pro hlavní cíl a vContinuum součásti. Pokud vaše zdroje jsou všechny servery, hlavního cíle, konfigurační Server, Server proces a RX již na update4 Scout obnovení lokality budete muset použít aktualizaci 5 jenom na hlavním cílovém serveru. 

**Nová podpora platformy**
* SUSE Linux Enterprise Server 11, aktualizace Service Pack 4(SP4)

> [!NOTE]
> 64bitový SLES 11 SP4 **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** je součástí balíčku základní balíček Scout GA **InMage_Scout_Standard_8.0.1 GA.zip**. Stáhněte si balíček Scout GA z portálu, jak je uvedeno v [krok 1](#step-1-create-a-vault).
>

**Opravy chyb a vylepšení**

* Zvýšit spolehlivost podpora clusteru se systémem Windows
    * Fixed – Přetrvával některé P2V MSCS diskům clusteru stane RAW po obnovení
    * Obnovení clusteru fixed-P2V MSCS selže kvůli neshodě pořadí disku
    * Přidat disky, které operace se nezdaří s neshoda velikost disku clusteru fixed-MSCS
    * V velikost ověření selže clusteru MSCS zdroj fixed-s kontroly připravenosti mapování RDM logických jednotek
    * Fixed-jeden uzel clusteru ochrana se nezdaří z důvodu problému neshoda SCSI 
    * Fixed-znovu nastavit ochranu P2V server systému Windows clusteru selže, pokud nejsou cílové disky clusteru. 
    
* Během navrácení služeb po obnovení ochrany, pokud vybrané MT není na stejném serveru ESXi jako chráněný zdrojového počítače (během dopředného ochrany), pak vContinuum převezme nesprávný MT během obnovení navrácení služeb po obnovení a následně operaci obnovení selže.

> [!NOTE]
> 
> * Výše P2V clusteru se použít jenom tyto fyzické MSCS clustery, které jsou nově chráněné službou Site Recovery Scout update5 opravy. Díky jednotnému clusteru opravy na již chráněného clusteru P2V MSCS s aktualizací starší, musíte dodržet postup upgradu, které jsou uvedené v části 12, Upgrade chráněné P2V MSCS clusterů do Scout aktualizace 5 [poznámky k verzi](https://aka.ms/asr-scout-release-notes) .
> 
> * Znovu nastavit ochranu fyzického clusteru MSCS můžete znovu použít stávající cílové disky pouze pokud v době znovu zapnout ochrana, jsou na všech uzlech clusteru aktivní stejnou sadu disků, jako když původně chráněný. Pokud ne, pak existují ruční kroky, jak je uvedeno v části 12 [poznámky k verzi](https://aka.ms/asr-scout-release-notes) přesunout cílové disky straně cestu správné úložiště znovu je použít během znovu zapnout ochrana. Pokud znovu nastavte ochranu clusteru MSCS v režimu P2V bez následující kroky upgradu se budou na cílovém serveru ESXi vytvořit nový disk. Budete muset ručně odstranit staré disky z úložiště.
> 
> * Vždy, když zdroje SLES11 nebo SLES11 s jakýkoli server service pack je řádně restartovat a pak ručně měli označit **kořenové** jako nebudete nijak upozorněni v uživatelském rozhraní CX disku páry replikace pro znovu synchronizovat. Pokud nemáte označit kořenovém disku pro nové synchronizace, může se zobrazit problémy s integritou (DI) data.
> 

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Recovery Scout 8.0.1 aktualizací 4
Scout Update 4 je kumulativní aktualizace. Obsahuje všechny opravy aktualizaci1 do update3 a následující nové oprav chyb a vylepšení.

**Nová podpora platformy**

* Byla přidána podpora pro vCenter/vSphere 6.0, 6.1 a 6.2
* Byla přidána podpora pro následující operační systémy Linux
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 a 7.2
  * CentOS 7.0, 7.1 a 7.2
  * Red Hat Enterprise Linux (RHEL) 6.8
  * CentOS 6.8

> [!NOTE]
> RHEL nebo CentOS 7 64bitové **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** je součástí balíčku základní balíček Scout GA **InMage_Scout_Standard_8.0.1 GA.zip**. Stáhněte si balíček Scout GA z portálu, jak je uvedeno v [krok 1](#step-1-create-a-vault).
>
>

**Opravy chyb a vylepšení**

* Vylepšené vypnutí zpracování pro následující operační systémy Linux a klonovat, aby se zabránilo problémům nežádoucí znovu synchronizovat.
  * Red Hat Enterprise Linux (RHEL) 6.x
  * Oracle Linux (OL) 6.x
* Pro systémy Linux proveďte přístup ke složce, které jsou oprávnění v instalační adresář agenta jednotná nyní omezena pouze pro místní uživatele.
* V systému Windows načíst vypršení časového limitu problém při vydání běžné označit kniha distribuované konzistence na výraznou distribuované aplikace jako clustery SQL a bod sdílené složky.
* Přidání protokolu související oprava v CX základní instalační služby.
* Odkaz ke stažení VMware vCLI 6.0 je přidán do základní instalační služby systému Windows hlavního cíle.
* Během převzetí služeb při selhání a zotavení po Havárii cvičení přidat další kontroly a protokoly pro změny konfigurace sítě.
* Informace o zachování přetrvával není hlášena CX.  
* Pro fyzických clusterů svazek znovu velikost operace prostřednictvím Průvodce vContinuum selže při zmenšení svazku zdroje.
* Cluster ochrany se nezdařila s chybou "Nepodařilo se najít podpisu disku" Pokud disk clusteru je PRDM disk.
* cxps přenosu zhroucení serveru z důvodu výjimky out-of-range.
* Název serveru a sloupce IP je teď s možností změny velikosti nabízené instalace stránce průvodce vContinuum.
* Vylepšení RX rozhraní API
  * Poskytuje pět nejnovější dostupné společné body konzistence (pouze zaručena značek).
  * Poskytuje kapacitu a podrobnosti volného místa pro všechny chráněné zařízení.
  * Poskytuje stav Scout ovladačů na zdrojovém serveru.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** základní balíček nyní aktualizoval základní instalační program CX **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** a základní instalační služby systému Windows hlavního cíle **InMage_ Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Pro všechny nové instalace použijí nové bits GA CX a Windows hlavního cíle.
> * Aktualizace 4 můžete použít přímo na 8.0.1 Všeobecné
> * Konfigurační server a aktualizace RX nedá se vrátit zpět, jakmile se uplatňují v systému.
>
>

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Recovery Scout 8.0.1 aktualizací 3
Aktualizace 3 zahrnuje následující oprav chyb a vylepšení:

* Konfigurační server a RX nezdaří jejich registrace do trezoru Site Recovery, kdy jsou za proxy serveru.
* Počet hodin, které nebyly splněny plánovaného bodu obnovení (RPO) se neaktualizuje v sestavě stavu.
* Konfigurační server není synchronizuje s RX, pokud ESX hardwaru údaje nebo podrobnosti o síti obsahují znaky znakové sady UTF-8.
* Řadiče domény systému Windows Server 2008 R2 se nepodaří spustit po obnovení.
* Offline synchronizace nefungují podle očekávání.
* Po převzetí služeb při selhání virtuálního počítače (VM) odstranění replikace páru vázne v uživatelském rozhraní CX po dlouhou dobu a uživatelé nebo nelze dokončit navrácení služeb po obnovení operace obnovení.
* Celkové snímků, které byly optimalizovány operace, které provádějí úlohu konzistence snížit aplikace odpojí jako klienti SQL.
* Vylepšili jsme výkon nástroje konzistence (VACP.exe) tím, že snižuje využití paměti, která je požadována pro vytváření snímků v systému Windows.
* Vynucené instalace služby dojde k chybě při heslo je větší než 16 znaků.
* vContinuum není kontrolu a dotaz na nová pověření vCenter, pokud došlo ke změně pověření.
* V systému Linux není správce mezipaměti hlavní cíl (cachemgr) stahování souborů z procesového serveru, což vede k omezení pár replikace.
* Pokud pořadí převzetí služeb při selhání fyzického disku clusteru (MSCS) není stejná ve všech uzlech, replikace není nastaven pro některé svazky clusteru.
  <br/>Všimněte si, že clusteru musí být znovu využít této opravy.  
* Funkce SMTP nefungují podle očekávání po upgradu RX Scout 7.1 k Scout 8.0.1.
* V protokolu operaci vrácení zpět ke sledování doby pořízení dokončit, protože se byly přidány další statistiky.
* Byla přidána podpora pro operační systémy Linux na zdrojovém serveru:
  * Red Hat Enterprise Linux (RHEL) 6 aktualizace 7
  * CentOS 6 aktualizace 7
* CX a RX uživatelského rozhraní můžete nyní zobrazit oznámení pro dvojici, která přejde do režimu rastrového obrázku.
* V RX byly přidány následující opravy zabezpečení:

| **Popis problému** | **Implementace postupů** |
| --- | --- |
| Autorizace vynechat prostřednictvím parametru manipulaci |Omezený přístup k uživatelům nepoužitelné. |
| Padělání požadavku posílaného mezi weby |Implementováno koncept stránky token, který generuje náhodně na každé stránce. <br/>S tímto uvidíte: <li> Není k dispozici pouze jedna přihlášení instance pro stejného uživatele.</li><li>Aktualizace stránky nefunguje – přesměruje na řídicí panel.</li> |
| Nahrávání škodlivý souborů |Soubory s omezeným přístupem pro určité rozšíření. Povolené jsou rozšíření: 7z, aiff, amp, avi, bmp, csv, dokumentů, docx, fla, flv, gif, gz, gzip, jpeg, jpg, protokolu, mid mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, RT, paměti ram, rar, rm, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, vkládání , tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml a zip. |
| Trvalé skriptování mezi servery |Přidat vstupní ověření. |

> [!NOTE]
> * Všechny Site Recovery aktualizace jsou kumulativní. Update 3 obsahuje všechny opravy Update 1 a Update 2. Update 3 můžete použít přímo na 8.0.1 Všeobecné
> * Konfigurační server a aktualizace RX nedá se vrátit zpět, jakmile se uplatňují v systému.
>
>

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Recovery Scout 8.0.1 Update 2 (03 aktualizace DEC – 15)
Opravy v aktualizaci Update 2 patří:

* **Konfigurační server**: oprava problému, která zabránila funkci volné měření 31 dnů fungovat podle očekávání, pokud se konfigurační server se zaregistroval v Site Recovery.
* **Jednotná agenta**: oprava problému v Update 1, jejichž výsledkem není nainstalována na hlavním cílovém serveru, při upgradu z verze 8.0 k 8.0.1 aktualizace.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Recovery Scout 8.0.1 Update 1
Aktualizace 1 zahrnuje následující oprav chyb a nové funkce:

* 31 dní volné ochrany na instanci serveru. To umožňuje testování funkčnosti nebo nastavit testování konceptu.
  * Všechny operace na serveru, včetně převzetí služeb při selhání a navrácení služeb po obnovení, jsou volné první 31 dnů od doby, nejprve chráněný server pomocí Scout obnovení lokality.
  * Dnem 32nd dále každý chráněný server se bude účtovat poplatek rychlostí standardní instance pro ochranu Azure Site Recovery k lokalitě vlastněných zákazníků.
  * Počet chráněných serverů, které jsou aktuálně účtovat kdykoli, je k dispozici na stránce řídicího panelu trezoru Azure Site Recovery.
* Přidána podpora pro rozhraní příkazového řádku (vCLI) vSphere 5.5 Update 2.
* Byla přidána podpora pro operační systémy Linux na zdrojovém serveru:
  * RHEL 6 aktualizaci 6
  * RHEL 5 aktualizovat 11
  * CentOS 6 aktualizací 6
  * Aktualizace centOS 5 11
* Opravy chyb pro adres následující problémy:
  * Pro konfigurační server nebo RX server selhání registrace trezoru.
  * Svazky clusteru nezobrazí podle očekávání, pokud virtuálních počítačů v clusteru se znovu po jejich obnovení.
  * Navrácení služeb po obnovení selže, když hlavní cílový server je hostovaná na jiném serveru ESXi z místní produkční virtuálních počítačů.
  * Při upgradu na 8.0.1, které ovlivňují ochranu a operace se změní konfiguraci souboru oprávnění.
  * Prahová hodnota Opakovaná synchronizace se nevynucuje podle očekávání, což vede k replikaci nekonzistentní chování.
  * Nastavení plánovaného bodu obnovení se nezobrazují správně ve rozhraní konfigurace serveru. Hodnota nekomprimovaných dat se nesprávně zobrazuje komprimované hodnota.
  * Operace odebrání nedojde k odstranění podle očekávání v Průvodci vContinuum a replikace není odstraněný z rozhraní konfigurace serveru.
  * V Průvodci vContinuum disku je automaticky při kliknutí na tlačítko **podrobnosti** v zobrazení disk během ochrany MSCS virtuálních počítačů.
  * Během tohoto scénáře fyzického na virtuální (P2V) nejsou požadované HP služby, jako je například CIMnotify a CqMgHost, přesunout na ruční v obnovení virtuálního počítače. Výsledkem další spuštění.
  * Ochranu virtuálních počítačů Linux selže, pokud jsou více než 26 disků na hlavním cílovém serveru.

## <a name="next-steps"></a>Další kroky
POST nějaké dotazy, které mají na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
