<properties
    pageTitle="Připojení k Microsoft Azure Stack POC | Microsoft Azure"
    description="Naučte se připojit k portálu Azure Stack POC jako správce nebo tenant služby."
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
    ms.date="08/01/2016"
    ms.author="erikje"/>


# Přihlášení k virtuálnímu počítači Azure Stack POC

K virtuálnímu počítači Azure Stack POC se můžete přihlásit jako:

- [**správce služby**](#log-in-as-a-service-administrator) – můžete spravovat poskytovatele prostředků, nabídky tenantů, plány, služby, kvóty a ceny.

nebo

- [**tenant**](#log-in-as-a-tenant) – můžete zřizovat, sledovat a spravovat služby, k jejichž odběru jste se přihlásili, například webové aplikace, úložiště a virtuální počítače.

## Přihlášení jako správce služby

1.  Přihlaste se k fyzickému počítači Azure Stack POC.

2.  Na ploše poklikejte na ikonu **ClientVM.AzureStack.local.rdp** a otevřete připojení ke vzdálené ploše virtuálního klientského počítače.
 
    ![](media/azure-stack-connect-azure-stack/clientvmazurestacklocalicon.png)
    
    Tím se automaticky použije účet AzureStack\\AzureStackUser, který byl vytvořen skriptem nasazení. Po výzvě **Zadejte heslo pro předdefinovaný účet správce** použijte heslo správce, které jste zadali v kroku 5 skriptovacího procesu.

3.  Na ploše ClientVM.AzureStack.local poklikejte na ikonu **Portál Microsoft Azure Stack POC** (https://portal.azurestack.local/) a otevřete [portál](azure-stack-key-features.md#portal).

    ![](media/azure-stack-connect-azure-stack/microsoftazurestackpocprtalicon.png)

4.  Přihlaste se pomocí účtu správce služby.

## Přihlášení jako tenant

Správce služby se může přihlásit jako tenant, aby otestoval plány, nabídky a předplatné, které můžou jeho tenanti používat.
Pokud ho ještě nemáte, [vytvořte si účet tenanta](azure-stack-add-new-user-aad.md) se kterým se potom přihlásíte.

1.  Přihlaste se k fyzickému počítači Azure Stack.

2.  Na ploše poklikejte na ikonu **ClientVM.AzureStack.local.rdp** a otevřete připojení ke vzdálené ploše virtuálního klientského počítače. 

    ![](media/azure-stack-connect-azure-stack/clientvmazurestacklocalicon.png)

    Tím se automaticky použije účet AzureStack\\AzureStackUser, který byl vytvořen skriptem nasazení. Po výzvě **Zadejte heslo pro předdefinovaný účet správce** použijte heslo správce, které jste zadali v kroku 5 skriptovacího procesu.

3.  Na ploše ClientVM.AzureStack.local poklikejte na ikonu **Portál Microsoft Azure Stack POC** (https://portal.azurestack.local/) a otevřete [portál](azure-stack-key-features.md#portal).

    ![](media/azure-stack-connect-azure-stack/microsoftazurestackpocprtalicon.png)

4.  Přihlaste se pomocí účtu tenanta.

RDP může omezit počet uživatelů s přístupem k fyzickému hostiteli Microsoft Azure POC. Pokud chcete povolit více uživatelů, přečtěte si článek [Povolení několika souběžně připojených uživatelů](azure-stack-enable-multiple-concurrent-users.md).

## Další kroky

[První úkoly](azure-stack-first-scenarios.md)



<!--HONumber=Sep16_HO3-->


