---
title: "Vytvářet a spravovat pravidla brány firewall MySQL v Azure Database pro databázi MySQL | Microsoft Docs"
description: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 11/27/2017
ms.openlocfilehash: 63ea6337b35193420924096690ed15cc1d5ede25
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-portal"></a>Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure
Pravidla brány firewall na úrovni serveru umožňují správcům přístup k databázi Azure pro Server databáze MySQL z zadaná IP adresa nebo rozsah IP adres. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Vytvoření pravidla brány firewall na úrovni serveru na webu Azure Portal

1. Na stránce server MySQL, v části nastavení klikněte na **zabezpečení připojení** chcete otevřít stránku zabezpečení připojení pro databázi Azure pro databázi MySQL.

   ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klikněte na tlačítko **přidat Moje IP** na panelu nástrojů k vytvoření pravidla s IP adresou tohoto počítače, kterou posuzuje systému Azure.

   ![Portál Azure – klikněte na tlačítko Přidat Moje IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Před uložením konfigurace ověřte IP adresu. V některých situacích IP adresu zjištěnými pomocí portálu Azure se liší od adresa IP použitá při přístupu k Internetu a servery Azure. Proto musíte změnit počáteční IP a koncová IP adresa, aby funkce pravidla podle očekávání.

   Pomocí vyhledávacího webu nebo jiný nástroj, online zkontrolujte vlastní IP adresu (například hledání "co je adresa IP").

   ![Bing co je můj IP](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Přidáte další adresní rozsahy. V pravidlech pro databázi Azure pro bránu firewall MySQL můžete zadat jednu IP adresu nebo rozsah adres. Pokud chcete omezit platnost pravidla pro jednu IP adresu, zadejte stejnou adresu v polích Počáteční IP a koncová IP adresa. Otevření brány firewall umožňuje správcům a uživatelům umožňuje přístup k jakékoli databázi na serveru databáze MySQL, ke kterým mají platné přihlašovací údaje.

   ![Portál Azure – pravidla brány firewall ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. Klikněte na tlačítko **Uložit** na panelu nástrojů Uložit toto pravidlo brány firewall na úrovni serveru. Počkejte, než pro potvrzení, že aktualizace, která se pravidla brány firewall byla úspěšně dokončena.

   ![Portál Azure – klikněte na tlačítko Uložit](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-by-using-the-azure-portal"></a>Spravovat existující pravidla brány firewall na úrovni serveru pomocí portálu Azure
Opakujte kroky ke správě pravidla brány firewall.
* Chcete-li přidat počítač, klikněte na tlačítko **+ přidat Moje IP**.
* Pokud chcete přidat další IP adresy, zadejte **název pravidla**, **počáteční IP**, a **KONCOVÁ IP adresa**.
* Pokud chcete upravit existující pravidlo, klikněte na pole v pravidle a potom upravte.
* Pokud chcete odstranit existující pravidlo, klikněte na tlačítko se třemi tečkami [...] a pak klikněte na tlačítko **odstranit**.
* Kliknutím na **Uložit** uložte změny.

## <a name="next-steps"></a>Další kroky
Pomoc při připojování k databázi Azure pro server databáze MySQL, najdete v tématu [knihovny připojení pro databázi Azure pro databázi MySQL](./concepts-connection-libraries.md)
