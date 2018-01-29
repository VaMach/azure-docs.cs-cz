---
title: "Databáze Azure pro pravidla brány firewall serveru MySQL | Microsoft Docs"
description: "Popisuje pravidla brány firewall pro vaši databázi Azure pro server databáze MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/20/2018
ms.openlocfilehash: 15bf032280c9a1d874daa77a6351e092392fee05
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a>Databáze Azure pro pravidla brány firewall serveru MySQL
Brány firewall zabránit veškerý přístup k databázovému serveru, dokud nezadáte, které počítače mají oprávnění. Brána firewall uděluje přístup k serveru na základě původní IP adresy každého požadavku.

Nakonfigurovat bránu firewall, vytvořte pravidla firewallu, která zadejte rozsahy IP adres, přijatelný. Můžete vytvořit pravidla brány firewall na úrovni serveru.

**Pravidla brány firewall:** tato pravidla povolit klientům přístup k vaší celou databázi Azure pro server databáze MySQL, to znamená, všechny databáze v rámci stejného logického serveru. Pravidla brány firewall na úrovni serveru můžete nakonfigurovat pomocí portálu Azure nebo rozhraní příkazového řádku Azure. Pokud chcete vytvořit pravidla brány firewall na úrovni serveru, musí být vlastník předplatného nebo jeho přispěvatelem předplatného.

## <a name="firewall-overview"></a>Přehled brány firewall
Všechny databáze přístup k vaší databázi Azure MySQL serveru je ve výchozím nastavení blokován branou firewall. Pokud chcete začít používat server z jiného počítače, je třeba zadat jeden nebo více pravidel brány firewall na úrovni serveru pro povolení přístupu k serveru. Použití pravidel brány firewall můžete určit IP rozsahy adres z Internetu, a povolit. Přístup k webu portálu Azure, samotné nebude ovlivněné pravidla brány firewall.

Pokusy o připojení z Internetu a Azure musí nejdřív projít přes bránu firewall než dosáhnou Azure databáze pro databázi MySQL, jak je znázorněno v následujícím diagramu:

![Příklad toku fungování brány firewall](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Připojení z Internetu
Pravidla brány firewall na úrovni serveru platí pro všechny databáze na databázi Azure pro server databáze MySQL.

Pokud je IP adresa požadavku v rámci jednoho z rozsahů určených v pravidlech brány firewall na úrovni serveru, je připojení udělen.

Pokud IP adresa požadavku je mimo rozsahů určených v žádné z pravidel brány firewall na úrovni databáze nebo úrovni serveru, požadavek na připojení se nezdaří.

## <a name="connecting-from-azure"></a>Připojení z Azure
Povolit aplikacím z Azure se připojit k vaší databázi Azure pro server databáze MySQL, musí být povoleno Azure připojení. Například k hostování aplikace Azure Web Apps nebo aplikaci, která běží ve virtuálním počítači Azure nebo pro připojení z brány správy dat Azure Data Factory. Prostředky se nemusíte být ve stejné virtuální síť (VNET) nebo skupinu prostředků pro pravidlo brány firewall povolit těchto připojení. Když se aplikace z Azure pokusí připojit k vašemu databázovému serveru, brána firewall ověří, zda jsou povolená připojení Azure. Existuje několik metod, které chcete povolit tyto typy připojení. Nastavení brány firewall s počáteční i koncovou adresou odpovídající 0.0.0.0 znamená, že jsou tato připojení povolená. Alternativně můžete nastavit **povolit přístup ke službám Azure** možnost k **ON** na portálu ze **zabezpečení připojení** podokně a stiskněte klávesu **Uložit**. Pokud pokus o připojení není povolena, požadavek nebylo dosaženo databáze Azure pro server databáze MySQL.

> [!IMPORTANT]
> Touto možností se brána firewall nakonfiguruje tak, aby povolovala všechna připojení z Azure, včetně připojení z předplatných ostatních zákazníků. Když vyberete tuto možnost, ujistěte se, že vaše přihlašovací a uživatelská oprávnění omezují přístup pouze na autorizované uživatele.
> 

![Povolit přístup ke službám Azure konfigurovat na portálu](./media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Programová správa pravidel brány firewall
Kromě portálu Azure můžete pravidla brány firewall programově spravovat pomocí rozhraní příkazového řádku Azure. Viz také [vytvořit a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure](./howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-firewall"></a>Řešení potíží s branou firewall databáze
Při přístupu k databázi Microsoft Azure pro službu MySQL serveru není chovat podle očekávání, zvažte následující body:

* **Změny v seznamu povolených ještě nevstoupilo v platnost:** může být co nejvíce pět minut zpoždění pro změny databáze Azure pro konfiguraci brány firewall serveru MySQL vstoupily v platnost.

* **Přihlášení nemá oprávnění nebo nesprávné heslo byl použit:** Pokud přihlášení nemá oprávnění v databázi Azure pro server databáze MySQL nebo je chybné heslo použít, připojení k databázi Azure MySQL serveru byl odepřen. Vytvoření nastavení brány firewall klientům pouze poskytuje možnost pokusit se o připojení k vašemu serveru – každý klient musí dodat potřebné zabezpečené přihlašovací údaje.

* **Dynamickou IP adresu:** Pokud máte připojení k Internetu pomocí dynamické IP adresy a máte potíže získávání přes bránu firewall, můžete použít jednu z následujících řešení:

* Požádejte poskytovatele služeb Internetu (ISP) pro rozsah IP adres přiřazené v klientských počítačích, které přístup k databázi Azure pro server databáze MySQL a pak přidat rozsah IP adres jako pravidlo brány firewall.

* Získejte pro své klientské počítače statické přidělování IP adres a následně přidejte tyto IP adresy jako pravidla brány firewall.

## <a name="next-steps"></a>Další postup

[Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure](./howto-manage-firewall-using-portal.md)
[vytvořit a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure](./howto-manage-firewall-using-cli.md)
