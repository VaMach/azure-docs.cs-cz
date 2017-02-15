---
title: "Vytvoření účtu Azure Batch | Dokumentace Microsoftu"
description: "Naučte se vytvořit účet Azure Batch na webu Azure Portal, abyste mohli spouštět velké paralelní úlohy v cloudu."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/21/2016
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: dfcf1e1d54a0c04cacffb50eca4afd39c6f6a1b1
ms.openlocfilehash: 8875c5e91a2a1ae848ebf725ca00bd43ec667dce


---
# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Vytvoření účtu Azure Batch na webu Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Knihovna Batch Management .NET](batch-management-dotnet.md)
> 
> 

Přečtěte si, jak vytvořit účet Azure Batch na webu [Azure Portal][azure_portal] a kde najít důležité vlastnosti účtu, jako jsou přístupové klíče a adresy URL účtu. Probereme také ceny účtů Batch a propojení účtu Azure Storage s účtem Batch, mohli používat [balíčky aplikací](batch-application-packages.md) a [zachovat výstup úloh a úkolů](batch-task-output.md).

## <a name="create-a-batch-account"></a>Vytvoření účtu Batch
1. Přihlaste se na web [Azure Portal][azure_portal].
2. Klikněte na **Nový** > **Compute** > **Batch Service**.
   
    ![Batch na webu Marketplace][marketplace_portal]
3. Zobrazí se okno **Nový účet Batch**. Projděte si položky *a* až *e* a seznamte se s popisem jednotlivých prvků okna.
   
    ![Vytvoření účtu Batch][account_portal]
   
    a. **Název účtu**: Jedinečný název vašeho účtu Batch. Tento název musí být jedinečný pro oblast Azure, kde je účet vytvořený (viz *Umístění* níže). Smí obsahovat jenom malá písmena a čísla a musí být dlouhý 3 až 24 znaků.
   
    b. **Předplatné**: Předplatné, ve kterém chcete účet Batch vytvořit. Pokud máte jenom jedno předplatné, bude ve výchozím nastavení vybrané.
   
    c. **Skupina prostředků**: Existující skupina prostředků vašeho nového účtu Batch, popřípadě si vytvořte novou.
   
    d. **Umístění**: Oblast Azure, ve které chcete účet Batch vytvořit. Jako možnosti se zobrazí jenom oblasti, které podporuje vaše předplatné a skupina prostředků.
   
    e. **Účet úložiště** (volitelné): Účet úložiště pro **obecné účely**, který přidružíte (propojíte) k novému účtu Batch. Další informace najdete v části [Propojený účet Azure Storage](#linked-azure-storage-account) dole.
4. Kliknutím na **Vytvořit** vytvořte účet.
   
   Portál bude informovat, že účet **nasazuje** a po dokončení zobrazí v části *Oznámení* zprávu **Nasazení proběhla úspěšně**.

## <a name="view-batch-account-properties"></a>Zobrazení vlastností účtu Batch
Po vytvoření účtu můžete otevřít **okno účtu Batch** pro přístup k jeho nastavením a vlastnostem. Všechna nastavení a vlastnosti účtu jsou přístupná pomocí levé nabídky okna účtu Batch.

![Okno účtu Batch na webu Azure Portal][account_blade]

* **Adresa URL účtu Batch**: aplikace vytvořené s použitím vývojářských rozhraní [Batch API](batch-technical-overview.md#batch-development-apis) budou potřebovat adresu URL účtu pro správu prostředků a spouštění úloh v účtu. Adresa URL účtu Batch má následující formát:
  
    `https://<account_name>.<region>.batch.azure.com`

![Adresa URL účtu Batch na portálu][account_url]

* **Přístupové klíče**: vaše aplikace k práci s prostředky v účtu Batch potřebují také přístupový klíč. Chcete-li zobrazit nebo obnovit přístupový klíč účtu Batch, zadejte `keys` do pole **Hledat** v levé nabídce v okně účtu Batch a vyberte položku **Klíče**.
  
    ![Klíče účtu Batch na webu Azure Portal][account_keys]

## <a name="pricing"></a>Ceny
Účty Batch jsou nabízeny jen v úrovni Free, což znamená, že za samotný účet Batch nic platit nemusíte. Účtujeme je za výpočetní prostředky Azure, které váš účet Batch spotřebuje, a za prostředky spotřebované jinými službami, když jsou spuštěné vaše úlohy. Poplatky účtujeme například za výpočetní uzly ve vašich fondech a za prostředky služby Azure Storage, které využíváte k ukládání vstupu nebo výstupu vašich úkolů. Podobně pokud v účtu Batch používáte funkci [balíčků aplikací](batch-application-packages.md), budeme vám účtovat prostředky služby Azure Storage, které využíváte k ukládání balíčků aplikací. Další informace najdete v článku [Ceny služby Batch][batch_pricing].

## <a name="linked-azure-storage-account"></a>Propojený účet Azure Storage
Jak bylo zmíněno dříve, můžete s účtem Batch volitelně propojit účet úložiště pro **obecné účely**. Funkce [balíčků aplikací](batch-application-packages.md) účtu Batch využívá úložiště Blob Storage v propojeném účtu úložiště pro obecné účely, stejně jako knihovna .NET [Batch File Conventions](batch-task-output.md). Tyto volitelné funkce vám pomohou při nasazení aplikací spouštěných vašimi úkoly Batch a při zachování dat, které vytvářejí.

Služba Batch aktuálně podporuje *jenom* typ účtu úložiště **pro obecné účely**, jak je popsáno v kroku 5 [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech úložiště Azure](../storage/storage-create-storage-account.md). Při propojování účtu Azure Storage s účtem Batch propojte *jenom* účet úložiště pro **obecné účely**.

![Vytvoření účtu úložiště „pro obecné účely“][storage_account]

Doporučujeme vytvořit účet úložiště pro výhradní použití vaším účtem Batch.

> [!WARNING]
> Při obnovování přístupových klíčů v propojeném účtu úložiště buďte velmi opatrní. Obnovte vždy jen jeden klíč účtu úložiště a v okně propojeného účtu úložiště klikněte na možnost **Synchronizovat klíče**. Počkejte&5; minut, aby se klíče rozšířily do výpočetních uzlů ve fondech, a potom v případě potřeby znovu vygenerujte a synchronizujte další klíč. Pokud byste obnovili (znovu vygenerovali) oba klíče najednou, výpočetní uzly by nedokázaly synchronizovat ani jeden klíč a vy byste ztratili přístup k účtu úložiště.
> 
> 

  ![Obnovování klíčů účtu úložiště][4]

## <a name="batch-service-quotas-and-limits"></a>Kvóty a omezení služby Batch
Pamatujte, že podobně jako pro předplatné a další služby Azure, i pro účty Batch platí určité [kvóty a omezení](batch-quota-limit.md). Aktuální kvóty účtu Batch se zobrazují na portálu ve **vlastnostech** účtu.

![Kvóty účtu Batch na webu Azure Portal][quotas]

Pamatujte na tyto kvóty při navrhování a škálování zatížení vašich úloh Batch. Například pokud váš fond nedosahuje cílového počtu výpočetních uzlů, které jste určili, mohli jste dosáhnout základní kvóty pro účet Batch.

Všimněte si také, že se nemusíte omezit na jeden účet Batch pro vaše předplatné Azure. Můžete spustit několik dávkových úloh v jednom účtu Batch najednou, nebo můžete úlohy rozložit mezi více účtů Batch v jednom předplatném, ale v různých oblastech Azure.

Mnohé z těchto kvót je možné zvýšit jednoduchým požadavkem na bezplatnou podporu, odeslaným pomocí webu Azure Portal. Podrobnosti o vyžádání zvýšení kvóty viz [Kvóty a omezení pro službu Azure Batch](batch-quota-limit.md).

## <a name="other-batch-account-management-options"></a>Další možnosti správy účtu Batch
Kromě webu Azure Portal můžete účty Batch vytvářet a spravovat následujícími způsoby:

* [Rutiny PowerShellu pro účty Batch](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [Knihovna Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Další kroky
* Další informace o konceptech a funkcích služby Batch najdete v článku [Přehled funkcí Azure Batch](batch-api-basics.md). Článek popisuje primární prostředky služby Batch, například fondy, výpočetní uzly a úlohy a nabízí přehled funkcí služby, které umožňují spouštění velkých výpočetních úloh.
* Seznamte se se základy vývoje aplikací, které podporují službu Batch pomocí [klientské knihovny Batch .NET](batch-dotnet-get-started.md). Tento [úvodní článek](batch-dotnet-get-started.md) vás provede funkční aplikací, která používá službu Batch ke spouštění úlohy na několika výpočetních uzlech, a představí vám použití služby Azure Storage k přípravě a načítání souborů úloh.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Obnovování klíčů účtu úložiště"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png



<!--HONumber=Dec16_HO2-->


