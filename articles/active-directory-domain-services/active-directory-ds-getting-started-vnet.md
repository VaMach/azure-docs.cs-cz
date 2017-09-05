---
title: "Služba Azure Active Directory Domain Services: Vytvoření nebo výběr virtuální sítě | Dokumentace Microsoftu"
description: "Povolení služby Azure Active Directory Domain Services pomocí portálu Azure Classic"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: maheshu
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: c0d3f90c0f15fbb7aad00fe24c2946738e24ffd8
ms.contentlocale: cs-cz
ms.lasthandoff: 08/29/2017

---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a>Vytvoření nebo výběr virtuální sítě pro Azure Active Directory Domain Services

> [!IMPORTANT]
> Prostředí portálu Azure Classic pro povolení služby Azure AD Domain Services ukázané v tomto článku bude brzy odebráno. Pro nová nasazení [**místo toho použijte nové (ve verzi Preview) prostředí webu Azure Portal**](active-directory-ds-getting-started.md).
>

## <a name="before-you-begin"></a>Než začnete
Přečtěte si článek [Důležité informace o sítích pro Azure Active Directory Domain Services](active-directory-ds-networking.md).

## <a name="task-2-create-an-azure-virtual-network"></a>Úloha 2: Vytvořte virtuální síť Azure
Další úlohou konfigurace je vytvoření virtuální sítě Azure a podsítě v ní. V této podsíti v rámci své virtuální sítě povolíte službu Azure Active Directory Domain Services. Pokud máte existující virtuální síť, kterou chcete použít, můžete tento krok přeskočit.

> [!NOTE]
> Ujistěte se, že virtuální síť Azure, kterou vytváříte nebo si vyberete pro použití se službou Azure Active Directory Domain Services, patří do oblasti Azure podporované službou Azure Active Directory Domain Services. Oblasti Azure, ve kterých je služba Azure Active Directory Domain Services k dispozici, najdete v článku [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).
>
>Poznamenejte si název virtuální sítě, abyste při povolování služby Azure Active Directory Domain Services v dalším kroku konfigurace zajistili výběr správné virtuální sítě.


Chcete-li vytvořit virtuální síť Azure, ve které chcete povolit Azure Active Directory Domain Services, postupujte podle těchto konfiguračních pokynů:

1. Přejděte na [portál Azure Classic](https://manage.windowsazure.com).
2. V levém podokně vyberte **Sítě**.

    ![Uzel sítí](./media/active-directory-domain-services-getting-started/networks-node.png)  
    Otevře se okno **Virtuální sítě**.
3. V podokně úloh ve spodní části okna klikněte na **Nová**.

    ![Okno Virtuální sítě](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. Klikněte na **Síťové služby** a vyberte možnost **Virtuální síť**.

    ![Virtuální síť – rychle vytvořit](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. Vytvořte virtuální síť kliknutím na **Rychle vytvořit**.

6. Zadejte **Název** své virtuální sítě a zvažte následující možnosti:
    * Pro tuto síť můžete nakonfigurovat **Adresní prostor** nebo **Maximální počet virtuálních počítačů**.
    * Prozatím můžete ponechat možnost **Server DNS** nastavenou na hodnotu **Žádný**. Nastavení můžete aktualizovat poté, co povolíte službu Azure Active Directory Domain Services.
7. Z rozevíracího seznamu **Umístění** vyberte podporovanou oblast Azure.  
    Oblasti Azure, ve kterých je služba Azure Active Directory Domain Services k dispozici, pro ověření najdete v článku [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).
8. Vytvořte virtuální síť kliknutím na **Vytvořit virtuální síť**.

    ![Vytvoření virtuální sítě pro Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. Po vytvoření virtuální sítě vyberte název virtuální sítě a pak klikněte na kartu **Konfigurovat**.

    ![Vytvoření podsítě](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. V části **adresních prostorů virtuální sítě** klikněte na **přidat podsíť**a pak zadejte podsíť s názvem **AaddsSubnet**.

    ![Vytvoření podsítě pro Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. Kliknutím na **Uložit** vytvořte podsíť.


## <a name="next-step"></a>Další krok
[Úloha 3: Povolení služby Azure Active Directory Domain Services](active-directory-ds-getting-started-enableaadds.md)

