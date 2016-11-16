---
title: "Služba Azure AD Domain Services: Vytvoření nebo výběr virtuální sítě | Dokumentace Microsoftu"
description: "Začínáme se službou Azure Active Directory Domain Services"
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
ms.date: 10/03/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 0617dbff2ba281091ba5775e7969edfead4d80c8


---
# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Vytvoření nebo výběr virtuální sítě pro Azure AD Domain Services
## <a name="guidelines-to-select-an-azure-virtual-network"></a>Pokyny k výběru virtuální sítě Azure
> [!NOTE]
> **Než začnete**: Přečtěte si článek [Důležité informace o sítích pro Azure AD Domain Services](active-directory-ds-networking.md).
> 
> 

## <a name="task-2-create-an-azure-virtual-network"></a>Úloha 2: Vytvořte virtuální síť Azure
Další úlohou konfigurace je vytvoření virtuální sítě Azure a podsítě v ní. V této podsíti v rámci své virtuální sítě povolíte službu Azure AD Domain Services. Pokud již máte existující virtuální síť, kterou chcete použít, můžete tento krok přeskočit.

> [!NOTE]
> Ujistěte se, že virtuální síť Azure, kterou vytváříte nebo si vyberete pro použití se službou Azure AD Domain Services patří do oblasti Azure podporované službou Azure AD Domain Services. Na stránce [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/) se dozvíte, pro jaké oblasti je dostupná služba Azure AD Domain Services.
> 
> 

Poznamenejte si název virtuální sítě, abyste při povolování služby Azure AD Domain Services v dalším kroku konfigurace vybrali správnou virtuální síť.

Proveďte následující kroky konfigurace a vytvořte virtuální síť Azure, ve které chcete povolit službu Azure AD Domain Services.

1. Přejděte do **portálu Azure Classic** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. V levém podokně vyberte uzel **Sítě**.
   
    ![Uzel sítí](./media/active-directory-domain-services-getting-started/networks-node.png)
3. Klikněte na **Nová** v podokně úloh ve spodní části stránky.
   
    ![Uzel virtuálních sítí](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. V uzlu **Síťové služby** vyberte možnost **Virtuální sít**.
5. Vytvořte virtuální síť kliknutím na **Rychle vytvořit**.
   
    ![Virtuální síť – rychle vytvořit](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
6. Zadejte **Název** vaší virtuální sítě. Pro tuto síť také můžete nakonfigurovat **Adresní prostor** nebo **Maximální počet virtuálních počítačů**. Prozatím můžete ponechat možnost **Server DNS** nastavenou na hodnotu „Žádný“. Nastavení serveru DNS můžete aktualizovat poté, co povolíte službu Azure AD Domain Services.
7. Ujistěte se, že vybíráte podporovanou oblast Azure z rozevíracího seznamu **Umístění**. Na stránce [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/) se dozvíte, pro jaké oblasti je dostupná služba Azure AD Domain Services.
8. Vytvořte virtuální síť kliknutím na tlačítko **Vytvořit virtuální síť**.
   
    ![Vytvoření virtuální sítě pro službu Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. Po vytvoření virtuální sítě vyberte příslušnou virtuální síť a klikněte na kartu **KONFIGUROVAT**.
   
    ![Vytvoření podsítě](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. Přejděte do sekce **adresních prostorů virtuální sítě**. Klikněte na možnost **přidat podsíť** a zadejte podsíť s názvem **AaddsSubnet**. Kliknutím na možnost **Uložit** vytvořte podsíť.
    
    ![Vytvoření podsítě pro službu Azure AD Domain Services](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

<br>

## <a name="task-3-enable-azure-ad-domain-services"></a>Úloha 3 – Povolení služby Azure AD Domain Services
Další úlohou konfigurace je [povolení služby Azure AD Domain Services](active-directory-ds-getting-started-enableaadds.md).




<!--HONumber=Nov16_HO2-->


