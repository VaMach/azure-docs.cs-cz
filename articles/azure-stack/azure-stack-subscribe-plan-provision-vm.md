<properties
    pageTitle="Přihlášení k odběru nabídky a následné zřízení virtuálního počítače v Azure Stack (tenant) | Microsoft Azure"
    description="Zjistíte, jak se v Azure Stack jako tenant přihlásit k odběru nabídky a pak zřídit virtuální počítač."
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


# Přihlášení k odběru nabídky

[Nabídku už jste vytvořili](azure-stack-create-offer.md) a teď vyzkoušejte, jestli si tenanti můžou vytvořit předplatné (odběr).

1.  Na počítači Azure Stack POC se přihlaste `https://portal.azurestack.local` jako [tenant](azure-stack-connect-azure-stack.md#log-in-as-a-tenant) a klikněte na **Získat předplatné**.

    ![](media/azure-stack-subscribe-plan-provision-vm/image1.png)

2.  Do pole **Zobrazovaný název** zadejte název předplatného.

    ![](media/azure-stack-subscribe-plan-provision-vm/image2.png)

3.  Klikněte na **Nabídka** a ověřte si, že v okně **Vybrat nabídku** je vámi vytvořená nabídka. Klikněte na ni, klikněte na **Vybrat** a potom na **OK**.  

    ![](media/azure-stack-subscribe-plan-provision-vm/image3.png)

4.  Vytvořené předplatné zobrazíte kliknutím na **Procházet** a potom na **Předplatná**.  

    ![](media/azure-stack-subscribe-plan-provision-vm/image4.png)

Po přihlášení k odběru nabídky počkejte, dokud se stav předplatného neobjeví jako synchronizovaný. Potom aktualizujte stránku portálu, aby se zobrazily služby, které jsou součástí nového předplatného.




## Další kroky

[Účty úložiště](azure-stack-provision-storage-account.md)



<!--HONumber=Sep16_HO3-->


