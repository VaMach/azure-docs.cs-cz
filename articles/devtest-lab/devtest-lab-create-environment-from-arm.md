---
title: "Vytvoření prostředí více virtuálních počítačů a PaaS prostředky pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Naučte se vytvořit prostředí více virtuálních počítačů a PaaS prostředky v Azure DevTest Labs z šablony Azure Resource Manager"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: tarcher
ms.openlocfilehash: 4e1aae6c041e4572e7e2281203f969e7649e1480
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Vytvoření prostředí více virtuálních počítačů a PaaS prostředky pomocí šablony Azure Resource Manager

[Portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) umožňuje snadno [vytvořte a přidejte virtuální počítač do testovacího prostředí](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-add-vm). To funguje dobře pro vytvoření virtuálního počítače, jeden v čase. Ale pokud prostředí obsahuje víc virtuálních počítačů, každý virtuální počítač má vytvoření jednotlivě. Pro scénáře, jako je vícevrstvé webové aplikace nebo farmy služby SharePoint je nutné mechanismus k povolení pro vytvoření více virtuálních počítačů v jediném kroku. Pomocí šablony Azure Resource Manager, můžete teď určit infrastrukturu a konfiguraci řešení Azure a opakovaně nasazovat více virtuálních počítačů v konzistentním stavu. Tato funkce poskytuje následující výhody:

- Šablony Azure Resource Manageru jsou načítány přímo ze svým úložištěm řízení zdrojů (GitHub nebo Team Services Git).
- Po nakonfigurování mohou uživatelé vytvářet prostředí výběrem šablonu Azure Resource Manageru z portálu Azure jako co mohou provádět u jiných typů [virtuálních počítačů základů](./devtest-lab-comparing-vm-base-image-types.md).
- V prostředí z šablony Azure Resource Manager kromě virtuální počítače IaaS se dá zřídit prostředky Azure PaaS.
- Náklady na prostředí můžete sledovat v testovacím prostředí kromě jednotlivé virtuální počítače vytvořené jiné typy základů.
- PaaS prostředky se vytvoří a zobrazí se v náklady sledování; vypnutí automatického virtuálních počítačů však nevztahuje na PaaS prostředky.
- Uživatelé mají stejné řízení zásad virtuálních počítačů pro prostředí, musí být stejné pro virtuální počítače jedním prostředí.

Další informace o dalších [výhody používání šablon Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) Pokud chcete nasadit, aktualizovat nebo odstranit všechny vaše prostředky testovacího prostředí v rámci jedné operace.

> [!NOTE]
> Při použití šablony Resource Manageru jako základ pro vytvoření testovacího prostředí více virtuálních počítačů, existují určité rozdíly mít na paměti, ať již vytváříte více – virtuální počítače nebo virtuální jednoho počítače. Virtuální počítač pomocí šablony Azure Resource Manageru vysvětluje tyto rozdíly podrobněji.
>
>

## <a name="configure-azure-resource-manager-template-repositories"></a>Konfigurace úložiště šablony Azure Resource Manager

Jako jeden osvědčených postupů s jako kód infrastruktury a konfigurace jako kód by měl spravovat šablony prostředí ve správě zdrojového kódu. Azure DevTest Labs následuje tento postup a načte všechny šablony Azure Resource Manager přímo z vašeho úložiště GitHub nebo služby VSTS Git. V důsledku toho šablony správce prostředků lze napříč celou verzi cyklu z testovacího prostředí do produkčního prostředí.

Existuje několik pravidel pro organizování šablon Azure Resource Manager úložiště:

- Musí mít název souboru hlavní šablony `azuredeploy.json`. 

    ![Soubory šablon klíč Azure Resource Manager](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Pokud chcete použít hodnoty parametrů, které jsou definované v souboru parametrů, musí mít název souboru parametr `azuredeploy.parameters.json`.
- Můžete použít parametry `_artifactsLocation` a `_artifactsLocationSasToken` vytvořit hodnota identifikátoru URI parametersLink, a umožnil DevTest Labs automaticky spravovat vnořené šablony. V tématu [jak Azure DevTest Labs usnadňuje vnořené Resource Manager šablona nasazení pro testovací prostředí](https://blogs.msdn.microsoft.com/devtestlab/2017/05/23/how-azure-devtest-labs-makes-nested-arm-template-deployments-easier-for-testing-environments/) Další informace.
- Metadata lze definovat za účelem zadejte zobrazovaný název šablony a popis. Tato metadata musí být v souboru s názvem `metadata.json`. Soubor metadat následující příklad ukazuje, jak se zadejte zobrazovaný název a popis: 

```json
{
 
"itemDisplayName": "<your template name>",
 
"description": "<description of the template>"
 
}
```

Následující postup vás provede přidání úložiště do testovacího prostředí pomocí portálu Azure. 

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.
1. Ze seznamu labs vyberte požadované testovací prostředí.   
1. V okně v prostředí, vyberte **konfiguraci a zásady**.

    ![Konfigurace a zásady](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Z **konfiguraci a zásady** seznam nastavení, vyberte **úložiště**. **Úložiště** okno uvádí úložiště, které byly přidány do testovacího prostředí. Úložiště s názvem `Public Repo` se automaticky vygeneroval pro všechna prostředí a připojí k [úložiště DevTest Labs GitHub](https://github.com/Azure/azure-devtestlab) obsahující různých artefaktů virtuálních počítačů pro vaše použití.

    ![Veřejné úložiště](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Vyberte **přidat +** přidat úložiště šablony Azure Resource Manager.
1. Když druhý **úložiště** otevře se okno, zadejte nezbytné informace následujícím způsobem:
    - **Název** – zadejte název úložiště, který se používá v testovacím prostředí.
    - **Adresa URL klonu Git** – zadejte adresu URL klonu GIT HTTPS z Githubu nebo Visual Studio Team Services.  
    - **Větev** – zadejte název větve pro přístup k vaší definice šablony Azure Resource Manager. 
    - **Osobní přístupový token** -osobní přístupový token se používá k bezpečný přístup k úložišti. Chcete-li získat tokenu z Visual Studio Team Services, vyberte  **&lt;jméno >> Můj profil > zabezpečení > veřejný přístup token**. K získání tokenu z Githubu, vyberte miniatury a potom výběrem **Nastavení > veřejný přístup token**. 
    - **Cesty ke složce zadat** -pomocí jednoho ze dvou vstupních polí, zadejte cestu ke složce, která se spustí s lomítkem - / - a je relativní vzhledem k vaší klonu Git URI buď artefaktů definic (první vstupní pole) nebo definic šablony Azure Resource Manager .   
    
        ![Veřejné úložiště](./media/devtest-lab-create-environment-from-arm/repo-values.png)

1. Jakmile všechna povinná pole se zadávají a předat ověření, vyberte **Uložit**.

V další části vás provede procesem vytvoření prostředí z šablony Azure Resource Manager.

## <a name="create-an-environment-from-an-azure-resource-manager-template"></a>Vytvoření prostředí z šablony Azure Resource Manager

Po byl nakonfigurován jako úložiště šablony Azure Resource Manager v testovacím prostředí, můžete vytvořit vaši uživatelé testovacím prostředí pomocí portálu Azure pomocí následujících kroků:

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.
1. Ze seznamu labs vyberte požadované testovací prostředí.   
1. V okně v prostředí, vyberte **přidat +**.
1. **Zvolte na základní** zobrazuje základní Image můžete používat s šablon Azure Resource Manageru uvedená jako první. Vyberte požadované šablony Azure Resource Manager.

    ![Vyberte na základní](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. Na **přidat** okno, zadejte **název prostředí** hodnotu. Název prostředí je to, co se zobrazí uživatelům v testovacím prostředí. Zbývající vstupních polí jsou definovány v šablony Azure Resource Manageru. Pokud výchozí hodnoty jsou definovány v šabloně nebo `azuredeploy.parameter.json` je soubor k dispozici, výchozí hodnoty jsou zobrazeny v těchto vstupních polí. Pro parametry typu *zabezpečení řetězec*, můžete použít tajné klíče uložené v tomto prostředí [osobním úložišti tajný](https://azure.microsoft.com/en-us/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store).

    ![Okno Přidání](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Existuje několik hodnot parametrů, které - i v případě, že zadaný - se zobrazují jako prázdné hodnoty. Proto pokud uživatelé přiřadit tyto hodnoty na parametry v šablonu Azure Resource Manager, DevTest Labs nezobrazí hodnoty; Místo toho zobrazující prázdný vstupní pole kdy je potřeba zadat hodnotu, při vytváření prostředí pro lab uživatelů.
    > 
    > - GEN JEDINEČNÝ
    > - -JEDINEČNÉ - GENERACE [N]
    > - GEN--PUB-KLÍČ SSH
    > - GEN-HESLO 
 
1. Vyberte **přidat** vytvořte prostředí. Prostředí spustí zřizování okamžitě s stav zobrazení v **Můj virtuální počítače** seznamu. Automaticky vytvoří novou skupinu prostředků se testovací prostředí pro zajištění všech prostředků, které jsou definované v šablony Azure Resource Manageru.
1. Po vytvoření prostředí, vyberte prostředí **Můj virtuální počítače** seznamu procházet všechny prostředky, které jsou zřízené v prostředí a otevřete okno skupiny prostředků.
    
    ![Moje seznam virtuálních počítačů](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   Můžete také rozšířit prostředí tak, aby zobrazení seznamu virtuálních počítačů, které jsou zřízené v prostředí.
    
    ![Moje seznam virtuálních počítačů](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Kliknutím na jakýkoli z prostředí, chcete-li zobrazit dostupné akce – například použití artefaktů, připojení datových disků, změny času automatického vypnutí a další.

    ![Akce prostředí](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="next-steps"></a>Další kroky
* Po vytvoření virtuálního počítače, můžete připojit k virtuálnímu počítači tak, že vyberete **Connect** v okně Virtuálního počítače.
* Zobrazit a spravovat prostředky v prostředí s výběrem prostředí v **Můj virtuální počítače** seznamu ve svém testovacím prostředí. 
* Prozkoumejte [šablon Azure Resource Manageru z Galerie šablon Azure rychlý start](https://github.com/Azure/azure-quickstart-templates)
