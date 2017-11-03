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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 511076935d06ed0908341044e37069e74530be49
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Účet úložiště Azure integrovat Azure CDN
CDN se dá nastavit do mezipaměti obsah z úložiště Azure. Nabízí vývojářům globální řešení pro doručování širokopásmového obsahu pomocí ukládání do mezipaměti objektů BLOB a statického obsahu výpočetní instance na fyzických uzlech v USA, Evropa, Asie, Austrálie a Jižní Ameriky.

## <a name="step-1-create-a-storage-account"></a>Krok 1: Vytvoření účtu úložiště
Použijte následující postup k vytvoření nového účtu úložiště pro předplatné Azure. Účet úložiště poskytuje přístup ke službám úložiště Azure. Účet úložiště představuje nejvyšší úroveň oboru názvů pro přístup k jednotlivých součástí služby Azure storage: objektu Blob služby, služba fronty a tabulky služby. Další informace najdete v části [Úvod do Microsoft Azure Storage](../storage/common/storage-introduction.md).

Pokud chcete vytvořit účet úložiště, musí být buď správce služby správce nebo spolusprávce pro předplatné přidružený.

> [!NOTE]
> Existuje několik metod, které můžete použít k vytvoření účtu úložiště, včetně portálu Azure a prostředí Powershell.  V tomto kurzu budeme používat portál Azure.  
> 
> 

**Chcete-li vytvořit účet úložiště pro předplatné Azure**

1. Přihlaste se k [Portálu Azure](https://portal.azure.com).
2. V levém horním rohu vyberte **nový**. V **nový** vyberte **Data + úložiště**, pak klikněte na tlačítko **účet úložiště**.
    
    **Vytvořit účet úložiště** otevře se okno.   

    ![Vytvořit účet úložiště][create-new-storage-account]  

3. V **název** pole, zadejte název subdomény. Tato položka může obsahovat 3 až 24 malých písmen a číslic.
   
    Tato hodnota se změní na název hostitele v rámci identifikátoru URI, který slouží k adresování objektů Blob, fronty nebo tabulky prostředky pro předplatné. Chcete-li vyřešit prostředek kontejneru ve službě Blob, použijte identifikátor URI v následujícím formátu, kde  *&lt;StorageAccountLabel&gt;*  odkazuje na hodnotu, která jste zadali v **zadejte adresu URL**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;můj_kontejner&gt;*
   
    **Důležité:** adresy URL popisku forms subdoméně účtu úložiště URI a musí být jedinečný mezi všechny hostované služby v Azure.
   
    Tato hodnota se používá jako název tohoto účtu úložiště na portálu, nebo při přístupu k tomuto účtu prostřednictvím kódu programu.
4. Ponechte výchozí nastavení pro **modelu nasazení**, **účet druhu**, **výkonu**, a **replikace**. 
5. Vyberte **předplatné** který účet úložiště se použije s.
6. Vyberte nebo vytvořte **skupinu prostředků**.  Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Vyberte umístění vašeho účtu úložiště.
8. Klikněte na možnost **Vytvořit**. Proces vytvoření účtu úložiště může trvat několik minut na dokončení.

## <a name="step-2-enable-cdn-for-the-storage-account"></a>Krok 2: Povolení CDN pro účet úložiště

S nejnovější integraci můžete teď povolit CDN pro váš účet úložiště bez opuštění portálu rozšíření úložiště. 

1. Vyberte účet úložiště, prohledávání "CDN" nebo projděte dolů v levé navigační nabídce a pak klikněte na tlačítko "Azure CDN".
    
    **Azure CDN** otevře se okno.

    ![povolení navigace CDN][cdn-enable-navigation]
    
2. Vytvoření nového koncového bodu tak, že zadáte požadované informace
    - **Profil CDN**: můžete vytvořit novou nebo použít stávající profil.
    - **Cenová úroveň**: potřebujete Vyberte cenovou úroveň, pokud vytvoříte nový profil CDN.
    - **Název koncového bodu CDN**: Zadejte název koncového bodu podle svého výběru.

    > [!TIP]
    > Vytvoření koncového bodu CDN používá název hostitele vašeho účtu úložiště jako původní ve výchozím nastavení.

    ! [cdn nové vytvoření koncového bodu] [cdn--koncový bod vytvoření nové]

3. Po vytvoření nový koncový bod se zobrazí v seznamu koncových bodů výše.

    ![Nový koncový bod CDN úložiště][cdn-storage-new-endpoint]

> [!NOTE]
> Můžete také přejít na rozšíření Azure CDN povolit CDN. [Kurzu](#Tutorial-cdn-create-profile).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a>Krok 3: Povolení dalších funkcí CDN

V okně "Azure CDN" účet úložiště klikněte na koncový bod CDN ze seznamu otevřete okno Konfigurace CDN. Můžete povolit další funkce CDN pro doručení, jako je například kompresi, řetězce dotazu, geograficky filtrování. Můžete také přidat mapování vlastní doménu pro koncový bod CDN a povolit HTTPS pro vlastní doménu.
    
![Konfigurace cdn úložiště CDN][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a>Krok 4: Přístup k obsahu CDN
Chcete-li získat přístup k obsahu v mezipaměti na CDN, použijte CDN adresy URL poskytnuté na portálu. Adresa pro uložené v mezipaměti objektů blob bude podobný následujícímu:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [!NOTE]
> Jakmile povolíte CDN přístup k účtu úložiště, jsou způsobilé pro ukládání do mezipaměti CDN edge všechny veřejně dostupné objekty. Pokud upravíte objekt, který je aktuálně uložené do mezipaměti v CDN, nový obsah nebudete mít k dispozici prostřednictvím CDN, dokud CDN aktualizuje jeho obsah, když vyprší platnost mezipaměti obsahu time to live období.
> 
> 

## <a name="step-5-remove-content-from-the-cdn"></a>Krok 5: Odeberte obsah z CDN
Pokud již nechcete ukládat do mezipaměti objekt v Content Delivery Network (CDN) Azure, můžete provést jednu z následujících kroků:

* Můžete provést kontejner privátní místo veřejné. Další informace viz [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](../storage/blobs/storage-manage-access-to-resources.md).
* Můžete zakázat nebo odstranit koncový bod CDN pomocí portálu pro správu.
* Můžete upravit vaší hostované služby už reagovat na požadavky pro objekt.

Objekt již uložené v mezipaměti v CDN zůstane v mezipaměti, dokud neuplyne období time to live pro objekt, nebo dokud se vyprazdňují koncový bod. Po vypršení doby time to live CDN bude zkontrolujte, jestli je stále platný koncový bod CDN a objekt anonymně přístupné. Pokud není, bude mezipaměti už tento objekt.

## <a name="additional-resources"></a>Další zdroje
* [Postup mapování obsahu CDN do vlastní domény](cdn-map-content-to-custom-domain.md)
* [Povolit HTTPS pro vaši vlastní doménu.](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
