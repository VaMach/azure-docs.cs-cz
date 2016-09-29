<properties
    pageTitle="Vytvoření nabídky ve službě Azure Stack | Microsoft Azure"
    description="Jako správce služby se naučte vytvořit nabídku pro své tenanty ve službě Azure Stack."
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
    ms.date="05/25/2016"
    ms.author="erikje"/>


# Vytvoření nabídky ve službě Azure Stack

[Nabídky](azure-stack-key-features.md#services-plans-offers-and-subscriptions) jsou skupiny z jednoho nebo několika plánů, které poskytovatelé předkládají tenantům ke koupi (přihlášení k odběru). V tomto příkladu vytvoříte nabídku, která obsahuje [plán, který jste vytvořili](azure-stack-create-plan.md) v posledním kroku. To přidá odběratelům do nabídky možnost zřizování virtuálních počítačů.

1.  [Přihlaste se](azure-stack-connect-azure-stack.md#log-in-as-a-service-administrator) k portálu jako správce služby.
    ![](media/azure-stack-create-offer/image1.png)

2.  Klikněte na možnost **Nové**.

3.  Klikněte na **Nabídky a plány pro tenanty** a potom klikněte na **Nabídka**.
    ![](media/azure-stack-create-offer/image2.png)

4.  V okně **Nová nabídka** vyplňte následující:

    1.  Vyplňte **Zobrazovaný název** a **Název prostředku**. Zobrazovaný název představuje popisný název nabídky. Název prostředku může vidět jenom správce. Je to název, který správci používají pro práci s nabídkou jako s prostředkem Azure Resource Manageru.

    2.  Vyberte novou nebo existující **skupinu prostředků**.

        ![](media/azure-stack-create-offer/image3.png)

5.  Klikněte na **Základní plány** a v okně **Plán** vyberte plány, které chcete do nabídky zahrnout, a potom klikněte na **Vybrat**. Kliknutím na **Vytvořit** vytvořte svou nabídku.

    ![](media/azure-stack-create-offer/image4.png)

6.  Klikněte na **Změnit stav** a potom na **Veřejné**.
Plány a nabídky musíte zveřejnit, abyste tenantům umožnili úplné zobrazení při přihlašování k odběru. Pokud je plán soukromý a nabídka je veřejná, tenanti budou mít přístup k nabídce, ale nebudou moct zobrazit podrobnosti plánu. Plány a nabídky můžou být:

    -   **Veřejné**: Viditelné pro tenanty.

    -   **Soukromé**: Viditelné jenom pro správce služby. To je užitečné, když pracujete na návrhu plánu nebo nabídky, nebo když chce správce služby jednotlivě schvalovat každé přihlášení k odběru.

    -   **Vyřazeno z provozu**: Uzavřené pro nové odběratele. Správce služby může možnost Vyřazeno z provozu použít k tomu, aby zabránil dalším přihlášením k odběru, ale přitom nechá aktuální předplatitele beze změny.

    ![](media/azure-stack-create-offer/image6.png)

Změny plánu nebo nabídky nevidí tenant okamžitě. Aby změny viděl, musí být stav předplatného Synchronizované a potom musí tenant aktualizovat portál nebo se odhlásit a přihlásit.

Dokonce i po vytvoření a synchronizaci dalšího předplatného může být nutné se odhlásit a znovu přihlásit, abyste při vytváření nových prostředků nebo skupin prostředků viděli nové předplatné v části Výběr předplatného.

## Další kroky

[Přihlášení k odběru nabídky a zřízení virtuálního počítače](azure-stack-subscribe-plan-provision-vm.md)



<!--HONumber=Sep16_HO3-->


