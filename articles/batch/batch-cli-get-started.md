---
title: "Začínáme se službou Azure Batch pomocí příkazového řádku Azure CLI | Dokumentace Microsoftu"
description: "Rychlý úvod k příkazům Batch v rozhraní příkazového řádku Azure CLI pro správu prostředků služby Azure Batch"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/11/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 5bbeb9d4516c2b1be4f5e076a7f63c35e4176b36
ms.openlocfilehash: 19014e65920b16d2efbaa475b7c17b2a4e3a8471
ms.contentlocale: cs-cz
ms.lasthandoff: 06/13/2017


---
# Správa prostředků služby Batch pomocí Azure CLI
<a id="manage-batch-resources-with-azure-cli" class="xliff"></a>

Rozhraní příkazového řádku Azure CLI 2.0 představuje nové prostředí příkazového řádku Azure pro správu prostředků Azure. Je možné používat ho v systémech macOS, Linux a Windows. Rozhraní příkazového řádku Azure CLI 2.0 je optimalizováno pro správu prostředků Azure z příkazového řádku. Rozhraní příkazového řádku Azure CLI můžete používat ke správě účtů služby Azure Batch a ke správě prostředků, jako jsou fondy, úlohy a úkoly. V rozhraní příkazového řádku Azure CLI můžete používat skripty pro mnoho stejných úkolů, které se provádějí prostřednictvím rozhraní API služby Batch, webu Azure Portal a rutin PowerShellu služby Batch.

Tento článek obsahuje přehled používání rozhraní [Azure CLI verze 2.0](https://docs.microsoft.com/cli/azure/overview) se službou Batch. V článku [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) najdete přehled používání rozhraní příkazového řádku CLI s Azure.

Společnost Microsoft doporučuje používat nejnovější verzi rozhraní příkazového řádku Azure CLI – verzi 2.0. Další informace o verzi 2.0 najdete v článku [Příkazový řádek Azure 2.0 je nyní veřejně k dispozici](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/).

## Instalace rozhraní příkazového řádku Azure CLI
<a id="set-up-the-azure-cli" class="xliff"></a>

Pokud chcete nainstalovat rozhraní příkazového řádku Azure CLI, postupujte podle kroků uvedených v článku [Instalace rozhraní příkazového řádku Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli.md).

> [!TIP]
> Instalaci rozhraní příkazového řádku Azure CLI doporučujeme často aktualizovat, abyste mohli využívat výhody, které vám přinášejí aktualizace a vylepšení služby.
> 
> 

## Nápověda k příkazům
<a id="command-help" class="xliff"></a>

Pro každý příkaz v rámci rozhraní příkazového řádku Azure CLI můžete zobrazit nápovědu, pokud za název příkazu přidáte parametr `-h`. Jiné parametry vynechejte. Například:

* Pokud chcete zobrazit nápovědu pro příkaz `az`, zadejte: `az -h`
* Pokud chcete vypsat seznam všech příkazů Batch v rámci rozhraní příkazového řádku, zadejte: `az batch -h`
* Pokud chcete získat nápovědu k vytvoření účtu Batch, zadejte: `az batch account create -h`

Pokud si nejste jisti, pomocí parametru příkazového řádku `-h` můžete zobrazit nápovědu pro kterýkoli příkaz rozhraní příkazového řádku Azure CLI.

> [!NOTE]
> Starší verze rozhraní příkazového řádku Azure CLI používaly před příkazy CLI příznak `azure`. Ve verzi 2.0 začínají všechny příkazy příznakem `az`. Nezapomeňte aktualizovat skripty, aby u verze 2.0 používaly novou syntaxi.
>
>  

Kromě toho si prohlédněte referenční dokumentaci rozhraní příkazového řádku Azure CLI, kde najdete podrobnosti o [příkazech rozhraní příkazového řádku Azure CLI pro službu Batch](https://docs.microsoft.com/cli/azure/batch). 

## Přihlášení a ověření
<a id="log-in-and-authenticate" class="xliff"></a>

Pokud chcete používat rozhraní příkazového řádku Azure CLI se službou Batch, potřebujete se přihlásit a provést ověření. Je třeba provést dva jednoduché kroky:

1. **Přihlaste se k Azure.** Přihlášení k Azure vám umožní přístup k příkazům správce Azure Resource Manager včetně příkazů [služby Batch Management](batch-management-dotnet.md).  
2. **Přihlaste se ke svému účtu Batch.** Přihlášení k účtu Batch vám umožní přístup k příkazům služby Batch.   

### Přihlášení k Azure
<a id="log-in-to-azure" class="xliff"></a>

Existuje několik různých způsobů přihlášení k Azure, které jsou podrobně popsány v článku [Přihlášení pomocí Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):

1. [Interaktivní přihlášení](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in): Přihlaste se interaktivně, pokud spouštíte příkazy rozhraní příkazového řádku Azure CLI sami z příkazového řádku.
2. [Přihlášení pomocí instančního objektu](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal): Pokud spouštíte příkazy rozhraní příkazového řádku Azure CLI ze skriptu nebo aplikace, přihlaste se pomocí instančního objektu.

Pro účely tohoto článku vám ukážeme, jak se k Azure přihlásit interaktivně. V příkazovém řádku napište [az login](https://docs.microsoft.com/cli/azure/#login):

```azurecli
# Log in to Azure and authenticate interactively.
az login
```

Příkaz `az login` vrátí token, který budete používat k ověření, jak je vidět zde. Postupem podle zobrazených pokynů otevřete webovou stránku a odešlete token do služby Azure:

![Přihlášení k Azure](./media/batch-cli-get-started/az-login.png)

Příklady uvedené v části [Ukázkové skripty prostředí](#sample-shell-scripts) také ukazují, jak spustit relaci rozhraní příkazového řádku Azure CLI pomocí interaktivního přihlášení k Azure. Jakmile se přihlásíte, můžete volat příkazy pro práci s prostředky služby Batch Management včetně účtů Batch, klíčů, balíčků aplikací a kvót.  

### Přihlášení k účtu Batch
<a id="log-in-to-your-batch-account" class="xliff"></a>

Pokud chcete rozhraní příkazového řádku Azure CLI používat ke správě prostředků služby Batch, jako jsou fondy, úlohy a úkoly, musíte se přihlásit k účtu Batch a provést ověření. Ke službě Batch se přihlásíte pomocí příkazu [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login). 

Máte dvě možnosti ověření proti účtu Batch:

- **Ověření pomocí služby Azure Active Directory (Azure AD)** 

    Ověření pomocí služby Azure AD je výchozí možností, pokud používáte rozhraní příkazového řádku Azure CLI se službou Batch. Tuto možnost doporučujeme pro většinu scénářů. 
    
    Při interaktivním přihlášení k Azure, které je popsáno v předchozí části, jsou vaše přihlašovací údaje uloženy v mezipaměti, takže rozhraní příkazového řádku Azure CLI vás může přihlásit k účtu Batch pomocí stejných přihlašovacích údajů. Pokud se k Azure přihlásíte pomocí instančního objektu, použijí se tyto přihlašovací údaje také k přihlášení k účtu Batch.

    Výhoda služby Azure AD je, že nabízí řízení přístupu na základě role (RBAC). Při řízení přístupu na základě role (RBAC) závisí přístup uživatelů na jejich přiřazené roli, a ne na tom, jestli mají nebo nemají klíče účtu. Místo správy klíčů účtu můžete spravovat role RBAC a nechat řízení přístupu a ověřování na službě Azure AD.  

    Ověřování pomocí služby Azure AD je vyžadováno, pokud jste při vytvoření účtu služby Azure Batch nastavili pro Režim přidělování fondů možnost Předplatné uživatele. 

    Pokud se chcete přihlásit k účtu Batch pomocí služby Azure AD, použijte volání příkazu [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login): 

    ```azurecli
    az batch account login -g myresource group -n mybatchaccount
    ```

- **Ověření pomocí sdíleného klíče**

    [Ověření pomocí sdíleného klíče](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) používá k ověření příkazů rozhraní příkazového řádku Azure CLI pro službu Batch klíče pro přístup k účtu.

    Pokud vytváříte skripty rozhraní příkazového řádku Azure CLI k automatizaci volání příkazů služby Batch, můžete použít buď ověření pomocí sdíleného klíče, anebo instanční objekt služby Azure AD. V některých scénářích může být ověření pomocí sdíleného klíče jednodušší než vytvoření instančního objektu.  

    Pokud se chcete přihlásit s ověřením pomocí sdíleného klíče, zahrňte v příkazovém řádku možnost `--shared-key-auth`:

    ```azurecli
    az batch account login -g myresourcegroup -n mybatchaccount --shared-key-auth
    ```

Příklady uvedené v části [Ukázkové skripty prostředí](#sample-shell-scripts) ukazují, jak se k účtu Batch přihlásit v rozhraní příkazového řádku Azure CLI jak pomocí služby Azure AD, tak i pomocí sdíleného klíče.

## Ukázkové skripty prostředí
<a id="sample-shell-scripts" class="xliff"></a>

Ukázkové skripty uvedené v následující tabulce ukazují, jak provádět běžné úkoly pomocí rozhraní příkazového řádku Azure CLI se službou Batch a službou Batch Management. Tyto ukázkové skripty zahrnují řadu příkazů, které jsou k dispozici v rozhraní příkazového řádku Azure CLI pro službu Batch. 

| Skript | Poznámky |
|---|---|
| [Vytvoření účtu služby Batch](./scripts/batch-cli-sample-create-account.md) | Vytvoří účet Batch a přidruží ho k účtu úložiště. |
| [Přidání aplikace](./scripts/batch-cli-sample-add-application.md) | Přidá aplikaci a nahraje zabalené binární soubory.|
| [Správa fondů služby Batch](./scripts/batch-cli-sample-manage-pool.md) | Ukazuje vytváření, změny velikosti a správu fondů. |
| [Spuštění úlohy a úkolů pomocí služby Batch](./scripts/batch-cli-sample-run-job.md) | Ukazuje spuštění úlohy a přidávání úkolů. |

## Soubory JSON pro vytváření prostředků
<a id="json-files-for-resource-creation" class="xliff"></a>

Při vytváření prostředků Batch, jako jsou fondy a úlohy, můžete určit soubor JSON obsahující konfiguraci nového prostředku namísto předávání jejích parametrů v podobě parametrů příkazového řádku. Například:

```azurecli
az batch pool create my_batch_pool.json
```

Ačkoli mnoho prostředků služby Batch můžete vytvářet pouze prostřednictvím parametrů příkazového řádku, některé funkce vyžadují, abyste určili soubor ve formátu JSON obsahující podrobnosti o prostředku. Soubor JSON je například třeba použít, pokud chcete určit soubory prostředků pro úkol při spuštění.

Pokud si chcete prohlédnout syntaxi souboru JSON vyžadovanou k vytvoření prostředku, prostudujte si dokumentaci [Reference k rozhraní REST API služby Batch][rest_api]. Každé z témat Přidání *typ prostředku* v dokumentaci Reference k rozhraní REST API obsahuje ukázkové skripty JSON pro vytvoření příslušného prostředku. Tyto ukázkové skripty JSON můžete použít jako šablony pro soubory JSON a používat je v rozhraní příkazového řádku Azure CLI. Pokud si například chcete prohlédnout syntaxi skriptu JSON pro vytvoření fondu, podívejte se na téma [Přidání fondu k účtu][rest_add_pool].

Ukázkový skript, který určuje soubor JSON, najdete v článku [Spuštění úlohy a úkolů pomocí služby Batch](./scripts/batch-cli-sample-run-job.md).

> [!NOTE]
> Pokud při vytváření prostředku určíte soubor JSON, budou všechny ostatní parametry zadané v příkazovém řádku pro příslušný prostředek ignorovány.
> 
> 

## Efektivní dotazy na prostředky služby Batch
<a id="efficient-queries-for-batch-resources" class="xliff"></a>

Každý typ prostředku Batch podporuje příkaz `list`, který zadá dotaz na účet Batch a vypíše seznam prostředků příslušného typu. Můžete například vypsat seznam fondů v rámci vašeho účet a seznam úkolů v rámci úloh:

```azurecli
az batch pool list
az batch task list --job-id job001
```

Při dotazování služby Batch pomocí operace `list` můžete určit klauzuli OData a omezit tak množství vrácených dat. Protože veškeré filtrování probíhá na straně serveru, přenášejí se pouze data, která požadujete. Při provádění operací vypsání seznamu můžete pomocí těchto klauzulí snížit využití šířky pásma (a tedy čas).

Následující tabulka popisuje klauzule OData podporované službou Batch:

| Klauzule | Popis |
|---|---|
| `--select-clause [select-clause]` | Vrátí podmnožinu vlastností pro každou entitu. |
| `--filter-clause [filter-clause]` | Vrátí pouze ty entity, které odpovídají zadanému výrazu OData. |
| `--expand-clause [expand-clause]` | Získá informace o entitách v rámci jediného základního volání REST. Klauzule expand v současné době podporuje pouze vlastnost `stats`. |

Ukázkový skript, který ukazuje, jak používat klauzuli OData, najdete v článku [Spuštění úlohy a úkolů pomocí služby Batch](./scripts/batch-cli-sample-run-job.md).

Další informace o provádění efektivních dotazů pomocí příkazu list s klauzulemi OData najdete v článku [Efektivní dotazování na službu Azure Batch](batch-efficient-list-queries.md).

## Rady pro řešení potíží
<a id="troubleshooting-tips" class="xliff"></a>

Následující tipy mohou pomoci při řešení potíží s rozhraním příkazového řádku Azure CLI:

* Použijte parametr `-h` k získání **textu nápovědy** pro kterýkoli příkaz rozhraní příkazového řádku CLI.
* Pomocí parametrů `-v` a `-vv` zobrazíte **podrobný** výstup příkazu. Pokud zahrnete příznak `-vv`, zobrazí rozhraní příkazového řádku Azure CLI příslušné požadavky a odpovědi služby REST. Tyto přepínače jsou užitečné pro zobrazení úplného chybového výstupu.
* Pomocí parametru `--json` můžete zobrazit **výstup příkazu ve formátu JSON**. Příkaz `az batch pool show pool001 --json` například zobrazí vlastnosti fondu pool001 ve formátu JSON. Pak můžete tento výstup zkopírovat a upravit pro použití v příkazu `--json-file` (viz [soubory JSON](#json-files) dříve v tomto článku).
* [Fórum služby Batch][batch_forum] je sledováno členy týmu služby Batch. Pokud narazíte na problémy nebo hledáte pomoc s konkrétní operací, můžete tu uveřejnit své otázky.

## Další kroky
<a id="next-steps" class="xliff"></a>

* Další informace o rozhraní příkazového řádku Azure CLI najdete v [dokumentaci k rozhraní příkazového řádku Azure CLI](https://docs.microsoft.com/cli/azure/overview).
* Další informace o prostředcích služby Batch najdete v článku [Přehled služby Azure Batch pro vývojáře](batch-api-basics.md).
* V článku [Nasazení aplikace pomocí balíčků aplikací v Azure Batch](batch-application-packages.md) se dozvíte, jak lze tuto funkci použít ke správě a nasazení aplikací, které spouštíte ve výpočetních uzlech Batch.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx

