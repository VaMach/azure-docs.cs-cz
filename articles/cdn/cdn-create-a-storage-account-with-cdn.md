---
title: "Účet úložiště Azure integrovat Azure CDN | Microsoft Docs"
description: "Další informace o použití sítě doručování obsahu (CDN) Azure do poskytovat širokopásmového obsahu díky ukládání do mezipaměti objektů blob z úložiště Azure."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.author: mazha
ms.openlocfilehash: 022071f7825cb9184bd4c815c09e1c202a0a6f91
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2018
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Účet úložiště Azure integrovat Azure CDN
Azure sítě pro doručování obsahu (CDN) můžete povolit obsah do mezipaměti na úložiště Azure. Azure CDN nabízí vývojářům globální řešení pro doručování širokopásmového obsahu. Ho může ukládat do mezipaměti objektů BLOB a statického obsahu výpočetní instance na fyzických uzlech v USA, Evropa, Asie, Austrálie a Jižní Ameriky.

## <a name="step-1-create-a-storage-account"></a>Krok 1: Vytvoření účtu úložiště
Použijte následující postup k vytvoření nového účtu úložiště pro předplatné Azure. Účet úložiště poskytuje přístup ke službám Azure Storage. Účet úložiště představuje nejvyšší úroveň oboru názvů pro přístup k jednotlivých součástí služby Azure Storage: úložiště Azure Blob, fronty a tabulky. Další informace najdete v tématu [Úvod do Microsoft Azure Storage](../storage/common/storage-introduction.md).

Pokud chcete vytvořit účet úložiště, musí být buď správce služby, nebo spolusprávce pro předplatné přidružený.

> [!NOTE]
> Chcete-li vytvořit účet úložiště, včetně portálu Azure a prostředí PowerShell můžete použít několik metod. Tento kurz ukazuje, jak pomocí portálu Azure.   
> 

**Chcete-li vytvořit účet úložiště pro předplatné Azure**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V levém horním rohu vyberte **vytvořit prostředek**. V **nový** podokně, vyberte **úložiště**a potom vyberte **účet úložiště – objekt blob, soubor, tabulka, fronta**.
    
    **Vytvořit účet úložiště** podokně se zobrazí.   

    ![Vytvoření podokně účtu úložiště](./media/cdn-create-a-storage-account-with-cdn/cdn-create-new-storage-account.png)

3. V **název** zadejte název subdomény. Tato položka může obsahovat 3 až 24 malých písmen a číslic.
   
    Tato hodnota se změní na název hostitele v rámci identifikátoru URI, který se používá k řešení objektů blob, fronty nebo tabulky prostředky pro předplatné. Chcete-li vyřešit kontejner prostředku v úložišti objektů Blob, používání identifikátoru URI v následujícím formátu:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*

    kde  *&lt;StorageAccountLabel&gt;*  odkazuje na hodnotu, která jste zadali v **název** pole.
   
    > [!IMPORTANT]    
    > Adresa URL popisku tvoří subdoménu dané účet úložiště URI a musí být jedinečný mezi všechny hostované služby v Azure.
   
    Tato hodnota se používá jako název účtu úložiště na portálu, nebo když tento účet chcete získat přístup prostřednictvím kódu programu.
    
4. Použijte výchozí hodnoty pro **modelu nasazení**, **účet druhu**, **výkonu**, a **replikace**. 
    
5. Pro **předplatné**, vyberte předplatné pro použití s účtem úložiště.
    
6. Pro **skupiny prostředků**vyberte nebo vytvořte skupinu prostředků. Informace o skupinách prostředků najdete v tématu [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).
    
7. Pro **umístění**, vyberte umístění vašeho účtu úložiště.
    
8. Vyberte **Vytvořit**. Proces vytvoření účtu úložiště může trvat několik minut na dokončení.

## <a name="step-2-enable-cdn-for-the-storage-account"></a>Krok 2: Povolení CDN pro účet úložiště

Můžete povolit CDN pro váš účet úložiště přímo z vašeho účtu úložiště. 

1. Vyberte účet úložiště z řídicího panelu a pak vyberte **Azure CDN** v levém podokně. Pokud **Azure CDN** není tlačítko okamžitě viditelné, můžete zadat CDN v **vyhledávání** pole v levém podokně.
    
    **Azure Content Delivery Network** podokně se zobrazí.

    ![Vytvoření koncového bodu CDN](./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png)
    
2. Vytvořte nový koncový bod tak, že zadáte požadované informace:
    - **Profil CDN**: vytvoření nového profilu CDN nebo použít stávající profil CDN.
    - **Cenová úroveň**: Vyberte cenovou úroveň, pouze v případě, že vytvoříte profil CDN.
    - **Název koncového bodu CDN**: Zadejte název koncového bodu CDN.

    > [!TIP]
    > Ve výchozím nastavení používá nový koncový bod CDN název hostitele vašeho účtu úložiště jako zdrojový server.

3. Vyberte **Vytvořit**. Po vytvoření koncového bodu se zobrazí v seznamu koncových bodů.

    ![Úložiště nový koncový bod CDN](./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png)

> [!NOTE]
> Pokud chcete určit pokročilou konfiguraci nastavení pro koncový bod CDN, jako je například typ optimalizace, můžete místo toho použít [rozšíření Azure CDN](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint) k vytvoření koncového bodu CDN, nebo profil CDN.

## <a name="step-3-enable-additional-cdn-features"></a>Krok 3: Povolení dalších funkcí CDN

Z účtu úložiště **Azure CDN** podokně, vyberte ze seznamu a otevřete podokno CDN konfigurace koncového bodu CDN. Další funkce CDN můžete povolit pro doručení, například komprese, řetězec dotazu a geografická filtrování. Můžete také přidat mapování vlastní doménu pro koncový bod CDN a povolit HTTPS pro vlastní doménu.
    
![Konfigurace koncového bodu CDN úložiště](./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png)

## <a name="step-4-access-cdn-content"></a>Krok 4: Přístup k obsahu CDN
Chcete-li získat přístup k obsahu v mezipaměti na CDN, použijte CDN adresy URL poskytnuté na portálu. Adresy pro v mezipaměti objektů blob má následující formát:

http://<*EndpointName*\>.azureedge.net/<*myPublicContainer*\>/<*BlobName*\>

> [!NOTE]
> Jakmile povolíte CDN přístup k účtu úložiště, jsou způsobilé pro ukládání do mezipaměti CDN edge všechny veřejně dostupné objekty. Pokud upravíte objekt, který je aktuálně uložené do mezipaměti v CDN, nový obsah nebudete mít k dispozici prostřednictvím CDN, dokud CDN aktualizuje svůj obsah po uplynutí období time to live pro obsah v mezipaměti.

## <a name="step-5-remove-content-from-the-cdn"></a>Krok 5: Odeberte obsah z CDN
Pokud již nechcete ukládat do mezipaměti na objekt v Azure CDN, můžete provést jednu z následujících kroků:

* Zkontrolujte kontejner privátní místo veřejné. Další informace najdete v tématu [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](../storage/blobs/storage-manage-access-to-resources.md).
* Zakázat nebo odstranit koncový bod CDN pomocí portálu Azure.
* Upravte vaší hostované služby už reagovat na požadavky pro objekt.

Objekt, který je již uložené v mezipaměti v Azure CDN zůstává v mezipaměti, dokud neuplyne období time to live pro objekt, nebo dokud se vyprazdňují koncový bod. Když vyprší platnost období time to live, Azure CDN ověří, zda koncového bodu CDN je stále platný a je objekt stále anonymně přístupný. Pokud tomu tak není, objekt bude už do mezipaměti.

## <a name="additional-resources"></a>Další zdroje informací:
* [Přidání vlastní domény do koncového bodu CDN](cdn-map-content-to-custom-domain.md)
* [Nakonfigurujte protokol HTTPS na vlastní domény služby Azure CDN](cdn-custom-ssl.md)

