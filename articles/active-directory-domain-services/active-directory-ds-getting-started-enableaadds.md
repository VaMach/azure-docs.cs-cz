---
title: "Azure Active Directory Domain Services: Povolení služby Azure Active Directory Domain Services | Dokumentace Microsoftu"
description: "Povolení služby Azure Active Directory Domain Services pomocí portálu Azure Classic"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: ed72325ca9db99405c6173eb882a92f80cd77f47
ms.contentlocale: cs-cz
ms.lasthandoff: 06/30/2017


---
<a id="enable-azure-active-directory-domain-services-using-the-azure-classic-portal" class="xliff"></a>

# Povolení služby Azure Active Directory Domain Services pomocí portálu Azure Classic

<a id="task-3-enable-azure-active-directory-domain-services" class="xliff"></a>

## Úloha 3: Povolení služby Azure Active Directory Domain Services
V této úloze povolíte službu Azure Active Directory Domain Services (Azure AD DS) pro svůj adresář pomocí následujících kroků:

1. Přejděte na [portál Azure Classic](https://manage.windowsazure.com).
2. V levém podokně vyberte tlačítko **Active Directory**.
3. Vyberte klienta (adresář) Azure Active Directory (Azure DS), pro kterého chcete povolit službu Azure AD DS.

    ![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Na stránce **Náhled adresáře** klikněte na kartu **Konfigurovat**.

    ![Karta konfigurace adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. V části **Domain Services** změňte možnost **Povolit službu Domain Services pro tento adresář** na **Ano**.  
    Na stránce se zobrazí další možnosti konfigurace služby Azure Active Directory Domain Services.

    ![Povolení doménových služeb](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > Když pro svého klienta povolíte službu Azure Active Directory Domain Services, Azure AD bude generovat a ukládat hodnoty hash přihlašovacích údajů protokolů Kerberos a NTLM potřebné pro ověřování uživatelů.
   >
   >
6. Zadejte **název domény DNS doménových služeb**.

   * Ve výchozím nastavení je vybrán výchozí název domény adresáře (s příponou **.onmicrosoft.com**).

   * Seznam obsahuje všechny domény, které byly nakonfigurované pro váš adresář služby Azure AD – včetně ověřených i neověřených domén, které konfigurujete na kartě **Domény**.

   * Můžete zadat i vlastní název domény. V tomto příkladu je použit vlastní název domény *contoso100.com*.

     > [!WARNING]
     > Předpona zadaného názvu domény (například *contoso100* v případě názvu domény *contoso100.com*) musí obsahovat nejvýše 15 znaků. Nelze vytvořit domény služby Azure Active Directory Domain Services s předponou obsahující více než 15 znaků.
     >
     >
7. Ujistěte se, že vámi zvolený název domény DNS pro spravovanou doménu ještě ve virtuální síti neexistuje. Konkrétně zkontrolujte následující body:

   * Ve virtuální síti již existuje doména se stejným názvem domény DNS.

   * Vybraná virtuální síť má připojení VPN k vaší místní síti, ve které máte doménu se stejným názvem domény DNS.

   * Ve virtuální síti existuje cloudová služba s tímto názvem.
8. Vyberte virtuální síť, ve které má být k dispozici služba Azure Active Directory Domain Services. Z rozevíracího seznamu **Připojit službu Domain Services k této virtuální síti** vyberte vytvořenou virtuální síť a vyhrazenou podsíť. Zvažte také následující body:

   * Ujistěte se, že určená virtuální síť patří do oblasti Azure podporované službou Azure Active Directory Domain Services. Oblasti Azure, ve kterých je služba Azure Active Directory Domain Services k dispozici, pro ověření najdete v článku [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).

   * Virtuální sítě patřící do oblasti, ve které není služba Azure Active Directory Domain Services podporovaná, se v rozevíracím seznamu nezobrazí.

   * Pro službu Azure Active Directory Domain Services použijte vyhrazenou podsíť ve virtuální síti. *Nevybírejte* podsíť brány. Viz téma o [aspektech sítí](active-directory-ds-networking.md).

   * Podobně se v rozevíracím seznamu nezobrazí ani virtuální sítě vytvořené pomocí Azure Resource Manageru. Virtuální sítě na bázi Resource Manageru nejsou v současné době službou Azure Active Directory Domain Services podporované.
9. Službu Azure Active Directory Domain Services povolíte kliknutím na **Uložit** v podokně úloh v dolní části stránky.
    * Zatímco probíhá povolování služby Azure Active Directory Domain Services pro váš adresář, na stránce se zobrazuje stav *Čeká na vyřízení*.

        ![Povolení okna Domain Services](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > Služba Azure Active Directory Domain Services poskytuje vysokou dostupnost vaší spravované domény. Když službu Azure Active Directory Domain Services povolíte, postupně se zobrazují IP adresy, na kterých je služba Domain Services ve virtuální síti k dispozici. Druhá IP adresa se zobrazí krátce po první adrese, jakmile služba povolí vysokou dostupnost vaší domény. Když je vysoká dostupnost vaší domény nakonfigurovaná a aktivní, měli byste vidět dvě IP adresy v oddílu **doménové služby** na kartě **Konfigurace**.
        >
        >
    * Přibližně po 20 až 30 minutách se první IP adresa, na které je k dispozici služba Domain Services ve virtuální síti, zobrazí v poli **IP adresa** na stránce **Konfigurace**.

        ![Okno Domain Services se zobrazenou první zřízenou IP adresou](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * Když je vysoká dostupnost vaší domény funkční, zobrazí se na stránce dvě IP adresy. Na těchto dvou IP adresách je k dispozici vaše spravovaná doména ve vybrané virtuální síti.

10. Poznamenejte si obě tyto IP adresy, abyste mohli aktualizovat nastavení DNS pro svou virtuální síť. Tento postup umožňuje virtuálním počítačům ve virtuální síti připojit se k doméně kvůli operacím, jako je například připojení k doméně.

    ![Okno služby Domain Service se zobrazením obou zřízených IP adres](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Synchronizace se spravovanou doménou bude v závislosti na velikosti klienta Azure AD (například na počtu uživatelů nebo skupin) chvíli trvat. Proces synchronizace se odehrává na pozadí. U velkých klientů s desítkami tisíc objektů může trvat i několik dní, než se synchronizují všichni uživatelé, členství ve skupinách a přihlašovací údaje.
>
>

<a id="next-step" class="xliff"></a>

## Další krok
[Úloha 4: Aktualizace nastavení DNS pro virtuální síť Azure](active-directory-ds-getting-started-update-dns.md)

