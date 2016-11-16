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
ms.date: 08/17/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d5c003c9240d8625d2921666e1361ce0512138bb


---
# <a name="getting-started-with-storage-explorer-preview"></a>Začínáme se Storage Explorerem (Preview)
## <a name="overview"></a>Přehled
Microsoft Azure Storage Explorer (Preview) je samostatná aplikace, která umožňuje jednoduchá práci s daty v úložišti Azure Storage v operačních systémech Windows, OS X a Linux. V tomto článku se dozvíte, jakými různými způsoby se můžete připojovat k účtům Azure Storage a spravovat je.

![Microsoft Azure Storage Explorer (Preview)][15]

## <a name="prerequisites"></a>Požadavky
* [Stažení a instalace Storage Exploreru (Preview)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Připojení k účtu úložiště nebo službě
Storage Explorer (Preview) nabízí celou řadu způsobů, jak se připojit k účtům úložiště. Umožňuje například připojovat se k účtům úložiště, které máte přidružené k předplatným Azure, a k účtům úložiště a službám sdíleným z jiných předplatných Azure a umožňuje také připojit se k místnímu úložišti pomocí emulátoru úložiště Azure (a pomocí něj ho i spravovat):

* [Připojení k předplatnému Azure:](#connect-to-an-azure-subscription) Umožňuje spravovat prostředky úložiště, které patří do vašeho předplatného Azure.
* [Práce s místním vývojovým úložištěm:](#work-with-local-development-storage) Umožňuje spravovat místní úložiště pomocí emulátoru úložiště Azure Storage. 
* [Připojení k externímu úložišti:](#attach-or-detach-an-external-storage-account) Umožňuje spravovat prostředky úložiště, které patří do jiného předplatného Azure, pomocí názvu a klíče účtu úložiště.
* [Připojení k účtu úložiště pomocí SAS:](#attach-storage-account-using-sas) Umožňuje spravovat prostředky úložiště, které patří do jiného předplatného Azure, pomocí SAS.
* [Připojení služby pomocí SAS:](#attach-service-using-sas) Umožňuje spravovat konkrétní službu úložiště (kontejner objektů blob, fronty nebo tabulky) patřící do jiného předplatného Azure, pomocí SAS.

## <a name="connect-to-an-azure-subscription"></a>Připojení k předplatnému Azure
> [!NOTE]
> Pokud nemáte účet Azure, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
> 
> 

1. V nástroji Storage Explorer (Preview) vyberte **Azure Account Settings** (Nastavení účtu Azure). 
   
    ![Nastavení účtu Azure][0]
2. V levém podokně se teď zobrazí všechny účty Microsoft, ke kterým jste přihlášeni. Pokud se chcete připojit k jinému účtu, vyberte položku **Add an account ** (Přidat účet) a podle pokynů v dialogových oknech se přihlaste účtem Microsoft, který je přidružený minimálně k jednomu aktivnímu předplatnému Azure.
   
    ![Přidání účtu][1]
3. Jakmile se úspěšně přihlásíte s účtem Microsoft, zobrazí se v levém podokně předplatná Azure přidružená k tomuto účtu. Vyberte předplatná Azure, se kterými chcete pracovat, a pak vyberte **Apply** (Použít). (Zaškrtnutím políčka **All subscriptions** (Všechna předplatná) přepínáte výběr všech nebo žádných z uvedených předplatných Azure.)
   
    ![Výběr předplatných Azure][3]
4. V levém podokně se zobrazí všechny účty úložišť, přidružené k vybraným předplatným Azure.
   
    ![Vybraná předplatná Azure][4]

## <a name="work-with-local-development-storage"></a>Práce s místním vývojovým úložištěm
Storage Explorer (Preview) umožňuje pracovat s místním úložištěm pomocí emulátoru úložiště Azure. Můžete tak psát kód pro místní úložiště a otestovat ho, aniž byste museli mít nasazený účet úložiště v Azure (protože účet úložiště je emulovaných emulátorem úložiště Azure).

> [!NOTE]
> Emulátor úložiště Azure je momentálně podporován pouze pro Windows. 
> 
> 

1. V levém podokně Storage Exploreru (Preview) rozbalte uzel **(Local and Attached** > **Storage Accounts** > **(Development)** (Místní a připojené účty úložiště (vývoj)).
   
    ![Místní vývojový uzel][21]
2. Pokud jste ještě nenainstalovali emulátor úložiště Azure, budete k tomu vyzváni prostřednictvím informačního panelu. Pokud se zobrazí informační panel, vyberte možnost **Download the latest version** (Stáhnout nejnovější verzi) a nainstalujte si emulátor. 
   
    ![Výzva ke stažení emulátoru úložiště Azure Storage][22]
3. Po nainstalování emulátoru budete mít možnost vytvářet místní objekty Blob, fronty a tabulky a pracovat s nimi. To, jak pracovat s jednotlivými typu účtu úložiště, zjistíte po kliknutí na příslušný odkaz níže:
   
   * [Správa prostředků Azure Blob Storage](vs-azure-tools-storage-explorer-blobs.md)
   * Správa prostředků Azure File Share – *připravujeme*
   * Správa prostředků Azure Queue Storage – *připravujeme*
   * Správa prostředků Azure Table Storage – *připravujeme*

## <a name="attach-or-detach-an-external-storage-account"></a>Připojení nebo odpojení externího účtu úložiště
Storage Explorer (Preview) umožňuje připojovat se k externím účtům úložiště, aby bylo možné účty úložiště snadno sdílet. Tato část vysvětluje, jak se připojit k externím účtům úložiště (a odpojit se od nich).

### <a name="get-the-storage-account-credentials"></a>Získání přihlašovacích údajů účtu úložiště
Aby bylo možné sdílet externí účet úložiště, musí vlastník tohoto účtu nejdřív pro účet získat přihlašovací údaje (název účtu a klíč) a potom tyto informace nasdílet uživateli, který se chce k tomuto (externímu) účtu připojit. Přihlašovací údaje k účtu úložiště je možné získat prostřednictvím Portálu Azure pomocí následujících kroků: 

1. Přihlaste se k [Portálu Azure](https://portal.azure.com).
2. Vyberte **Procházet**.
3. Vyberte **Účty úložiště**.
4. V okně **Účty úložiště** vyberte požadovaný účet úložiště.
5. V okně **Nastavení** pro vybraný účet úložiště vyberte možnost **Přístupové klíče**.
   
   ![Možnost Přístupové klíče][5]
6. V okně **Přístupové klíče** zkopírujte hodnoty **NÁZEV ÚČTU ÚLOŽIŠTĚ** a **KLÍČ 1**, aby je bylo možné použít při připojování k účtu úložiště. 
   
   ![Přístupové klíče][6]

### <a name="attach-to-an-external-storage-account"></a>Připojení k externímu účtu úložiště
Pokud se chcete připojit k účtu externího úložiště, budete potřebovat název účtu a klíč. Část *Získání přihlašovacích údajů účtu úložiště* vysvětluje, jak tyto hodnoty získat z portálu Azure. Uvědomte si však, že na portálu se klíč účtu nazývá „klíč 1“, proto pokud vás Storage Explorer (Preview) požádá o klíč k účtu, zadejte (nebo vložte ze schránky) hodnotu „klíč 1“. 

1. V nástroji Storage Explorer (Preview) vyberte položku **Connect to Azure storage** (Připojit k úložišti Azure).
   
   ![Možnost Připojit k úložišti Azure][23]
2. V dialogovém okně **Connect to Azure Storage** (Připojit k úložišti Azure) zadejte klíč účtu (hodnota „klíč 1“ z portálu Azure) a pak vyberte **Next** (Další).
   
   ![Dialogové okno Připojit k úložišti Azure][24] 
3. V dialogovém okně **Attach External Storage** (Připojit externí úložiště) zadejte název účtu úložiště do pole **Account name** (Název účtu), zadejte případná další požadovaná nastavení a po dokončení vyberte možnost **Next** (Další). 
   
   ![Dialogové okno Připojit externí úložiště][8]
4. V dialogovém okně **Connection Summary** (Souhrn připojení) zkontrolujte zadané informace. Pokud chcete něco změnit, vyberte možnost **Back** (Zpět) a požadovaná nastavení zadejte znovu. Po dokončení vyberte možnost **Connect** (Připojit).
5. Po připojení se externí účet úložiště zobrazí s textem **(External)** připojeným k názvu účtu úložiště. 
   
   ![Výsledek připojení k externímu účtu úložiště][9]

### <a name="detach-from-an-external-storage-account"></a>Odpojení od externího účtu úložiště
1. Klikněte pravým tlačítkem na externí účet úložiště, který chcete odpojit, a v místní nabídce vyberte **Detach** (Odpojit).
   
   ![Možnost Odpojit od úložiště][10]
2. Jakmile se zobrazí potvrzovací okno se zprávou, potvrďte výběrem možnosti **Ano** odpojení od externího účtu úložiště.

## <a name="attach-storage-account-using-sas"></a>Připojení účtu úložiště pomocí sdíleného přístupového podpisu (SAS)
[Sdílený přístupový podpis (SAS)](storage/storage-dotnet-shared-access-signature-part-1.md) umožňuje správci předplatného Azure udělovat dočasný přístup k úložišti, aniž by musel zadávat přihlašovací údaje k předplatnému Azure. 

Ukážeme si to na příkladu. Řekněme, že uživatel A je správcem předplatného Azure a uživatel A chce uživateli B povolit přístup k účtu úložiště po omezenou dobu s určitými oprávněními:

1. Uživatele A vygeneruje sdílený přístupový podpis (tvořený připojovacím řetězcem pro účet úložiště) pro určité časové období a s požadovanými oprávněními.
2. Uživatele A sdílený přístupový podpis nasdílí uživateli B, který potřebuje přístup k účtu úložiště.  
3. Uživatel B se pomocí Storage Exploreru (Preview) připojí k účtu patřícímu uživateli A pomocí sdíleného přístupového podpisu. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Získání sdíleného přístupového podpisu pro účet, který chcete sdílet
1. Ve Storage Exploreru (Preview) klikněte pravým tlačítkem myši na účet úložiště, který chcete sdílet, a z místní nabídky vyberte **Get Shared Access Signature** (Získat sdílený přístupový podpis).
   
   ![Možnost místní nabídky Získat sdílený přístupový podpis][13]
2. V dialogovém okně **Shared Access Signature** (Sdílený přístupový podpis) zadejte pro účet časový rámec a oprávnění a vyberte **Create** (Vytvořit).
   
    ![Dialogové okno Získat SAS][14]
3. Zobrazí se druhé dialogové okno **Shared Access Signature** (Sdílený přístupový podpis) zobrazující sdílený přístupový podpis. Vedle možnosti **Connection String** (Připojovací řetězec) vyberte **Copy** (Kopírovat) a zkopírujte ho do schránky. Výběrem možnosti **Close** (Zavřít) zavřete dialogové okno.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Připojení ke sdílenému účtu pomocí sdíleného přístupového podpisu
1. V nástroji Storage Explorer (Preview) vyberte položku **Connect to Azure storage** (Připojit k úložišti Azure).
   
   ![Možnost Připojit k úložišti Azure][23]
2. V dialogovém okně**Connect to Azure Storage** (Připojit k úložišti Azure) zadejte připojovací řetězec a pak vyberte **Next** (Další).
   
   ![Dialogové okno Připojit k úložišti Azure][24] 
3. V dialogovém okně **Connection Summary** (Souhrn připojení) zkontrolujte zadané informace. Pokud chcete něco změnit, vyberte možnost **Back** (Zpět) a požadovaná nastavení zadejte znovu. Po dokončení vyberte možnost **Connect** (Připojit).
4. Po připojení se účet úložiště zobrazí s textem (SAS) připojeným k názvu účtu, který jste zadali.
   
   ![Výsledek připojení k účtu pomocí SAS][17]

## <a name="attach-service-using-sas"></a>Připojení služby pomocí SAS
V části [Připojení účtu úložiště pomocí sdíleného přístupového podpisu (SAS)](#attach-storage-account-using-sas) se vysvětluje, jak může správce předplatného Azure udělit dočasný přístup k účtu úložiště vygenerováním (a nasdílením) sdíleného přístupového podpisu (SAS) pro účet úložiště. Podobně je možné sdílený přístupový podpis (SAS) vygenerovat pro konkrétní službu (kontejner objektů blob, frontu nebo tabulku) v rámci účtu úložiště.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Jak pro službu vygenerovat sdílený přístupový podpis (SAS), který chcete nasdílet
V tomto kontextu může být službou kontejner objektů blob, fronta nebo tabulka. Následující části popisují, jak vygenerovat sdílený přístupový podpis (SAS) pro uvedenou službu:

* [Získání sdíleného přístupového podpisu (SAS) pro kontejner objektů blob](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Získání sdíleného přístupového podpisu (SAS) pro sdílenou složku – *připravujeme*
* Získání sdíleného přístupového podpisu (SAS) pro frontu – *připravujeme*
* Získání sdíleného přístupového podpisu (SAS) pro tabulku – *připravujeme*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Připojení ke službě sdíleného účtu pomocí sdíleného přístupového podpisu
1. V nástroji Storage Explorer (Preview) vyberte položku **Connect to Azure storage** (Připojit k úložišti Azure).
   
   ![Možnost Připojit k úložišti Azure][23]
2. V dialogovém okně**Connect to Azure Storage** (Připojit k úložišti Azure) zadejte SAS URI a pak vyberte **Next** (Další).
   
   ![Dialogové okno Připojit k úložišti Azure][24] 
3. V dialogovém okně **Connection Summary** (Souhrn připojení) zkontrolujte zadané informace. Pokud chcete něco změnit, vyberte možnost **Back** (Zpět) a požadovaná nastavení zadejte znovu. Po dokončení vyberte možnost **Connect** (Připojit).
4. Po připojení se nově připojená služba zobrazí v uzlu **(Service SAS)** (SAS služby).
   
   ![Výsledek připojení ke sdílené službě pomocí SAS][20]

## <a name="search-for-storage-accounts"></a>Vyhledávání účtů úložiště
Pokud máte dlouhý seznam účtů úložiště, můžete rychle vyhledat konkrétní účet úložiště pomocí vyhledávacího pole v horní části levého podokna. 

Při psaní do vyhledávacího pole se v levém podokně zobrazí pouze účty úložiště odpovídající hledané hodnotě, kterou jste zatím zadali. Na následujícím snímku obrazovky vidíte příklad hledání. Hledal jsem všechny účty úložiště, kde název účtu úložiště obsahuje text „tarcher“.

![Vyhledávání účtu úložiště][11]

Když budete chtít hledání vymazat, vyberte ve vyhledávacím poli tlačítko **x**.

## <a name="next-steps"></a>Další kroky
* [Správa prostředků úložiště Azure Blob Storage pomocí Storage Exploreru (Preview)](vs-azure-tools-storage-explorer-blobs.md)

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



<!--HONumber=Nov16_HO2-->


