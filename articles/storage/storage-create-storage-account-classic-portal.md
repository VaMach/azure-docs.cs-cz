<properties
    pageTitle="Jak vytvořit, spravovat nebo odstranit účet úložiště na Portálu Azure Classic | Microsoft Azure"
    description="Přečtěte si, jak vytvořit nový účet úložiště, spravovat klíče pro přístup k účtu nebo odstranit účtu na Portálu Azure. Získejte informace o účtech úložiště Standard a Premium."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# Informace o účtech Azure Storage

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## Přehled

S účtem úložiště Azure získáte přístup ke službám Azure Blob, Queue, Table a File v Azure Storage. Váš účet úložiště poskytuje jedinečný obor názvů pro datové objekty Azure Storage. Ve výchozím nastavení jsou data ve vašem účtu dostupná pouze pro vás, vlastníka účtu.

Jsou dva druhy účtů úložiště:

- Standardní účet úložiště zahrnuje úložiště Blob, Table, Queue a File.
- Prémiový účet úložiště aktuálně podporuje jen disky virtuálních počítačů Azure. Podrobné vysvětlení úložiště Premium Storage najdete v tématu [Premium Storage: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md).

## Fakturace účtu úložiště

Fakturuje se využívání Azure Storage podle vašeho účtu úložiště. Náklady na úložiště se odvíjí od čtyř faktorů: kapacity úložiště, schématu replikace, transakcích úložiště a odchozích dat.

- Kapacita úložiště znamená, kolik plnění svého účtu úložiště využíváte k uložení dat. Náklady na prosté uložení dat se odvíjí od toho, kolik dat máte uložených a jak se replikují.
- Replikace udává, kolik kopií vašich dat se spravuje zároveň a v jakých umístěních.
- Transakce znamenají všechny operace čtení a zápisu v Azure Storage.
- Odchozí data zahrnují data přenesená ven z oblasti Azure. Když k datům v účtu úložiště přistupuje aplikace, která neběží ve stejné oblasti, ať už se jedná o cloudovou službu nebo jiný typ aplikace, budou se vám účtovat odchozí data. (Ve službách Azure můžete svoje data seskupit do datových center a tak snížit nebo úplně zabránit odchozím datům a s nimi spojeným nákladům.)  

Na stránce [Azure Storage – Ceny](https://azure.microsoft.com/pricing/details/storage) najdete podrobné informace o cenách za kapacitu úložiště, replikaci a transakce. Na stránce [Informace o cenách datových přenosů](https://azure.microsoft.com/pricing/details/data-transfers/) najdete podrobné informace o sazbách za odchozí data.

Podrobné informace o kapacitě úložiště a cílech výkonnosti najdete v tématu [Škálovatelnost a cíle výkonnosti Azure Storage](storage-scalability-targets.md).

> [AZURE.NOTE] Když vytvoříte virtuální počítač Azure, účet úložiště se vám vytvoří automaticky v umístění nasazení, pokud ještě nemáte účet úložiště v tomto umístění. Není proto podle následujících kroků nutné vytvořit účet úložiště pro disky virtuálního počítače. Název účtu úložiště bude založený na názvu virtuálního počítače. Další podrobnosti najdete v [dokumentaci ke službě Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). 

## vytvořit účet úložiště

1. Přihlaste se k [Azure Portal Classic](https://manage.windowsazure.com).

2. Na hlavním panelu v dolní části stránky klikněte na **Nový**. Vyberte **Datové služby** | **Úložiště** a klikněte na **Rychlé vytvoření**.

    ![NovýÚčetÚložiště](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. Do pole **Adresa URL** zadejte název účtu úložiště.

    > [AZURE.NOTE] Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom čísla a malá písmena.
    >  
    > Váš název účtu úložiště musí být jedinečný v rámci Azure. Pokud se vybraný název účtu úložiště už používá, bude tato informace uvedená na Portálu Azure Classic.

    Podrobnosti o tom, jak se bude název vašeho účtu úložiště používat k adresování vašich objektů v úložišti Azure Storage, najdete níže v tématu [Koncové body účtu úložiště](#storage-account-endpoints).

4. V **Umístění / Skupině vztahů** vyberte umístění vašeho účtu úložiště, které je blízko vás i vašich zákazníků. Pokud k datům ve vašem účtu úložiště bude přistupovat jiná služba Azure, jako například virtuální počítač Azure nebo cloudová služba, mohli byste v seznamu vybrat skupinu vztahů a celý svůj účet úložiště tak seskupit do jednoho datového centra s ostatními službami Azure, které používáte – dosáhli byste tak většího výkonu a snížení nákladů.

    Nezapomeňte, že při vytváření účtu taky musíte vybrat nějakou skupinu vztahů. Existující účet nemůžete přesunout do skupiny vztahů. Další informace o skupinách vztahů najdete níže v tématu [Společné umístění služeb pomocí skupiny vztahů](#service-co-location-with-an-affinity-group).

    >[AZURE.IMPORTANT] Pokud chcete zjistit, která umístění jsou dostupná pro vaše předplatné, můžete zavolat operaci [Zobrazit seznam všech poskytovatelů prostředků](https://msdn.microsoft.com/library/azure/dn790524.aspx). Pokud chcete zobrazit seznam poskytovatelů z prostředí PowerShell, zavolejte [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). V prostředí .NET použijte metodu [List](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) třídy ProviderOperationsExtensions.
    >
    >V tématu [Oblasti Azure](https://azure.microsoft.com/regions/#services) najdete další informace o tom, které služby jsou dostupné v konkrétních oblastech.


5. Pokud máte více než jedno předplatné, zobrazí se pole **Předplatné**. Do pole **Předplatné** zadejte předplatné Azure, se kterým chcete používat účet úložiště.

6. V položce **Replikace** vyberte požadovanou úroveň replikace pro váš účet úložiště. Doporučuje se vybrat geo-redundantní replikaci, která vašim datům poskytuje maximální odolnost. Další informace o možnostech replikace pro Azure Storage najdete v tématu [Replikace Azure Storage](storage-redundancy.md).

6. Klikněte na **Vytvořit účet úložiště**.

    Vytvoření účtu může chvíli trvat. Pokud chcete zkontrolovat stav, můžete sledovat oznámení ve spodní části portálu Azure Classic. Po vytvoření účtu úložiště má váš nový účet úložiště stav **Online** a je připravený k použití.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### Koncové body účtu úložiště

Každý objekt, který uložíte v úložišti Azure Storage, má jedinečnou adresu URL. Název účtu úložiště tvoří subdoménu dané adresy. Kombinace názvu domény a subdomény, která je pro každou službu specifická, tvoří *koncový bod* vašeho účtu úložiště.

Pokud je například název účtu úložiště *můj_účet_úložiště*, pak jsou výchozí koncové body pro váš účet úložiště tyto:

- Služba objektů blob: http://*můj_účet_úložiště*.blob.core.windows.net

- Služba Table: http://*můj_účet_úložiště*.table.core.windows.net

- Služba front: http://*můj_účet_úložiště*.queue.core.windows.net

- Služba File: http://*můj_účet_úložiště*.file.core.windows.net

Po vytvoření účtu můžete na řídicím panelu úložiště v [portálu Azure Classic](https://manage.windowsazure.com) vidět koncové body svého účtu úložiště.

Adresa URL pro přístup k objektu v účtu úložiště se sestaví připojením umístění objektu v účtu úložiště ke koncovému bodu. Například adresa účtu pro objekty blob může mít tento formát: http://*můj_účet_úložiště*.blob.core.windows.net/*můj_kontejner*/*můj_objekt_blob*.

Můžete také nakonfigurovat vlastní název domény, který budete chtít se svým účtem úložiště používat. Podrobnější informace najdete v tématu [Konfigurace vlastního názvu doménu pro koncový bod služby Blob Storage](storage-custom-domain-name.md).

### Společné umístění služeb pomocí skupiny vztahů

*Skupina vztahů* je geografické seskupení vašich služeb Azure a virtuálních počítačů a vaším účtem úložiště Azure. Skupina vztahů může zlepšit výkon služby vyhledáním úloh počítačů ve stejném datovém centru nebo v blízkosti cílové skupiny uživatelů. Navíc se vám neúčtují žádné poplatky za odchozí data, pokud k datům ve vašem úložišti přistupují jiné služby, které patří do stejné skupiny vztahů.

> [AZURE.NOTE]  Pokud chcete vytvořit skupinu vztahů, otevřete oblast <b>Nastavení</b> [portálu Azure Classic](https://manage.windowsazure.com), klikněte na <b>Skupiny vztahů</b>, a potom klikněte na <b>Přidat skupinu vztahů</b> nebo tlačítko <b>Přidat</b>. Skupiny vztahů také můžete také vytvořit a spravovat přes Azure Service Management API. Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">Operace pro skupiny vztahů</a>.

## Zobrazení, kopírování a opětovné vygenerování přístupových klíčů k úložišti

Při vytváření účtu úložiště vygeneruje Azure dva 512bitové přístupové klíče k úložišti, které se používají pro ověřování přístupu k účtu úložiště. Poskytnutím dvou přístupových klíčů k úložišti vám Azure umožňuje znovu vygenerovat klíče bez přerušení poskytování vaší služby úložiště nebo přístupu k této službě.

> [AZURE.NOTE] Doporučujeme vám přístupové klíče k úložišti s nikým nesdílet. Pokud budete chtít povolit přístup k prostředkům úložiště bez poskytnutí přístupových klíčů, můžete použít *sdílený přístupový podpis*. Sdílený přístupový podpis poskytuje přístup k prostředku v rámci vašeho účtu pro časový interval, který nadefinujete, a s oprávněními, která zadáte. Další informace najdete v tématu [Sdílené přístupové podpisy: vysvětlení modelu SAS](storage-dotnet-shared-access-signature-part-1.md).

V [portálu Azure Classic](https://manage.windowsazure.com) na řídicím panelu nebo na stránce **Úložiště** použijte **Správu klíčů**, kde můžete zobrazit, kopírovat a opět vytvořit přístupové klíče k úložišti, které se používají pro přístup ke službám Blob, Table a Queue.

### Zkopírování přístupového klíče k úložišti  

Pomocí **Správy klíčů** můžete zkopírovat přístupový klíč k úložišti pro použití v připojovacím řetězci. Připojovací řetězec potřebuje při ověření použít název účtu úložiště a klíč. Informace o konfiguraci připojovacího řetězce pro přístup ke službám úložiště Azure najdete v tématu [Nakonfigurování připojovacích řetězců Azure Storage](storage-configure-connection-string.md).

1. V [portálu Azure Classic](https://manage.windowsazure.com) klikněte na **Úložiště** a potom na název účtu úložiště, tím se otevře řídicí panel.

2. Klikněte na **Správu klíčů**.

    Otevře se **Správa přístupových klíčů**.

    ![Správaklíčů](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Pokud chcete zkopírovat přístupový klíč k úložišti, označte text klíče. Potom na něj klikněte pravým tlačítkem a klikněte na **Kopírovat**.

### Opětovné vygenerování přístupových klíčů k úložišti
Doporučujeme přístupové klíče k účtu úložiště pravidelně měnit, aby byla připojení k úložišti stále zabezpečená. Dva přístupové klíče se přiřazují proto, abyste mohli pro připojení k účtu úložiště používat jeden přístupový klíč, zatímco si znovu vygenerujete druhý přístupový klíč.

> [AZURE.WARNING] Opětovné generování přístupových klíčů může mít vliv na služby v Azure a také na vaše vlastní aplikace, které jsou na účtu úložiště závislé. Všichni klienti, kteří používají přístupový klíč pro přístup k účtu úložiště, se musí aktualizovat na používání nového klíče.

**Media Services** – Pokud máte služby Media Services, které jsou závislé na vašem účtu úložiště, musíte přístupové klíče znovu synchronizovat s touto službou po opětovném vygenerování klíčů.

**Aplikace** – Pokud máte webové aplikace nebo cloudové služby, které používají účet úložiště, dojde při opětovném vygenerování klíčů ke ztrátě připojení, pokud klíče nezaregistrujete. 

**Průzkumníci úložiště** – Pokud používáte některou z [aplikací průzkumníka úložiště](storage-explorers.md), budete pravděpodobně muset aktualizovat klíč úložiště používaný těmito aplikacemi.

Tady je proces pro výměnu přístupových klíčů k úložišti:

1. Aktualizujte připojovací řetězce v kódu aplikace tak, aby odkazovaly na sekundární přístupový klíč účtu úložiště.

2. Znovu vygenerujte primární přístupový klíč pro účet úložiště. V [portálu Azure Classic](https://manage.windowsazure.com) na řídicím panelu nebo na stránce **Konfigurace** klikněte na **Správu klíčů**. Klikněte na **Znovu vygenerovat** pod primárním přístupovým klíčem a klikněte na **Ano** pro potvrzení, že chcete vygenerovat nový klíč.

3. Aktualizujte připojovací řetězce v kódu tak, aby odkazovaly na nový primární přístupový klíč.

4. Stejným způsobem pak opětovně vygenerujte sekundární přístupový klíč.

## Odstranění účtu úložiště

Pokud chcete odstranit účet úložiště, který už nepoužíváte, použijte funkci **Odstranit** na řídicím panelu nebo na stránce **Konfigurace**. **Odstranit** odstraní celý účet úložiště včetně všech objektů blob, tabulek a v účtu.

> [AZURE.WARNING] Odstraněný účet úložiště není možné obnovit ani není možné načíst žádný obsah, který byl součástí účtu před jeho odstraněním. Nezapomeňte si před odstraněním účtu zazálohovat všechno, co chcete uložit. To platí také pro všechny prostředky v rámci účtu – po odstranění jsou objekt blob, tabulka, fronta nebo soubor odstraněny trvale.
>
> Pokud váš účet úložiště obsahuje soubory virtuálního pevného disku pro virtuální počítač Azure, pak musíte odstranit všechny image a disky, které používají tyto soubory virtuálních pevných disků, aby bylo možné odstranit účet úložiště. Nejdřív zastavte virtuální počítač, pokud je spuštěný, a pak ho odstraňte. Pokud chcete odstranit disky, přejděte na záložku **Disky** a odstraňte požadované disky. Pokud chcete odstranit image, přejděte na kartu **Image** a odstraňte všechny image, které jsou uložené v účtu.

1. Na [portálu Azure Classic](https://manage.windowsazure.com) klikněte na **Úložiště**.

2. Klikněte kamkoli do položky účtu úložiště kromě názvu, a potom klikněte na **Odstranit**.

     - nebo -

    Klikněte na název účtu úložiště, tím se otevře řídicí panel, a potom klikněte na **Odstranit**.

3. Kliknutím na **Ano** potvrďte, že chcete účet úložiště odstranit.

## Další kroky

- Další informace o službě Azure Storage najdete v [dokumentaci pro Azure Storage](https://azure.microsoft.com/documentation/services/storage/).
- Navštivte [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/).
- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)



<!---HONumber=Aug16_HO4-->


