---
title: "Zabezpečení databáze Azure SQL | Microsoft Docs"
description: "Další informace o techniky a funkce zabezpečení databáze Azure SQL."
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 06/28/2017
ms.author: daredis
ms.openlocfilehash: 90c03f1538197e1cd1c90165417a4ec74c9c5961
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="secure-your-azure-sql-database"></a>Zabezpečení databáze Azure SQL

Služba SQL Database chrání vaše data omezením přístupu k databázi pomocí pravidel brány firewall, ověřovacími mechanismy vyžadujícími po uživatelích prokázání identity a autorizací přístupu k datům prostřednictvím členství a oprávnění na základě rolí, stejně jako prostřednictvím zabezpečení na úrovni řádku a dynamického maskování dat.

Můžete zvýšit ochranu databáze před uživateli se zlými úmysly a neoprávněným přístupem pomocí několika jednoduchých kroků. V tomto kurzu jste postup: 

> [!div class="checklist"]
> * Nastavit pravidla brány firewall na úrovni serveru pro váš server na portálu Azure
> * Nastavit pravidla brány firewall na úrovni databáze pro vaši databázi pomocí aplikace SSMS
> * Připojení k vaší databázi pomocí zabezpečené připojovací řetězec
> * Spravovat přístup uživatelů
> * Chraňte svá data pomocí šifrování
> * Povolit auditování databáze SQL
> * Povolení detekce hrozeb databáze SQL

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, ujistěte se, že máte následující:

- Nainstalovanou nejnovější verzi [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). 
- Nainstalovaný Microsoft Excel
- Vytvoření služby Azure SQL server a databáze - najdete v části [vytvoření Azure SQL database na portálu Azure](sql-database-get-started-portal.md), [vytvářet izolované databáze Azure SQL pomocí rozhraní příkazového řádku Azure](sql-database-get-started-cli.md), a [vytvořit jeden SQL Azure databáze pomocí prostředí PowerShell](sql-database-get-started-powershell.md). 

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Vytvoření pravidla brány firewall na úrovni serveru na webu Azure Portal

Databáze SQL jsou chráněné bránou firewall v Azure. Ve výchozím nastavení všechna připojení k serveru a databází uvnitř serveru odmítají s výjimkou připojení z jiných služeb systému Azure. Další informace najdete v tématu [Pravidla brány firewall na úrovni serveru a databáze služby Azure SQL Database](sql-database-firewall-configure.md).

Nejbezpečnější konfiguraci je nastavení "Povolit přístup ke službám Azure" na hodnotu OFF. Pokud potřebujete připojení k databázi ze služby virtuální počítač Azure nebo cloudu, měli byste vytvořit [vyhrazenou IP adresu](../virtual-network/virtual-networks-reserved-public-ip.md) a povolit pouze vyhrazené IP adresy přístup přes bránu firewall. 

Postupujte podle těchto kroků můžete vytvořit [pravidlo brány firewall na úrovni serveru SQL Database](sql-database-firewall-configure.md) pro váš server umožňuje připojení z konkrétní IP adresu. 

> [!NOTE]
> Pokud jste vytvořili ukázkové databáze v Azure pomocí jedné z předchozích kurzy nebo – elementy QuickStart a fungují v tomto kurzu v počítači se stejnou adresou IP, který měl při si projít tyto kurzy, můžete tento krok přeskočit, protože už je nutné vytvořit ated pravidlo brány firewall na úrovni serveru.
>

1. Klikněte na tlačítko **databází SQL** z nabídky levé straně a klikněte na databázi, kterou chcete nakonfigurovat bránu firewall pravidla pro u **databází SQL** stránky. Otevře se stránka Přehled pro vaši databázi, ukazuje název plně kvalifikovaný serveru (například **mynewserver 20170313.database.windows.net**) a poskytuje možnosti pro další konfiguraci.

      ![pravidlo brány firewall serveru](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Klikněte na **Nastavit bránu firewall serveru** na panelu nástrojů, jak je vidět na předchozím obrázku. Otevře se stránka **Nastavení brány firewall** pro server služby SQL Database. 

3. Klikněte na tlačítko **přidat IP adresu klienta** na panelu nástrojů přidejte veřejnou IP adresu počítače, připojení se k portálu a zadejte pravidlo brány firewall ručně a pak klikněte na tlačítko **Uložit**.

      ![nastavení pravidla brány firewall serveru](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. Kliknutím na **OK** a pak na **X** zavřete stránku **Nastavení brány firewall**.

Nyní můžete připojit k jakékoli databázi na serveru se zadanou IP adresu nebo rozsah IP adres.

> [!NOTE]
> SQL Database komunikuje přes port 1433. Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 1433 bránou firewall vaší sítě povolený. Pokud je to tak, nebudete se moct připojit k serveru Azure SQL Database, dokud vaše IT oddělení neotevře port 1433.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>Vytvoření pravidla brány firewall na úrovni databáze pomocí aplikace SSMS

Pravidla brány firewall na úrovni databáze umožňují vytvořit jinou bránu firewall nastavení pro jiné databáze v rámci stejného logického serveru a k vytvoření pravidla brány firewall, které jsou přenosné - znamená, že následují databáze během [převzetíslužebpřiselhání](sql-database-geo-replication-overview.md) místo uložené na serveru SQL server. Pravidla brány firewall na úrovni databáze může být pouze nakonfigurovaná pomocí příkazů Transact-SQL a pouze po nakonfigurování prvního pravidla brány firewall na úrovni serveru. Další informace najdete v tématu [Pravidla brány firewall na úrovni serveru a databáze služby Azure SQL Database](sql-database-firewall-configure.md).

Postupujte podle následujících kroků k vytvoření pravidla brány firewall pro specifické pro databázi.

1. Připojení ke své databázi, například pomocí [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. V Průzkumníku objektů, klikněte pravým tlačítkem na databázi, kterou chcete přidat pravidlo brány firewall pro a klikněte na **nový dotaz**. Otevře se prázdné okno dotazu připojené k vaší databázi.

3. V okně dotazu změnit IP adresu na veřejnou IP adresu a potom spusťte následující dotaz:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Na panelu nástrojů klikněte na tlačítko **Execute** a vytvořte tak pravidlo brány firewall.

## <a name="view-how-to-connect-an-application-to-your-database-using-a-secure-connection-string"></a>Zobrazit postup připojení aplikace k vaší databázi pomocí zabezpečené připojovací řetězec

Aby zabezpečené, šifrované připojení mezi klientská aplikace a SQL Database, připojovací řetězec má nakonfigurovat tak, aby:

- Žádosti o šifrované připojení, a
- Není důvěřovat certifikátu serveru. 

Tím se naváže připojení pomocí zabezpečení TLS (Transport Layer) a snižuje riziko útoků man-in-the-middle. Můžete získat správně nakonfigurované připojovací řetězce pro vaši databázi SQL pro podporované klientské ovladače z portálu Azure jak je vidět na tomto snímku obrazovky pro technologii ADO.net.

1. Vyberte **databází SQL** z nabídky na levé straně a klikněte na databázi **databází SQL** stránky.

2. Na **přehled** stránky pro vaši databázi, klikněte na tlačítko **zobrazit databázové připojovací řetězce**.

3. Zkontrolujte úplný připojovací řetězec **ADO.NET**.

    ![Připojovací řetězec pro ADO.NET](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a>Vytváření uživatelů databáze

Před vytvořením všechny uživatele, musíte nejprve zvolit jednu z Azure SQL Database podporuje dva typy ověřování: 

**Ověřování SQL**, který používá uživatelské jméno a heslo pro přihlášení a uživatele, které jsou platné pouze v kontextu konkrétní databáze v rámci logického serveru. 

**Ověřování služby Azure Active Directory**, identity, které jsou spravované službou Azure Active Directory, který používá. 

Pokud chcete použít [Azure Active Directory](./sql-database-aad-authentication.md) k ověření proti databázi SQL, musí existovat vyplněná Azure Active Directory, abyste mohli pokračovat.

Postupujte podle těchto kroků můžete vytvořit uživatele pomocí ověřování SQL:

1. Připojení ke své databázi, například pomocí [SQL Server Management Studio](./sql-database-connect-query-ssms.md) pomocí svých přihlašovacích údajů správce serveru.

2. V Průzkumníku objektů, klikněte pravým tlačítkem na databázi, kterou chcete přidat nového uživatele na a klikněte na tlačítko **nový dotaz**. Otevře okno prázdné dotazu, který je připojen k vybrané databázi.

3. Do okna dotazu zadejte následující dotaz:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Na panelu nástrojů klikněte na tlačítko **Execute** vytvořit uživateli.

5. Ve výchozím nastavení uživatel může připojit k databázi, ale nemá oprávnění číst nebo zapisovat data. Udělení těchto oprávnění pro nově vytvořeného uživatele, spusťte následující dva příkazy v novém okně dotazu

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

Je vhodné vytvořit tyto účty bez oprávnění správce na úrovni databáze pro připojení k databázi, pokud potřebujete provést úlohy správce, jako je vytváření nových uživatelů. Přečtěte si [kurz služby Azure Active Directory](./sql-database-aad-authentication-configure.md) o tom, jak ověřit pomocí služby Azure Active Directory.


## <a name="protect-your-data-with-encryption"></a>Chraňte svá data pomocí šifrování

Azure SQL Database transparentní šifrování dat (šifrování TDE) automaticky šifruje vaše data v klidu, bez nutnosti změny k aplikaci přístup k databázi šifrované. Pro nově vytvořené databáze je ve výchozím nastavení šifrování TDE. Chcete-li povolit šifrování TDE pro vaši databázi nebo ověřte, zda je šifrování TDE na, postupujte takto:

1. Vyberte **databází SQL** z nabídky na levé straně a klikněte na databázi **databází SQL** stránky. 

2. Klikněte na **transparentní šifrování dat** otevřete stránku konfigurace pro šifrování TDE.

    ![Transparentní šifrování dat](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. V případě potřeby nastavte **šifrování dat** na hodnotu ON a klikněte na tlačítko **Uložit**.

Spustí se proces šifrování na pozadí. Průběh můžete sledovat pomocí připojení k databázi SQL pomocí [SQL Server Management Studio](./sql-database-connect-query-ssms.md) dotazováním encryption_state sloupec `sys.dm_database_encryption_keys` zobrazení.

## <a name="enable-sql-database-auditing-if-necessary"></a>Povolit auditování databáze SQL, v případě potřeby

Auditování databáze SQL Azure sleduje události databáze a zápisu, které mají auditu protokolu ve vašem účtu úložiště Azure. Auditování vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou případné porušení zabezpečení. Postupujte podle těchto kroků můžete vytvořit výchozí zásady pro vaši databázi SQL auditování:

1. Vyberte **databází SQL** z nabídky na levé straně a klikněte na databázi **databází SQL** stránky. 

2. V okně Nastavení vyberte **auditování a detekce hrozeb**. Všimněte si, že auditování na úrovni serveru je diabled a zda je **zobrazit nastavení serveru** odkaz, který umožňuje zobrazit nebo upravit nastavení auditování ze tento kontext serveru.

    ![Auditování okno](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Pokud dáváte přednost umožňuje typ auditu (nebo umístění?), než je zadaná na úrovni serveru, zapněte **ON** auditování a vyberte **Blob** typu auditování. Pokud je auditování objektů Blob serveru je povoleno, audit databáze nakonfigurována, budou existovat node souběžně s auditování objektů Blob serveru.

    ![Zapnout auditování](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. Vyberte **podrobnosti úložiště** otevřete okno úložiště protokoly auditu. Vyberte účet úložiště Azure, kde budou uloženy protokoly, a pak klikněte na dobu uchování, po jejímž uplynutí se odstraní starých protokolů, **OK** dole. 

   > [!TIP]
   > Použijte stejný účet úložiště pro všechny databáze, auditované k plnému využití mimo auditování šablony sestavy.
   > 

5. Klikněte na **Uložit**.

> [!IMPORTANT]
> Pokud chcete přizpůsobit auditované události, můžete k tomu pomocí prostředí PowerShell nebo rozhraní REST API – viz [auditování databáze SQL](sql-database-auditing.md) další podrobnosti.
>

## <a name="enable-sql-database-threat-detection"></a>Povolení detekce hrozeb databáze SQL

Detekce hrozeb poskytuje novou vrstvu zabezpečení, která uživatelům umožňuje zjistit a reagovat na potenciální hrozby, kdy k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity. Uživatele můžete prozkoumat podezřelé události pomocí auditování databáze SQL k určení, pokud vyplývají z pokus o přístup, porušení nebo využívat data v databázi. Detekce hrozeb zjednodušuje na potenciální hrozby adres do databáze bez nutnosti odborné zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.
Například detekce hrozeb zjistila určité nezvyklé databázové aktivity, které indikují potenciální pokusu o Injektáž SQL. Injektáž SQL je jedním z běžné problémy zabezpečení webových aplikací na Internetu, slouží k útokům na základě dat aplikace. Útočníci využít výhod vložit škodlivý příkazy SQL, do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi aplikace ohrožení zabezpečení.

1. Přejděte do okna konfigurace SQL databáze, kterou chcete monitorovat. V okně Nastavení vyberte **auditování a detekce hrozeb**.

    ![Navigační podokno](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. V **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby.

3. Zapnout **ON** detekce hrozby.

4. Konfigurace seznamu e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklé databázové aktivity.

5. Klikněte na tlačítko **Uložit** v **auditování a detekce hrozeb** uložíte nové nebo aktualizované auditování a zásady detekce hrozby.

    ![Navigační podokno](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Pokud se zjistila nezvyklé databázové aktivity, obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity. E-mailu bude poskytují informace o události podezřelé zabezpečení, včetně povahu neobvyklé aktivity, název databáze, název serveru a čas události. Kromě toho bude poskytovat informace na možné příčiny a doporučené akce ke zkoumání a zmírnit potenciální hrozbu do databáze. V dalších krocích vás prostřednictvím co dělat, by měl obdržíte e-mailu:

    ![E-mailu detekce hrozeb](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. V e-mailu, klikněte na **protokolu auditování databáze SQL Azure** odkaz, který bude spuštění portálu Azure a zobrazit relevantní auditování záznamy v době podezřelé události.

    ![Záznamy auditu](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Klikněte na záznamy auditu zobrazíte další podrobnosti v databázi podezřelé aktivity, například příkazy SQL, selhání důvod a klient IP.

    ![Podrobnosti záznamu](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. V okně auditování záznamy, klikněte na tlačítko **otevřete v aplikaci Excel** otevřete předem nakonfigurovaná v aplikaci excel šablony k importu a spuštění hlubší analýzy protokol auditu v době podezřelé události.

    > [!NOTE]
    > V aplikaci Excel 2010 nebo novější, Power Query a **rychle zkombinovat** nastavení je povinné.

    ![Otevřené záznamy v aplikaci Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. Ke konfiguraci **rychle zkombinovat** nastavení - v **POWER QUERY** pásu karet vyberte **možnosti** zobrazíte dialogové okno Možnosti. Vyberte v části o ochraně osobních údajů a vyberte druhou možnost - 'Ignorovat úrovně soukromí a potenciálně tak vylepšit výkon':

    ![Kombinování rychlé aplikace Excel](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. Načíst protokoly auditu SQL, zajistěte, aby parametrů v nastavení kartě jsou správně nastavena a poté vyberte pásu karet, Data a klikněte na tlačítko 'aktualizovat vše.

    ![Parametry aplikace Excel](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. Výsledky se zobrazí v **protokoly auditu SQL** listu, která umožňuje spustit hlubší analýzu neobvyklé aktivity, které byly zjištěny a zmírnit dopad událostí zabezpečení ve vaší aplikaci.


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli pro zvýšení ochrany proti vaší databáze před uživateli se zlými úmysly a neoprávněným přístupem pomocí jenom pár jednoduchých kroků.  Naučili jste se tyto postupy: 

> [!div class="checklist"]
> * Nastavení pravidel brány firewall pro váš server nebo databáze
> * Připojení k vaší databázi pomocí zabezpečené připojovací řetězec
> * Spravovat přístup uživatelů
> * Chraňte svá data pomocí šifrování
> * Povolit auditování databáze SQL
> * Povolení detekce hrozeb databáze SQL

Přechodu na v dalším kurzu se dozvíte, jak implementovat geograficky distribuovaná databáze.

> [!div class="nextstepaction"]
>[Implementace geograficky distribuované databáze](sql-database-implement-geo-distributed-database.md)

