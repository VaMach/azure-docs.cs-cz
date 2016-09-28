<properties
    pageTitle="Vytvoření testovacího prostředí ve službě Azure DevTest Labs | Microsoft Azure"
    description="Vytvoření testovacího prostředí ve službě Azure DevTest Labs pro virtuální počítače"
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
    ms.date="09/12/2016"
    ms.author="tarcher"/>


# Vytvoření testovacího prostředí v Azure DevTest Labs

## Požadavky

K vytvoření testovacího prostředí potřebujete:

- Předplatné Azure. Informace o možnostech nákupu Azure najdete v tématech [Jak koupit Azure](https://azure.microsoft.com/pricing/purchase-options/) nebo [Bezplatná zkušební verze na jeden měsíc](https://azure.microsoft.com/pricing/free-trial/). Abyste mohli vytvořit testovací prostředí, musíte být vlastníky předplatného.

## Postup vytvoření testovacího prostředí ve službě Azure DevTest Labs

Následující kroky ukazují postup vytvoření testovacího prostředí ve službě Azure DevTest Labs pomocí webu Azure Portal. 

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **Další služby** a poté ze seznamu vyberte **DevTest Labs**.

1. V okně **DevTest Labs** vyberte **Přidat**.

    ![Přidání testovacího prostředí](./media/devtest-lab-create-lab/add-lab-button.png)

1. V okně pro **vytvoření testovacího prostředí DevTest**:

    1. Zadejte **Název testovacího prostředí** pro nové prostředí.
    
    1. Vyberte **Předplatné**, které bude přidruženo k testovacímu prostředí.
    
    1. Vyberte **Umístění**, do kterého se testovací prostředí uloží.
    
    1. Chcete-li povolit a nastavit parametry pro automatické vypínání virtuálních počítačů v testovacím prostředí, vyberte **Automatické vypnutí**.
    
    1. Vyberte **Typ úložiště** a určete typ úložného disku pro virtuální počítače v testovacím prostředí. 
    
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


<!--HONumber=Sep16_HO3-->


