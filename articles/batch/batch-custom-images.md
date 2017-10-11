---
title: "Zřídit fondech Azure Batch z vlastních bitových kopií | Microsoft Docs"
description: "Můžete vytvořit dávce fondu vlastní image zřídit výpočetní uzly, které obsahují software a data, která je nutné pro vaši aplikaci. Vlastní Image jsou účinný způsob, jak nakonfigurovat výpočetních uzlů k spouštět úlohy Batch."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/07/2017
ms.author: tamram
ms.openlocfilehash: 3d655766b4f2a5efb0c8c29ffa81a89f84b3e17c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-a-custom-image-to-create-a-pool-of-virtual-machines"></a>Použít vlastní bitovou kopii k vytvoření fondu virtuálních počítačů

Při vytváření fondu virtuálních počítačů v Azure Batch, zadejte image virtuálního počítače (VM), které poskytuje operační systém pro každý výpočetní uzel ve fondu. Fond virtuálních počítačů můžete vytvořit buď pomocí image Azure Marketplace, nebo zadáním vlastní image virtuálního pevného disku, který jste připravili. Když zadáte vlastní image, máte kontrolu nad konfigurace operačního systému v době, který je zajištěný každý výpočetní uzel. Vlastní image může zahrnovat aplikace a referenčních dat, které jsou k dispozici na výpočetním uzlu, jakmile je zřízený.

Pomocí vlastní image můžete ušetřit čas při získávání váš fond výpočetních uzlů, které jsou připravené ke spuštění úlohy Batch. Zatímco můžete vždy použít image Azure Marketplace a instalovat software na každém výpočetním uzlu po byla zřízení, může být tento přístup míň efektivní než pomocí vlastní image. 

Chcete-li použít vlastní obrázek, který je nakonfigurovaný pro váš scénář důvodů museli:

- **Konfigurace operačního systému (OS)** žádnou zvláštní konfiguraci operačního systému lze provést na vlastní image. 
- **Nainstalujte velké aplikace.** Instalace aplikace na vlastní image je efektivnější než je nainstalujete na každém výpočetním uzlu po je zřízený.
- **Zkopírujte poměrně velké množství dat.** Pokud ale data se zkopírují do vlastní image, pouze se musí zkopírovat jednou, nikoli na každém výpočetním uzlu, ukládání čas a šířku pásma.
- **Restartování virtuálního počítače během procesu instalace.** Restartování virtuálního počítače může být časově náročné, zvláště pokud máte počet výpočetních uzlů.

## <a name="prerequisites"></a>Požadavky

- **Účet Batch vytvořit s režimem přidělení fondu předplatné uživatele.** Pokud chcete používat vlastní image pro zřízení virtuálního počítače fondy, vytvořte vašeho účtu Batch s předplatným uživatele [režim přidělení fondu](batch-api-basics.md#pool-allocation-mode). S tímto režimem se fondy Batch přidělují do předplatného, ve které je tento účet umístěný. Najdete v článku [účet](batch-api-basics.md#account) kapitoly [rozsáhlé paralelní vývoj výpočetní řešení pomocí služby Batch](batch-api-basics.md) informace o nastavení režimu přidělení fondu při vytváření účtu Batch.

- **Účet služby Azure Storage.** Vytvoření fondu virtuálních počítačů pomocí vlastní image, musíte standardní, obecný účet úložiště Azure ve stejném předplatném a oblast. Pokud vytvoříte vlastní image z virtuálního počítače Azure, bude kopírovat bitovou kopii k účtu úložiště, kde se nachází disk operačního systému Virtuálního počítače, a nebude je potřeba vytvořit účet samostatného úložiště. 
    
## <a name="prepare-a-custom-image"></a>Příprava vlastní image

Příprava vlastní image pro použití se službou Batch, musí generalize existující instalace systému Linux nebo Windows. Generalizací instalace operačního systému odebere informace specifické pro počítač. Výsledkem je obrázek, který lze nainstalovat na jiných počítačích nebo virtuálních počítačů.  

> [!IMPORTANT]
> Batch aktuálně nepodporuje použití Azure spravované bitové kopie k přidělení fondu. Vlastní image, které můžete použít k přidělení fondu musí být uložen v úložišti Azure Storage. 
>
> Při přípravě vlastní image, mějte na paměti následující body:
> - Zajistěte, aby image základního operačního systému, kterou použijete ke zřizování fondů Batch, neobsahovala žádná předinstalovaná rozšíření Azure, jako je například rozšíření vlastních skriptů. Pokud image obsahuje nějaké předinstalované rozšíření, v Azure může docházet k problémům při nasazování virtuálního počítače.
> - Zajistěte, aby poskytnutá image základního operačního systému používala výchozí dočasnou jednotku, protože ji očekává agent uzlu Batch.
>
>

Všechny existující připravené systému Windows nebo Linux bitové kopie můžete použít jako vlastní image. Například pokud chcete použít místní bitovou kopii, pak nahrajte bitovou kopii do účtu Azure Storage, který je ve stejném předplatném, oblasti jako váš účet Batch pomocí [AzCopy](../storage/storage-use-azcopy.md) nebo jiný nástroj pro odesílání.

Můžete také připravit vlastní image z nového nebo existujícího virtuálního počítače Azure. Pokud vytváříte nový virtuální počítač, můžete image Azure Marketplace použít jako základní bitovou kopii pro vlastní image a pak ji přizpůsobit. Chcete-li přizpůsobit základní bitovou kopii, vytvořte virtuální počítač Azure a do ní přidejte aplikace nebo data. Potom generalize virtuálního počítače sloužit jako vlastní image a uložte ho do úložiště Azure. 

Pokud chcete připravit vlastní image z virtuálního počítače Azure, postupujte takto:

1. Vytvoření **nespravované** virtuálního počítače Azure z image Azure Marketplace. Azure Marketplace obsahuje bitové kopie pro obě [Windows](../virtual-machines/windows/quick-create-portal.md) a [Linux](../virtual-machines/linux/quick-create-portal.md).
    
    V kroku 3 tohoto procesu vytváření virtuálních počítačů, ujistěte se, že jste vybrali **ne** pro **úložiště: použijte spravované disky** možnost. Také si poznamenejte název účtu úložiště pro disk s operačním systémem Virtuálního počítače, protože tento účet úložiště se taky, kde Azure uloží vlastní bitovou kopii:

    ![Vytvoření nespravované virtuálního počítače a poznamenejte si název účtu úložiště](media/batch-custom-images/vm-create-storage.png)
 
2. Dokončete proces vytváření virtuálního počítače a počkejte na její přidělování Azure. Tady je obrázek, který ukazuje na portálu Azure v běžícím stavu virtuálního počítače:

    ![Vytvoření virtuálního počítače z image pořízenou prostřednictvím marketplace](media/batch-custom-images/vm-status-running.png)

3. Jakmile je virtuální počítač spuštěný, lze k němu připojte pomocí protokolu RDP (pro Windows) nebo SSH (pro Linux). Nainstalujte potřebný software nebo zkopírujte požadované data a pak generalize virtuálního počítače. Postupujte podle kroků popsaných v [Generalize virtuální počítač](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized.md#generalize-the-vm). 
   
4. Postup [přihlášení k prostředí Azure PowerShell](../virtual-machines/windows/sa-copy-generalized.md#log-in-to-azure-powershell). Chcete-li nainstalovat Azure PowerShell, přečtěte si téma [Přehled prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.2.0). 

5. Potom postupujte podle kroků pro [zrušit přidělení virtuálního počítače a nastavit stav na zobecněný](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized#deallocate-the-vm-and-set-the-state-to-generalized). 

    Na portálu Azure Všimněte si, že je virtuální počítač navrácený:

    ![Ujistěte se, že je virtuální počítač navrácený](media/batch-custom-images/vm-status-deallocated.png)

6.  Vytvořte a uložte bitovou kopii virtuálního počítače k úložišti Azure pomocí [uložit AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) rutiny prostředí PowerShell. Postupujte podle pokynů v tématu [vytvoření bitové kopie](../virtual-machines/windows/sa-copy-generalized.md#create-the-image).
    
    Image virtuálního počítače je uložena do účtu úložiště Azure vytvořili při vytvoření virtuálního počítače, jak je znázorněno v kroku 1 tohoto postupu. Uložit AzureRmVMImage rutina uloží obrázek, který má **systému** kontejneru v daném účtu úložiště. `-DestinationContainername` Parametr názvy virtuální adresáře v rámci **systému** kontejneru. `-VHDNamePrefix` Parametr určuje předponu pro název objektu blob. Před touto předponou názvu objektu blob s pomlčkou. 

    Předpokládejme například, že zavoláte AzureRmVMImage uložit s následujícími parametry:  

        Save-AzureRmVMImage -ResourceGroupName sample-resource-group -Name vm-custom-image -DestinationContainerName batchimages -VHDNamePrefix custom -Path C:\Temp\Images\vm-custom-image.json

    Výsledný obraz je uložena do umístění a název objektu blob znázorněno zde:

    ![Umístění uloženého virtuálního pevného disku v kontejneru systému](media/batch-custom-images/vhd-in-vm-storage-account.png)

    > [!NOTE]
    > Azure nespravované virtuálního počítače vytvoří několik účtů úložiště pro jiné účely. Pokud při vytvoření virtuálního počítače není poznamenejte si název kontejneru úložiště pro disk operačního systému, pak Najít účet přidruženého úložiště, který obsahuje **systému** kontejneru. Procházet **systému** kontejneru najít vlastní image, pomocí hodnoty zadané pro **uložit AzureRmVMImage** příkaz.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Vytvořit fond z vlastní image na portálu

Jakmile jste uložili vlastní bitovou kopii a znáte jeho umístění, můžete vytvořit fondu služby Batch z této bitové kopie. Postupujte podle těchto kroků můžete vytvořit fond z portálu Azure:

1. Na webu Azure Portal přejděte ke svému účtu Batch. Tento účet musí být ve stejném předplatném, oblasti jako účet úložiště, který obsahuje vlastní image. 
2. V **nastavení** okno na levé straně vyberte **fondy** položku nabídky.
3. V **fondy** vyberte **přidat** příkaz.
4. Na **přidat fond** vyberte **vlastní obrázek (Linux nebo Windows)** z **typ obrázku** rozevíracího seznamu. Na portálu se zobrazí výběr **Vlastní image**. Přejděte na účet úložiště, kde se nachází vlastní image a vyberte požadované virtuální pevný disk z kontejneru. 
5. Vyberte správný **vydavatele nebo nabídka nebo Sku** pro vlastního virtuálního pevného disku.
6. Zadejte zbývající požadované nastavení, včetně **velikost uzlu**, **cílové uzly vyhrazené**, a **nízkou prioritu uzly**, stejně jako všechny potřeby volitelná nastavení.

    Například pro Microsoft Windows Server Datacenter 2016 vlastní image **přidat fond** okno se zobrazí, jak je vidět tady:

    ![Přidejte fond z vlastní bitovou kopii systému Windows](media/batch-custom-images/add-pool-custom-image.png)
  
Pokud chcete zkontrolovat, zda existující fond je založena na vlastní image, najdete v části **operačního systému** vlastnost v souhrnu oddílu prostředků **fondu** okno. Pokud fond byl vytvořen z vlastní image, je nastavený na **vlastní Image virtuálního počítače**.

Zobrazí se všechny vlastní VHD přidružený fond na fondu **vlastnosti** okno.
 
## <a name="next-steps"></a>Další kroky

- Podrobnější přehled služby Batch, najdete v tématu [rozsáhlé paralelní vývoj výpočetní řešení pomocí služby Batch](batch-api-basics.md).
- Informace o vytvoření účtu Batch najdete v tématu [vytvořit účet Batch pomocí portálu Azure](batch-account-create-portal.md).