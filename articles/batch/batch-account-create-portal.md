<properties
    pageTitle="Vytvoření účtu Azure Batch | Microsoft Azure"
    description="Naučte se vytvořit účet Azure Batch na portálu Azure, abyste mohli spouštět velké paralelní úlohy v cloudu."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="marsma"/>

# Vytvoření a správa účtu Azure Batch na portálu Azure

> [AZURE.SELECTOR]
- [Portál Azure](batch-account-create-portal.md)
- [Knihovna Batch Management .NET](batch-management-dotnet.md)

 [Portál Azure][azure_portal] vám poskytuje nástroje potřebné k vytvoření a správě účtu Azure Batch, který můžete použít ke zpracování velkých paralelních úloh. V tomto článku vás provedeme vytvořením účtu Batch pomocí portálu a upozorníme na důležitá nastavení a vlastnosti účtu Batch. Například aplikace a služby, které vyvíjíte pomocí služby Batch, potřebují adresu URL vašeho účtu a přístupový klíč, aby mohly komunikovat s rozhraním API služby Batch. Obojí najdete na portálu Azure.

>[AZURE.NOTE] Portál Azure aktuálně podporuje jenom podmnožinu funkcí služby Batch, do které patří vytvoření účtu, správa nastavení a vlastností účtu Batch a vytvoření a sledování fondů a úloh. Úplná sada funkcí služby Batch je vývojářům dostupná prostřednictvím rozhraní API služby Batch.

## Vytvoření účtu Batch

1. Přihlaste se k [portálu Azure][azure_portal].

2. Klikněte na **Nový** > **Virtuální počítače** > **Služba Batch**.

    ![Batch na webu Marketplace][marketplace_portal]

3. Zobrazí se okno **Nový účet Batch**. Projděte si položky *a* až *e* a seznamte se s popisem jednotlivých prvků okna.

    ![Vytvoření účtu Batch][account_portal]

    a. **Název účtu**: Jedinečný název vašeho účtu Batch. Tento název musí být jedinečný pro oblast Azure, kde je účet vytvořený (viz *Umístění* níže). Smí obsahovat jenom malá písmena a čísla a musí být dlouhý 3 až 24 znaků.

    b. **Předplatné**: Předplatné, ve kterém chcete účet Batch vytvořit. Pokud máte jenom jedno předplatné, bude ve výchozím nastavení vybrané.

    c. **Skupina prostředků**: Existující skupina prostředků vašeho nového účtu Batch, popřípadě si vytvořte novou.

    d. **Umístění**: Oblast Azure, ve které chcete účet Batch vytvořit. Jako možnosti se zobrazí jenom oblasti, které podporuje vaše předplatné a skupina prostředků.

    e. **Účet úložiště** (volitelné): Účet úložiště pro **obecné účely**, který přidružíte (propojíte) k novému účtu Batch. Funkce [balíčků aplikací](batch-application-packages.md) služby Batch použije propojený účet úložiště k ukládání a načítání balíčků aplikací. Další informace o této funkci najdete v článku [Nasazení aplikací pomocí balíčků aplikací Azure Batch](batch-application-packages.md).

     > [AZURE.IMPORTANT] Obnovení klíčů v připojeném účtu služby Storage má zvláštní požadavky. Další podrobnosti najdete níže v článku [Důležité informace týkající se účtů Batch](#considerations-for-batch-accounts).

4. Kliknutím na **Vytvořit** vytvořte účet.

  Portál bude informovat, že účet **nasazuje** a po dokončení zobrazí v části *Oznámení* zprávu **Nasazení proběhla úspěšně**.

## Zobrazení vlastností účtu Batch

Okno účtu Batch zobrazuje několik vlastností účtu a poskytuje také přístup k dalším nastavením, například k přístupovým klíčům, kvótám, uživatelům a přidružení účtu úložiště.

* **Adresa URL účtu Batch**: Tato adresa URL zajišťuje přístup k účtu Batch, když používáte rozhraní API, například rozhraní API [Batch REST][api_rest] rozhraní API nebo klientskou knihovnu [Batch .NET][api_net] a dodržuje následující formát:

    `https://<account_name>.<region>.batch.azure.com`

* **Přístupový klíč**: Pokud chcete zobrazit a spravovat přístupový klíč účtu Batch, klikněte na ikonu klíče a otevřete okno **Správa klíčů**, nebo klikněte na **Všechna nastavení** > **Klíče**. Při komunikaci s rozhraním API služby Batch bude vyžadován přístupový klíč, například [Batch REST][api_rest] nebo klientská knihovna [dávky .NET][api_net].

    ![Klíče účtu Batch][account_keys]

* **Všechna nastavení**: Pokud chcete spravovat všechna nastavení účtu Batch nebo zobrazit jeho vlastnosti, klikněte na **Všechna nastavení** a otevřete okno **Nastavení**. Toto okno poskytuje přístup ke všem nastavením a vlastnostem účtu včetně zobrazení kvót účtu, výběru účtu Azure Storage, který chcete propojit s účtem Batch, a správy uživatelů.

    ![Okna nastavení a vlastností účtu Batch][5]

## Důležité informace týkající se účtu Batch

* Účty Batch můžete vytvářet a spravovat také pomocí [rutin PowerShellu ve službě Batch](batch-powershell-cmdlets-get-started.md) a knihovny [rozhraní Batch Management .NET](batch-management-dotnet.md).

* Za účet Batch samotný vám žádné poplatky neúčtujeme. Účtujeme je za veškeré prostředky řešení Batch, které spotřebujete, a za prostředky spotřebované jinými službami, když jsou spuštěné úlohy. Poplatky účtujeme například za výpočetní uzly ve fondech a pokud používáte funkci [balíčků aplikací](batch-application-packages.md), budeme vám účtovat prostředky služby Azure Storage, které spotřebujete k ukládání verzí balíčků aplikací. Další informace najdete v článku [Ceny služby Batch][batch_pricing].

* V jednom účtu Batch můžete spustit několik úloh služby Batch najednou, nebo můžete úlohy distribuovat mezi účty Batch v různých oblastech Azure.

* Pokud spouštíte několik velkých úloh služby Batch, dávejte pozor na určité [kvóty a omezení služby Batch](batch-quota-limit.md), které se týkají vašeho předplatného Azure a každého účtu Batch. Aktuální kvóty účtu Batch se zobrazují na portálu ve vlastnostech účtu.

* Pokud přidružíte (propojíte) účet úložiště s účtem Batch, postupujte při opakovaném generování přístupových klíčů k účtu úložiště opatrně. Vždy znovu generujte jenom jeden klíč k účtu úložiště. Klikněte v okně propojeného účtu úložiště na **Synchronizace klíčů**, počkejte 5 minut, aby se klíče rozšířily do výpočetních uzlů ve fondech, a potom v případě potřeby znovu vygenerujte a synchronizujte další klíč. Pokud byste znovu generovali oba klíče najednou, výpočetní uzly by nedokázaly synchronizovat ani jeden klíč a vy byste ztratili přístup k účtu úložiště.

  ![Opakované generování klíčů k účtu úložiště][4]

> [AZURE.IMPORTANT] Služba Batch aktuálně podporuje *jenom* typ účtu úložiště **pro obecné účely**, jak je popsáno v kroku č. 5 [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech úložiště Azure](../storage/storage-create-storage-account.md). Při propojování účtu Azure Storage s účtem Batch, propojte *jenom* účet úložiště pro **obecné účely**.

## Další kroky

* Další informace o konceptech a funkcích služby Batch najdete v článku [Přehled funkcí Azure Batch](batch-api-basics.md). Článek popisuje primární prostředky služby Batch, například fondy, výpočetní uzly a úlohy a nabízí přehled funkcí služby, které umožňují spouštění velkých výpočetních úloh.

* Seznamte se se základy vývoje aplikací, které podporují službu Batch pomocí [klientské knihovny Batch .NET](batch-dotnet-get-started.md). Tento [úvodní článek](batch-dotnet-get-started.md) vás provede funkční aplikací, která používá službu Batch ke spouštění úlohy na několika výpočetních uzlech, a představí vám použití služby Azure Storage k přípravě a načítání souborů úloh.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Opakované generování klíčů k účtu úložiště"
[5]: ./media/batch-account-create-portal/batch_acct_05.png "Okna nastavení a vlastností účtu Batch"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG



<!--HONumber=Jun16_HO2-->


