---
title: "Storage Explorer připojení k předplatnému Azure zásobníku"
description: "Zjistěte, jak se připojit k předplatnému Azure zásobníku úložiště Exporer"
services: azure-stack
documentationcenter: 
author: xiaofmao
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: xiaofmao
ms.openlocfilehash: c7e6d70148d39fd74f6409a0a239833f8e9f7614
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/12/2017
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription"></a>Storage Explorer připojení k předplatnému Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Azure Storage Explorer (Preview) je samostatná aplikace, která umožňuje snadno pracovat s daty Azure zásobníku úložiště v systému Windows, systému macOS a Linux. Existuje několik nástrojů zohledněním pro přesun dat do a z zásobník úložiště Azure. Další informace najdete v tématu [přenos dat nástrojů pro úložiště Azure zásobníku](azure-stack-storage-transfer.md).

V tomto článku zjistěte, jak se připojit k Azure zásobníku účtů úložiště pomocí Průzkumníka úložiště. 

Pokud jste ještě nenainstalovali Storage Explorer [Stáhnout](http://www.storageexplorer.com/) a a nainstalujte ji.

Když se připojíte ke svému předplatnému Azure zásobníku, můžete použít [Azure Storage Explorer články](../../vs-azure-tools-storage-manage-with-storage-explorer.md) pro práci s daty Azure zásobníku. 

## <a name="prepare-an-azure-stack-subscription"></a>Příprava předplatné Azure zásobníku

Je nutné použít na hostitelském počítači Azure zásobníku plocha nebo připojení k síti VPN pro Storage Explorer přístup k předplatnému Azure zásobníku. Další informace o tom, jak nastavit připojení VPN ke službě Azure Stack, najdete v tématu [Připojení ke službě Azure Stack pomocí sítě VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

Pro Azure Development Kit zásobníku musíte exportovat certifikát od kořenové autority zásobník Azure.

### <a name="to-export-and-then-import-the-azure-stack-certificate"></a>Chcete-li exportovat a importovat certifikát Azure zásobníku

1. Otevřete `mmc.exe` na hostitelském počítači služby Azure zásobníku nebo místního počítače pomocí připojení VPN do protokolů Azure. 

2. V **soubor**, vyberte **přidat nebo odebrat modul Snap-in**a poté přidejte **certifikáty** ke správě **uživatelským účtem**.



3. V části **Root\Certificated konzoly (místní počítač) \Trusted Root Certification Authorities\Certificates** najít **AzureStackSelfSignedRootCert**.

    ![Načtení kořenového certifikátu služby Azure Stack pomocí mmc.exe][25]

4. Pravým tlačítkem na certifikát, vyberte **všechny úlohy** > **exportovat**a pak postupujte podle pokynů pro export certifikátu s **X.509 s kódováním Base-64 (. CER)**.  

    Vyexportovaný certifikát se použije v dalším kroku.
5. Otevřete Storage Explorer (Preview), a pokud se zobrazí **připojit k Azure Storage** dialogové okno pole, zrušte ji.

6. Na **upravit** nabídky, přejděte na příkaz **certifikáty SSL**a potom klikněte na **importu certifikátů**. Pomocí dialogového okna pro výběr souborů najděte a otevřete certifikát, který jste exportovali v předchozím kroku.

    Po dokončení importu se zobrazí výzva k restartování Storage Exploreru.

    ![Import certifikátu do Storage Exploreru (Preview)][27]

Nyní jste připraveni k připojení k předplatnému zásobník Azure Storage Explorer.

### <a name="to-connect-an-azure-stack-subscription"></a>Pro připojení k předplatnému Azure zásobníku


1. Po restartování Storage Exploreru (Preview) vyberte nabídku **Úpravy** a ujistěte se, že je vybraná možnost **Target Azure Stack** (Cíl – Azure Stack). Pokud není, vyberte ji a restartujte Storage Explorer, aby se tato změna projevila. Tato konfigurace je nutná pro kompatibilitu s vaším prostředím Azure Stack.

    ![Kontrola, že položka Target Azure Stack je vybraná][28]

7. V levém podokně vyberte **Spravovat účty**.  
    Zobrazí se všechny účty Microsoft, ke kterým jste přihlášeni.

8. Pokud se chcete připojit k účtu Azure Stack, vyberte **Přidat účet**.

    ![Přidání účtu Azure Stack][29]

9. V **připojit k Azure Storage** dialogovém **prostředí Azure**, vyberte **prostředí zásobníku Azure použijte**a pak klikněte na tlačítko **Další**.

10. Přihlaste se pomocí účtu Azure zásobníku, který je spojen s minimálně jedno aktivní předplatné Azure zásobníku, vyplňte **přihlášení k prostředí Azure zásobníku** dialogové okno.  

    Podrobnosti k jednotlivým polím:

    * **Environment name** (Název prostředí): Toto pole může uživatel přizpůsobit.
    * **ARM resource endpoint** (Koncový bod prostředku ARM): Toto jsou ukázky koncových bodů prostředků Azure Resource Manageru:

        * Pro operátor cloudu:<br> https://adminmanagement.Local.azurestack.external   
        * Pro klienta:<br> https://Management.Local.azurestack.external
 
    * **Id klienta**: volitelné. Hodnota je uvedená jenom v případě, že musí být zadaný adresář.

12. Po úspěšném přihlášení pomocí účtu Azure Stack se v levém podokně zobrazí předplatná Azure Stack přidružená k tomuto účtu. Vyberte předplatná Azure Stack, se kterými chcete pracovat, a pak vyberte **Apply** (Použít). (Zaškrtnutím nebo zrušením zaškrtnutí políčka **All subscriptions** (Všechna předplatná) přepínáte výběr všech nebo žádných z uvedených předplatných Azure Stack.)

    ![Výběr předplatných Azure Stack po vyplnění dialogového okna Custom Cloud Environment][30]  
    V levém podokně se zobrazí všechny účty úložišť přidružené k vybraným předplatným Azure Stack.

    ![Seznam účtů úložiště, včetně účtů předplatného Azure Stack][31]

## <a name="next-steps"></a>Další kroky
* [Začínáme se Storage Explorerem (Preview)](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Zásobník úložiště Azure: rozdíly a aspekty](azure-stack-acs-differences.md)


* Další informace o službě Azure Storage najdete v tématu [Úvod do Microsoft Azure Storage](../../storage/common/storage-introduction.md)

[25]: ./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png
[26]: ./media/azure-stack-storage-connect-se/export-root-cert-azure-stack.png
[27]: ./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png
[28]: ./media/azure-stack-storage-connect-se/select-target-azure-stack.png
[29]: ./media/azure-stack-storage-connect-se/add-azure-stack-account.png
[30]: ./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png
[31]: ./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png
