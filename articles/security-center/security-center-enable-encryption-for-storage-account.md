---
title: "Povolit šifrování pro účet úložiště v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak implementovat Azure Security Center doporučení ** povolit šifrování pro rozhraní Azure úložiště účet **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.openlocfilehash: b7b2e8a12cbab68da9c8fcc348e8e3c543607007
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a>Povolit šifrování pro účet úložiště Azure v Azure Security Center
Azure Security Center může doporučujeme povolit šifrování služby úložiště Azure pro data v klidovém stavu.

Šifrování služby úložiště (SSE) funguje tak, že šifrování dat, když je zapsán do úložiště Azure a dešifrování dat před načtení.  SSE aktuálně nejsou k dispozici pouze pro službu Azure Blob lze použít pro objekty BLOB bloku, objektů BLOB stránky a doplňovacích objektů BLOB.  Další informace najdete v tématu [šifrování služby úložiště pro data v klidovém stavu](../storage/common/storage-service-encryption.md).


> [!Note]
> Po povolení šifrování, se šifrují jenom nová data. Všechny existující objekty BLOB ve vašem účtu úložiště zůstat nezašifrovaný. K šifrování existující objekty BLOB, najdete v článku [nejčastější dotazy týkající se úložiště služby šifrování](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).
>
>

Šifrování služby úložiště je podporována pouze na účty úložiště Resource Manager. Klasické účty úložiště nejsou aktuálně podporovány. Abyste pochopili, classic a modelech nasazení Resource Manager, najdete v části [modelech nasazení Azure](../azure-classic-rm.md).

> [!NOTE]
> Tento dokument vám tuto službu představí formou ukázkového nasazení.  Tento dokument není to podrobný průvodce.
>
>

## <a name="implement-the-recommendation"></a>Implementace doporučení
1. V **doporučení** vyberte **povolit šifrování pro účet úložiště Azure**.
   ![Povolení šifrování pro účet úložiště][1]
2. **Povolit šifrování úložiště** otevře se okno. Toto okno obsahuje seznam účtů úložiště Azure, kde je zakázaný šifrování úložiště. V tomto příkladu budeme vyberte **storageacct1**.
   ![Povolit šifrování úložiště][2]
3. **Šifrování** okně **storageacct1** otevře. Vyberte **povoleno**.
   ![Okno šifrování][3]
4. Vyberte **Uložit**.

Nyní jste povolili šifrování úložiště pro **storageacct1**.


## <a name="see-also"></a>Viz také
Tento dokument vám ukázal, jak provést doporučení Security Center "Povolit šifrování pro účet služby Azure Storage." Další informace o šifrování služby úložiště Azure, naleznete v následujících tématech:

* [Šifrování služby úložiště Azure pro Data v klidovém stavu](../storage/common/storage-service-encryption.md)

Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití této služby.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
