---
title: "Začínáme s Azure protokolu integrace | Microsoft Docs"
description: "Naučte se nainstalovat integrační službu Azure protokolu a integrovat protokoly z úložiště Azure, Azure Security Center výstrahy a protokolování auditu Azure."
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 07/26/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 9d39ecd513386b75b4b640721f80991caaf9ade8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Integrace Azure protokolu s protokolování diagnostiky Azure a předávání událostí systému Windows
Protokol integrace se službou Azure (AzLog) umožňuje integraci nezpracovaná protokoly z vašich prostředků Azure do místní informace o zabezpečení a událostí správy (SIEM) systémy. Tato integrace umožňuje mít na řídicí panel jednotná zabezpečení pro všechny vaše prostředky, místně nebo v cloudu, tak, že můžete agregovat, korelovat, analyzovat a výstrahu pro události zabezpečení související s vašimi aplikacemi.
>[!NOTE]
Další informace o protokolu integrace se službou Azure, můžete zkontrolovat [Přehled integrace protokolu Azure](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview).

Tento článek vám pomůže začít s protokolu integrace se službou Azure zaměření na instalaci služby Azlog a integrace služby s Azure Diagnostics. Služba integrace se službou Azure protokolu pak bude moct shromažďovat informace protokolu událostí systému Windows z tohoto kanálu událostí zabezpečení systému Windows z virtuálních počítačů nasazených v Azure IaaS. Toto je velmi podobné "Předávání událostí" může mít použitá na místě.

>[!NOTE]
>Schopnost převést výstup integrace Azure protokolu v do systému SIEM je poskytovaný SIEM sám sebe. Podrobnosti najdete v článku [integrace protokolu integrace se službou Azure s vašeho systému SIEM místní](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) Další informace.

Být velmi jasně - Azure protokolu integrační služba běží na fyzický nebo virtuální počítač, který používá Windows Server 2008 R2 nebo novější operační systém (Windows Server 2012 R2 nebo Windows Server 2016 jsou upřednostňované).

Fyzický počítač můžete spustit místně (nebo na lokalitu hostitel). Pokud zvolíte protokol integrace se službou Azure službu spustit na virtuálním počítači, aby virtuální počítač může být nacházejí na místních nebo ve veřejném cloudu, jako je Microsoft Azure.

Fyzický nebo virtuální počítač, spuštěna služba integrace se službou protokolu Azure vyžaduje připojení k síti na veřejném cloudu Azure. Kroky v tomto článku poskytují podrobnosti o konfiguraci.

## <a name="prerequisites"></a>Požadavky
Minimálně vyžaduje instalaci AzLog následující položky:
* **Předplatné**. Pokud žádné nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/)
* A **účet úložiště** který lze použít pro protokolování diagnostiky systému Windows Azure (můžete použít účet předem nakonfigurovaná úložiště nebo vytvořte novou – jsme ukazují, jak nakonfigurovat účet úložiště později v tomto článku)
  >[!NOTE]
  V závislosti na vašem scénáři nemusí být vyžaduje účet úložiště. Azure Diagnostics je potřeba popsaná v tomto článku, jeden scénář.
* **Dvěma systémy**: počítač, který bude spuštěna služba integrace se službou protokolu Azure a počítači, ve kterém se bude monitorovat a mít jeho protokolování informace odesílané do počítač Azlog služby.
   * Počítač, který chcete sledovat – to je virtuální počítač spuštěn jako [virtuální počítač Azure](../virtual-machines/virtual-machines-windows-overview.md)
   * Počítač, který bude spuštěna služba integrace Azure protokolu; Tento počítač bude shromažďovat všechny informace protokolu, který bude později importovat do vašeho systému SIEM.
    * Tento systém může být místní nebo v Microsoft Azure.  
    * Je nutné používat x64 verzi systému Windows server 2008 R2 SP1 nebo vyšší a mít technologie .NET 4.5.1 nainstalována. Můžete určit verze rozhraní .NET nainstalované podle následujícího článku s názvem [postupy: určení rozhraní .NET Framework verze jsou nainstalované](https://msdn.microsoft.com/library/hh925568)  
    Musí mít připojení k účtu úložiště Azure pro Azure protokolování diagnostiky. Poskytujeme pokyny později v tomto článku na tom, jak si můžete ověřit tyto možnosti připojení

Pro rychlé ukázka proces vytváření virtuálního počítače pomocí portálu Azure podívejte se na video níže.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]



## <a name="deployment-considerations"></a>Aspekty nasazování
Během testování protokolu integrace se službou Azure, můžete použít libovolný systém, který splňuje požadavky na minimální verzi operačního systému. Ale pro produkční prostředí zatížení může vyžadovat plánování Škálováním.

Můžete spustit víc instancí služby protokolu integrace se službou Azure (jedna instance na fyzický nebo virtuální počítač), pokud je svazek událostí vysoké. Kromě toho můžete načíst zůstatek, který účty úložiště Azure Diagnostics pro Windows (WAD) a počet odběrů k poskytování instance by měla být založena na vaše kapacita.
>[!NOTE]
V tuto chvíli nemáme konkrétní doporučení pro kdy škálovat instance protokolů Azure integrace počítače (tj, počítačům, které jsou spuštěné integrační služba protokolů Azure) nebo pro účty úložiště nebo předplatných. Škálování rozhodnutí, která by měla být založena na vaše připomínky výkonu v každé z těchto oblastí.

Máte také možnost škálování Azure protokolu integrační služba s účelem zlepšení výkonu. V tématu Nastavení velikosti na počítače, které chcete spustit služba integrace protokolů Azure vám můžou pomoct následující metriky výkonu:
* Na počítači 8 procesor (základní) může jedna instance Azlog integrátor zpracovat přibližně 24 milión událostí za den (~1M/hour).

* Na počítači 4procesoru (základní) může jedna instance Azlog integrátor zpracovat asi 1,5 milionu událostí za den (~62.5K/hour).

## <a name="install-azure-log-integration"></a>Nainstalujte integrační protokolů Azure
K instalaci protokolu integrace se službou Azure, budete muset stáhnout [integrace Azure protokolu](https://www.microsoft.com/download/details.aspx?id=53324) instalační soubor. Spusťte pomocí rutiny instalační program a rozhodnout, pokud byste chtěli poskytnout telemetrické informace společnosti Microsoft.  

![Instalace obrazovky se zaškrtnutým políčkem telemetrie](./media/security-azure-log-integration-get-started/telemetry.png)

*
> [!NOTE]
> Doporučujeme vám, že povolíte společnosti Microsoft shromažďovat telemetrická data. Zrušením zaškrtnutí políčka tuto možnost můžete vypnout kolekce telemetrická data.
>


Služba integrace protokolu Azure shromažďuje telemetrická data z počítače, na kterém je nainstalována.  

Mezi shromažďovaná telemetrická data je:

* Výjimky, ke kterým došlo během provádění integrace protokolů Azure
* Metriky o počtu dotazy a zpracování událostí
* Statistiky, o které Azlog.exe jsou používány možnosti příkazového řádku

Proces instalace je popsaná v níže uvedené video.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]



## <a name="post-installation-and-validation-steps"></a>POST kroky instalace a ověření
Po dokončení rutiny základní nastavení, jste připravené krok k provedení po instalaci a ověření kroky:
1. Otevřete okno prostředí PowerShell se zvýšenými oprávněními a přejděte do **c:\Program Files\Microsoft Azure protokolu integrace**
2. Prvním krokem, které je nutné vzít je získat rutiny AzLog importovat. Můžete to udělat spuštěním skriptu **LoadAzlogModule.ps1** (Všimněte si, ". \" v následujícím příkazu). Typ **.\LoadAzlogModule.ps1** a stiskněte klávesu **ENTER**.  
Měli byste vidět něco podobného jako co se zobrazí na obrázku níže. </br></br>
![Instalace obrazovky se zaškrtnutým políčkem telemetrie](./media/security-azure-log-integration-get-started/loaded-modules.png) </br></br>
3. Teď je potřeba nakonfigurovat AzLog používat konkrétní prostředí Azure. "Prostředí Azure" je "typ" datového centra cloudu Azure, kterou chcete pracovat. Existuje několik prostředí Azure v tuto chvíli, aktuálně příslušné možnosti jsou buď **AzureCloud** nebo **AzureUSGovernment**.   Ujistěte se, abyste byli v v prostředí PowerShell zvýšenými **c:\program files\Microsoft Azure protokolu integrace\** </br></br>
    Jakmile existuje, spusťte příkaz: </br>
    ``Set-AzlogAzureEnvironment -Name AzureCloud``(pro Azure komerční)

      >[!NOTE]
      Pokud je příkaz úspěšná, neobdržíte žádné zpětnou vazbu.  Pokud chcete použít cloud Azure Government nám, byste použili **AzureUSGovernment** (pro proměnnou-Name) pro cloud vlády USA. V tuto chvíli nepodporuje ostatních cloudů Azure.  
4. Aby bylo možné sledovat systému potřebujete název účtu úložiště používá Azure Diagnostics.  Na portálu Azure přejděte do **virtuální počítače** a podívejte se na virtuální počítač, který budete sledovat. V **vlastnosti** zvolte **nastavení pro diagnostiku**.  Klikněte na **agenta** a poznamenejte si název účtu úložiště určený. Je nutné tento název účtu pro později.
![Nastavení diagnostiky Azure](./media/security-azure-log-integration-get-started/storage-account-large.png) </br></br>

      ![Nastavení diagnostiky Azure](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)
      >[!NOTE]
      Pokud monitorování není povoleno při vytvoření virtuálního počítače budete mít možnost povolit, jak je uvedeno výše.
5. Nyní jsme budete přepínat naše pozornost zpět na počítač integrace Azure protokolů. Musíme ověřit, že máte připojení k účtu úložiště ze systému nainstalovanou protokolu integrace se službou Azure. Fyzický počítač nebo virtuální počítač se systémem Azure protokolu integrační služba potřebuje přístup k účtu úložiště k načtení informací zaznamenaných Azure Diagnostics jako nastaveny na všech monitorovaných systémů.  
  1. Můžete si stáhnout Azure Storage Explorer [zde](http://storageexplorer.com/).
  2. Spusťte pomocí rutiny instalační program
  3. Po dokončení instalace klikněte na tlačítko **Další** a nechte políčko **spusťte Microsoft Azure Storage Explorer** zaškrtnutí.  
  4. Přihlaste se k Azure.
  5. Ověřte, zda se zobrazí účet úložiště, který jste nakonfigurovali pro Azure Diagnostics.  
![Účty úložiště](./media/security-azure-log-integration-get-started/storage-account.jpg) </br></br>
   6. Všimněte si, že nejsou k dispozici několik možností v části účty úložiště. Jeden z nich je **tabulky**. V části **tabulky** byste měli vidět jednu s názvem **WADWindowsEventLogsTable**. </br></br>
   ![Účty úložiště](./media/security-azure-log-integration-get-started/storage-explorer.png) </br>

## <a name="integrate-azure-diagnostic-logging"></a>Integrovat protokolování diagnostiky Azure
V tomto kroku nakonfigurujete počítači spuštěna služba integrace se službou Azure protokol pro připojení k účtu úložiště, který obsahuje soubory protokolu.
K dokončení tohoto kroku bude potřebujeme pár věcí předem.  
* **FriendlyNameForSource:** Toto je popisný název, který můžete použít k účtu úložiště, že jste nakonfigurovali virtuální počítač k ukládání informací z Azure Diagnostics
* **StorageAccountName:** jedná o název účtu úložiště, kterou jste zadali při konfiguraci Azure diagnotics.  
* **Klíč úložiště:** Toto je klíč úložiště pro účet úložiště se uloží informace o Azure Diagnostics pro tento virtuální počítač.  

Proveďte následující kroky k získání klíče úložiště:
 1. Vyhledejte [portál Azure](http://portal.azure.com).
 2. V navigačním podokně konzoly Azure, přejděte do dolní a klikněte na tlačítko **další služby.**

 ![Další služby](./media/security-azure-log-integration-get-started/more-services.png)
 3. Zadejte **úložiště** v **filtru** textové pole. Klikněte na tlačítko **účty úložiště** (to se zobrazí po zadání **úložiště**)

   ![pole filtru](./media/security-azure-log-integration-get-started/filter.png)
 4. Zobrazí se seznam účtů úložiště, dvakrát klikněte na účet, který jste přiřadili k úložiště protokolů.

   ![seznam účtů úložiště](./media/security-azure-log-integration-get-started/storage-accounts.png)
 5. Klikněte na **přístupové klíče** v **nastavení** části.

  ![Přístupové klávesy](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 6. Kopírování **key1** a umístí jej na bezpečném místě, který je k dispozici pro další krok.

   ![dva přístupové klíče](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 7. Na serveru nainstalovaný Azure protokolu integrace otevřete příkazový řádek se zvýšenými oprávněními (Všimněte si, že okno příkazového řádku se zvýšenými oprávněními používáme v tomto poli, není prostředí PowerShell konzolu se zvýšenými oprávněními).
 8. Přejděte na **c:\Program Files\Microsoft Azure protokolu integrace**
 9. Spusťte ``Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey> ``. </br> Například ``Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`` budete-li ID předplatného objeví události XML, připojí k popisný název ID předplatného: ``Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`` nebo například``Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==``

>[!NOTE]  
Počkejte, až 60 minut a potom zobrazit události, které jsou vyžádány z účtu úložiště. Pokud chcete zobrazit, Otevřít **Prohlížeč událostí > protokoly systému Windows > předávaných událostí ty** na integrátor Azlog.

Zde se zobrazí video nad kroky popsané výše.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Co v případě dat se nezobrazí ve složce předávaných událostí?
Pokud po hodině data se nezobrazí v **předávaných událostí ty** složku, pak:

1. Zkontrolujte počítači spuštěna služba integrace se službou protokolu Azure a ověřte, že má přístup k Azure. Chcete-li otestovat připojení, zkuste otevřete [portál Azure](http://portal.azure.com) z prohlížeče.
2. Ujistěte se, že uživatelský účet **Azlog** má oprávnění k zápisu do složky **users\Azlog**.
  <ol type="a">
   <li>Otevřete **Průzkumníka Windows** </li>
  <li> Přejděte na **c:\users** </li>
  <li> Klikněte pravým tlačítkem na **c:\users\Azlog** </li>
  <li> Klikněte na **zabezpečení**  </li>
  <li> Klikněte na **NT Service\Azlog** a zkontrolujte oprávnění pro účet. Pokud účet na této kartě chybí, nebo pokud příslušná oprávnění nejsou aktuálně ukazuje můžete udělit práva účtu na této kartě.</li>
  </ol>
3.Ujistěte se, účet úložiště přidat v příkazu **přidat zdroj Azlog** je uveden při spuštění příkazu **Azlog zdrojového seznamu**.
4. Přejděte na **Prohlížeč událostí > protokoly systému Windows > aplikace** chcete zobrazit, pokud nejsou žádné chyby nahlásila integrace protokolů Azure.


Pokud narazíte na potíže během instalace a konfigurace, otevřete prosím [žádost o podporu](../azure-supportability/how-to-create-azure-support-request.md), vyberte **integrace protokolu** jako službu, pro kterou jsou žádosti o podporu.

Další možností podpory je [fórum MSDN integrace protokolu Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). Zde komunity může podporovat vzájemně otázky, odpovědi, tipy a triky o tom, jak využít integrace se službou Azure protokolu maximum. Kromě toho týmem protokolu integrace se službou Azure sleduje toto fórum a pomůže vždy, když můžeme.

## <a name="next-steps"></a>Další kroky
Další informace o protokolu integrace se službou Azure, najdete v následujících dokumentech:

* [Microsoft Azure protokolu integrace pro Azure protokoly](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center podrobnosti, požadavky na systém a instalovat pokyny týkající se integrace protokolů Azure.
* [Úvod do integrace Azure protokolu](security-azure-log-integration-overview.md) – tento dokument vás seznámí s integrace Azure protokolu, jejích klíčových funkcích a jak to funguje.
* [Partner kroky konfigurace](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – tento příspěvek blogu ukazuje, jak nakonfigurovat integraci Azure protokolu pro práci s partnerských řešení Splunk, HP ArcSight a IBM QRadar. Toto je naše aktuální pokyny o tom, jak konfigurovat součásti systému SIEM. Zkontrolujte u dodavatele SIEM nejprve další podrobnosti.
* [Protokolů Azure integrace nejčastější dotazy (FAQ)](security-azure-log-integration-faq.md) – nejčastější dotazy týkající se tento odpovědi dotazy týkající se integrace protokolů Azure.
* [Integrace Security Center výstrahy s Azure protokolu integrace](../security-center/security-center-integrating-alerts-with-log-integration.md) – tento dokument ukazuje, jak synchronizovat výstrahy Security Center spolu s shromážděné pomocí diagnostiky Azure a Azure protokoly aktivity, analýzy protokolů nebo řešení SIEM události zabezpečení virtuálního počítače.
* [Nové funkce pro protokoly auditu Azure a Azure diagnostics](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – tento příspěvek blogu vás seznámí s protokoly auditu Azure a další funkce, které vám pomůžou získat přehled o činnosti vašich prostředků Azure.
