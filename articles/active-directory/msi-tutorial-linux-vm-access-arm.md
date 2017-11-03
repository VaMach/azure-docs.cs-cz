---
title: "Používat pro přístup k Azure Resource Manager MSI virtuálních počítačů Linux"
description: "Kurz vás provede procesem pomocí Linux virtuálního počítače spravované služby Identity (MSI) pro přístup k Azure Resource Manager."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2017
ms.author: elkuzmen
ms.openlocfilehash: a2ccae76ff548847b331c7993013f22a098525ce
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-resource-manager"></a>Používat pro přístup k Azure Resource Manager Linux virtuálního počítače spravované služby Identity (MSI)

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

V tomto kurzu se dozvíte, jak povolit identita spravované služby (MSI) pro virtuální počítač s Linuxem, a potom tuto identitu používat pro přístup k rozhraní API služby Azure Resource Manager. Identita spravované služby je automaticky prováděna nástrojem Azure a umožňují ověření pro služby, které podporují ověřování Azure AD bez nutnosti vložení přihlašovací údaje do vašeho kódu. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Povolit MSI na virtuální počítač s Linuxem 
> * Udělit přístup virtuálních počítačů do skupiny prostředků ve službě Správce prostředků Azure 
> * Získání přístupového tokenu pomocí identity virtuálního počítače a použít jej k vyvolání Azure Resource Manager 


Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k portálu Azure v [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Vytvořit virtuální počítač s Linuxem do nové skupiny prostředků

V tomto kurzu vytvoříme nový virtuální počítač s Linuxem. Můžete také povolit MSI na existující virtuální počítač.

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.
2. Vyberte **Compute** a potom vyberte **Ubuntu Server 16.04 LTS**.
3. Zadejte informace o virtuálním počítači. Pro **typ ověřování**, vyberte **veřejný klíč SSH** nebo **heslo**. Vytvořené pověření vám umožní přihlásit se k virtuálnímu počítači.

    ![Obrázek alternativní text](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Vyberte **předplatné** pro virtuální počítač v rozevírací nabídce.
5. Chcete-li vybrat nový **skupiny prostředků** chcete virtuální počítač lze vytvořit v, zvolte **vytvořit nový**. Jakmile budete hotovi, klikněte na **OK**.
6. Vyberte velikost virtuálního počítače. Pokud chcete zobrazit další velikosti, vyberte **zobrazit všechny** nebo změňte typ filtru disku podporované. V okně Nastavení ponechte výchozí nastavení a klikněte na **OK**.

## <a name="enable-msi-on-your-vm"></a>Povolit MSI na vašem virtuálním počítači

Virtuální počítač MSI umožňuje získat přístupové tokeny z Azure AD, aniž by bylo třeba uvést přihlašovací údaje do vašeho kódu. V pozadí, povolení MSI provádí dvě věci: nainstaluje rozšíření virtuálního počítače MSI na vašem virtuálním počítači a umožňuje MSI pro virtuální počítač.  

1. Vyberte **virtuálního počítače** , které chcete povolit MSI v.
2. V levém navigačním panelu klikněte na tlačítko **konfigurace**.
3. Zobrazí se **identita spravované služby**. Registrovat a povolit soubor MSI, vyberte **Ano**, pokud chcete zakázat, vyberte Ne.
4. Ujistěte se, kliknete na tlačítko **Uložit** konfiguraci uložíte.

    ![Obrázek alternativní text](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Pokud chcete zkontrolovat, jaká rozšíření jsou v tomto **virtuálního počítače s Linuxem**, klikněte na tlačítko **rozšíření**. Pokud je povoleno MSI, **ManagedIdentityExtensionforLinux** se zobrazí v seznamu.

    ![Obrázek alternativní text](media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="grant-your-vm-access-to-a-resource-group-in-azure-resource-manager"></a>Udělit přístup virtuálních počítačů do skupiny prostředků ve službě Správce prostředků Azure 

Pomocí Instalační služby MSI kódu můžete získat přístupové tokeny k ověřování k prostředkům, které podporují ověřování Azure AD. Rozhraní API služby Azure Resource Manager podporuje ověřování Azure AD. Nejprve musíme udělení tohoto Virtuálního počítače identitu přístup k prostředku v Azure Resource Manager, v tomto případě skupině prostředků, ve kterém se nachází virtuální počítač.  

1. Přejděte na kartu pro **skupiny prostředků**.
2. Vyberte konkrétní **skupiny prostředků** jste vytvořili dříve.
3. Přejděte na **přístup control(IAM)** na levém panelu.
4. Chcete-li **přidat** nové přiřazení role pro virtuální počítač. Zvolte **Role** jako **čtečky**.
5. V dalším rozevíracím **přiřadit přístup** prostředek **virtuální počítač**.
6. Dále zkontrolujte správné předplatné, je uvedena ve **předplatné** rozevíracího seznamu. A pro **skupiny prostředků**, vyberte **všechny skupiny zdrojů**.
7. Nakonec v **vyberte** vyberte virtuální počítač Linux rozevíracího seznamu a klikněte na **Uložit**.

    ![Obrázek alternativní text](media/msi-tutorial-linux-vm-access-arm/msi-permission-linux.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Získání přístupového tokenu pomocí identity Virtuálního počítače a použít jej k vyvolání Resource Manager 

K dokončení těchto kroků budete potřebovat klientem SSH. Pokud používáte systém Windows, můžete použít klienta SSH v [subsystému Windows pro Linux](https://msdn.microsoft.com/commandline/wsl/about). Pokud potřebujete pomoc konfigurace klíče klient SSH, přečtěte si téma [jak klíče použití SSH se systémem Windows v Azure](../virtual-machines/linux/ssh-from-windows.md), nebo [jak vytvořit a používat SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem v Azure](../virtual-machines/linux/mac-create-ssh-keys.md).

1. Na portálu, přejděte k virtuálním počítačům s Linuxem a v **přehled**, klikněte na tlačítko **Connect**.  
2. **Připojit** na virtuální počítač s klientem SSH podle svého výběru. 
3. V okně terminálu pomocí CURL, vytvořte žádost na místní koncový bod MSI se získat přístupový token pro Azure Resource Manager.  
 
    Níže je CURL požadavek na přístupový token.  
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true   
    ```
    
    > [!NOTE]
    > Hodnota parametru "prostředek" musí být přesná shoda pro očekávané službou Azure AD.  V případě ID prostředku Resource Manager je nutné zahrnout do adresy koncové lomítko, v identifikátoru URI. 
    
    Odpověď obsahuje přístupový token, potřebujete získat přístup k Azure Resource Manager. 
    
    Odpověď:  

    ```bash
    {"access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IkhIQnlLVS0wRHFBcU1aaDZaRlBkMlZXYU90ZyIsImtpZCI6IkhIQnlLVS0wRHFBcU1aaDZaRlBkMlZXYU90ZyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3LyIsImlhdCI6MTUwNDEyNjYyNywibmJmIjoxNTA0MTI2NjI3LCJleHAiOjE1MDQxMzA1MjcsImFpbyI6IlkyRmdZTGg2dENWSzRkSDlGWGtuZzgyQ21ZNVdBZ0E9IiwiYXBwaWQiOiI2ZjJmNmU2OS04MGExLTQ3NmEtOGRjZi1mOTgzZDZkMjUxYjgiLCJhcHBpZGFjciI6IjIiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwib2lkIjoiMTEyODJiZDgtMDNlMi00NGVhLTlmYjctZTQ1YjVmM2JmNzJlIiwic3ViIjoiMTEyODJiZDgtMDNlMi00NGVhLTlmYjctZTQ1YjVmM2JmNzJlIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoib0U5T3JVZFJMMHVKSEw4UFdvOEJBQSIsInZlciI6IjEuMCJ9.J6KS7b9kFgDkegJ-Vfff19LMnu3Cfps4dL2uNGucb5M76rgDM5f73VO-19wZSRhQPxWmZLETzN3SljnIMQMkYWncp79MVdBud_xqXYyLdQpGkNinpKVJhTo1j1dY27U_Cjl4yvvpBTrtH3OX9gG0GtQs7PBFTTLznqcH3JR9f-bTSEN4wUhalaIPHPciVDtJI9I24_vvMfVqxkXOo6gkL0mEPfpXZRLwrBNd607AzX0KVmLFrwA1vYJnCV-sSV8bwTh2t6CVEj240t0iyeVWVc2usJ0NY2rxPzKd_UckQ_zzrECG3kS4vuYePKz6GqNJFVzm2w2c61lX0-O1CwvQ9w","refresh_token":"","expires_in":"3599","expires_on":"1504130527","not_before":"1504126627","resource":"https://management.azure.com","token_type":"Bearer"} 
    ```
    
    Tento token přístupu můžete použít pro přístup k Azure Resource Manager, např. Chcete-li zobrazit podrobné informace o skupině prostředků, do které jste dříve udělen přístup tento virtuálních počítačů. Nahraďte hodnoty \<ID PŘEDPLATNÉHO\>, \<skupiny prostředků\>, a \<TOKENU přístupu\> s těmi, které jste vytvořili dříve. 
    
    > [!NOTE]
    > Adresa URL je velká a malá písmena, zajistěte proto, pokud používáte přesný případ stejné, jako jste použili dříve, když jste s názvem skupiny prostředků a velká písmena "G" v "resourceGroup".  
    
    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```
    
    Odpověď zpět s konkrétní informace o skupinách prostředků: 
     
    ```bash
    {"id":"/subscriptions/98f51385-2edc-4b79-bed9-7718de4cb861/resourceGroups/DevTest","name":"DevTest","location":"westus","properties":{"provisioningState":"Succeeded"}} 
    ```
     
## <a name="related-content"></a>Související obsah

- Přehled MSI najdete v tématu [identita spravované služby přehled](../active-directory/msi-overview.md).

Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.

