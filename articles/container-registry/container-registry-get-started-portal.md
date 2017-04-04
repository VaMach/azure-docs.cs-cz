---
title: "Vytvoření soukromého registru Dockeru – Azure Portal | Dokumentace Microsoftu"
description: "Začínáme vytvářet a spravovat soukromé registry kontejnerů Dockeru pomocí webu Azure Portal"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: e74c5428f0e31d9d3cf06b85aa8cefde868e9d67
ms.lasthandoff: 03/27/2017

---

# <a name="create-a-private-docker-container-registry-using-the-azure-portal"></a>Vytvoření soukromého registru kontejnerů Dockeru pomocí webu Azure Portal
Pomocí webu Azure Portal můžete vytvořit registr kontejnerů a spravovat jeho nastavení. Vytvořit a spravovat registry kontejnerů můžete také pomocí [příkazů Azure CLI 2.0 ](container-registry-get-started-azure-cli.md) nebo programově pomocí rozhraní [REST API služby Container Registry](https://go.microsoft.com/fwlink/p/?linkid=834376).

Související informace a koncepty najdete v tématu [s přehledem](container-registry-intro.md).



## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů
1. Na webu [Azure Portal](https://portal.azure.com) klikněte na **+ Nový**.
2. V Marketplace vyhledejte **Azure container registry**.
3. Vyberte **Azure Container Registry** s vydavatelem **Microsoft**.
    ![Služba Container Registry v Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)
4. Klikněte na možnost **Vytvořit**. Otevře se okno **Azure Container Registry**.

    ![Nastavení registru kontejnerů](./media/container-registry-get-started-portal/container-registry-settings.png)
5. V okně **Azure Container Registry** zadejte následující informace. Až budete hotovi, klikněte na **Vytvořit**.

    a. **Název registru:** Globálně jedinečný název domény nejvyšší úrovně konkrétního registru. V tomto příkladu je název registru *myRegistry01*, ale nahraďte jej vlastním jedinečným názvem. Název může obsahovat pouze písmena a číslice.

    b. **Skupina prostředků:** Vyberte existující [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků.

    c. **Umístění:** Vyberte umístění datového centra Azure, ve kterém je tato služba [dostupná](https://azure.microsoft.com/regions/services/), například **Střed USA – jih**.

    d. **Uživatel s právy pro správu:** Pokud chcete, povolte uživateli s právy pro správu přístup k registru. Toto nastavení můžete po vytvoření registru změnit.

    > [!IMPORTANT]
    > Kromě poskytování přístupu prostřednictvím účtu uživatele s právy pro správu registry kontejnerů podporují ověřování zajišťované instančními objekty Azure Active Directory. Další informace a aspekty najdete v tématu [Ověřování pomocí registru kontejnerů](container-registry-authentication.md).


    e. **Účet úložiště:** Použijte výchozí nastavení a vytvořte si [účet úložiště](../storage/storage-introduction.md), nebo vyberte existující účet úložiště ve stejném umístění. Storage úrovně Premium se v tuto chvíli nepodporuje.


## <a name="manage-registry-settings"></a>Správa nastavení registru
Po vytvoření registru začněte v okně portálu **Registry kontejnerů** a vyhledejte nastavení registru. Nastavení můžete potřebovat například k přihlášení ke svému registru, nebo můžete chtít povolit nebo zakázat uživatele s právy pro správu.

1. V okně **Kontejnery registrů** klikněte na název svého registru.

    ![Okno registru kontejneru](./media/container-registry-get-started-portal/container-registry-blade.png)
2. Chcete-li spravovat nastavení přístupu, klikněte na **Přístupový klíč**.

    ![Přístup k registru kontejneru](./media/container-registry-get-started-portal/container-registry-access.png)
3. Všimněte si těchto nastavení:

   * **Přihlašovací server** – Plně kvalifikovaný název, který slouží k přihlášení k registru. V tomto příkladu je to `myregistry01.azurecr.io`.
   * **Uživatel s právy pro správu** – Tímto přepínačem můžete povolit nebo zakázat uživatelský účet s právy pro správu registru.
   * **Uživatelské jméno** a **Heslo** – Přihlašovací údaje uživatelského účtu s právy pro správu (pokud je povolený), pomocí kterých se můžete přihlásit k registru. Hesla můžete volitelně vygenerovat znovu. Vytvoří se dvě hesla, abyste mohli spravovat připojení k registru pomocí jednoho hesla, zatímco znovu vygenerujete druhé heslo. Pokud chcete naopak provést ověření pomocí instančního objektu, přečtěte si popis [ověření pomocí privátního registru kontejnerů Dockeru](container-registry-authentication.md).

## <a name="next-steps"></a>Další kroky
* [Nahrání první image pomocí rozhraní příkazového řádku Dockeru](container-registry-get-started-docker-cli.md)

