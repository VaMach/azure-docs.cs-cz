---
title: "Databáze Azure pro pravidla brány firewall serveru PostgreSQL | Microsoft Docs"
description: "Popisuje pravidla brány firewall pro vaši databázi Azure pro PostgreSQL server."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: 7fec71f621ffeff2fc42a5a9464ae9011b2e2fee
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="azure-database-for-postgresql-server-firewall-rules"></a>Databáze Azure pro pravidla brány firewall serveru PostgreSQL
Azure databázi PostgreSQL serverová brána firewall brání veškerý přístup k databázovému serveru, dokud je určit, které počítače mají oprávnění. Brána firewall uděluje přístup k serveru na základě původní IP adresy každého požadavku.
Bránu firewall nakonfigurujete tak, že vytvoříte pravidla brány firewall určující rozsahy přípustných IP adres. Můžete vytvořit pravidla brány firewall na úrovni serveru.

**Pravidla brány firewall:** tato pravidla povolit klientům přístup k celé databáze Azure pro PostgreSQL Server, to znamená, všechny databáze v rámci stejného logického serveru. Pravidla brány firewall na úrovni serveru se dá nakonfigurovat pomocí portálu Azure nebo pomocí rozhraní příkazového řádku Azure. Pokud chcete vytvořit pravidla brány firewall na úrovni serveru, musí být vlastník předplatného nebo jeho přispěvatelem předplatného.

## <a name="firewall-overview"></a>Přehled brány firewall
Všechny databáze přístup k vaší databázi Azure PostgreSQL serveru je blokován branou firewall ve výchozím nastavení. Pokud chcete začít používat server z jiného počítače, je třeba zadat jeden nebo více pravidel brány firewall na úrovni serveru pro povolení přístupu k serveru. Použití pravidel brány firewall můžete určit IP rozsahy adres z Internetu, a povolit. Přístup k webu portálu Azure, samotné nebude ovlivněné pravidla brány firewall.
Pokusy o připojení z Internetu a Azure musí nejdřív projít přes bránu firewall než dosáhnou databáze PostgreSQL, jak je znázorněno v následujícím diagramu:

![Příklad toku fungování brány firewall](media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Připojení z Internetu
Pravidla brány firewall na úrovni serveru platí pro všechny databáze na databázi Azure pro PostgreSQL server. Pokud je IP adresa požadavku v jednom z rozsahů určených v pravidlech brány firewall na úrovni serveru, připojení je povoleno.
Pokud IP adresa požadavku není v rámci rozsahů určených v žádné z pravidel brány firewall na úrovni serveru, požadavek na připojení se nezdaří.
Například pokud vaše aplikace připojí k ovladač JDBC pro PostgreSQL, se může zobrazit tato chyba při pokusu připojit se, když brána firewall blokuje připojení.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: závažná: žádné pg\_hba.conf položka pro uživatele "adminuser" hostitel "123.45.67.890", databáze "postgresql", SSL

## <a name="programmatically-managing-firewall-rules"></a>Programová správa pravidel brány firewall
Kromě portálu Azure lze spravovat pravidla brány firewall programově pomocí rozhraní příkazového řádku Azure.
Viz také [vytvořit a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure](howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-server-firewall"></a>Řešení potíží s brány firewall serveru databáze
Pokud přístup k databázi Microsoft Azure pro službu PostgreSQL Server tak, jak očekáváte, vezměte v úvahu následující body:

* **Změny v seznamu povolených ještě nevstoupilo v platnost:** může být co nejvíce pět minut zpoždění pro změny databáze Azure pro konfiguraci brány firewall serveru PostgreSQL vstoupily v platnost.

* **Přihlášení nemá oprávnění nebo nesprávné heslo byl použit:** Pokud přihlášení nemá oprávnění v databázi Azure pro PostgreSQL server nebo je chybné heslo použít, připojení k databázi Azure PostgreSQL serveru byl odepřen. Vytvoření nastavení brány firewall pouze nabízí klientům s příležitost k pokusu o připojení k serveru. Každý klient musí stále zadejte potřebná zabezpečovací pověření.

Například pomocí klienta JDBC, může objevit následující chyba.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: závažná: selhalo ověřování hesla pro uživatele "uživatelské_jméno"

* **Dynamická IP adresa:** Pokud vaše internetové připojení používá dynamické přidělování IP adres a máte problémy dostat se přes bránu firewall, můžete zkusit jedno z následujících řešení:

* Požádejte poskytovatele služeb Internetu (ISP) pro rozsah IP adres přiřazené v klientských počítačích, které přístup k databázi Azure pro PostgreSQL Server a pak přidat rozsah IP adres jako pravidlo brány firewall.

* Získat statické IP adresy místo pro klientské počítače a poté přidejte statickou IP adresu jako pravidlo brány firewall.

## <a name="next-steps"></a>Další kroky
Články týkající se vytváření pravidel brány firewall na úrovni serveru najdete v části:
* [Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí portálu Azure](howto-manage-firewall-using-portal.md).
* [Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure](howto-manage-firewall-using-cli.md).
