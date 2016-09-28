<properties 
   pageTitle="Nasazení zařízení StorSimple (Update 1) | Microsoft Azure"
   description="Popisuje kroky a osvědčené postupy nasazení zařízení a služby StorSimple Update 1."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/17/2016"
   ms.author="alkohli" />


# Nasazení místního zařízení StorSimple (Update 1)

> [AZURE.SELECTOR]
- [Update 2](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Update 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [Verze GA](../articles/storsimple/storsimple-deployment-walkthrough.md)

## Přehled

Vítejte v průvodci nasazením zařízení Microsoft Azure StorSimple. Tyto kurzy nasazení se vztahují na řadu zařízení StorSimple 8000 Update 1.0. Tato série kurzů popisuje způsob konfigurace zařízení StorSimple a zahrnuje kontrolní seznam konfigurace, požadavky konfigurace a podrobné kroky konfigurace.

Informace v těchto kurzech předpokládají, že jste si přečetli informace o bezpečnostních opatřeních a že jste zařízení StorSimple vybalili, namontovali do racku a zapojili jeho kabeláž. Pokud jste tyto kroky ještě neprovedli, začněte prostudováním [bezpečnostních opatření](storsimple-safety.md). V závislosti na modelu zařízení poté můžete zařízení vybalit, namontovat do racku a zapojit jeho kabeláž podle pokynů v následujících článcích:

- [Zařízení 8100 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8100-hardware-installation.md)
- [Zařízení 8600 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8600-hardware-installation.md)

K dokončení této instalace a procesu konfigurace budete potřebovat oprávnění správce. Doporučujeme, abyste si před zahájením práce prošli kontrolní seznam konfigurace. Proces nasazení a konfigurace může trvat nějakou dobu.

> [AZURE.NOTE] Informace o nasazení zařízení StorSimple publikované na webu Microsoft Azure platí pouze pro řadu zařízení StorSimple 8000. Úplné informace o řadách zařízení 5000 a 7000 najdete na následující adrese: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Informace o nasazení řad 5000 a 7000 najdete v [Úvodní příručce k systému StorSimple](http://onlinehelp.storsimple.com/111_Appliance/).

## Kroky nasazení

Pomocí níže uvedených požadovaných kroků konfigurujte své zařízení StorSimple a připojte je ke službě StorSimple Manager. Kromě požadovaných kroků existují ještě některé volitelné kroky a postupy, které může být vhodné při nasazení provést. Informace o tom, kdy je vhodné provést jednotlivé volitelné kroky, jsou vždy uvedeny v příslušných požadovaných krocích nasazení.


| Krok                                                                                   | Popis                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **POŽADAVKY**                                                                      | Požadavky je nutné splnit v rámci přípravy na nasazení.                                                                                        |
| Kontrolní seznam konfigurace nasazení                                                     | Kontrolní seznam použijte ke shromáždění a zaznamenání informací před nasazením a během nasazení.                                                                       |
| Požadavky nasazení                                                               | Splnění požadavků nasazení kvalifikuje prostředí jako připravené na nasazení.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **PODROBNÝ POSTUP NASAZENÍ**                                                                   | Tyto kroky jsou požadované k produkčnímu nasazení zařízení StorSimple.                                                                                      |
| Krok 1: Vytvoření nové služby                                                         | Nastavte správu cloudu a úložiště pro zařízení StorSimple. Pokud máte existující službu pro jiná zařízení StorSimple, tento krok přeskočte.                |
| Krok 2: Získání registračního klíče služby                                               | Pomocí tohoto klíče zaregistrujte zařízení StorSimple ve službě správy a připojte je k ní.                                                                         |
| Krok 3: Konfigurace a registrace zařízení pomocí Windows PowerShellu pro StorSimple    | Připojte zařízení k síti, zaregistrujte je v Azure a dokončete instalaci pomocí služby pro správu.                                            |
| Krok 4: Dokončení minimální instalace zařízení</br>Volitelné: Aktualizace zařízení StorSimple      | Pomocí služby pro správu dokončete instalaci zařízení a aktivujte je k poskytování úložiště.                                                                      |
| Krok 5: Vytvoření kontejneru svazků                                                      | Vytvořte kontejner ke zřízení svazků. Kontejner svazků obsahuje účet úložiště, šířku pásma a nastavení šifrování pro všechny svazky, které jsou v něm obsažené.    |
| Krok 6: Vytvoření svazku                                                                | V zařízení StorSimple zřiďte svazky úložiště pro své servery.                                                                                        |
| Krok 7: Připojení, inicializace a formátování svazků</br>Volitelné: Konfigurace funkce MPIO            | Připojte své servery k úložišti iSCSI poskytovanému zařízením. Volitelně konfigurujte funkci MPIO, abyste zajistili, že servery budou tolerovat chyby připojení, sítě a rozhraní.                                                                                                                                                              |
| Krok 8: Provedení zálohy                                                                  | Nastavte zásady zálohování pro ochranu dat.                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **DALŠÍ POSTUPY**                                                                   | Tyto postupy můžete někdy potřebovat při nasazování svého řešení.                                                                                        |
| Konfigurace nového účtu úložiště pro službu                                      |                                                                                                                                                               |
| Použití klienta PuTTY k připojení ke konzole sériového portu zařízení                                    |                                                                                                                                                               |
| Získání názvu IQN hostitele se systémem Windows Server                                                   |                                                                                                                                                               |
| Vytvoření ruční zálohy                                                                 | 


## Kontrolní seznam konfigurace nasazení

Následující kontrolní seznam konfigurace nasazení popisuje informace, které je třeba shromáždit před konfigurací softwaru v zařízení StorSimple a během ní. Předběžná příprava některých z těchto informací vám pomůže urychlit a zjednodušit proces nasazení zařízení StorSimple ve vašem prostředí. Kontrolní seznam použijte také zaznamenání podrobností konfigurace během nasazování zařízení.

| Krok                                  | Parametr                                         | Podrobnosti                                                                                                                                                                | Hodnoty |
|----------------------------------------|---------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|
| **Zapojení kabeláže zařízení**                      | Sériový přístup                                     | Počátečního konfigurace zařízení                                                                  | Ano/Ne |
|   |   |  |  |
| **Konfigurace a registrace zařízení**          | Nastavení síťového rozhraní DATA 0                           | IP adresa síťového rozhraní DATA 0:</br>Maska podsítě:</br>Brána:</br>Primární server DNS:</br>Primární server NTP:</br>Webový proxy server protokolu IP nebo plně kvalifikovaný název domény (volitelné):</br>Port webového proxy serveru:|        |
|                                        | Heslo správce zařízení                     | Heslo musí být tvořeno 8 až 15 znaky a musí obsahovat malá písmena, velká písmena, číslice a speciální znaky. |        |
|                                        | Heslo správce Snapshot Manageru zařízení StorSimple              | Heslo musí být tvořeno 14 až 15 znaky a musí obsahovat malá písmena, velká písmena, číslice a speciální znaky.|        |
|                                        | Registrační klíč služby                          | Klíč se generuje na portálu Azure Classic.    |        |
|                                        | Datový šifrovací klíč služby                       | Tento klíč je vytvořen po registraci zařízení ve službě správy prostřednictvím Windows PowerShellu pro StorSimple. Klíč zkopírujte a uložte na bezpečném místě.|  |
|   |   |  |  |
| **Dokončení minimální instalace zařízení**          | Popisný název zařízení                     | Toto je popisný název zařízení. |        |
|                                        | Časové pásmo                                          | Toto časové pásmo bude zařízení používat pro všechny naplánované operace.  |        |
|                                        | Sekundární server DNS                              | Toto je požadovaná konfigurace.                                  |        |
|                                        | Síťové rozhraní: Pevné IP adresy řadiče síťového rozhraní DATA 0                                     | Tyto IP adresy musí být směrovatelné na internet.</br>Pevná IP adresa řadiče síťového rozhraní DATA 0:</br>Pevná IP adresa řadiče síťového rozhraní DATA 1:|
|   |   |  |  |
| **Další nastavení síťového rozhraní**  | Síťové rozhraní: DATA 1</br>Pokud je povolen standard iSCSI, neprovádějte konfiguraci brány.      | Účel: Cloud / iSCSI / nepoužito</br>IP adresa:</br>Maska podsítě:</br>Brána:|
|                                        | Síťové rozhraní: DATA 2</br>Pokud je povolen standard iSCSI, neprovádějte konfiguraci brány.      | Účel: Cloud / iSCSI / nepoužito</br>IP adresa:</br>Maska podsítě:</br>Brána:|
|                                        | Síťové rozhraní: DATA 3</br>Pokud je povolen standard iSCSI, neprovádějte konfiguraci brány.      | Účel: Cloud / iSCSI / nepoužito</br>IP adresa:</br>Maska podsítě:</br>Brána:|
|                                        | Síťové rozhraní: DATA 4</br>Pokud je povolen standard iSCSI, neprovádějte konfiguraci brány.      | Účel: Cloud / iSCSI / nepoužito</br>IP adresa:</br>Maska podsítě:</br>Brána:|
|                                        | Síťové rozhraní: DATA 5</br>Pokud je povolen standard iSCSI, neprovádějte konfiguraci brány.      | Účel: Cloud / iSCSI / nepoužito</br>IP adresa:</br>Maska podsítě:</br>Brána:|
|   |   |  |  |
| **Vytvoření kontejneru svazků**                      | Název kontejneru svazků:                            | Název kontejneru                                                                                                                                                 |        |
|                                        | Účet úložiště Azure:                            | Název účtu služby Storage a přístupový klíč k úložišti asociovaný s tímto kontejnerem svazků                                                                                              |        |
|                                        | Šifrovací klíč cloudového úložiště:                     | Šifrovací klíč pro úložiště v jednotlivých kontejnerech                                                                                                                           |        |
|   |   |  |  |
| **Vytvoření svazku**                        | Podrobnosti jednotlivých svazků                           | Název svazku:                                                                                                                                                           |        |
|                                        |                                                   | Velikost:                                                                                                                                                                  |        |
|                                        |                                                   | Typ použití:                                                                                                                                                            |        |
|                                        |                                                   | Název ACR:                                                                                                                                                              |        |
|                                        |                                                   | Výchozí zásady zálohování:                                                                                                                                                 |        |
|   |   |  |  |
| **Připojení, inicializace a formátování svazků** | Podrobnosti o každém hostitelském serveru, který se připojuje k úložišti | Název systému Windows Server:                                                                                                                                                   |        |
|                                        |                                                   | Název IQN systému Windows Server:                                                                                                                                                    |        |
|                                        |                                                   | Název svazku systému Windows Server:                                                                                                                                                   |        |
|                                        |                                                   | Přípojný bod systému souborů NTFS nebo písmeno jednotky:                                                                                                                                      |        |

## Požadavky nasazení

Následující části popisují požadavky konfigurace služby StorSimple Manager a zařízení StorSimple.

### Služba StorSimple Manager

Než začnete, ujistěte se, že:

- Máte účet Microsoft a přihlašovací údaje účtu.

- Máte účet služby Microsoft Azure Storage a přihlašovací údaje účtu.

- Vaše předplatné Microsoft Azure je povoleno pro službu StorSimple Manager. Předplatné by mělo být zakoupeno prostřednictvím [smlouvy Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/).

- Máte přístup k softwaru pro emulaci terminálu, jako je například PuTTY.

### Zařízení v datovém centru

Před konfigurací zařízení se ujistěte, že:

- Zařízení je zcela vybaleno, namontováno v racku a je kompletně zapojena jeho kabeláž napájení, sítě a sériového přístupu, jak je popsáno v následujících článcích:

    -  [Zařízení 8100 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8100-hardware-installation.md)
    -  [Zařízení 8600 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8600-hardware-installation.md)


### Síť v datovém centru

Než začnete, ujistěte se, že:

- Porty brány firewall vašeho datového centra jsou otevřeny, aby umožňovaly přenos dat standardu iSCSI a cloudu, jak je popsáno v [Požadavcích sítě pro zařízení StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).


## Podrobný postup nasazení

Pomocí následujících kroků nasaďte zařízení StorSimple v datovém centru.

## Krok 1: Vytvoření nové služby

Služba StorSimple Manager může spravovat více zařízení StorSimple. Pomocí následujících kroků vytvořte novou instanci služby StorSimple Manager.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Pokud jste nepovolili automatické vytvoření účtu úložiště při vytvoření služby, po úspěšném vytvoření služby bude nutné vytvořit alespoň jeden účet úložiště. Tento účet úložiště se použije při vytváření kontejneru svazku. 
>
> * Pokud jste nevytvořili účet úložiště automaticky, najdete podrobné pokyny k vytvoření účtu v tématu [Konfigurace nového účtu úložiště pro službu](#configure-a-new-storage-account-for-the-service). 
> * Pokud jste automatické vytvoření účtu úložiště povolili, pokračujte na [krok 2: Získání registračního klíče služby](#step-2-get-the-service-registration-key).

## Krok 2: Získání registračního klíče služby

Po vytvoření a spuštění instance služby StorSimple Manager je nutné získat registrační klíč služby. Tento klíč se používá k registraci a připojení zařízení StorSimple ke službě.

Na portálu Azure Classic proveďte následující kroky.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## Krok 3: Konfigurace a registrace zařízení pomocí Windows PowerShellu pro StorSimple

Pomocí Windows PowerShellu pro StorSimple dokončete počáteční nastavení zařízení StorSimple, jak je popsáno v následujícím postupu. K dokončení tohoto kroku budete potřebovat software pro emulaci terminálu. Další informace naleznete v tématu [Použití klienta PuTTY k připojení ke konzole sériového portu zařízení](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## Krok 4: Dokončení minimální instalace zařízení

K provedení minimální konfigurace zařízení StorSimple je nutné provést následující kroky: 

- Nastavit sekundární server DNS.
- Nejméně v jednom síťovém rozhraní povolit iSCSI.
- Oběma řadičům přiřadit pevné IP adresy.

K dokončení minimální instalace zařízení proveďte na portálu Azure Classic následující kroky.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## Krok 5: Vytvoření kontejneru svazků

Kontejner svazků obsahuje účet úložiště, šířku pásma a nastavení šifrování pro všechny svazky, které jsou v něm obsažené. Kontejner svazků je nutné vytvořit před zahájením zřizování svazků v zařízení StorSimple. 

Pomocí následujících kroků na portálu Azure Classic vytvořte kontejner svazků.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## Krok 6: Vytvoření svazku

Po vytvoření kontejneru svazků můžete v zařízení StorSimple zřídit svazek úložiště pro své servery. Pomocí následujících kroků na portálu Azure Classic vytvořte svazek.

> [AZURE.IMPORTANT] Služba StorSimple Manager umožňuje vytvářet pouze dynamicky zřizované svazky. Nelze vytvářet zcela či částečně zřizované svazky. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## Krok 7: Připojení, inicializace a formátování svazků

Následující kroky se provádějí v hostiteli se systémem Windows Server. 


> [AZURE.IMPORTANT]

> - K zajištění vysoké dostupnosti vašeho řešení StorSimple doporučujeme před konfigurací standardu iSCSI nakonfigurovat na hostitelských serverech funkci MPIO (volitelné). Konfigurace funkce MPIO na hostitelských serverech zajistí, že servery budou tolerovat chyby připojení, sítě a rozhraní.

> - Pokyny k instalaci a konfiguraci funkce MPIO a standardu iSCSI v hostiteli se systémem Windows Server najdete v tématu [Konfigurace funkce MPIO pro zařízení StorSimple](storsimple-configure-mpio-windows-server.md). Tyto kroky budou zahrnovat také kroky připojení, inicializace a formátování svazků zařízení StorSimple.

> - Pokyny k instalaci a konfiguraci funkce MPIO a standardu iSCSI v hostiteli se systémem Linux najdete v tématu [Konfigurace funkce MPIO pro hostitele zařízení StorSimple se systémem Linux](storsimple-configure-mpio-on-linux.md)

Pokud se rozhodnete, že funkci MPIO konfigurovat nebudete, připojte, inicializujte a formátujte svazky zařízení StorSimple v hostiteli se systémem Windows Server pomocí následujících kroků.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## Krok 8: Provedení zálohy

Zálohy vytvořené v určitých časových bodech poskytují ochranu a zvyšují možnost jejich obnovení při současném zkrácení doby potřebné k obnovení. Zařízení StorSimple lze zálohovat dvěma různými způsoby: pomocí místních snímků nebo pomocí cloudových snímků. Každý z těchto typů zálohování může být **naplánovaný** nebo **ruční**. 

Pomocí následujících kroků vytvořte na portálu Azure Classic naplánované zálohování.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Ruční zálohování lze provést kdykoliv. Popis potřebného postupu najdete v tématu [Vytvoření ruční zálohy](#create-a-manual-backup). 

## Konfigurace nového účtu úložiště pro službu

Toto je volitelný krok, který je potřeba provést pouze v případě, že jste ve své službě nepovolili automatické vytvoření účtu úložiště. K vytvoření kontejneru svazků zařízení StorSimple je potřeba účet služby Microsoft Azure Storage.

Pokud potřebujete vytvořit účet služby Azure Storage v jiné oblasti, postupujte podle pokynů v tématu [Účty Azure Storage](../storage/storage-create-storage-account.md).

Na portálu Azure Classic na stránce**Manažer StorSimple** proveďte následující kroky.

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## Použití klienta PuTTY k připojení ke konzole sériového portu zařízení

K připojení Windows PowerShellu pro StorSimple je nutné použít software pro emulaci terminálu, jako je například PuTTY. PuTTY můžete použít při přístupu k zařízení přímo pomocí konzoly sériového portu nebo otevřením relace Telnetu ze vzdáleného počítače.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]


## Vyhledání a instalace aktualizací

Aktualizace zařízení může trvat několik hodin. Aktualizace vyhledejte a nainstalujte v zařízení podle následujících kroků.
<!--can take 1-4 hours--> 

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### Aktualizace zařízení

1.  Na obrazovce **Rychlé zahájení** klikněte na **Zařízení**. Vyberte fyzické zařízení, klikněte na **Maintenance** (Údržba) a potom klikněte na **Scan Updates** (Vyhledat aktualizace).  

2.  Bude vytvořena úloha vyhledávání dostupných aktualizací. Pokud jsou k dispozici aktualizace, položka **Scan Updates** (Vyhledat aktualizace) se změní na **Install Updates** (Instalovat aktualizace). Klikněte na **Install Updates** (Instalovat aktualizace). 

3.  Vytvoří se úloha aktualizace. Přejděte na **Jobs** (Úlohy) a sledujte stav aktualizace.

    > [AZURE.NOTE] Při spuštění úlohy aktualizace se úloha okamžitě zobrazí jako dokončená z 50 %. Po dokončení aktualizace se úloha aktualizace zobrazí jako 100% dokončená. U aktualizací se nesleduje průběh v reálném čase.

4.  Po úspěšné aktualizaci zařízení povolte síťová rozhraní DATA 2 a DATA 3.

<!-- In step 2, you may be requested to disable Data 2 and Data 3 prior to installing the updates. You must disable these network interfaces or the updates may fail.-->

## Získání názvu IQN hostitele se systémem Windows Server

Pomocí následujících kroků získejte název IQN (iSCSI Qualified Name) hostitele Windows se systémem Windows Server® 2012.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## Vytvoření ruční zálohy

Pomocí následujících kroků na portálu Azure Classic vytvořte ruční zálohu jednoho svazku zařízení StorSimple.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## Konfigurace funkce MPIO

Funkce MPIO (Multipath I/O) je volitelná funkce a v systému Windows Server ve výchozím nastavení není nainstalována. Je nutné ji nainstalovat jako funkci pomocí Správce serveru. Pokyny k instalaci funkce MPIO najdete v tématu [Konfigurace funkce MPIO pro zařízení StorSimple](storsimple-configure-mpio-windows-server.md).

Pokyny k instalaci a konfiguraci funkce MPIO pro zařízení StorSimple připojené k hostiteli se systémem Linux najdete v tématu [Konfigurace funkce MPIO pro hostitele se systémem Linux](storsimple-configure-mpio-on-linux.md).


> [AZURE.NOTE] Funkce MPIO není podporována ve virtuálních zařízeních StorSimple. 



## Další kroky

- Konfigurace [virtuálního zařízení](storsimple-virtual-device-u2.md)

- Použití [služby StorSimple Manager](storsimple-manager-service-administration.md) ke správě zařízení StorSimple
 



<!--HONumber=Sep16_HO3-->


