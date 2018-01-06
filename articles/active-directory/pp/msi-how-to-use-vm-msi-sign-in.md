---
title: "Jak používat Azure virtuálního počítače spravované služby Identity pro přihlášení"
description: "Podrobné pokyny a příklady pro použití, přihlaste se objektu služby MSI virtuálních počítačů Azure pro skript klienta a prostředku přístup."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/05/2018
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c5f71d27a9e07cc6d6a260b809e91aaa2a50270c
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="sign-in-using-a-vm-user-assigned-managed-service-identity-msi"></a>Přihlaste se pomocí virtuální počítač přiřazený uživatelem identita spravované služby (MSI)

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]Tento článek obsahuje příklady skriptu rozhraní příkazového řádku pro přihlašování pomocí přiřazený uživatelem MSI instančního objektu a pokyny na důležité oblastech, jako je zpracování chyb.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Abyste mohli používat rozhraní příkazového řádku Azure příklady v tomto článku, nezapomeňte nainstalovat nejnovější verzi [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Všechny ukázkový skript v tomto článku předpokládá, že klient příkazového řádku běží na virtuálním počítači povolená MSI. Použijte funkci "Připojit" virtuální počítač na portálu Azure se vzdáleně připojit k virtuálnímu počítači. Informace o povolení MSI na virtuálním počítači, v [konfigurace virtuálních počítačů spravovaných služba Identity (MSI) pomocí rozhraní příkazového řádku Azure](msi-qs-configure-cli-windows-vm.md), nebo jeden z typu variant článků (pomocí prostředí PowerShell, portálu, šablonu nebo Azure SDK). 
> - Aby se zabránilo chybám při přihlášení a prostředku přístup, soubor MSI musí mít alespoň "Čtečky" získat přístup v oboru příslušné (virtuálního počítače nebo vyšší) umožňuje Azure Resource Manager operace na virtuálním počítači. V tématu [přiřadit identita spravované služby (MSI) přístup k prostředku pomocí rozhraní příkazového řádku Azure](msi-howto-assign-access-cli.md) podrobnosti.

## <a name="overview"></a>Přehled

Poskytuje MSI [instanční objekt](~/articles/active-directory/develop/active-directory-dev-glossary.md#service-principal-object), což je [při povolení MSI vytvářeny](msi-overview.md#how-does-it-work) ve virtuálním počítači. Objekt služby můžete poskytnut přístup k prostředkům Azure a použít jako identita skriptu nebo příkazového-řádku klienty pro přístup k přihlášení a prostředků. Tradičně aby bylo možné získat přístup k zabezpečeným prostředkům v rámci své vlastní identity, klient skriptu by bylo potřeba:  

   - zaregistrovat a dá souhlas s Azure AD jako důvěrné nebo webové klientské aplikace
   - Přihlaste se pod jeho instanční objekt, pomocí přihlašovacích údajů aplikace (které by mohly vložená ve skriptu)

Pomocí Instalační služby MSI vašeho skriptu klienta už nepotřebuje udělat, jak můžete přihlásit v rámci objektu služby MSI. 

## <a name="azure-cli"></a>Azure CLI

Následující skript ukazuje, jak:

1. Přihlaste se ke službě Azure AD v rámci objektu služby MSI přiřazený uživatelem.  
2. Volání Azure Resource Manager a při získávání umístění oblast Azure pro virtuální počítač. Rozhraní příkazového řádku se stará o správu tokenu získávání nebo použití pro vás automaticky. Nezapomeňte nahradit název vašeho virtuálního počítače pro `<VM NAME>`a uživatel přiřazen id prostředku MSI pro `<MSI ID>`. Id prostředku MSI je vrácený v `id` vlastnost během vytváření přiřazený uživatelem MSI (najdete v části [konfigurace přiřazený uživatelem spravované služby Identity (MSI) pro virtuální počítač, pomocí rozhraní příkazového řádku Azure](msi-qs-configure-cli-windows-vm.md) příklady `az identity create` příkaz ).

    ```azurecli
    az login --msi –u <MSI ID>
   
    vmLocation=$(az resource list -n <VM NAME> --query [*].location --out tsv)
    echo The VM region location is $vmLocation
    ```

    Příklad odpovědí:
   
    ```bash
    user@vmLinux:~$ az login --msi -u /subscriptions/80c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName
    [
      {
        "environmentName": "AzureCloud",
        "id": "90c696ff-5efa-4909-a64d-z1b616f423bl",
        "isDefault": true,
        "name": "MSIResource-/subscriptions/90c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName@50342",
        "state": "Enabled",
        "tenantId": "933a8f0e-ec41-4e69-8ad8-971zc4b533ll",
        "user": {
          "name": "userAssignedIdentity",
          "type": "servicePrincipal"
        }
      }
    ]  
    user@vmLinux:~$ vmLocation=$(az resource list -n vmLinux --query [*].location --out tsv)
    user@vmLinux:~$ echo The VM region location is $vmLocation
    The VM region location is westcentralus
    ```

## <a name="resource-ids-for-azure-services"></a>ID prostředků pro služby Azure

V tématu [služeb Azure, podpora Azure AD ověření](msi-overview.md#azure-services-that-support-azure-ad-authentication) seznam prostředků, které podporují Azure AD a byly testovány s MSI a jejich odpovídající ID prostředku.

## <a name="error-handling-guidance"></a>Pokyny pro zpracování chyb 

Následující odpovědi může znamenat, že soubor MSI nebyla nakonfigurována správně:

- Rozhraní příkazového řádku: *MSI: Nepodařilo se načíst token zabezpečení ze (http://localhost:50342/oauth2/token) s chybou "HTTPConnectionPool (hostitele = localhost, port = 50342)* 

Pokud se zobrazí jednu z těchto chyb, vraťte se do virtuálního počítače Azure v [portál Azure](https://portal.azure.com) a:

- Přejděte na **konfigurace** stránky a "Identita spravované služby" je nastavena na "Ano".
- Přejděte na **rozšíření** stránky a zkontrolujte příponou MSI úspěšně nasazena.

Pokud je buď nesprávný, musíte znovu přiřadit MSI pro daný prostředek, nebo vyřešit potíže s selhání nasazení. V tématu [konfigurace virtuálních počítačů spravovaných služba Identity (MSI) pomocí rozhraní příkazového řádku Azure](msi-qs-configure-cli-windows-vm.md) Pokud potřebujete pomoc s konfigurací virtuálních počítačů.

## <a name="next-steps"></a>Další postup

- Pokud chcete povolit MSI ve virtuálním počítači Azure, najdete v části [konfigurace virtuálních počítačů spravovaných služba Identity (MSI) pomocí rozhraní příkazového řádku Azure](msi-qs-configure-cli-windows-vm.md).

Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.








