<properties
    pageTitle="Začínáme se Storage Explorerem (Preview) | Microsoft Azure"
    description="Správa prostředků úložiště Azure Storage pomocí Storage Exploreru (Preview)"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="06/05/2016"
    ms.author="tarcher" />

# Začínáme se Storage Explorerem (Preview)

## Přehled 

Microsoft Azure Storage Explorer (Preview) je samostatná aplikace, která vám umožní snadno pracovat s daty Azure Storage ve Windows, na OSX a na Linuxu. V tomto článku se dozvíte, jakými různými způsoby se můžete připojovat k účtům Azure Storage a spravovat je.

## Požadavky

- [Stažení a instalace Storage Exploreru (Preview)](http://go.microsoft.com/fwlink/?LinkId=708343)

## Připojení k účtu úložiště nebo službě

Storage Explorer (Preview) nabízí celou řadu způsobů, jak se připojit k účtům úložiště. Umožňuje například připojovat se k účtům úložiště, které máte přidružené k předplatným Azure, a k účtům úložiště a službám sdíleným z jiných předplatných Azure a umožňuje také připojit se k místnímu úložišti pomocí emulátoru úložiště Azure (a pomocí něj ho i spravovat):

- [Připojení k předplatnému Azure:](#connect-to-an-azure-subscription) Umožňuje spravovat prostředky úložiště, které patří do vašeho předplatného Azure.
- [Připojení k místnímu úložišti:](#connect-to-local-storage) Umožňuje spravovat místní úložiště pomocí emulátoru úložiště Azure. 
- [Připojení k externímu úložišti:](#attach-or-detach-an-external-storage-account) Umožňuje spravovat prostředky úložiště, které patří do jiného předplatného Azure, pomocí názvu a klíče účtu úložiště.
- [Připojení k účtu pomocí SAS:](#attach-account-using-sas) Umožňuje spravovat prostředky úložiště, které patří do jiného předplatného Azure, pomocí SAS.
- [Připojení služby pomocí SAS:](#attach-service-using-sas) Umožňuje spravovat konkrétní službu úložiště (kontejner objektů blob, fronty nebo tabulky) patřící do jiného předplatného Azure, pomocí SAS.

## Připojení k předplatnému Azure

> [AZURE.NOTE] Pokud nemáte účet Azure, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Otevřete Storage Explorer (Preview). 

1. Pokud používáte Storage Explorer (Preview) poprvé nebo jste ho už někdy spustili, ale zatím jste se nepřipojili k účtu Azure, uvidíte informační panel, který vám umožní připojit se k účtu Azure.

    ![][0]
    
1. Vyberte **Připojit se k Microsoft Azure** a podle pokynů v dialogových oknech se přihlaste účtem Microsoft, který je přidružený minimálně k jednomu aktivnímu předplatnému Azure.

Jakmile se úspěšně přihlásíte účtem Microsoft, zobrazí se v levém podokně Storage Exploreru (Preview) účty úložiště přidružené ke všem předplatným Azure, která jsou přidružená k účtu Microsoft.

### Vyfiltrování předplatných Azure

Storage Explorer (Preview) vám umožní pomocí filtru vybrat, pro která předplatná Azure přidružená k účtům Microsoft, kterými jste přihlášení, budou v levém podokně zobrazené účty úložiště.

1. Vyberte ikonu **Nastavení** (ozubené kolo).

    ![][1]   

1.  V horní části levého podokna se vám zobrazí rozevírací seznam obsahující všechny účty Microsoft, ke kterým jste přihlášení. 

    ![][3]
     
1.  Výběrem šipky dolů vedle rozevíracího seznamu zobrazíte všechny účty Microsoft, ke kterým jste přihlášení, a také odkaz pro přidání dalších účtů Microsoft (přihlášení k nim).

    ![][4]

1.  Z rozevíracího seznamu vyberte požadovaný účet Microsoft.

1.  V levém podokně se zobrazí všechna předplatná Azure přidružená k vybranému účtu Microsoft.
Zaškrtávací políčko nalevo od každého předplatného Azure umožňuje určit, jestli má Storage Explorer (Preview) vypsat všechny účty úložiště přidružené k těmto předplatným Azure. Zaškrtnutím / zrušením zaškrtnutí políčka **Všechna předplatná** přepínáte výběr všech nebo žádných z uvedených předplatných Azure.

    ![][2]  

1.  Až vyberete předplatná Azure, která chcete spravovat, vyberte **Použít**. Levé podokno se aktualizuje a zobrazí seznam všech účtů úložiště pro každé vybrané předplatné Azure pro aktuální účet Microsoft. 

### Přidání dalších účtů Microsoft

Následující kroky vás provedou postupem připojení k dalším účtům Microsoft, abyste si mohli zobrazit předplatná Azure a účty úložiště pro jednotlivé účty.

1.  Vyberte ikonu **Nastavení** (ozubené kolo).

    ![][1]   

1.  V horní části levého podokna se vám zobrazí rozevírací seznam obsahující všechny aktuálně připojené účty Microsoft.

    ![][3]
     
1.  Výběrem šipky dolů vedle rozevíracího seznamu zobrazíte všechny účty Microsoft, ke kterým jste přihlášení, a také odkaz pro přidání dalších účtů Microsoft (přihlášení k nim).

    ![][4]

1.  Vyberte **Přidat účet** a podle pokynů v dialogových oknech se přihlaste k účtu, ke kterému je přidružené minimálně jedno aktivní předplatné Azure.

1.  Zaškrtněte políčka pro předplatná Azure, která chcete procházet. 

    ![][2]  

1.  Vyberte **Použít**.

### Přepínání mezi účty Microsoft

Můžete se sice připojit k několika účtům Microsoft, v levém podokně se ale zobrazí jenom účty úložiště přidružené k předplatným pro jeden (aktuální) účet Microsoft. Pokud se připojíte k několika účtům Microsoft, můžete mezi nimi takto přepínat:

1.  Vyberte ikonu **Nastavení** (ozubené kolo).

    ![][1]   

1.  V horní části levého podokna se vám zobrazí rozevírací seznam obsahující všechny aktuálně připojené účty Microsoft.

    ![][3]
     
1.  Výběrem šipky dolů vedle rozevíracího seznamu zobrazíte všechny účty Microsoft, ke kterým jste přihlášení, a také odkaz pro přidání dalších účtů Microsoft (přihlášení k nim).

    ![][4]

1.  Vyberte požadovaný účet Microsoft.

1.  Zaškrtněte políčka pro předplatná Azure, která chcete procházet. 

    ![][2]  

1.  Vyberte **Použít**.
  
## Připojení k místnímu úložišti

Storage Explorer (Preview) umožňuje pracovat s místním úložištěm pomocí emulátoru úložiště Azure. Můžete tak psát kód pro místní úložiště a otestovat ho, aniž byste museli mít nasazený účet úložiště v Azure (protože účet úložiště je emulovaných emulátorem úložiště Azure).

>[AZURE.NOTE] Emulátor úložiště Azure je momentálně podporován pouze pro Windows. 

1. Otevřete Storage Explorer (Preview). 

1. V levém podokně rozbalte uzel **(Vývoj)**.

    ![][21]

1. Pokud jste ještě nenainstalovali emulátor úložiště Azure, budete k tomu vyzváni prostřednictvím informačního panelu. Pokud se zobrazí informační panel, vyberte možnost **Download the latest version** (Stáhnout nejnovější verzi) a nainstalujte si emulátor. 

    ![][22]

1. Po nainstalování emulátoru budete mít možnost vytvářet místní objekty Blob, fronty a tabulky a pracovat s nimi. To, jak pracovat s jednotlivými typu účtu úložiště, zjistíte po kliknutí na příslušný odkaz níže:

    - [Správa prostředků Azure Blob Storage](./vs-azure-tools-storage-explorer-blobs.md)
    - Správa prostředků Azure Queue Storage – *připravujeme*
    - Správa prostředků Azure Table Storage – *připravujeme*

## Připojení nebo odpojení externího účtu úložiště

Storage Explorer (Preview) umožňuje připojovat se k externím účtům úložiště, aby bylo možné účty úložiště snadno sdílet. Tato část vysvětluje, jak se připojit k externím účtům úložiště (a odpojit se od nich).

### Získání přihlašovacích údajů účtu úložiště

Aby bylo možné sdílet externí účet úložiště, musí vlastník tohoto účtu nejdřív pro účet získat přihlašovací údaje (název účtu a klíč) a potom tyto informace nasdílet uživateli, který se chce k tomuto (externímu) účtu připojit. Přihlašovací údaje k účtu úložiště je možné získat prostřednictvím Portálu Azure pomocí následujících kroků: 

1.  Přihlaste se k [Portálu Azure](https://portal.azure.com).
1.  Vyberte **Procházet**.
1.  Vyberte **Účty úložiště**.
1.  V okně **Účty úložiště** vyberte požadovaný účet úložiště.
1.  V okně **Nastavení** pro vybraný účet úložiště vyberte možnost **Přístupové klíče**.

    ![][5]
    
1.  V okně **Přístupové klíče** zkopírujte hodnoty **NÁZEV ÚČTU ÚLOŽIŠTĚ** a **KLÍČ 1**, aby je bylo možné použít při připojování k účtu úložiště. 

    ![][6]

### Připojení k externímu účtu úložiště

1.  Ve Storage Exploreru (Preview) klikněte pravým tlačítkem na **Storage Accounts** (Účty úložiště) a v místní nabídce vyberte **Attach External Storage** (Připojit externí úložiště).

    ![][7]
    
1.  Část *Získání přihlašovacích údajů účtu úložiště* vysvětluje, jak získat hodnoty názvu účtu úložiště a klíč 1. Tyto hodnoty se použijí v tomto kroku. V dialogovém okně **Attach External Storage** (Připojit externí úložiště) zadejte název účtu úložiště do pole **Account Name** (Název účtu) a do pole **Account Key** (Klíč účtu) zadejte hodnotu Klíč 1. Až to budete mít, vyberte **OK**. 

    ![][8]

    Po připojení se externí účet úložiště zobrazí s textem **(External)** připojeným k názvu účtu úložiště. 

    ![][9]

### Odpojení od externího účtu úložiště

1.  Klikněte pravým tlačítkem na externí účet úložiště, který chcete odpojit, a v místní nabídce vyberte **Detach** (Odpojit).

    ![][10]

1.  Jakmile se zobrazí potvrzovací okno se zprávou, potvrďte výběrem možnosti **Ano** odpojení od externího účtu úložiště.

    ![][12]

## Připojení účtu pomocí sdíleného přístupového podpisu (SAS)

Sdílený přístupový podpis (SAS) umožňuje správci předplatného Azure udělit dočasně přístup k účtu úložiště bez nutnosti poskytnout přihlašovací údaje ke svému předplatnému Azure. 

Ukážeme si to na příkladu. Řekněme, že uživatel A je správcem předplatného Azure a uživatel A chce uživateli B povolit přístup k účtu úložiště po omezenou dobu s určitými oprávněními:

1. Uživatele A vygeneruje sdílený přístupový podpis (tvořený připojovacím řetězcem pro účet úložiště) pro určité časové období a s požadovanými oprávněními.
1. Uživatele A sdílený přístupový podpis nasdílí uživateli B, který potřebuje přístup k účtu úložiště.  
1. Uživatel B se pomocí Storage Exploreru (Preview) připojí k účtu patřícímu uživateli A pomocí sdíleného přístupového podpisu. 

### Získání sdíleného přístupového podpisu pro účet, který chcete sdílet

1.  Otevřete Storage Explorer (Preview).
1.  V levém podokně klikněte pravým tlačítkem myši na účet úložiště, který chcete sdílet, a z místní nabídky vyberte **Get Shared Access Signature** (Získat sdílený přístupový podpis).

    ![][13]

1. V dialogovém okně **Shared Access Signature** (Sdílený přístupový podpis) zadejte pro účet časový rámec a oprávnění a vyberte **Create** (Vytvořit).

    ![][14]
 
1. Zobrazí se druhé dialogové okno **Shared Access Signature** (Sdílený přístupový podpis) zobrazující sdílený přístupový podpis. Vedle možnosti **Connection String** (Připojovací řetězec) vyberte **Copy** (Kopírovat) a zkopírujte ho do schránky. Výběrem možnosti **Close** (Zavřít) zavřete dialogové okno.

### Připojení ke sdílenému účtu pomocí sdíleného přístupového podpisu

1.  Otevřete Storage Explorer (Preview).
1.  Ve levém podokně klikněte pravým tlačítkem na **Storage Accounts** (Účty úložiště) a v místní nabídce vyberte **Attach account using SAS** (Připojit účet pomocí sdíleného přístupového podpisu).
    ![][15]

1. V dialogovém okně **Attach account using SAS** (Připojit účet pomocí sdíleného přístupového podpisu):

    - **Account Name** (Název účtu): Zadejte název, který chcete přidružit k tomuto účtu. **POZNÁMKA:** Název účtu nemusí odpovídat původnímu názvu účtu úložiště, pro který se sdílený přístupový podpis vygeneroval. 
    - **Connection String** (Připojovací řetězec): Vložte připojovací řetězec, který jste zkopírovali dříve.
    - Až to budete mít, vyberte **OK**.
    
    ![][16]

Po připojení se účet úložiště zobrazí s textem (SAS) připojeným k názvu účtu, který jste zadali.

![][17]

## Připojení služby pomocí SAS

V části [Připojení účtu pomocí sdíleného přístupového podpisu (SAS)](#attach-account-using-sas) se vysvětluje, jak může správce předplatného Azure udělit dočasný přístup k účtu úložiště vygenerováním (a nasdílením) sdíleného přístupového podpisu (SAS) pro účet úložiště. Podobně je možné sdílený přístupový podpis (SAS) vygenerovat pro konkrétní službu (kontejner objektů blob, frontu nebo tabulku) v rámci účtu úložiště.  

### Jak pro službu vygenerovat sdílený přístupový podpis (SAS), který chcete nasdílet

V tomto kontextu může být službou kontejner objektů blob, fronta nebo tabulka. Následující části popisují, jak vygenerovat sdílený přístupový podpis (SAS) pro uvedenou službu:

- [Získání sdíleného přístupového podpisu (SAS) pro kontejner objektů blob](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Získání sdíleného přístupového podpisu (SAS) pro frontu – *připravujeme*
- Získání sdíleného přístupového podpisu (SAS) pro tabulku – *připravujeme*

### Připojení ke službě sdíleného účtu pomocí sdíleného přístupového podpisu

1.  Otevřete Storage Explorer (Preview).
1.  Ve levém podokně klikněte pravým tlačítkem na **Storage Accounts** (Účty úložiště) a v místní nabídce vyberte **Attach service using SAS** (Připojit službu pomocí sdíleného přístupového podpisu).
    ![][18]

1. V dialogovém okně **Attach Account using SAS** (Připojit účet pomocí sdíleného přístupového podpisu) vložte identifikátor URI SAS, který jste si předtím zkopírovali, a vyberte **OK**.

    ![][19]

Po připojení se nově připojená služba zobrazí v uzlu **(Service SAS)** (SAS služby). 

![][20]

## Vyhledávání účtů úložiště

Pokud máte dlouhý seznam účtů úložiště, můžete rychle vyhledat konkrétní účet úložiště pomocí vyhledávacího pole v horní části levého podokna. 

Při psaní do vyhledávacího pole se v levém podokně zobrazí pouze účty úložiště odpovídající hledané hodnotě, kterou jste zatím zadali. Na následujícím snímku obrazovky vidíte příklad hledání. Hledal jsem všechny účty úložiště, kde název účtu úložiště obsahuje text „tarcher“.

![][11]
    
Když budete chtít hledání vymazat, vyberte ve vyhledávacím poli tlačítko **x**.

## Další kroky
- [Správa prostředků úložiště Azure Blob Storage pomocí Storage Exploreru (Preview)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-gear.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/filter-subscriptions.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/filter-accounts.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/accounts-drop-down.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-context-menu.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-dlg.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-context-menu.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-dlg.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png



<!--HONumber=Jun16_HO2-->


