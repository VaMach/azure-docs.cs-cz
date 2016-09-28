Jsou dva druhy účtů úložiště:

### Účty služby Storage pro obecné účely

Účet úložiště pro obecné účely poskytuje přístup ke službám Azure Storage, například k tabulkám, frontám, souborům, objektům blob a diskům virtuálních počítačů Azure v rámci jednoho účtu. Tento typ účtu úložiště má dvě úrovně výkonu:

- Úroveň výkonu standardního úložiště, které umožňuje ukládání tabulek, front, souborů, objektů blob a disků virtuálních počítačů Azure.
- Úroveň výkonu prémiového úložiště, která aktuálně podporuje jenom disky virtuálních počítačů Azure. Podrobný popis služby Premium Storage najdete v článku [Úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../articles/storage/storage-premium-storage.md).

### Účty služby Blob Storage

Účet služby Blob Storage je specializovaný účet úložiště pro ukládání nestrukturovaných dat v podobě objektů blob do služby Azure Storage. Účty služby Blob Storage jsou podobné účtům služby Storage pro obecné účely a mají stejně vysokou odolnost, dostupnost, škálovatelnost a výkonnost, a navíc mají 100% konzistentnost rozhraní API pro objekty blob bloků a doplňovací objekty blob. V případě aplikací, které vyžadují jenom úložiště objektů blob bloku nebo objektů blob doporučujeme používat účty úložiště objektů blob.

> [AZURE.NOTE] Účty úložiště Blob podporují pouze objekty blob bloku a doplňovací objekty blob, nepodporují objekty blob stránky.

Účty služby Blob Storage exponují atribut **Úroveň přístupu**, který můžete zadat při vytváření účtů a později ho můžete podle potřeby upravit. Existují dva typy úrovně přístupu, které můžete zadat na základě vzoru pro přístup k datům:

- **Horká** úroveň přístupu, která znamená, že k objektům v účtu úložiště budete přistupovat častěji. Tato možnost nabízí ukládání dat s nižšími přístupovými náklady.
- **Studená** úroveň přístupu, která znamená, že k objektům v účtu úložiště nebudete přistupovat tak často. Tato možnost nabízí ukládání dat s nižšími náklady na úložiště dat.

Mezi úrovněmi přístupu můžete kdykoli přepnout, třeba pokud pro vaše data existuje vzor používání. Se změnou úrovně přístupu můžou být spojené další poplatky. Další podrobnosti najdete v článku [Účty služby Blob Storage – ceny a fakturace](../articles/storage/storage-blob-storage-tiers.md#pricing-and-billing).

Další podrobnosti najdete v článku [Azure Blob Storage: úrovně Cool a Hot](../articles/storage/storage-blob-storage-tiers.md).

Před vytvořením účtu úložiště si musíte pořídit předplatné Azure. Jedná se o tarif, který vám umožní přístup k různým službám Azure. Azure můžete začít používat s [bezplatným účtem](https://azure.microsoft.com/pricing/free-trial/). Jakmile se rozhodnete koupit plán předplatného, můžete si vybrat z různých [možností nákupu](https://azure.microsoft.com/pricing/purchase-options/). Pokud jste [předplatitelem MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), získáte bezplatné měsíční kredity, které můžete využít na služby Azure, včetně služby Azure Storage. Informace o cenách najdete v článku [Ceny služby Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Informace o vytvoření účtu úložiště najdete v článku [Vytvoření účtu úložiště](../articles/storage/storage-create-storage-account.md#create-a-storage-account). V rámci jednoho předplatného můžete vytvořit až 100 účtů úložiště s jedinečným názvem. Podrobné informace o omezeních účtu úložiště najdete v článku [Škálovatelnost a cíle výkonnosti služby Azure Storage](../articles/storage/storage-scalability-targets.md).


<!--HONumber=Sep16_HO3-->


