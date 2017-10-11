---
title: "Správa účtu úložiště StorSimple | Microsoft Docs"
description: "Vysvětluje, jak můžete použít stránku konfigurace StorSimple Manager můžete přidávat, upravovat, odstraňovat nebo otočit zabezpečení klíče pro účet úložiště."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 93207c40-e0eb-489e-8724-59fb94907081
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/29/2016
ms.author: v-sharos
ms.openlocfilehash: 68b767c9c93f2daff476a21029b9813f347590b5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>Použít službu StorSimple Manager ke správě vašeho účtu úložiště
## <a name="overview"></a>Přehled
**Konfigurace** stránky uvede všechny parametry globální služby, které lze vytvořit ve službě StorSimple Manager. Tyto parametry lze použít pro všechna zařízení připojená k službu a zahrnují:

* Účty úložiště 
* Šablony šířky pásma 
* Záznamy řízení přístupu 

Tento kurz popisuje, jak můžete použít **konfigurace** stránky Pokud chcete přidat, upravit nebo odstranit účty úložiště nebo otočit zabezpečení klíče pro účet úložiště.

 ![Konfigurace stránky](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Účty úložiště obsahovat přihlašovací údaje, které zařízení používá pro přístup k účtu úložiště pomocí poskytovatele cloudových služeb. Pro účty úložiště Microsoft Azure jsou tyto přihlašovací údaje, jako je například název účtu a primární přístupový klíč. 

Na **konfigurace** stránky, všech účtů úložišť, které jsou vytvořené pro fakturace předplatného se zobrazí v tabulkovém formátu, který obsahuje následující informace:

* **Název** – jedinečný název přiřazen k účtu, pokud byla vytvořena.
* **Povoleno zabezpečení SSL** – jestli SSL je povolená a komunikace zařízení cloud je přes zabezpečený kanál.
* **Používá** – počet svazků pomocí účtu úložiště.

Běžné úkoly související s účty úložiště, které lze provést na **konfigurace** jsou stránky:

* Přidání účtu úložiště 
* Upravit účet úložiště 
* Odstranění účtu úložiště 
* Střídání klíče účtů úložiště 

## <a name="types-of-storage-accounts"></a>Typy účtů úložiště
Existují tři typy účtů úložiště, které lze použít s zařízení StorSimple.

* **Účty úložiště automaticky generovaný** – jak již název naznačuje, tento typ účtu úložiště se automaticky generuje při prvním vytvoření služby. Další informace o tom, jak vytvořit účet úložiště, najdete v části [krok 1: vytvoření nové služby](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) v [nasazení místního zařízení StorSimple](storsimple-deployment-walkthrough.md). 
* **Účty úložiště v předplatném služby** – Toto jsou účty úložiště Azure, které jsou přidruženy ke stejnému předplatnému jako službu. Další informace o tom, jak tyto úložiště vytvoření účtů, najdete v části [o účtech úložiště Azure](../storage/common/storage-create-storage-account.md). 
* **Účty úložiště mimo předplatné služby** – Toto jsou účty úložiště Azure, které nejsou přidružené k vaší službě a pravděpodobně existovala předtím, než byla služba vytvořena.

## <a name="add-a-storage-account"></a>Přidání účtu úložiště
Můžete přidat účet úložiště tím, že poskytuje jedinečný popisný název a přihlašovací údaje přístup, které jsou propojeny s účet úložiště (s poskytovateli zadané cloudové služby). Máte také možnost povolení režimu zabezpečené sockets layer (SSL) vytvořte zabezpečený kanál pro síťovou komunikaci mezi vaším zařízením a cloudem.

Můžete vytvořit více účtů pro daný cloud poskytovatele služeb. Upozorňujeme ale, že po vytvoření účtu úložiště nemůžete změnit poskytovatele cloudové služby.

Během ukládání je účet úložiště, služba se pokouší komunikovat s poskytovatele cloudových služeb. V tuto chvíli se ověřit přihlašovací údaje a řádného materiálu přístupu, kterou jste zadali. Účet úložiště se vytvoří pouze v případě, že je ověřování úspěšné. Pokud se ověřování nezdaří, se zobrazí příslušná chybová zpráva.

Účty úložiště správce prostředků vytvořené na portálu Azure jsou podporovány také StorSimple. Účty správce prostředků úložiště nezobrazí v rozevíracím seznamu pro výběr při pokusu o vytvoření kontejneru svazků, jenom účty úložiště vytvořené na portálu Azure classic se zobrazí. Účty správce prostředků úložiště bude třeba přidat pomocí postupu přidání účtu úložiště popsané dole.

> [!NOTE]
> Postup pro přidání účtu úložiště se liší podle verze softwaru zařízení StorSimple, kterou používáte. Ujistěte se, postupujte podle pokynů správné pro vaši verzi zařízení StorSimple.
> 
> 

[!INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[!INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Upravit účet úložiště
Můžete upravit účet úložiště, který je používán kontejner svazků. Pokud chcete upravit účet úložiště, který se právě používá, je možné upravit pouze pole přístupový klíč pro účet úložiště. Můžete zadat nový přístupový klíč úložiště a uložte aktualizovaná nastavení.

#### <a name="to-edit-a-storage-account"></a>Chcete-li upravit účet úložiště
1. Na stránce cílové služby vyberte svoji službu, dvakrát klikněte na název služby a pak klikněte na tlačítko **konfigurace**.
2. Klikněte na tlačítko **přidat či upravit účty úložiště**.
3. V **účty úložiště, přidat či upravit** dialogové okno:
   
   1. V rozevíracím seznamu **účty úložiště**, zvolte existující účet, který chcete upravit. To může zahrnovat účty úložiště, které byly automaticky generovány při služby byl vytvořen.
   2. Pokud potřeby můžete změnit **povolit režim SSL** výběr.
   3. Můžete otočit klíče pro přístup k účtu úložiště. V tématu [klíče otočení účtů úložiště](#key-rotation-of-storage-accounts) Další informace o tom, jak provést střídání klíče.
   4. Klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) uložte nastavení. Nastavení budou aktualizovány na **konfigurace** stránky. Klikněte na tlačítko **Uložit** uložte nově aktualizovaných nastavení.
      
      ![Upravit účet úložiště](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)

## <a name="delete-a-storage-account"></a>Odstranění účtu úložiště
> [!IMPORTANT]
> Účet úložiště můžete odstranit pouze v případě, že není používán kontejner svazků. Pokud účet úložiště je používán kontejner svazků, nejprve odstranit kontejner svazků a pak odstraňte přidruženého účtu úložiště.
> 
> 

#### <a name="to-delete-a-storage-account"></a>Chcete odstranit účet úložiště
1. Na stránce Cílová služba StorSimple Manager vyberte svoji službu, dvakrát klikněte na název služby a pak klikněte na tlačítko **konfigurace**.
2. V tabulkovém seznam účtů úložiště najeďte myší na účet, který chcete odstranit.
3. Ikony odstranění (**x**) se zobrazí v pravém sloupci extrémně pro daný účet úložiště. Klikněte **x** ikonu Odstranit přihlašovací údaje.
4. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** Chcete-li pokračovat v odstraňování. Tabulkové výpis budou aktualizovány tak, aby odrážely změny.

## <a name="key-rotation-of-storage-accounts"></a>Střídání klíče účtů úložiště
Z bezpečnostních důvodů střídání klíčů je často požadavek v datových centrech. 

> [!NOTE]
> Následující klíče otočení informace a postup střídání použít jenom účty úložiště Microsoft Azure. Pokud používáte jiné poskytovatele cloudové služby, můžete spravovat klíče účtu úložiště prostřednictvím řídicího panelu tohoto zprostředkovatele.
> 
> 

Každé předplatné Microsoft Azure může mít jeden nebo více účtů úložiště. Přístup do těchto účtů řídí předplatného a přístupových klíčů pro každý účet úložiště. 

Při vytváření účtu úložiště vygeneruje Microsoft Azure dva 512bitové přístupové klíče k úložišti, které se používají pro ověření při přístupu k účtu úložiště. Máte dva přístupové klíče k úložišti, můžete vygenerovat klíče bez přerušení poskytování vaší služby úložiště nebo přístupu k této službě. Klíč, který se právě používá, je *primární* a zálohování klíč se označuje jako *sekundární* klíč. Jeden z těchto dvou klíčů musí zadat, když zařízení s Microsoft Azure StorSimple přistupuje k poskytovatele cloudových služeb úložiště.

## <a name="what-is-key-rotation"></a>Co je střídání klíčů?
Obvykle se aplikace používají pouze jeden z klíčů přistupovat k datům. Po určité době dobu může mít vaše aplikace přepnout na používání druhý klíč. Poté, co jste přepnuli aplikace k sekundární klíč, můžete vyřadit z provozu první klíč a pak vygenerovat nový klíč. Pomocí dvou klíčů tímto způsobem umožňuje vaší aplikace přístup k datům, aniž by docházelo k výpadkům.

Klíče účtu úložiště jsou vždy uloženy ve službě v šifrovaném formátu. To však mohou obnovit přes službu StorSimple Manager. Služby mohou získat primární klíč a sekundární klíč pro všechny účty úložiště ve stejném předplatném, včetně účtů vytvořených v rámci služby úložiště a také výchozí účty úložiště vygeneruje, když byla poprvé služby StorSimple Manager vytvořit. Služba StorSimple Manager bude vždy získat tyto klíče z portálu Azure classic a uložit je do šifrované způsobem.

## <a name="rotation-workflow"></a>Otočení pracovního postupu
Microsoft Azure správce můžete znovu vygenerovat nebo změnit primární nebo sekundární klíč tak, že přímý přístup k účtu úložiště (prostřednictvím služby Microsoft Azure Storage). Služby StorSimple Manager automaticky nezná tuto změnu.

K informování služby StorSimple Manager změny, budete potřebovat přístup ke službě StorSimple Manager přístup k účtu úložiště a potom synchronizovat primární nebo sekundární klíč (podle toho, která byla změněna). Tato služba pak získá nejnovější klíč, zašifruje klíče a odešle zašifrovaný klíč do zařízení.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>K synchronizaci klíčů pro účty úložiště ve stejném předplatném jako služba (pouze Azure)
1. Na **služby** klikněte na tlačítko **konfigurace** kartě.
2. Klikněte na tlačítko **přidat či upravit účty úložiště**.
3. V dialogovém okně postupujte takto:
   
   1. Vyberte účet úložiště s klíčem, který chcete synchronizovat. Klíče účtu úložiště se šifrují, pokud jsou zobrazeny.
   2. Ve službě StorSimple Manager budete muset aktualizovat klíč, který byl dříve změněn ve službě Microsoft Azure Storage. Pokud primární přístupový klíč byl změněn (znova vygeneroval), klikněte na tlačítko **synchronizovat primární klíč**. Pokud se sekundární klíč byl změněn, klikněte na tlačítko **synchronizovat sekundární klíč**.
      
      ![Synchronizovat klíče](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>K synchronizaci klíčů pro účty úložiště mimo předplatné služby
1. Na **služby** klikněte na tlačítko **konfigurace** kartě.
2. Klikněte na tlačítko **přidat či upravit účty úložiště**.
3. V dialogovém okně postupujte takto:
   
   1. Vyberte účet úložiště s přístupový klíč, který chcete aktualizovat.
   2. Budete muset aktualizovat přístupový klíč úložiště ve službě StorSimple Manager. V takovém případě se zobrazí přístupový klíč úložiště. Zadejte nový klíč v **přístupový klíč účtu úložiště**y pole. 
   3. Uložte provedené změny. Nyní je třeba aktualizovat přístupový klíč účtu úložiště.

## <a name="next-steps"></a>Další kroky
* Další informace o [zabezpečení zařízení StorSimple](storsimple-security.md).
* Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

