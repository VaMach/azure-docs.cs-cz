---
title: "Rychlý start: Vytvoření serveru Azure Database for MySQL – Azure Portal | Dokumentace Microsoftu"
description: "Tento článek vás provede rychlým vytvořením ukázkového serveru Azure Database for MySQL v pěti minutách pomocí portálu Azure Portal."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/15/2017
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 829c7e73cbf22d866bbd6fd54edc7a954ad7174c
ms.contentlocale: cs-cz
ms.lasthandoff: 08/16/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Vytvoření serveru Azure Database for MySQL pomocí portálu Azure Portal
Azure Database for MySQL je spravovaná služba, která umožňuje spouštět, spravovat a škálovat vysoce dostupné databáze MySQL v cloudu. V tomto rychlém startu se dozvíte, jak přibližně během pěti minut vytvořit server Azure Database for MySQL pomocí webu Azure Portal. 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.
Otevřete svůj webový prohlížeč a přejděte na [portál Microsoft Azure Portal](https://portal.azure.com/). Zadejte přihlašovací údaje pro přihlášení k portálu. Výchozím zobrazením je váš řídicí panel služby.

## <a name="create-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Server Azure Database for MySQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md). Server se vytvoří v rámci [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md).

Server Azure Database for MySQL vytvoříte pomocí tohoto postupu:

1. Klikněte na tlačítko **Nový** (+) v levém horním rohu webu Azure Portal.

2. Na stránce **Nový** vyberte **Databáze** a na stránce **Databáze** vyberte **Azure Database for MySQL**. Službu můžete najít také zadáním **MySQL** do vyhledávacího pole na stránce Nový.
![Azure Portal – nový – databáze - MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Vyplňte formulář podrobností nového serveru pomocí následujících informací, jak je vidět na předchozím obrázku:

    **Nastavení** | **Navrhovaná hodnota** | **Popis pole** 
    ---|---|---
    Název serveru | myserver4demo | Zvolte jedinečný název serveru, který identifikuje váš server Azure Database for MySQL. K názvu serveru, který zadáte pro připojování aplikací, se připojí název domény *mysql.database.azure.com*. Název serveru může obsahovat pouze malá písmena, číslice a znak spojovníku (-) a musí se skládat ze 3 až 63 znaků.
    Předplatné | Vaše předplatné | Předplatné Azure, které chcete použít pro váš server. Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém se prostředek účtuje.
    Skupina prostředků | myresourcegroup | Můžete vytvořit nový název skupiny prostředků nebo použít některý ze stávajících ve vašem předplatném.
    Přihlašovací jméno správce serveru | myadmin | Vytvořte si vlastní přihlašovací účet, který budete používat při připojování k serveru. Přihlašovací jméno správce nemůže být azure_superuser, admin, administrator, root, guest ani public.
    Heslo | *Nějaké si zvolte* | Vytvořte nové heslo pro účet správce serveru. Musí se skládat z 8 až 128 znaků. Heslo musí obsahovat znaky ze tří z těchto kategorií – velká písmena anglické abecedy, malá písmena anglické abecedy, číslice (0–9) a jiné než alfanumerických znaky (!, $, #, % apod.).
    Potvrzení hesla | *Nějaké si zvolte*| Potvrďte heslo účtu správce.
    Umístění | *Oblast nejbližší vašim uživatelům*| Vyberte umístění co nejblíže vašim uživatelům nebo dalším aplikacím Azure.
    Verze | *Zvolte nejnovější verzi*| Zvolte nejnovější verzi, pokud nemáte specifické požadavky.
    Cenová úroveň | **Basic**, **50 výpočetních jednotek**, **50 GB** | Klikněte na **Cenová úroveň** a určete úroveň služby a úroveň výkonu pro novou databázi. Na kartě v horní části zvolte **Úroveň Basic**. Klikněte na levý konec posuvníku **Výpočetní jednotky** a pro účely tohoto rychlého startu upravte hodnotu na nejnižší dostupné množství. Kliknutím na **OK** uložte výběr cenové úrovně. Viz následující snímek obrazovky.
    Připnutí na řídicí panel | Zaškrtnout | Zaškrtněte možnost **Připnout na řídicí panel**, abyste povolili snadné sledování vašeho serveru na přední stránce řídicího panelu na webu Azure Portal.

    > [!IMPORTANT]
    > Zde zadané přihlašovací jméno a heslo správce serveru se vyžadují pro přihlášení k serveru a jeho databázím dále v tomto rychlém startu. Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.
    > 

    ![Azure Portal – vytvoření databáze MySQL poskytnutím požadované vstupu formuláře](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

4.  Klikněte na **Vytvořit**, aby se server zřídil. Zřizování trvá několik minut, maximálně však 20 minut.
   
5.  Pokud chcete monitorovat proces nasazení, klikněte na **Oznámení** (ikona zvonku) na panelu nástrojů.

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurace pravidla brány firewall na úrovni serveru

Služba Azure Database for MySQL vytváří bránu firewall na úrovni serveru. Tato brána firewall brání externím aplikacím a nástrojům v připojení k serveru a kterékoli databázi na serveru, pokud není vytvořené pravidlo brány firewall k otevření brány firewall pro konkrétní IP adresy. 

1.  Po dokončení nasazení najděte váš server. V případě potřeby ho můžete vyhledat. Klikněte například na **Všechny prostředky** v levé nabídce a zadejte název serveru (například ukázkový *myserver4demo*). Vyhledáte tak nově vytvořený server. Klikněte na název vašeho serveru uvedený ve výsledcích hledání. Otevře se stránka **Přehled** vašeho serveru a poskytne vám možnosti další konfigurace.

2. Na stránce serveru vyberte **Zabezpečení připojení**.

3.  Pod nadpisem **Pravidla brány firewall** klikněte do prázdného textového pole ve sloupci **Název pravidla** a začněte vytvářet pravidlo brány firewall. 

    Pro účely tohoto rychlého startu povolíme provoz do serveru ze všech IP adres tak, že do textového pole v každém sloupci vyplníme následující hodnoty:

    Název pravidla | Počáteční IP adresa | Koncová IP adresa 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Na horním panelu nástrojů na stránce **Zabezpečení připojení** klikněte na **Uložit**. Chvíli počkejte, než se zobrazí oznámení o úspěšném dokončení aktualizace zabezpečení připojení, a pak pokračujte.

    > [!NOTE]
    > Připojení ke službě Azure Database for MySQL komunikují přes port 3306. Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 3306 bránou firewall vaší sítě povolený. Pokud je to tak, k serveru se nebudete moct připojit, dokud vaše IT oddělení neotevře port 3306.
    > 

## <a name="get-the-connection-information"></a>Získání informací o připojení
Pokud se chcete připojit ke svému databázovému serveru, je potřeba si vzpomenout na úplný název serveru a přihlašovací údaje správce. Pravděpodobně jste si tyto hodnoty poznamenali v dřívější části tohoto článku Rychlý start. Pokud ne, název serveru a přihlašovací informace můžete snadno vyhledat na stránce **Přehled** nebo **Vlastnosti** serveru na webu Azure Portal.

1. Otevřete stránku **Přehled** vašeho serveru. Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**. 
    Přejeďte kurzorem přes jednotlivá pole a vpravo od textu se zobrazí ikona kopírování. Podle potřeby hodnoty zkopírujte kliknutím na ikonu kopírování.

    V tomto příkladu je název serveru *myserver4demo.mysql.database.azure.com* a přihlašovací jméno správce serveru je *myadmin@myserver4demo*.

## <a name="connect-to-mysql-using-mysql-command-line-tool"></a>Připojení k MySQL pomocí nástroje pro příkazový řádek mysql
Existuje řada aplikací, které můžete použít pro připojení k vašemu serveru Azure Database for MySQL. Nejprve si ukážeme, jak se k serveru připojit pomocí nástroje pro příkazový řádek [mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).  Můžete použít webový prohlížeč a Azure Cloud Shell, jak je zde popsáno, aniž by bylo nutné instalovat další software. Pokud máte nástroj mysql nainstalovaný místně na svém počítači, můžete se připojit i z něj.

1. Pomocí ikony terminálu ( >_ ) vpravo nahoře na webu Azure Portal spusťte službu Azure Cloud Shell.

2. Azure Cloud Shell se otevře v prohlížeči a umožní vám zadávat příkazy prostředí Bash.

    ![Příkazový řádek – příklad příkazového řádku mysql](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

3. V příkazovém řádku služby Cloud Shell se zadáním příkazového řádku mysql na zeleném řádku připojte k serveru Azure Database for MySQL.

    Pro připojení k serveru Azure Database for MySQL pomocí nástroje mysql se používá následující formát:
    ```bash
    mysql --host <yourserver> --user <server admin login> --password
    ```

    Například následující příkaz se připojí k našemu ukázkovému serveru:
    ```azurecli-interactive
    mysql --host myserver4demo.mysql.database.azure.com --user myadmin@myserver4demo --password
    ```

    Parametr mysql |Navrhovaná hodnota|Popis
    ---|---|---
    --host | *název serveru* | Zadejte hodnotu názvu serveru, kterou jste použili dříve při vytváření služby Azure Database for MySQL. Náš ukázkový server v příkladu je myserver4demo.mysql.database.azure.com. Použijte plně kvalifikovaný název domény (\*.mysql.database.azure.com), jak je znázorněno v příkladu. Pokud si název vašeho serveru nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. 
    --user | *přihlašovací jméno správce serveru* |Zadejte přihlašovací uživatelské jméno správce serveru, které jste zadali dříve při vytváření služby Azure Database for MySQL. Pokud si uživatelské jméno nepamatujete, získejte informace o připojení pomocí postupu v předchozí části.  Formát je *username@servername*.
    --password | *počkejte na zobrazení výzvy* | Po zadání příkazu se zobrazí výzva k zadání hesla. Po zobrazení výzvy zadejte stejné heslo, které jste zadali při vytváření serveru.  Poznámka: při zadávání hesla se na příkazovém řádku Bash zadávané znaky nezobrazí. Po zadání všech znaků stiskněte enter, abyste se ověřili a připojili.

   Jakmile budete připojeni, nástroj mysql zobrazí příkazový řádek `mysql>`, kde můžete zadávat příkazy. 

    Příklad výstupu nástroje mysql:
    ```bash
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 65505
    Server version: 5.6.26.0 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql>
    ```
    > [!TIP]
    > Pokud brána firewall není nakonfigurovaná k povolení IP adres služby Azure Cloud Shell, dojde k následující chybě:
    >
    > ERROR 2003 (28000): Client with IP address 123.456.789.0 is not allowed to access the server. (CHYBA 2003 (28000): Klient s IP adresou 123.456.789.0 nemá povolený přístup k tomuto serveru.)
    >
    > Pokud chcete chybu vyřešit, ujistěte se, že konfigurace serveru odpovídá postupu v části *Konfigurace pravidla brány firewall na úrovni serveru* tohoto článku.

4. Zobrazte stav serveru a ujistěte se, že je připojení funkční. Po připojení zadejte na příkazovém řádku mysql> `status`.
    ```sql
    status
    ```

   > [!TIP]
   > Další příkazy najdete v [Referenční příručce k MySQL 5.7 – v kapitole 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

5.  Vytvořte prázdnou databázi zadáním následujícího příkazu na příkazovém řádku mysql>:
    ```sql
    CREATE DATABASE quickstartdb;
    ```
    Dokončení příkazu může chvíli trvat. 

    V rámci serveru Azure Database for MySQL můžete vytvořit jednu nebo několik databází. Můžete se rozhodnout vytvořit jednu databázi na server pro využití všech prostředků nebo vytvořit několik databází, které budou prostředky sdílet. Neexistuje žádné omezení počtu databází, které je možné vytvořit. Více databází ale sdílí stejné prostředky serveru. 

6. Zobrazte výpis databází zadáním následujícího příkazu na příkazovém řádku mysql>:

    ```sql
    SHOW DATABASES;
    ```

7.  Zadejte `\q` a stisknutím klávesy ENTER ukončete nástroj mysql. Jakmile budete hotovi, můžete zavřít Azure Cloud Shell.

Právě jste se připojili ke službě Azure Database for MySQL a vytvořili prázdnou uživatelskou databázi. Pokračujte k další části, kde zopakujete podobné cvičení a ke stejnému serveru se připojíte pomocí dalšího běžného nástroje MySQL Workbench.

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Připojení k serveru pomocí nástroje grafického uživatelského rozhraní MySQL Workbench
Připojení k serveru Azure MySQL pomocí nástroje s grafickým uživatelským rozhraním MySQL Workbench:

1.  Na klientském počítači spusťte aplikaci MySQL Workbench. MySQL Workbench můžete stáhnout a nainstalovat [odtud](https://dev.mysql.com/downloads/workbench/).

2.  V dialogovém okně pro **nastavení nového připojení** zadejte na kartě **Parametry** následující informace:

    ![nastavení nového připojení](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

    | **Nastavení** | **Navrhovaná hodnota** | **Popis pole** |
    |---|---|---|
    |   Název připojení | Ukázkové připojení | Zadejte popisek pro toto připojení. |
    | Způsob připojení | Standard (TCP/IP) | Standard (TCP/IP) je dostačující. |
    | Název hostitele | *název serveru* | Zadejte hodnotu názvu serveru, kterou jste použili dříve při vytváření služby Azure Database for MySQL. Náš ukázkový server v příkladu je myserver4demo.mysql.database.azure.com. Použijte plně kvalifikovaný název domény (\*.mysql.database.azure.com), jak je znázorněno v příkladu. Pokud si název vašeho serveru nepamatujete, získejte informace o připojení pomocí postupu v předchozí části.  |
    | Port | 3306 | Při připojování ke službě Azure Database for MySQL vždy používejte port 3306. |
    | Uživatelské jméno |  *přihlašovací jméno správce serveru* | Zadejte přihlašovací uživatelské jméno správce serveru, které jste zadali dříve při vytváření služby Azure Database for MySQL. Uživatelské jméno v našem příkladu je myadmin@myserver4demo. Pokud si uživatelské jméno nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. Formát je *username@servername*.
    | Heslo | vaše heslo | Kliknutím na tlačítko Uložit v trezoru... heslo uložte. |

    Pokud chcete otestovat, jestli jsou všechny parametry správně nakonfigurované, klikněte na **Test připojení**. Kliknutím na tlačítko OK uložte připojení. 

    > [!NOTE]
    > Ve výchozím nastavení se na vašem serveru vynucuje SSL, což vyžaduje další konfiguraci, která zajistí úspěšné připojení. Podívejte se na téma [Konfigurace připojení SSL v aplikaci pro zabezpečené připojení k Azure Database for MySQL](./howto-configure-ssl.md).  Pokud chcete pro tento rychlý start zakázat SSL, přejděte na web Azure Portal, klikněte na stránku Zabezpečení připojení a deaktivujte přepínací tlačítko Vynucení připojení SSL.

## <a name="clean-up-resources"></a>Vyčištění prostředků
Vyčistěte prostředky, které jste vytvořili v rámci tohoto rychlého startu, buď odstraněním [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md), což zahrnuje odstranění všech prostředků ve skupině prostředků, nebo odstraněním příslušného prostředku serveru, pokud chcete ostatní prostředky ponechat beze změn.

> [!TIP]
> Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. Pokud chcete pokračovat v práci s dalšími rychlými starty, neprovádějte čištění prostředků vytvořených v rámci tohoto rychlého startu. Pokud pokračovat nechcete, pomocí následujících kroků odstraňte všechny prostředky vytvořené tímto rychlým startem na portálu Azure Portal.
>

Pokud chcete odstranit celou skupinu prostředků, včetně nově vytvořeného serveru:
1.  Vyhledejte skupinu prostředků na webu Azure Portal. V nabídce vlevo na webu Azure Portal klikněte na **Skupiny prostředků** a potom klikněte na název vaší skupiny prostředků, jako je **myresourcegroup** v našem příkladu.
2.  Na stránce vaší skupiny prostředků klikněte na **Odstranit**. Pak pro potvrzení odstranění zadejte do textového pole název vaší skupiny prostředků, jako je **myresourcegroup** v našem příkladu, a klikněte na **Odstranit**.

Pokud místo toho chcete odstranit nově vytvořený server:
1.  Vyhledejte server na webu Azure Portal, pokud ho ještě nemáte otevřený. V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte server, který jste vytvořili.
2.  Na stránce **Přehled** klikněte na tlačítko **Odstranit** v horním podokně.
![Azure Database for MySQL – odstranění serveru](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png)
3.  Potvrďte název serveru, který chcete odstranit, a zobrazte jeho databáze, které tím ovlivníte. Do textového pole zadejte název vašeho serveru, jako je **myserver4demo** v našem příkladu, a potom klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Návrh první databáze Azure Database for MySQL](./tutorial-design-database-using-portal.md)


