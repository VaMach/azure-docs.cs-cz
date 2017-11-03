---
title: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí portálu Azure | Microsoft Docs"
description: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí portálu Azure"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 5d62a138313ae3568470cbf5a8eb191321cd6cc7
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-the-azure-portal"></a>Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí portálu Azure
Pravidla brány firewall na úrovni serveru umožňují správcům přístup k databázi Azure pro PostgreSQL Server z zadaná IP adresa nebo rozsah IP adres. 

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- Server [vytvoření databáze Azure pro PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Vytvoření pravidla brány firewall na úrovni serveru na webu Azure Portal
1. V okně PostgreSQL server v části nastavení klikněte na **zabezpečení připojení** otevřete okno zabezpečení připojení pro databázi Azure pro PostgreSQL.

  ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klikněte na tlačítko **přidat Moje IP** na panelu nástrojů. Automaticky tím se vytvoří pravidlo brány firewall s veřejnou IP adresu počítače, kterou posuzuje systému Azure.

  ![Portál Azure – klikněte na tlačítko Přidat Moje IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Před uložením konfigurace ověřte IP adresu. V některých situacích IP adresu zjištěnými pomocí portálu Azure se liší od adresa IP použitá při přístupu k Internetu a servery Azure. Proto musíte změnit počáteční IP a koncová IP adresa, aby funkce pravidla podle očekávání.
Pomocí vyhledávacího webu nebo jiný nástroj, online zkontrolujte vlastní IP adresu (například Bing vyhledávání se "Jaký je můj IP").

  ![Co je můj IP Bing hledání](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Přidáte další adresní rozsahy. V pravidlech brány firewall pro databázi Azure pro PostgreSQL můžete zadat jednu IP adresu nebo rozsah adres. Pokud chcete omezit platnost pravidla pro jednu IP adresu, zadejte stejnou adresu v poli pro počáteční IP a koncová IP adresa. Otevření brány firewall umožňuje správci, uživatelé a aplikace se můžete přihlásit k jakékoli databázi na serveru PostgreSQL, k němuž mají platné přihlašovací údaje.

  ![Portál Azure – pravidla brány firewall ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Klikněte na tlačítko **Uložit** na panelu nástrojů Uložit toto pravidlo brány firewall na úrovni serveru. Počkejte, než pro potvrzení, že aktualizace, která se pravidla brány firewall bylo úspěšné.

  ![Portál Azure – klikněte na tlačítko Uložit](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Správa stávajících pravidel brány firewall na úrovni serveru na webu Azure Portal
Opakujte kroky ke správě pravidla brány firewall.
* Pokud chcete přidat počítač, kliknutím na tlačítko + **přidat Moje IP**. Kliknutím na **Uložit** uložte změny.
* Pro přidání dalších IP adres zadejte Název pravidla, Počáteční IP adresu a Koncovou IP adresu. Kliknutím na **Uložit** uložte změny.
* Pokud chcete upravit stávající pravidlo, klikněte na libovolné pole pravidla a upravte ho. Kliknutím na **Uložit** uložte změny.
* Pokud chcete odstranit existující pravidlo, klikněte na tlačítko se třemi tečkami [...] a klikněte na tlačítko **odstranit** pro odebrání pravidla. Kliknutím na **Uložit** uložte změny.

## <a name="next-steps"></a>Další kroky
- Podobně můžete skript pro [vytvořit a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure](howto-manage-firewall-using-cli.md).
- Pomoc při připojování k databázi Azure pro PostgreSQL server najdete v tématu [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md).
