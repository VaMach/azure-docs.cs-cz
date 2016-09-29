<properties
    pageTitle="Vytvoření plánu ve službě Azure Stack | Microsoft Azure"
    description="Jako správce služby vytvořte plán, který předplatitelům umožní zřizovat virtuální počítače."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="erikje"/>


# Vytvoření plánu ve službě Azure Stack

[Plány](azure-stack-key-features.md#services-plans-offers-and-subscriptions) představují seskupení jedné nebo více služeb. Jako poskytovatel můžete vytvořit plány, které nabídnete svým tenantům. Vaši tenanti se pak můžou přihlásit k odběru nabídek, aby mohli používat plány a služby, které k nim patří. V tomto příkladu vytvoříte plán, který zahrnuje poskytovatele výpočetních prostředků, síťových prostředků a prostředků úložiště. To přidá odběratelům do plánu možnost zřizování virtuálních počítačů.

1.  V internetovém prohlížeči přejděte na web https://portal.azurestack.local.

2.  [Přihlaste se](azure-stack-connect-azure-stack.md#log-in-as-a-service-administrator) k portálu Azure Stack jako správce služby a zadejte přihlašovací údaje správce služby (to je účet vytvořený během kroku 5 v části [Spuštění powershellového skriptu pro nasazení](azure-stack-run-powershell-script.md)) a potom klikněte na **Přihlásit se**.

    Správci služby můžou vytvářet nabídky a plány a můžou spravovat uživatele.

3.  Pokud chcete vytvořit plán a nabídku, k jejichž odběru se tenanti můžou přihlásit, klikněte na **Nový**.

    ![](media/azure-stack-create-plan/image1.png)

4.  V okně Vytvoření klikněte na **Nabídky a plány pro tenanty** a potom klikněte na **Plán**.

    ![](media/azure-stack-create-plan/image2.png)

5.  Vyplňte **Zobrazovaný název** a **Název prostředku**. Zobrazovaný název představuje popisný název plánu. Název prostředku může vidět jenom správce. Je to název, který správci používají pro práci s plánem jako s prostředkem Azure Resource Manageru.

    ![](media/azure-stack-create-plan/image3.png)

6.  Vyberte nebo vytvořte novou **skupinu prostředků** jako kontejner plánu. Ve výchozím nastavení budou všechny plány a nabídky patřit do skupiny prostředků s názvem OffersAndPlans.

7.  Klikněte na **Nabízené služby**, pomocí klávesy Shift vyberte všechny tři poskytovatele (**Poskytovatel výpočtů**, **Poskytovatel úložiště** a **Poskytovatel sítě**) a potom klikněte na **Vybrat**.

    ![](media/azure-stack-create-plan/image4.png)

8.  Klikněte na **Microsoft.Compute** a potom na **Potřebuje nakonfigurovat**.

    ![](media/azure-stack-create-plan/image5.png)

9.  V okně **Nastavit kvóty** přijměte všechny výchozí hodnoty, klikněte na tlačítko **OK** a potom ještě jednou na tlačítko **OK**.

    ![](media/azure-stack-create-plan/image6.png)

10. Klikněte na **Microsoft.Network** a potom na **Potřebuje nakonfigurovat**.

    ![](media/azure-stack-create-plan/image7.png)

11. V okně **Nastavit kvóty** zaškrtněte všechna políčka, klikněte na tlačítko **OK** a potom ještě jednou na tlačítko **OK**.

    ![](media/azure-stack-create-plan/image8.png)

12. Klikněte na **Microsoft.Storage**, na **Potřebuje nakonfigurovat** a potom v okně **Nastavit kvóty** přijměte všechny výchozí hodnoty. Klikněte na tlačítko **OK** a potom ještě jednou na tlačítko **OK**. Nakonec kliknutím na **Vytvořit** vytvořte plán.

    ![](media/azure-stack-create-plan/image9.png)

13. Teď můžete plán zahrnout do nabídky. Kliknutím na zvonek vpravo nahoře můžete zobrazit oznámení.

    ![](media/azure-stack-create-plan/image10.png)

## Další kroky

[Vytvoření nabídky](azure-stack-create-offer.md)



<!--HONumber=Sep16_HO3-->


