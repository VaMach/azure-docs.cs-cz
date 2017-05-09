---
title: "Začínáme se Storage Explorerem (Preview) | Dokumentace Microsoftu"
description: "Správa prostředků úložiště Azure Storage pomocí Storage Exploreru (Preview)"
services: storage
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: tarcher
ms.translationtype: Human Translation
ms.sourcegitcommit: a3ca1527eee068e952f81f6629d7160803b3f45a
ms.openlocfilehash: fbcd35529c5d2360f5b0c9de4d3c9c4a08a0cc8f
ms.contentlocale: cs-cz
ms.lasthandoff: 04/27/2017


---
# <a name="get-started-with-storage-explorer-preview"></a>Začínáme se Storage Explorerem (Preview)
## <a name="overview"></a>Přehled
Azure Storage Explorer (Preview) je samostatná aplikace, která umožňuje jednoduchou práci s daty ve službě Azure Storage v systémech Windows, macOS a Linux. V tomto článku se dozvíte, jakými různými způsoby se můžete připojovat k účtům Azure Storage a spravovat je.

![Microsoft Azure Storage Explorer (Preview)][15]

## <a name="prerequisites"></a>Požadavky
* [Stažení a instalace Storage Exploreru (Preview)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Připojení k účtu úložiště nebo službě
Storage Explorer (Preview) nabízí několik způsobů, jak se připojit k účtům úložiště. Můžete například provést následující věci:
* Připojit se k účtům úložiště přidruženým k vašim předplatným Azure.
* Připojit se účtům úložiště a službám, které s vámi sdílí jiná předplatná Azure.
* Připojit se k místnímu úložišti a spravovat ho pomocí emulátoru úložiště Azure. 

Kromě toho můžete pracovat s účty úložiště v globálním i národním Azure:

* [Připojení k předplatnému Azure:](#connect-to-an-azure-subscription) Umožňuje spravovat prostředky úložiště, které patří do vašeho předplatného Azure.
* [Práce s místním vývojovým úložištěm:](#work-with-local-development-storage) Umožňuje spravovat místní úložiště pomocí emulátoru úložiště Azure.
* [Připojení k externímu úložišti:](#attach-or-detach-an-external-storage-account) Umožňuje spravovat prostředky úložiště, které patří do jiného předplatného Azure nebo jiného národního cloudu Azure, pomocí názvu, klíče a koncových bodů účtu úložiště.
* [Připojení účtu úložiště pomocí SAS:](#attach-storage-account-using-sas) Umožňuje spravovat prostředky úložiště, které patří do jiného předplatného Azure pomocí sdíleného přístupového podpisu (SAS).
* [Připojení služby pomocí SAS:](#attach-service-using-sas) Umožňuje spravovat konkrétní službu úložiště (kontejner objektů blob, fronty nebo tabulky), která patří do jiného předplatného Azure, pomocí sdíleného přístupového podpisu (SAS).

## <a name="connect-to-an-azure-subscription"></a>Připojení k předplatnému Azure
> [!NOTE]
> Pokud nemáte účet Azure, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. V nástroji Storage Explorer (Preview) vyberte **Azure Account Settings** (Nastavení účtu Azure).

    ![Nastavení účtu Azure][0]

2. V levém podokně se zobrazí všechny účty Microsoft, ke kterým jste přihlášeni. Pokud se chcete připojit k jinému účtu, vyberte **Add an account** (Přidat účet) a podle pokynů se přihlaste účtem Microsoft, který je přidružený minimálně k jednomu aktivnímu předplatnému Azure.

    >[!NOTE]
    >Připojení k národnímu Azure (jako je Azure Germany, Azure Government a Azure China přes přihlášení) se v současné době nepodporuje. Informace o připojení k účtům úložiště národního Azure najdete v části Připojení nebo odpojení externího účtu úložiště.

3. Jakmile se úspěšně přihlásíte účtem Microsoft, zobrazí se v levém podokně předplatná Azure přidružená k tomuto účtu. Vyberte předplatná Azure, se kterými chcete pracovat, a pak vyberte **Apply** (Použít). (Zaškrtnutím políčka **All subscriptions** (Všechna předplatná) přepínáte výběr všech nebo žádných z uvedených předplatných Azure.)

    ![Výběr předplatných Azure][3]  
    V levém podokně se zobrazí všechny účty úložišť, přidružené k vybraným předplatným Azure.

    ![Vybraná předplatná Azure][4]

## <a name="connect-to-an-azure-stack-subscription"></a>Připojení k předplatnému Azure Stack

Pro zajištění vzdáleného přístupu Storage Exploreru k předplatnému služby Azure Stack potřebujete připojení VPN. Další informace o tom, jak nastavit připojení VPN ke službě Azure Stack, najdete v tématu [Připojení ke službě Azure Stack pomocí sítě VPN](azure-stack/azure-stack-connect-azure-stack.md#connect-with-vpn).

Pro testování konceptu služby Azure Stack je potřeba vyexportovat kořenový certifikát autority Azure Stack. Postupujte následovně:

1. Otevřete `mmc.exe` v MAS-CON01, hostitelském počítači služby Azure Stack nebo místním počítači s připojením VPN ke službě Azure Stack. 

2. V části **Soubor** vyberte **Přidat nebo odebrat modul snap-in** a pak přidejte **certifikáty** pro správu **účtu** **místního počítače**.

    ![Načtení kořenového certifikátu služby Azure Stack pomocí mmc.exe][25]   

3. Vyhledejte **AzureStackCertificationAuthority** ve složce **Console Root\Certificated (Local Computer)\Trusted Root Certification Authorities\Certificates**. 

4. Klikněte na položku pravým tlačítkem myši, vyberte **Všechny úlohy** > **Exportovat**, postupujte podle pokynů a vyexportujte certifikát s použitím formátu **X.509, kódování Base-64 (.CER)**.  

    Vyexportovaný certifikát se použije v dalším kroku.   

    ![Export kořenového certifikátu autority služby Azure Stack][26]   

5. Ve Storage Exploreru (Preview) v nabídce **Úpravy** přejděte na **Certifikáty SSL** a pak vyberte **Importovat certifikáty**. Pomocí dialogového okna pro výběr souborů najděte a otevřete certifikát, který jste exportovali v předchozím kroku.  

    Po dokončení importu se zobrazí výzva k restartování Storage Exploreru.

    ![Import certifikátu do Storage Exploreru (Preview)][27]

6. Po restartování Storage Exploreru (Preview) vyberte nabídku **Úpravy** a ujistěte se, že je vybraná možnost **Target Azure Stack** (Cíl – Azure Stack). Pokud není, vyberte ji a restartujte Storage Explorer, aby se tato změna projevila. Tato konfigurace je nutná pro kompatibilitu s vaším prostředím Azure Stack.

    ![Kontrola, že položka Target Azure Stack je vybraná][28]

7. V levém podokně vyberte **Spravovat účty**.  
    Zobrazí se všechny účty Microsoft, ke kterým jste přihlášeni.

8. Pokud se chcete připojit k účtu Azure Stack, vyberte **Přidat účet**.

    ![Přidání účtu Azure Stack][29]

9. V dialogovém okně **Přidat nový účet** v části **Prostředí Azure** vyberte **Create Custom Environment** (Vytvořit vlastní prostředí) a potom klikněte na **Další**.

10. Zadejte všechny požadované informace pro vlastní prostředí Azure Stack a potom klikněte na **Přihlásit se**. 

11. Pokud se chcete přihlásit pomocí účtu Azure Stack, který je přidružený alespoň k jednomu aktivnímu předplatnému Azure Stack, vyplňte dialogové okno **Sign in to a Custom Cloud environment**(Přihlásit se k vlastnímu cloudovému prostředí).  

    Podrobnosti k jednotlivým polím:

    * **Environment name** (Název prostředí): Toto pole může uživatel přizpůsobit.
    * **Authority** (Autorita): Hodnotou by měla být adresa https://login.windows.net. Pro Azure China použijte https://login.chinacloudapi.cn.
    * **Sign in resource id** (ID prostředku přihlášení): Hodnotu načtěte spuštěním jednoho z následujících skriptů PowerShellu:

        Pokud jste správce cloudu:

        ```powershell
        PowerShell (Invoke-RestMethod -Uri https://adminmanagement.local.azurestack.external/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]
        ```

        Pokud jste tenant:

        ```powershell
        PowerShell (Invoke-RestMethod -Uri https://management.local.azurestack.external/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]
        ```

    * **Graph endpoint** (Koncový bod grafu): Hodnotou by měla být adresa https://graph.windows.net. Pro Azure China použijte https://graph.chinacloudapi.cn.
    * **ARM resource id** (ID prostředku ARM): Použijte stejnou hodnotu jako pro **Sign in resource id**.
    * **ARM resource endpoint** (Koncový bod prostředku ARM): Toto jsou ukázky koncových bodů prostředků Azure Resource Manageru:

        * Pro správce cloudu: https://adminmanagement.local.azurestack.external   
        * Pro tenanta: https://management.local.azurestack.external
 
    * **Tenant Ids** (ID tenanta): Volitelné. Hodnota je uvedená jenom v případě, že musí být zadaný adresář.

12. Po úspěšném přihlášení pomocí účtu Azure Stack se v levém podokně zobrazí předplatná Azure Stack přidružená k tomuto účtu. Vyberte předplatná Azure Stack, se kterými chcete pracovat, a pak vyberte **Apply** (Použít). (Zaškrtnutím nebo zrušením zaškrtnutí políčka **All subscriptions** (Všechna předplatná) přepínáte výběr všech nebo žádných z uvedených předplatných Azure Stack.)

    ![Výběr předplatných Azure Stack po vyplnění dialogového okna Custom Cloud Environment][30]  
    V levém podokně se zobrazí všechny účty úložišť přidružené k vybraným předplatným Azure Stack.

    ![Seznam účtů úložiště, včetně účtů předplatného Azure Stack][31]

## <a name="work-with-local-development-storage"></a>Práce s místním vývojovým úložištěm
Storage Explorer (Preview) umožňuje pracovat s místním úložištěm pomocí emulátoru úložiště Azure. Můžete tak psát kód pro místní úložiště a otestovat ho, aniž byste museli mít nasazený účet úložiště v Azure, protože účet úložiště je emulovaných emulátorem úložiště Azure.

> [!NOTE]
> Emulátor úložiště Azure je momentálně podporován pouze pro Windows.
>
>

1. V levém podokně Storage Exploreru (Preview) rozbalte uzel **(Local and Attached)** > **Storage Accounts** > **(Development)** (Místní a připojené (Účty úložiště (vývoj))).

    ![Místní vývojový uzel][21]

2. Pokud jste ještě nenainstalovali emulátor úložiště Azure, budete k tomu vyzváni prostřednictvím informačního panelu. Pokud se zobrazí informační panel, vyberte možnost **Download the latest version** (Stáhnout nejnovější verzi) a nainstalujte si emulátor.

    ![Výzva ke stažení emulátoru úložiště Azure Storage][22]

3. Po nainstalování emulátoru můžete vytvářet místní objekty blob, fronty a tabulky a pracovat s nimi. Jak pracovat s jednotlivými typu účtu úložiště zjistíte na následujících odkazech:

    * [Správa prostředků Azure Blob Storage](vs-azure-tools-storage-explorer-blobs.md)
    * Správa prostředků Azure File Share Storage: *Připravuje se*
    * Správa prostředků Azure Queue Storage: *Připravuje se*
    * Správa prostředků Azure Table Storage: *Připravuje se*

## <a name="attach-or-detach-an-external-storage-account"></a>Připojení nebo odpojení externího účtu úložiště
Storage Explorer (Preview) umožňuje připojovat se k externím účtům úložiště, aby bylo možné účty úložiště snadno sdílet. Tato část vysvětluje, jak se připojit k externím účtům úložiště (a odpojit se od nich).

### <a name="get-the-storage-account-credentials"></a>Získání přihlašovacích údajů účtu úložiště
Aby bylo možné sdílet externí účet úložiště, musí vlastník tohoto účtu nejprve pro účet získat přihlašovací údaje (název účtu a klíč) a potom tyto informace nasdílet uživateli, který se chce k tomuto (externímu) účtu připojit. Přihlašovací údaje k účtu úložiště můžete získat prostřednictvím webu Azure Portal provedením následujícího postupu:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. Vyberte **Procházet**.

3. Vyberte **Účty úložiště**.

4. V okně **Účty úložiště** vyberte požadovaný účet úložiště.

5. V okně **Nastavení** pro vybraný účet úložiště vyberte **Přístupové klíče**.

    ![Možnost Přístupové klíče][5]

6. V okně **Přístupové klíče** zkopírujte hodnoty **Název účtu úložiště** a **klíč 1**, aby je bylo možné použít při připojování k účtu úložiště.

    ![Přístupové klíče][6]

### <a name="attach-to-an-external-storage-account"></a>Připojení k externímu účtu úložiště
Pokud se chcete připojit k účtu externího úložiště, budete potřebovat název účtu a klíč. Část Získání přihlašovacích údajů účtu úložiště vysvětluje, jak tyto hodnoty získat z webu Azure Portal. Na portálu se ale klíč účtu nazývá **klíč1**. Takže když vás Storage Explorer vyzve k zadání klíče účtu, zadáte hodnotu **klíč1**.

1. V nástroji Storage Explorer (Preview) vyberte položku **Connect to Azure storage** (Připojit k úložišti Azure).

    ![Možnost Připojit k úložišti Azure][23]

2. V dialogovém okně **Connect to Azure Storage** (Připojit ke službě Azure Storage) zadejte klíč účtu (hodnota **klíč1** z webu Azure Portal) a pak vyberte **Next** (Další).

    > [!NOTE]
    > Můžete zadat připojovací řetězec úložiště z účtu úložiště v národním Azure. Pokud se například chcete připojit k účtům úložiště Azure Germany, zadejte připojovací řetězce podobné těmto: 
    >
    >* DefaultEndpointsProtocol=https
    >* AccountName=cawatest03
    >* AccountKey=<klíč_účtu_úložiště>
    >* EndpointSuffix=core.cloudapi.de
    
    >Připojovací řetězec můžete získat z webu Azure Portal stejným způsobem, jako je popsán v části Získání přihlašovacích údajů účtu úložiště.

    ![Dialogové okno Připojit k úložišti Azure][24]

3. V dialogovém okně **Attach External Storage** (Připojit externí úložiště) zadejte do pole **Account name** (Název účtu) název účtu úložiště, zadejte případná další požadovaná nastavení a pak vyberte **Next** (Další).

    ![Dialogové okno Připojit externí úložiště][8]

4. V dialogovém okně **Connection Summary** (Souhrn připojení) zkontrolujte zadané informace. Pokud chcete něco změnit, vyberte možnost **Back** (Zpět) a požadovaná nastavení zadejte znovu. 

5. Vyberte **Connect** (Připojit).

6. Jakmile se externí účet úložiště úspěšně připojí, zobrazí se s textem **(External)** připojeným k názvu účtu úložiště.

    ![Výsledek připojení k externímu účtu úložiště][9]

### <a name="detach-from-an-external-storage-account"></a>Odpojení od externího účtu úložiště
1. Klikněte pravým tlačítkem myši na externí účet úložiště, který chcete odpojit, a vyberte **Detach** (Odpojit).

    ![Možnost Odpojit od úložiště][10]

2. V potvrzovací zprávě potvrďte výběrem možnosti **Yes** (Ano) odpojení od externího účtu úložiště.

## <a name="attach-a-storage-account-by-using-an-sas"></a>Připojení účtu úložiště pomocí sdíleného přístupového podpisu (SAS)
[SAS](storage/storage-dotnet-shared-access-signature-part-1.md) umožňuje správci předplatného Azure udělit dočasný přístup k účtu úložiště, aniž by musel zadávat přihlašovací údaje předplatného Azure.

Ukažme si tento scénář na příkladu. Řekněme, že uživatel A je správcem předplatného Azure a uživatel A chce uživateli B povolit přístup k účtu úložiště po omezenou dobu s určitými oprávněními:

1. Uživatel A vygeneruje sdílený přístupový podpis (tvořený připojovacím řetězcem pro účet úložiště) pro určité časové období a s požadovanými oprávněními.

2. Uživatel A sdílený přístupový podpis nasdílí uživateli (v našem příkladu uživateli B), který potřebuje přístup k účtu úložiště.  

3. Uživatel B se pomocí Storage Exploreru (Preview) a sdíleného přístupového podpisu připojí k účtu patřícímu uživateli A.

### <a name="get-an-sas-for-the-account-you-want-to-share"></a>Získání sdíleného přístupového podpisu pro účet, který chcete sdílet
1. Ve Storage Exploreru (Preview) klikněte pravým tlačítkem myši na účet úložiště, který chcete sdílet, a vyberte **Get Shared Access Signature** (Získat sdílený přístupový podpis).

    ![Možnost místní nabídky Získat sdílený přístupový podpis][13]

2. V dialogovém okně **Shared Access Signature** (Sdílený přístupový podpis) zadejte pro účet požadovaný časový rámec a oprávnění a vyberte **Create** (Vytvořit).

    ![Dialogové okno Získat SAS][14]  
    Otevře se dialogové okno **Shared Access Signature** (Sdílený přístupový podpis) zobrazující sdílený přístupový podpis.

3. Vedle možnosti **Connection String** (Připojovací řetězec) vyberte **Copy** (Kopírovat) a zkopírujte ho do schránky. Pak vyberte **Close** (Zavřít).

### <a name="attach-to-the-shared-account-by-using-the-sas"></a>Připojení ke sdílenému účtu pomocí sdíleného přístupového podpisu (SAS)
1. V nástroji Storage Explorer (Preview) vyberte položku **Connect to Azure storage** (Připojit k úložišti Azure).

    ![Možnost Připojit k úložišti Azure][23]

2. V dialogovém okně**Connect to Azure Storage** (Připojit ke službě Azure Storage) zadejte připojovací řetězec a pak vyberte **Next** (Další).

    ![Dialogové okno Připojit k úložišti Azure][24]

3. V dialogovém okně **Connection Summary** (Souhrn připojení) zkontrolujte zadané informace. Pokud chcete provést změny, vyberte **Back** (Zpět) a zadejte požadovaná nastavení. 

4. Vyberte **Connect** (Připojit).

5. Po připojení se účet úložiště zobrazí s textem **(SAS)** připojeným k názvu účtu, který jste zadali.

    ![Výsledek připojení k účtu pomocí sdíleného přístupového podpisu (SAS)][17]

## <a name="attach-a-service-by-using-an-sas"></a>Připojení služby pomocí sdíleného přístupového podpisu (SAS)
V části Připojení účtu úložiště pomocí sdíleného přístupového podpisu (SAS) se vysvětluje, jak může správce předplatného Azure udělit dočasný přístup k účtu úložiště vygenerováním a nasdílením sdíleného přístupového podpisu (SAS) pro účet úložiště. Podobně je možné sdílený přístupový podpis (SAS) vygenerovat pro konkrétní službu (kontejner objektů blob, frontu nebo tabulku) v rámci účtu úložiště.  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Vygenerování sdíleného přístupového podpisu (SAS) pro službu, kterou chcete sdílet
V tomto kontextu může být službou kontejner objektů blob, fronta nebo tabulka. Pokud chcete vygenerovat sdílený přístupový podpis (SAS) pro uvedenou službu, přečtěte si:

* [Získání sdíleného přístupového podpisu (SAS) pro kontejner objektů blob](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Získání sdíleného přístupového podpisu (SAS) pro sdílenou složku: *Připravuje se*
* Získání sdíleného přístupového podpisu (SAS) pro frontu: *Připravuje se*
* Získání sdíleného přístupového podpisu (SAS) pro tabulku: *Připravuje se*

### <a name="attach-to-the-shared-account-service-by-using-the-sas"></a>Připojení ke službě sdíleného účtu pomocí sdíleného přístupového podpisu (SAS)
1. V nástroji Storage Explorer (Preview) vyberte položku **Connect to Azure storage** (Připojit k úložišti Azure).

    ![Možnost Připojit k úložišti Azure][23]

2. V dialogovém okně**Connect to Azure Storage** (Připojit k úložišti Azure) zadejte identifikátor URI sdíleného přístupového podpisu a pak vyberte **Next** (Další).

    ![Dialogové okno Připojit k úložišti Azure][24]

3. V dialogovém okně **Connection Summary** (Souhrn připojení) zkontrolujte zadané informace. Pokud chcete provést změny, vyberte **Back** (Zpět) a zadejte požadovaná nastavení. 

4. Vyberte **Connect** (Připojit).

5. Po připojení se nově připojená služba zobrazí v uzlu **(Service SAS)** (SAS služby).

    ![Výsledek připojení ke sdílené službě pomocí sdíleného přístupového podpisu (SAS)][20]

## <a name="search-for-storage-accounts"></a>Vyhledávání účtů úložiště
Pokud máte dlouhý seznam účtů úložiště, můžete rychle vyhledat konkrétní účet úložiště pomocí vyhledávacího pole v horní části levého podokna.

Při psaní do vyhledávacího pole se v levém podokně zobrazí pouze účty úložiště odpovídající hledané hodnotě, kterou jste zatím zadali. Na následujícím snímku obrazovky je například ukázka vyhledávání všech účtů úložiště, jejichž název obsahuje text **tarcher**:

![Vyhledávání účtu úložiště][11]

## <a name="next-steps"></a>Další kroky
* [Správa prostředků služby Azure Blob Storage pomocí Storage Exploreru (Preview)](vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png

