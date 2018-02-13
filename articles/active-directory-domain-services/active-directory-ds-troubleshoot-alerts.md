---
title: "Azure Active Directory Domain Services: Řešení potíží s výstrahy | Microsoft Docs"
description: "Řešení potíží s výstrahy pro Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: ergreenl
ms.openlocfilehash: 8a0b30e6c975bd8f3bfbe70a64c085b729115f24
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Služba Azure AD Domain Services – řešení výstrah
Tento článek obsahuje řešení problémů s příručky pro všechny výstrahy, které mohou nastat ve vaší spravované domény.


Vyberte kroky řešení potíží, které odpovídají nebo výstrah ID nebo zpráv, na které narazíte.

| **ID výstrahy** | **Zpráva s výstrahou** | **Řešení** |
| --- | --- | :--- |
| AADDS001 | *Zabezpečený LDAP přes internet je povoleno pro spravované domény. Přístup k portu 636 však není uzamčené pomocí skupiny zabezpečení sítě. To může vystavit uživatelské účty ve spravované doméně k útoku hrubou silou heslo.* | [Nesprávná konfigurace zabezpečeného LDAP](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *Adresář Azure AD, které jsou přidružené k vaší spravované domény může být odstraněna. Spravované domény už není podporovaná konfigurace. Microsoft nelze monitorovat, spravovat, opravy a synchronizovat vaší spravované domény.* | [Chybí složka](#aadds100-missing-directory) |
| AADDS101 | *Azure AD Domain Services nelze povolit v adresáři služby Azure AD B2C.* | [Azure AD B2C je spuštěna v tomto adresáři](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Objekt služby vyžaduje pro správné fungování Azure AD Domain Services se odstranil z adresáře služby Azure AD. Tato konfigurace ovlivní schopnost společnosti Microsoft monitorovat, spravovat, opravy a synchronizovat vaší spravované domény.* | [Chybí instančního objektu](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Rozsah IP adres pro virtuální síť, ve kterém jste povolili službu Azure AD Domain Services je v rozsahu veřejné IP. Služba Azure AD Domain Services musí být povolena ve virtuální síti s rozsah privátních IP adres. Tato konfigurace ovlivní schopnost společnosti Microsoft monitorovat, spravovat, opravy a synchronizovat vaší spravované domény.* | [Adresa je v rozsahu veřejné IP](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Společnost Microsoft se nelze spojit s řadiče domény pro toto spravované domény. To může dojít, pokud skupina zabezpečení sítě (NSG) nakonfigurované na vaší virtuální sítě blokuje přístup k spravované doméně. Jiné možných příčin je, pokud je trasu definovanou uživatelem aby bloky příchozí provoz z Internetu.* | [Chyba sítě](active-directory-ds-troubleshoot-nsg.md) |

## <a name="aadds100-missing-directory"></a>AADDS100: Chybí adresáře
**Zpráva s výstrahou:**

*Adresář Azure AD, které jsou přidružené k vaší spravované domény může být odstraněna. Spravované domény už není podporovaná konfigurace. Microsoft nelze monitorovat, spravovat, opravy a synchronizovat vaší spravované domény.*

**Náprava:**

Tato chyba je obvykle způsobeno nesprávně přesun předplatného Azure k nové Azure AD adresář a odstraňování starý adresář Azure AD, která je stále spojena s Azure AD Domain Services.

Tato chyba Neopravitelná. Pokud chcete vyřešit, je nutné [odstranit stávající spravované domény](active-directory-ds-disable-aadds.md) a znovu ji vytvořte nový adresář. Pokud máte potíže při odstraňování, obraťte se na Azure Active Directory Domain Services produktový tým [pro podporu](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C je spuštěna v tomto adresáři
**Zpráva s výstrahou:**

*Azure AD Domain Services nelze povolit v adresáři služby Azure AD B2C.*

**Náprava:**

>[!NOTE]
>Chcete-li pokračovat v používání služby Azure AD Domain Services, musíte znovu vytvořit vaší instanci Azure AD Domain Services v adresáři Azure AD B2C.

Chcete-li obnovit služby, postupujte takto:

1. [Odstranit vaší spravované domény](active-directory-ds-disable-aadds.md) z existující adresář Azure AD.
2. Vytvořte nový adresář, který není adresář služby Azure AD B2C.
3. Postupujte podle [Začínáme](active-directory-ds-getting-started.md) příručka k opětovnému vytvoření spravované domény.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Adresa je v rozsahu veřejné IP

**Zpráva s výstrahou:**

*Rozsah IP adres pro virtuální síť, ve kterém jste povolili službu Azure AD Domain Services je v rozsahu veřejné IP. Služba Azure AD Domain Services musí být povolena ve virtuální síti s rozsah privátních IP adres. Tato konfigurace ovlivní schopnost společnosti Microsoft monitorovat, spravovat, opravy a synchronizovat vaší spravované domény.*

**Náprava:**

> [!NOTE]
> Chcete-li tento problém vyřešit, musíte odstranit stávající spravované domény a znovu vytvořit ve virtuální síti s rozsah privátních IP adres. Tento proces je rušivý.

Než začnete, přečtěte si **privátní v4 adresní prostor IP adres** kapitoly [v tomto článku](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

Počítače ve virtuální síti, může provádět požadavky prostředky Azure, které jsou v stejného rozsahu IP adres jako porty nakonfigurované pro podsíť. Ale vzhledem k tomu, že virtuální síť je konfigurována pro tento rozsah, v rámci virtuální sítě, budou směrovány tyto požadavky a nebude mít přístup k prostředkům určený web. To může vést k vést k neočekávaným chybám s Azure AD Domain Services.

**Pokud jste vlastníkem rozsahu IP adres v Internetu, který je konfigurován ve virtuální síti, tuto výstrahu můžete ignorovat. Ale Azure AD Domain Services nelze potvrzovat [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)] pomocí této konfigurace, protože může vést k neočekávaným chybám.**


1. [Odstranit vaší spravované domény](active-directory-ds-disable-aadds.md) z adresáře.
2. Opravte rozsah IP adres podsítě
  1. Přejděte na [stránky virtuální sítě na portálu Azure](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Vyberte virtuální síť, které chcete použít pro Azure AD Domain Services.
  3. Klikněte na **adresní prostor** v části Nastavení
  4. Aktualizujte rozsah adres tak, že kliknete na existující rozsah adres a úprav nebo přidání další rozsah adres. Zkontrolujte, zda že je nový rozsah adres v privátní IP rozsahu. Uložte provedené změny.
  5. Klikněte na **podsítě** v levém navigačním panelu.
  6. Klikněte na podsíť, kterou chcete upravit v tabulce.
  7. Aktualizujte rozsah adres a uložte změny.
3. Postupujte podle [průvodci získávání spuštění pomocí Azure AD Domain Services](active-directory-ds-getting-started.md) k opětovnému vytvoření vaší spravované domény. Ujistěte se, že vyberete virtuální síť s rozsah privátních IP adres.
4. Připojení k doméně virtuální počítače do nové domény, postupujte podle [Tato příručka](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Kvůli zajištění bezpečnosti výstrahu vyřešit, zkontrolujte stav vaší doméně dvou hodin.


## <a name="contact-us"></a>Kontaktujte nás
Obraťte se na produktový tým Azure Active Directory Domain Services na [sdílet zpětnou vazbu nebo pro podporu](active-directory-ds-contact-us.md).
