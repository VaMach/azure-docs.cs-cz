---
title: "Azure Active Directory Domain Services: Připojení virtuálního počítače Windows serveru ke spravované doméně | Microsoft Docs"
description: "Připojení k virtuálnímu počítači s Windows serverem na Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: maheshu
ms.openlocfilehash: 1ea3f7271bd165bf42d520e4a0267a80dcca58d5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Připojení virtuálního počítače Windows Server ke spravované doméně
Tento článek ukazuje postup nasazení virtuálního počítače s Windows serverem pomocí portálu Azure. Potom ukazuje, jak připojit virtuální počítač k spravované doméně služby Azure AD Domain Services.

## <a name="step-1-create-the-windows-server-virtual-machine"></a>Krok 1: Vytvoření virtuálního počítače Windows serveru
Proveďte následující kroky k vytvoření virtuálního počítače s Windows připojený k virtuální síť, ve kterém jste povolili Azure AD Domain Services.

1. Přihlaste se k webu Azure Portal na adrese [http://portal.azure.com](http://portal.azure.com).
2. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.
3. Vyberte **Compute** a potom vyberte **Windows Server 2016 Datacenter**.

    ![Vyberte bitovou kopii](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. Nakonfigurovali základní nastavení pro virtuální počítač na **Základy** stránce průvodce.

    ![Nakonfigurovali základní nastavení pro virtuální počítač](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > Uživatelské jméno a heslo, které tady zadáte, jsou pro účet místního správce používané pro přihlášení k virtuálnímu počítači. Vyberte silné heslo k ochraně virtuálního počítače před útoky hrubou silou heslo. Nezadávejte zde přihlašovací údaje pro účet uživatele domény.
    >

5. Vyberte **velikost** pro virtuální počítač. Pokud chcete zobrazit další velikosti, vyberte **Zobrazit všechny** nebo změňte filtr **Podporovaný typ disku**.

    ![Vyberte velikost virtuálního počítače](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. Na **nastavení** stránce průvodce, vyberte nasazení virtuální sítě, ve kterém Azure AD Domain Services spravované domény. Vyberte jinou podsíť než ten, který je nasazený vaší spravované domény do. U ostatních nastavení, ponechejte výchozí hodnoty a klikněte na tlačítko **OK**.

    ![Vyberte virtuální síť pro virtuální počítač](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Vyberte správnou virtuální síť a podsíť.**
    > Vyberte buď virtuální síť, ve kterém je nasazený vaší spravované domény nebo virtuální síť, která je připojena k pomocí virtuální sítě, partnerský vztah. Pokud vyberete bez připojení virtuální sítě, nemůže připojit k virtuální počítač k spravované doméně.
    > Doporučujeme, abyste nasazení vaší spravované domény do vyhrazené podsíť. Proto není vyberte podsíť, ve kterém jste povolili vaší spravované domény.

7. Na **nákupu** , zkontrolujte nastavení a klikněte na tlačítko **OK** pro nasazení virtuálního počítače.
8. Nasazení virtuálního počítače je připnuli k řídicímu panelu portálu Azure.

    ![Hotovo](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
9. Po dokončení nasazení se zobrazí informace o virtuálním počítači v **přehled** stránky.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Krok 2: Připojení k virtuálnímu počítači Windows serveru pomocí účtu místního správce
Teď připojte k nově vytvořený virtuální počítač Windows serveru pro připojení k doméně. Pomocí přihlašovacích údajů místního správce, které jste zadali při vytváření virtuálního počítače.

Proveďte následující kroky pro připojení k virtuálnímu počítači.

1. Klikněte **připojit** na tlačítko **přehled** stránky. Soubor Remote Desktop Protocol (.rdp) je vytvořen a stáhli.

    ![Připojení k systému Windows virtuálního počítače](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. Chcete-li se připojit k virtuálnímu počítači, otevřete stažený soubor protokolu RDP. Pokud se zobrazí výzva, klikněte na **Připojit**.
3. Do příkazového řádku přihlášení zadejte vaše **přihlašovací údaje místního správce**, který jste zadali při vytváření virtuálního počítače. Například jsme v tomto příkladu jste použili 'localhost\mahesh'.
4. Během procesu přihlášení se může zobrazit upozornění certifikátu. Klikněte na tlačítko Ano nebo můžete dál pokračovat v připojení.

V tomto okamžiku je by měl být přihlášeni k nově vytvořeného virtuálního počítače s Windows pomocí přihlašovacích údajů místního správce. Dalším krokem je připojení virtuálního počítače k doméně.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Krok 3: Připojení virtuálního počítače Windows serveru k spravované doméně AAD DS
Proveďte následující kroky k připojení virtuálního počítače Windows serveru k spravované doméně AAD DS.

1. Připojte se k systému Windows Server, jak je znázorněno v kroku 2. Na úvodní obrazovce otevřete **správce serveru**.
2. Klikněte na tlačítko **místní Server** v levém podokně okna Správce serveru.

    ![Spusťte správce serveru na virtuálním počítači](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Klikněte na tlačítko **pracovní skupiny** pod **vlastnosti** části. V **vlastnosti systému** stránce vlastností, klikněte na tlačítko **změnu** pro připojení k doméně.

    ![Stránka vlastností systému](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Zadejte název domény vaší spravované domény služby Azure AD Domain Services v **domény** textové pole a klikněte na tlačítko **OK**.

    ![Určení domény pro připojení](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. Zobrazí se výzva k zadání pověření pro připojení k doméně. Ujistěte se, které jste **zadat pověření pro uživatele patřícího k správci řadič domény AAD** skupiny. Pouze členové této skupiny mají oprávnění k připojení počítačů k spravované doméně.

    ![Zadejte pověření pro připojení k doméně](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. Můžete zadat přihlašovací údaje v některém z následujících způsobů:

   * **Formát UPN: (doporučeno)** zadejte příponu UPN pro uživatelský účet, jak nakonfigurovat ve službě Azure AD. V tomto příkladu je přípona UPN uživatele "bob",bob@domainservicespreview.onmicrosoft.com'.
   * **Formát SAMAccountName:** můžete zadat název účtu ve formátu SAMAccountName. V tomto příkladu by uživatel "bob" muset zadat 'CONTOSO100\bob'.

     > [!TIP]
     > **Doporučujeme použít formát UPN a zadejte přihlašovací údaje.**
     > Pokud uživatele (UPN) předpona je příliš dlouhý (například "joehasareallylongname"), může být SAMAccountName automaticky generovaný. Pokud v klientovi služby Azure AD mají více uživatelé stejnou předponou hlavní název uživatele (například "bob"), může být automaticky vygenerované službou jejich SAMAccountName formátu. V těchto případech můžete formát UPN spolehlivě použít k přihlášení k doméně.
     >

7. Po úspěšné připojení k doméně, zobrazí se následující zpráva vás přivítá doméně. Restartujte virtuální počítač pro dokončení operace připojení k doméně.

    ![Vítejte v doméně](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Řešení potíží s připojení k doméně
### <a name="connectivity-issues"></a>Problémy s připojením
Pokud je virtuální počítač se nepodařilo najít doménu, podívejte se na následující kroky řešení potíží:

* Ujistěte se, že virtuální počítač je připojený ke stejné virtuální síti, který jste povolili Domain Services. Pokud ne, virtuální počítač se nemůže připojit k doméně a proto se nepodařilo připojit k doméně.
* Zkontrolujte, zda že je virtuální počítač na virtuální síť, která je pak připojen k virtuální síti, ve kterém jste povolili Domain Services.
* Pokuste se příkazem ping otestovat doménu s názvem domény spravované domény (například "ping contoso100.com"). Pokud jste to možné, zkuste příkazem ping otestovat IP adresy pro doménu na stránku, kde jste povolili službu Azure AD Domain Services (například "ping 10.0.0.4'). Pokud budete moct odeslat příkaz ping IP adresu, ale nikoli doménu, DNS je pravděpodobně nesprávně nakonfigurována. Zkontrolujte, zda jsou IP adresy domény nakonfigurován jako servery DNS pro virtuální síť.
* Zkuste, abyste vyprázdnili mezipaměť Překladač DNS na virtuálním počítači ("ipconfig/flushdns").

Pokud se pro dialogové okno s dotazem, pro přihlašovací údaje pro připojení k doméně, nemáte problémy s připojením.

### <a name="credentials-related-issues"></a>Problémy související s přihlašovací údaje
Pokud máte problémy s přihlašovacími údaji a nelze k připojení k doméně naleznete následující kroky.

* Použijte formát UPN a zadejte přihlašovací údaje. Pokud existuje více uživatelů se stejnou předponou UPN v klientovi nebo pokud vaši předponu UPN je příliš dlouhý, může být automaticky generovaný SAMAccountName pro váš účet. Proto SAMAccountName formát pro váš účet může lišit od co můžete očekávat, nebo použít v místní doméně.
* Zkuste použít přihlašovací údaje uživatelského účtu, který patří do skupiny 'AAD řadič domény správci'.
* Zkontrolujte, že jste [povolili synchronizaci hesel](active-directory-ds-getting-started-password-sync.md) podle kroků uvedených v příručce Začínáme.
* Zkontrolujte, jestli hlavní název uživatele uživatele používají jako nakonfigurovaný v Azure AD (například "bob@domainservicespreview.onmicrosoft.com') pro přihlášení.
* Ujistěte se, že jste čekali dost dlouho kvůli synchronizaci hesel pro dokončení uvedeného v příručce Začínáme.

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
