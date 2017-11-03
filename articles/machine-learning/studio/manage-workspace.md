---
title: "Spravovat pracovní prostor Machine Learning | Microsoft Docs"
description: "Správa přístupu k Azure Machine Learning pracovní prostory a nasadit a spravovat rozhraní API pro ML webové služby"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 2f90234bdd5c917a502d24cd16256bc11c7fbed0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Správa pracovního prostoru Azure Machine Learning

> [!NOTE]
> Informace týkající se správy webové služby v portálu webové služby Machine Learning najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](manage-new-webservice.md).
> 
> 

Machine Learning pracovní prostory v portálu Azure nebo portálu Azure classic můžete spravovat.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Použití webu Azure Portal

Správa prostoru na portálu Azure:

1. Přihlaste se k [portál Azure](https://portal.azure.com/) pomocí účtu správce předplatného Azure.
2. Do vyhledávacího pole v horní části stránky, zadejte "počítač pracovní prostory learning" a potom vyberte **Machine Learning pracovních prostorů**.
3. Klikněte na pracovní prostor, který chcete spravovat.

Kromě informací o správu standardní prostředku a možnosti, které jsou k dispozici můžete:

- Zobrazení **vlastnosti** – Tato stránka zobrazuje informace o pracovním prostoru a prostředků, a můžete změnit předplatném nebo skupině prostředků, tento pracovní prostor je propojená s.
- **Nové synchronizace klíčů k úložišti** -pracovním prostoru udržuje klíče k účtu úložiště. Pokud účet úložiště změny klíčů, pak můžete kliknout na **nové synchronizace klíče** synchronizovat klíče s pracovním prostoru.

Ke správě webových služeb přidružený tento pracovní prostor, použití portálu webové služby Machine Learning. V tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](manage-new-webservice.md) úplné informace.

> [!NOTE]
> Nasadit nebo spravovat nové webové služby musí mít přiřazenou roli Přispěvatel nebo správce na předplatné, která je nasazena webová služba. Pokud můžete pozvat jiného uživatele na pracovní prostor machine learning, musíte je přiřadit k roli Přispěvatel nebo správce na předplatné, než můžete nasadit nebo spravovat webové služby. 
> 
>Další informace o nastavení oprávnění přístupu najdete v tématu [zobrazení přiřazení přístupu pro uživatele a skupiny na portálu Azure – ve verzi Public preview](../../active-directory/role-based-access-control-manage-assignments.md).

## <a name="use-the-azure-classic-portal"></a>Použití portálu Azure classic

Pomocí portálu Azure classic, můžete spravovat vaše Machine Learning pracovních prostorů můžete:

* Sledování využití pracovním prostoru
* Nakonfigurovat v pracovním prostoru povolí nebo odepře přístup
* Správa webové služby vytvořené v pracovním prostoru
* Odstranit pracovní prostor

Kromě toho řídicí panel poskytuje přehled vaší využití prostoru a rychlý přehled informací pracovního prostoru.  

> [!TIP]
> V nástroji Azure Machine Learning Studio na **webové služby** kartě, můžete přidat, aktualizovat nebo odstranit Machine Learning webové služby.
> 
> 

Správa prostoru na portálu Azure classic:

1. Přihlaste se k [portál Azure classic](https://manage.windowsazure.com/) pomocí Microsoft Azure účet – použijte účet, který je spojen s předplatným služby Azure.
2. Na panelu služby Microsoft Azure, klikněte na tlačítko **MACHINE LEARNING**.
3. Klikněte na pracovní prostor, který chcete spravovat.

Stránka pracovní prostor obsahuje tři karty:

* **Řídicí panel** -umožňuje zobrazení využití prostoru a informace
* **KONFIGURACE** -umožňuje spravovat přístup k pracovním prostoru
* **WEBOVÉ služby** -vám umožní spravovat webové služby, které byly publikovány z tohoto pracovního prostoru

### <a name="to-monitor-how-the-workspace-is-being-used"></a>Ke sledování, jak je používán pracovním prostoru
Klikněte **řídicí panel** kartě.

Na řídicím panelu můžete zobrazit celkový použití pracovního prostoru a získat rychlý přehled informací pracovního prostoru.

* **Výpočetní** graf znázorňuje výpočetní prostředky, které používá pracovní prostor. Můžete změnit zobrazení relativní nebo absolutní hodnoty a můžete změnit v grafu časový rámec.
* **Přehled využití** zobrazí používá pracovním prostoru úložiště Azure.
* **Rychlého přehledu** poskytuje souhrnné informace o pracovním prostoru a užitečné odkazy.

> [!NOTE]
> **Přihlášení k ML Studio** odkaz otevře Machine Learning Studio pomocí Account Microsoft jste aktuálně přihlášeni. Account Microsoft, který jste použili pro přihlášení k portálu Azure classic k vytvoření pracovního prostoru automaticky nemá oprávnění k otevření tohoto pracovního prostoru. Otevřete pracovní prostor, musíte být přihlášeni k Account Microsoft, která byla definována jako vlastník pracovního prostoru, nebo je potřeba přijmout pozvánku od vlastníka pro připojení k pracovním prostoru.
> 
> 

### <a name="to-grant-or-suspend-access-for-users"></a>Udělení nebo pozastavit přístup pro uživatele
Klikněte **konfigurace** kartě.

Na kartě konfigurace můžete:

* Přístup do pracovního prostoru Machine Learning pozastavte kliknutím na tlačítko zakázat. Uživatelé už nebude moct v nástroji Machine Learning Studio otevřete pracovní prostor. Chcete-li obnovit přístup, klikněte na tlačítko Povolit.

Chcete-li spravovat další účty, kteří mají přístup do pracovního prostoru v nástroji Machine Learning Studio, klikněte na tlačítko **přihlášení k ML Studio** v **řídicí panel** karta (viz poznámka předcházející ohledně **přihlášení k ML Studio**). Otevře se v pracovním prostoru v nástroji Machine Learning Studio. Zde klikněte na tlačítko **nastavení** kartu a potom **uživatelé**. Můžete kliknout na **POZVAT uživatele více** uživatelům přístup k pracovním prostoru, nebo vyberte uživatele a klikněte na tlačítko **odebrat**.

### <a name="to-manage-web-services-in-this-workspace"></a>Ke správě webových služeb v tomto pracovním prostoru
Klikněte **webové služby** kartě.

Zobrazí se seznam webových služeb publikována z tohoto pracovního prostoru.
Chcete-li spravovat webové služby, klikněte na název v seznamu a otevřete stránku webové služby.

Webové služby může mít jeden nebo více koncové body definované.

* Můžete definovat další koncové body kromě "Výchozí" koncový bod. Chcete-li přidat koncový bod, klikněte na tlačítko **spravovat koncové body** v dolní části řídicího panelu webové služby Azure Machine Learning portál otevřít.
* Chcete-li odstranit koncový bod (nejde odstranit koncový bod "Výchozí"), klikněte na zaškrtávací políčko na začátku řádku koncový bod a klikněte na tlačítko **odstranit**. Tím se odebere koncový bod z webové služby.
  
  > [!NOTE]
  > Pokud aplikace používá koncový bod webové služby se odstraní koncový bod, aplikace dojde k chybě při příštím pokusu o přístup ke službě.
  > 
  > 

Klikněte na název koncový bod webové služby a ten se otevře. 

Na řídicím panelu můžete zobrazit celkové využití webové služby v časovém intervalu. Můžete vybrat období zobrazení období rozevírací nabídce v pravé horní části grafy využití. Řídicí panel zobrazuje následující informace:

* **Požadavky v čase** zobrazí graf krok počet požadavků za vybrané časové období. Může pomoct určit, zda dochází k špičky využití.
* **Požadavky požadavků a odpovědí** zobrazí celkový počet požadavků a odpovědí volání, které Služba obdržela přes zvolené časové období a kolik z nich se nezdařilo.
* **Průměrná doba výpočetní požadavků a odpovědí** Průměrná doba potřebná k provedení přijatých požadavků.
* **Požadavky služby batch** zobrazí celkový počet dávkových žádostí, které Služba obdržela přes zvolené časové období a kolik z nich se nezdařilo.
* **Průměrná latence úlohy** Průměrná doba potřebná k provedení přijatých požadavků.
* **Chyby** zobrazí souhrnný počet chyb, k nimž došlo v volání webové služby.
* **Služby náklady** zobrazí poplatky za fakturační plán spojené s touto službou.

Na stránce konfigurace můžete aktualizovat následující vlastnosti:

* **Popis** můžete zadat popis pro webovou službu. Popis je povinné pole.
* **Protokolování** umožňuje povolit nebo zakázat protokolování na koncovém bodu. Další informace o protokolování naleznete v tématu Povolení [protokolování pro webové služby Machine Learning](web-services-logging.md).
* **Povolit ukázková data** umožňuje poskytovat ukázková data, která můžete použít k testování služby požadavků a odpovědí. Pokud jste vytvořili webovou službu v nástroji Machine Learning Studio, ukázkových dat je převzat ze data vaší použité k natrénování modelu. Pokud jste vytvořili službu prostřednictvím kódu programu, data je převzat ze příklad dat, které jste zadali jako součást balíčku JSON.

