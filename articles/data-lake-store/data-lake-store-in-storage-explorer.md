---
title: "Správa prostředků Azure Data Lake Store v Průzkumníku služby Azure Storage"
description: "Umožněte uživatelům přístup k datům a prostředkům ADLS a jejich správu v Průzkumníku služby Azure Storage."
Keywords: Azure Data Lake Store, Azure Storage Explorer
services: Data Lake Store
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
ms.assetid: 
ms.service: Data Lake Store
ms.custom: Azure Data Lake Store
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 02/05/2018
ms.author: jejiang
ms.openlocfilehash: 4c69acebceac6719cf3f12ff0a93879f1b5a8943
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="manage-azure-data-lake-store-resources-with-storage-explorer-preview"></a>Správa prostředků Azure Data Lake Store pomocí Průzkumníka služby Storage (Preview)

[Azure Data Lake Store (ADLS)](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview) je služba pro ukládání velkých objemů nestrukturovaných dat, jako jsou textová nebo binární data. Uživatel může k datům získat přístup odkudkoli přes protokol HTTP nebo HTTPS. ADLS v Průzkumníku služby Azure Storage umožňuje uživatelům přístup k datům a prostředkům ADLS a jejich správu spolu s dalšími entitami Azure, jako jsou objekty blob a fronty. Uživatelé teď můžou pomocí stejného nástroje spravovat různé entity Azure na jednom místě.

Další výhodou je, že uživatelé ke správě dat ADLS nepotřebují mít oprávnění k předplatnému. V Průzkumníku služby Storage můžou uživatelé připojit cestu ADLS k uzlu Místní a připojené, pokud jim k tomu ostatní udělí oprávnění.

## <a name="prerequisites"></a>Požadavky
K dokončení kroků v tomto článku budete potřebovat následující:

*   Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial).
*   Účet Azure Data Lake Store. Pokyny k jeho vytvoření najdete v tématu [Začínáme s Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="installation"></a>Instalace

Nainstalujte nejnovější části Průzkumníka služby Azure Storage: [Průzkumník služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/). V současné době podporujeme verze pro Windows, Linux a Mac.

## <a name="connect-to-an-azure-subscription"></a>Připojení k předplatnému Azure

1. Po instalaci **Průzkumníka služby Azure Storage** klikněte na ikonu modulu **plug-in** na levé straně, jak je znázorněno na následujícím obrázku.
       
   ![Ikona modulu plug-in](./media/data-lake-store-in-storage-explorer/plug-in-icon.png)
 
2. Vyberte **Přidat účet Azure** a pak klikněte na **Přihlásit**.

   ![Připojení k předplatnému Azure](./media/data-lake-store-in-storage-explorer/connect-to-azure-subscription.png)

2. V dialogovém okně **Přihlášení k Azure** vyberte **Přihlásit** a zadejte své přihlašovací údaje Azure.

    ![Přihlášení](./media/data-lake-store-in-storage-explorer/sign-in.png)

3. Vyberte ze seznamu své předplatné a klikněte na **Použít**.

    ![Použít](./media/data-lake-store-in-storage-explorer/apply-subscription.png)

    Podokno Průzkumníka se aktualizuje a zobrazí účty ve vybraném předplatném.

    ![Seznam účtů](./media/data-lake-store-in-storage-explorer/account-list.png)

    Právě jste úspěšně propojili svůj **účet Azure Data Lake Store** se svým předplatným Azure.

## <a name="connect-to-data-lake-store"></a>Připojení ke službě Data Lake Store
Představte si, že chcete získat přístup k prostředkům, které neexistují ve vašem předplatném. Ostatní vám však udělí oprávnění k získání jejich identifikátoru URI. V takovém případě se po přihlášení můžete připojit ke službě Data Lake Store s použitím tohoto identifikátoru URI. Postup najdete v následujících krocích.
1. Otevřete Storage Explorer (Preview).
2. V levém podokně rozbalte **Místní a připojené**.
3. Klikněte pravým tlačítkem na **Data Lake Store** a v místní nabídce vyberte **Připojit ke službě Data Lake Store**.

      ![místní nabídka Připojit ke službě Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Zadejte identifikátor URI a nástroj vás pak přesměruje na adresu URL, kterou jste právě zadali.

      ![dialogové okno Připojit ke službě Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

      ![výsledek připojení ke službě Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="view-an-azure-data-lake-store-accounts-contents"></a>Zobrazení obsahu účtu Azure Data Lake Store
Prostředky účtu Azure Data Lake Store obsahují složky a soubory.

Následující postup ukazuje, jak zobrazit obsah účtu ADLS v Průzkumníku služby Storage (Preview):

1. Otevřete Storage Explorer (Preview).
2. V levém podokně rozbalte předplatné obsahující účet Azure Data Lake Store, který chcete zobrazit.
3. Rozbalte **Data Lake Store**.
4. Klikněte pravým tlačítkem na uzel účtu Azure Data Lake Store, který chcete zobrazit, a v místní nabídce vyberte **Otevřít**. Účet ADLS můžete otevřít také tak, že na něj dvakrát kliknete. 
5. V hlavním podokně se zobrazí obsah účtu ADLS.

     ![hlavní podokno](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-toolbar-mainpane.png) 

## <a name="azure-data-lake-store-resources-management"></a>Správa prostředků Azure Data Lake Store

Prostředky Azure Data Lake Store můžete spravovat prováděním následujících operací:
*   Procházení prostředků ADLS v několika účtech ADL  
*   Použití připojovacího řetězce k připojení k ADLS a přímé správě 
*   Zobrazení prostředků ADLS sdílených ostatními prostřednictvím seznamu ACL v části Místní a připojené
*   Provádění operací CRUD se soubory nebo složkami: podpora rekurzivních složek a vícenásobně vybraných souborů 
*   Přetahování a přidávání složek do rychlého přístupu a nedávných umístění, které odpovídají prostředí desktopového průzkumníka souborů 
*   Kopírování a otevírání hypertextových odkazů ADL jedním kliknutím pomocí Průzkumníka služby Storage 
*   Zobrazení protokolu aktivit se stavem aktivit v pravém dolním podokně
*   Zobrazení statistik složek a vlastností souborů

## <a name="manage-resources-in-azure-storage-explorer"></a>Správa prostředků v Průzkumníku služby Azure Storage
Po vytvoření účtu Azure Data Lake Store můžete nahrávat složky a soubory, stahovat prostředky a otevírat je na místním počítači. Máte také možnosti Připnout na Rychlý přístup, Nová složka, Kopírovat adresu URL a Vybrat vše. Navíc máte možnosti Kopírovat, Vložit, Přejmenovat, Odstranit, Statistiky složky a Aktualizovat.

Následující položky ukazují, jak spravovat prostředky v účtu Azure Data Lake Store. V závislosti na úloze, kterou chcete provést, postupujte podle těchto kroků:
   * **Nahrání souborů**

     1. Na panelu nástrojů v hlavním podokně vyberte **Nahrát** a pak v rozevírací nabídce vyberte **Nahrát soubory**.

        ![Nabídka Nahrát soubory](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-menu.png) 

     2. V dialogovém okně **Vyberte soubory k nahrání** vyberte soubory, které chcete nahrát.

        ![Dialogové okno Nahrát složku](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-dialog.png)

     3. Vyberte **Otevřít** a zahajte nahrávání.
   * **Nahrání složky**

     1. Na panelu nástrojů v hlavním podokně vyberte **Nahrát** a pak v rozevírací nabídce vyberte **Nahrát složku**.

        ![Nabídka Nahrát složku](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-menu.png) 
     2. V dialogovém okně **Vyberte složku k nahrání** vyberte složku, kterou chcete nahrát.

        ![Dialogové okno Nahrát složku](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-dialog.png)      

     3. Vyberte **Vybrat složku** a zahajte nahrávání složky.

        ![Dialogové okno Nahrát složku](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-drag.png) 

      > [!NOTE] 
          > Nahrávání můžete implementovat přímým přetahováním složek a souborů na místním počítači. 
       
   * **Stažení složek nebo souborů na místní počítač**

     1. Vyberte složky nebo soubory, které chcete stáhnout.
     2. Na panelu nástrojů v hlavním podokně vyberte **Stáhnout**.
     3. V dialogovém okně **Vyberte složku, do které se mají stažené soubory uložit** zadejte umístění, kam chcete složky nebo soubory stáhnout. Zadejte také název složky.
     4. Vyberte **Uložit**.
   * **Otevření složky nebo souboru z místního počítače**

     1. Vyberte složku nebo soubor, který chcete otevřít.
     2. Na panelu nástrojů v hlavním podokně vyberte **Otevřít** nebo klikněte pravým tlačítkem na vybranou složku nebo soubor a v místní nabídce klikněte na **Otevřít**.
     3. Soubor se stáhne a otevře pomocí aplikace přidružené k základnímu typu tohoto souboru. Nebo se v hlavním podokně otevře složka.

        ![otevření souboru](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-open.png) 

   * **Kopírování složek nebo souborů do schránky**

     1. Vyberte složky nebo soubory, které chcete kopírovat.
     2. Na panelu nástrojů v hlavním podokně vyberte **Kopírovat** nebo klikněte pravým tlačítkem na vybrané složky nebo soubory a v místní nabídce klikněte na **Kopírovat**.
     3. V levém podokně přejděte do jiného účtu ADLS a dvojím kliknutím ho zobrazte v hlavním podokně.
     4. Na panelu nástrojů v hlavním podokně vyberte **Vložit** a vytvořte kopii. Nebo klikněte na **Vložit** v místní nabídce cíle.

        ![kopírování a vložení složky nebo souboru](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-copy-paste.png)

      > [!NOTE] 
          > + Kopírování a vkládání mezi různými typy úložiště se **nepodporuje**. Můžete kopírovat složky nebo soubory ADLS a vkládat je do jiného účtu ADLS. **Nemůžete** však kopírovat složky nebo soubory ADLS a vkládat je do úložiště objektů blob ani naopak. 
          > + Kopírování a vkládání funguje tak, že se složky nebo soubory stáhnou do místního počítače a pak se nahrají do cíle. Nástroj **neprovádí** tuto akci na pozadí. Kopírování a vkládání velkých souborů je pomalé. V současné době probíhá optimalizace vysoce výkonného kopírování a přesouvání souborů.
   * **Odstranění složek nebo souborů**

     1. Vyberte složky nebo soubory, které chcete odstranit.
     2. Na panelu nástrojů v hlavním podokně vyberte **Odstranit** nebo klikněte pravým tlačítkem na vybrané složky nebo soubory a v místní nabídce klikněte na **Odstranit**.
     3. V potvrzovacím dialogovém okně vyberte **Ano**.

        ![kopírování a vložení složky nebo souboru](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-delete.png)

   * **Připnutí na Rychlý přístup**

     1. Zvolte složku, kterou chcete připnout.
     2. Na panelu nástrojů v hlavním podokně vyberte **Připnout na Rychlý přístup**.
     3. V levém podokně se zobrazí přidání vybrané složky do uzlu **Rychlý přístup**.

        ![připnutí na Rychlý přístup](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-quick-access.png)
     4. Po vytvoření rychlého přístupu máte snadný přístup k prostředkům.
   * **Přímé odkazy**
     1. Pokud máte adresu URL, stačí ji zadat do adresního řádku v **Průzkumníku souborů** nebo v prohlížeči.
     2. Automaticky se pak spustí **Storage Explorer.exe** a přejde na adresu URL, kterou jste právě zadali.

        ![přímý odkaz v Průzkumníku souborů](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-deep-link.png)


## <a name="next-steps"></a>Další kroky
* Podívejte se na [nejnovější poznámky k verzi a videa pro Storage Explorer (Preview)](http://www.storageexplorer.com).
* Zjistěte, jak [spravovat službu Azure Cosmos DB v Průzkumníku služby Azure Storage](https://docs.microsoft.com/en-us/azure/cosmos-db/storage-explorer).
* Přečtěte si další informace o Průzkumníku služby Storage v tématu [Začínáme s Průzkumníkem služby Storage (Preview)](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer).
* Začněte pracovat se službou Azure Data Lake Store (ADLS): [Přehled služby Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview).
* Podívejte se na následující video a zjistěte, [jak používat službu Azure Cosmos DB v Průzkumníku služby Azure Storage](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
