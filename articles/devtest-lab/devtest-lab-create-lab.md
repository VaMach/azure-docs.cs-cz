<properties
    pageTitle="Vytvoření testovacího prostředí v DevTest Labs | Microsoft Azure"
    description="Vytvoření nového testovacího prostředí v DevTest Labs pro virtuální počítače"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# Vytvoření testovacího prostředí v Azure DevTest Labs

## Požadavky

K vytvoření testovacího prostředí budete potřebovat:

- Předplatné Azure. Informace o možnostech nákupu Azure najdete v tématech [Jak koupit Azure](https://azure.microsoft.com/pricing/purchase-options/) nebo [Bezplatná zkušební verze na jeden měsíc](https://azure.microsoft.com/pricing/free-trial/). Abyste mohli vytvořit testovací prostředí, musíte být vlastníky předplatného.
- Skupinu prostředků Azure pro testovací prostředí. Přečtěte si témata [Přehled Azure Resource Manageru](../resource-group-overview.md) a [Řízení přístupu Azure na základě rolí](../active-directory/role-based-access-control-configure.md).

## Vytvoření testovacího prostředí

1. Přihlaste se k webu [Portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **Procházet**.

1. Ze seznamu vyberte **DevTest Labs**.

1. V okně **DevTest Labs** vyberte **Přidat**.

    ![Přidání testovacího prostředí](./media/devtest-lab-create-lab/add-lab-button.png)

1. V okně pro **vytvoření testovacího prostředí DevTest**:

    1. Zadejte **Název testovacího prostředí** pro nové prostředí.
    1. Vyberte **Předplatné**, které bude přidruženo k testovacímu prostředí.
    1. Vyberte **Umístění**, do kterého se testovací prostředí uloží.
    1. Vyberte **Vytvořit**.

    ![Okno pro vytvoření testovacího prostředí](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## Další kroky

Po vytvoření testovacího prostředí je zde několik kroků, které je vhodné zvážit:

- [Zabezpečení přístupu k testovacímu prostředí](devtest-lab-add-devtest-user.md)

- [Nastavení zásad testovacího prostředí](devtest-lab-set-lab-policy.md)

- [Vytvoření šablony testovacího prostředí](devtest-lab-create-template.md)

- [Vytvoření vlastních artefaktů pro virtuální počítače](devtest-lab-artifact-author.md)

- [Přidání virtuálního počítače s artefakty do testovacího prostředí](devtest-lab-add-vm-with-artifacts.md)


<!--HONumber=ago16_HO5-->


