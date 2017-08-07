---
title: "Vykreslování v cloudu pomocí služby Azure Batch Rendering | Dokumentace Microsoftu"
description: "Úlohy vykreslování na virtuálních počítačích Azure přímo z aplikace Maya a s platbami za použití."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: tamram
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: 4d22f92cafdbceee5213361d6d2b2f38904d12c6
ms.contentlocale: cs-cz
ms.lasthandoff: 08/01/2017

---

# <a name="get-started-with-the-batch-rendering-service"></a>Začínáme se službou Batch Rendering

Služba Azure Batch Rendering nabízí možnosti vykreslování škálované na cloud a s platbami za použití. Služba Batch Rendering zajišťuje plánování úloh a jejich zařazování do fronty, správu chyb a opakování a automatické škálování úloh vykreslování. Služba Batch Rendering podporuje aplikace Autodesk Maya, 3ds Max a Arnold a podpora dalších se připravuje. Modul plug-in služby Batch pro aplikaci Maya 2017 umožňuje snadné spouštění úloh vykreslování v Azure přímo z plochy. 

## <a name="supported-applications"></a>Podporované aplikace

Služba Batch Rendering aktuálně podporuje následující aplikace:

- Autodesk Maya
- Autodesk 3ds Max
- Autodesk Arnold

## <a name="prerequisites"></a>Požadavky

Pokud chcete používat službu Batch Rendering, potřebujete:

- [Účet Azure](https://azure.microsoft.com/free/). 
- **Účet Azure Batch.** Pokyny k vytvoření účtu Batch na webu Azure Portal najdete v tématu [Vytvoření účtu Batch pomocí webu Azure Portal](batch-account-create-portal.md).
- **Účet služby Azure Storage.** Ve službě Azure Storage se ukládají prostředky používané pro vaši úlohu vykreslování. Účet úložiště můžete vytvořit automaticky při nastavování účtu Batch. Můžete také použít existující účet úložiště. Další informace o účtech služby Storage najdete v tématu [Vytvoření, správa nebo odstranění účtu úložiště na webu Azure Portal](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

Pokud chcete používat modul plug-in služby Batch pro aplikaci Maya, potřebujete:

- **Maya 2017**
- **Arnold pro Maya**

Pomocí webu [Azure Portal](https://portal.azure.com) můžete také vytvořit fondy virtuálních počítačů s předem nakonfigurovanými aplikacemi Maya, 3ds Max a Arnold. Pomocí portálu můžete monitorovat úlohy a diagnostikovat úkoly, které selhaly, stažením aplikačních protokolů a vzdáleným připojením k jednotlivým virtuálním počítačům pomocí protokolu RDP nebo SSH.

## <a name="basic-batch-concepts"></a>Koncepty služby Batch úrovně Basic

Než začnete používat službu Batch Rendering, je vhodné se seznámit s několika koncepty služby Batch, mezi které patří výpočetní uzly, fondy a úlohy. Další obecné informace o službě Azure Batch najdete v tématu [Spouštění vnitřně paralelních úloh pomocí služby Batch](batch-technical-overview.md).

### <a name="pools"></a>Fondy

Batch je služba platformy pro spouštění úloh náročných na výpočetní výkon, jako je vykreslování, ve **fondu** **výpočetních uzlů**. Každý výpočetní uzel ve fondu je virtuální počítač Azure s Linuxem nebo Windows. 

Další informace o fondech a výpočetních uzlech služby Batch najdete v částech [Fond](batch-api-basics.md#pool) a [Výpočetní uzel](batch-api-basics.md#compute-node) v tématu [Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch](batch-api-basics.md).

### <a name="jobs"></a>Úlohy

**Úloha** služby Batch je kolekce úkolů, které se spouštějí na výpočetních uzlech ve fondu. Když odešlete úlohu vykreslování, služba Batch úlohu rozdělí na úkoly a ty distribuuje ke spuštění do výpočetních uzlů ve fondu.

Další informace o úlohách služby Batch najdete v části [Úloha](batch-api-basics.md#job) v tématu [Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch](batch-api-basics.md).

## <a name="use-the-batch-plug-in-for-maya-to-submit-a-render-job"></a>Odeslání úlohy vykreslování pomocí modulu plug-in služby Batch pro aplikaci Maya

S modulem plug-in služby Batch pro aplikaci Maya můžete úlohu odeslat do služby Batch Rendering přímo z aplikace Maya. Následující části popisují, jak v modulu plug-in nakonfigurovat úlohu a pak ji odeslat. 

### <a name="load-the-batch-plug-in-in-maya"></a>Načtení modulu plug-in služby Batch v aplikaci Maya

Modul plug-in služby Batch je k dispozici na [GitHubu](https://github.com/Azure/azure-batch-maya/releases). Rozbalte archiv do libovolného adresáře. Modul plug-in můžete načíst přímo z adresáře *azure_batch_maya*.

Načtení modulu plug-in v aplikaci Maya:

1. Spusťte aplikaci Maya.
2. Otevřete **Window** (Okno) > **Settings/Preferences** (Nastavení/Předvolby) > **Plug-in Manager** (Správce modulů plug-in).
3. Klikněte na **Browse** (Procházet).
4. Přejděte k souboru *azure_batch_maya/plug-in/AzureBatch.py* a vyberte ho.

### <a name="authenticate-access-to-your-batch-and-storage-accounts"></a>Ověření přístupu k účtům Batch a Storage

Abyste mohli modul plug-in používat, je potřeba se přihlásit pomocí klíčů účtů Azure Batch a Azure Storage. Načtení klíčů účtů:

1. Zobrazte modul plug-in v aplikaci Maya a vyberte kartu **Config** (Konfigurace).
2. Přejděte na [Azure Portal](https://portal.azure.com).
3. Z nabídky vlevo vyberte **Účty Batch**. V případě potřeby klikněte na **Další služby** a vyfiltrujte _Batch_.
4. V seznamu vyhledejte požadovaný účet Batch.
5. Vyberte položku nabídky **Klíče**. Zobrazí se název účtu, adresa URL účtu a přístupové klíče:
    - Vložte adresu URL účtu Batch do pole **Service** (Služba) v modulu plug-in služby Batch.
    - Vložte název účtu do pole **Batch Account** (Účet Batch).
    - Vložte primární klíč účtu do pole **Batch Key** (Klíč služby Batch).
7. Z nabídky vlevo vyberte Účty úložiště. V případě potřeby klikněte na **Další služby** a vyfiltrujte _Storage_.
8. V seznamu vyhledejte požadovaný účet služby Storage.
9. Vyberte položku nabídky **Přístupové klíče**. Zobrazí se název účtu úložiště a klíče.
    - Vložte název účtu služby Storage do pole **Storage Account** (Účet služby Storage) v modulu plug-in služby Batch.
    - Vložte primární klíč účtu do pole **Storage Key** (Klíč služby Storage).
10. Klikněte na **Authenticate** (Ověřit) a ujistěte se, že má modul plug-in přístup k oběma účtům.

Jakmile úspěšně proběhne ověření, modul plug-in nastaví pole stavu na hodnotu **Authenticated** (Ověřeno): 

![Ověření účtů Batch a Storage](./media/batch-rendering-service/authentication.png)

### <a name="configure-a-pool-for-a-render-job"></a>Konfigurace fondu pro úlohu vykreslování

Po ověření účtů Batch a Storage nastavte fond pro úlohu vykreslování. Modul plug-in ukládá vaše výběry mezi relacemi. Jakmile nastavíte upřednostňovanou konfiguraci, nebude nutné ji upravovat, dokud se nezmění.

Následující části vás provedou dostupnými možnostmi na kartě **Submit** (Odeslat):

#### <a name="specify-a-new-or-existing-pool"></a>Zadání nového nebo existujícího fondu

Pokud chcete zadat fond, ve kterém se má spustit úloha vykreslování, vyberte kartu **Submit** (Odeslat). Tato karta nabízí možnosti pro vytvoření fondu nebo výběr existujícího fondu:

- Můžete automaticky zřídit fond pro tuto úlohu (výchozí možnost **Auto provision a pool for this job**). Pokud vyberete tuto možnost, služba Batch vytvoří fond výhradně pro aktuální úlohu a po dokončení úlohy vykreslování tento fond automaticky odstraní. Tato možnost je nejlepší v případě, že máte na dokončení jedinou úlohu vykreslování.
- Můžete znovu použít existující trvalý fond (možnost **Reuse an existing persistent pool**). Pokud máte existující fond, který je nečinný, můžete ho zadat pro spuštění úlohy vykreslování výběrem z rozevíracího seznamu. Opětovné použití existujícího trvalého fondu šetří čas potřebný ke zřízení fondu.  
- Můžete vytvořit nový trvalý fond (možnost **Create a new persistent pool**). Pokud vyberete tuto možnost, pro spuštění úlohy se vytvoří nový fond. Po dokončení úlohy se fond neodstraní, takže ho můžete znovu použít pro budoucí úlohy. Tuto možnost vyberte, pokud budete i nadále potřebovat spouštět úlohy vykreslování. U dalších úloh můžete vybrat možnost **Reuse an existing persistent pool** (Znovu použít existující trvalý fond) a použít trvalý fond, který jste vytvořili pro první úlohu.

![Zadání fondu, image operačního systému, velikosti virtuálního počítače a licence](./media/batch-rendering-service/submit.png)

Další informace o tom, jak nabíhají poplatky za virtuální počítače Azure, najdete v [nejčastějších dotazech k cenám pro Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#faq) a [nejčastějších dotazech k cenám pro Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/#faq).

#### <a name="specify-the-os-image-to-provision"></a>Zadání image operačního systému ke zřízení

Typ image operačního systému, který se použije ke zřízení výpočetních uzlů ve fondu, můžete zadat na kartě **Env** (Environment, Prostředí). Služba Batch aktuálně pro úlohy vykreslování podporuje následující možnosti imagí:

|Operační systém  |Image  |
|---------|---------|
|Linux     |Batch CentOS Preview |
|Windows     |Batch Windows Preview |

#### <a name="choose-a-vm-size"></a>Výběr velikosti virtuálního počítače

Velikost virtuálního počítače můžete zadat na kartě **Env** (Prostředí). Další informace o dostupných velikostech virtuálních počítačů najdete v tématech [Velikosti virtuálních počítačů s Linuxem v Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) a [Velikosti virtuálních počítačů s Windows v Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes). 

![Zadání image operačního systému a velikosti virtuálního počítače na kartě Env (Prostředí)](./media/batch-rendering-service/environment.png)

#### <a name="specify-licensing-options"></a>Zadání možností licencování

Licence, které chcete použít, můžete zadat na kartě **Env** (Prostředí). Mezi možnosti patří:

- **Maya** – Tato možnost je povolená ve výchozím nastavení.
- **Arnold** – Tato možnost je povolená, pokud se v aplikaci Maya zjistí Arnold jako aktivní vykreslovací jádro.

 Pokud chcete vykreslovat s použitím vlastní licence, můžete nakonfigurovat koncový bod licence přidáním odpovídajících proměnných prostředí do tabulky. Pokud to uděláte, nezapomeňte zrušit výběr výchozích možností licencování.

> [!IMPORTANT]
> Používání licencí se vám účtuje po dobu spuštění virtuálních počítačů ve fondu, i když se tyto virtuální počítače právě nepoužívají pro vykreslování. Pokud se chcete vyhnout dalším poplatkům, přejděte na kartu **Pools** (Fondy) a změňte velikost fondu na 0 uzlů, dokud nebudete připraveni spustit další úlohu vykreslování. 
>
>

#### <a name="manage-persistent-pools"></a>Správa trvalých fondů

Existující trvalý fond můžete spravovat na kartě **Pools** (Fondy). Po výběru fondu ze seznamu se zobrazí jeho aktuální stav.

Na kartě **Pools** (Fondy) můžete také odstranit fond a měnit počet virtuálních počítačů ve fondu. Můžete změnit velikost fondu na 0 uzlů a vyhnout se tak účtování poplatků mezi úlohami.

![Zobrazení, změna velikosti a odstranění fondů](./media/batch-rendering-service/pools.png)

### <a name="configure-a-render-job-for-submission"></a>Konfigurace úlohy vykreslování pro odeslání

Po zadání parametrů pro fond, ve kterém se spustí úloha vykreslování, nakonfigurujte samotnou úlohu. 

#### <a name="specify-scene-parameters"></a>Zadání parametrů scény

Modul plug-in služby Batch rozpozná, které vykreslovací jádro v aplikaci Maya aktuálně používáte, a na kartě **Submit** (Odeslat) zobrazí odpovídající nastavení. Tato nastavení zahrnují počáteční snímek, konečný snímek, předponu výstupu a krokování snímků. Nastavení vykreslení souboru scény můžete přepsat zadáním jiného nastavení v modulu plug-in. Změny, které provedete v nastavení modulu plug-in, se neuchovávají v nastavení vykreslení souboru scény, takže můžete provádět změny pro jednotlivé úlohy, aniž byste museli znovu nahrávat soubor scény.

Modul plug-in vás upozorní, pokud vybrané vykreslovací jádro v aplikaci Maya není podporováno.

Pokud načítáte novou scénu, když je modul plug-in otevřený, klikněte na tlačítko **Refresh** (Aktualizovat), abyste zajistili aktualizaci nastavení.

#### <a name="resolve-asset-paths"></a>Překlad cest k prostředkům

Modul plug-in po načtení vyhledá v souboru scény všechny odkazy na externí soubory. Tyto odkazy se zobrazí na kartě **Assets** (Prostředky). Pokud odkazovanou cestu není možné přeložit, modul plug-in se pokusí soubor najít v několika výchozích umístěních, mezi která patří:

- Umístění souboru scény 
- Adresář _sourceimages_ aktuálního projektu
- Aktuální pracovní adresář 

Pokud prostředek stáje není možné najít, je uvedený s ikonou upozornění:

![Chybějící prostředky jsou zobrazené s ikonou upozornění](./media/batch-rendering-service/missing_assets.png)

Pokud znáte umístění nepřeloženého odkazu na soubor, můžete kliknout na ikonu upozornění a zobrazí se výzva k přidání cesty pro hledání. Modul plug-in se pak s použitím této cesty pro hledání pokusí přeložit chybějící prostředky. Můžete přidat libovolný počet dalších cest pro hledání.

Když se odkaz přeloží, je uvedený s ikonou zeleného světla:

![Přeložené prostředky s ikonou zeleného světla](./media/batch-rendering-service/found_assets.png)

Pokud vaše scéna vyžaduje další soubory, které modul plug-in nerozpoznal, můžete přidat další soubory nebo adresáře. Použijte k tomu tlačítka **Add Files** (Přidat soubory) a **Add Directory** (Přidat adresář). Pokud načítáte novou scénu, když je modul plug-in otevřený, nezapomeňte kliknout na **Refresh** (Aktualizovat), aby se aktualizovaly odkazy scény.

#### <a name="upload-assets-to-an-asset-project"></a>Nahrání prostředků do projektu prostředků

Po odeslání úlohy vykreslování se odkazované soubory zobrazené na kartě **Assets** (Prostředky) automaticky nahrají do služby Azure Storage jako projekt prostředků. Soubory prostředků můžete také nahrát nezávisle na úloze vykreslování pomocí tlačítka **Upload** (Nahrát) na kartě **Assets** (Prostředky). Název projektu prostředků se zadává v poli **Project** (Projekt) a ve výchozím nastavení je pojmenovaný podle aktuálního projektu v aplikaci Maya. Při nahrávání souborů prostředků je zachována místní struktura souborů. 

Po nahrání může na prostředky odkazovat jakýkoli počet úloh vykreslování. Všechny nahrané prostředky jsou k dispozici pro jakoukoli úlohu, která odkazuje na projekt prostředků, bez ohledu na to, jestli jsou prostředky součástí scény. Pokud chcete u další úlohy změnit projekt prostředků, na který odkazuje, změňte název v poli **Project** (Projekt) na kartě **Assets** (Prostředky). Pokud chcete nějaké odkazované soubory vyloučit z nahrání, zrušte jejich výběr pomocí zeleného tlačítka vedle příslušného záznamu.

#### <a name="submit-and-monitor-the-render-job"></a>Odeslání a monitorování úlohy vykreslování

Po nakonfigurování úlohy vykreslování v modulu plug-in kliknutím na tlačítko **Submit Job** (Odeslat úlohu) na kartě **Submit** (Odeslat) úlohu odešlete do služby Batch:

![Odeslání úlohy vykreslování](./media/batch-rendering-service/submit_job.png)

Probíhající úlohu můžete monitorovat na kartě **Jobs** (Úlohy) modulu plug-in. Pokud chcete zobrazit aktuální stav úlohy, vyberte úlohu ze seznamu. Na této kartě můžete také rušit a odstraňovat úlohy nebo stáhnout výstupy a protokoly vykreslování. 

Pokud chcete stáhnout výstupy, upravte pole **Outputs** (Výstupy) a nastavte požadovaný cílový adresář. Kliknutím na ikonu ozubeného kola spusťte proces na pozadí, který sleduje úlohu a průběžně stahuje výstupy: 

![Zobrazení stavu úlohy a stažení výstupů](./media/batch-rendering-service/jobs.png)

Aplikaci Maya můžete zavřít, aniž by se přerušil proces stahování.

## <a name="next-steps"></a>Další kroky

Další informace o službě Batch najdete v tématu [Spouštění vnitřně paralelních úloh pomocí služby Batch](batch-technical-overview.md).
