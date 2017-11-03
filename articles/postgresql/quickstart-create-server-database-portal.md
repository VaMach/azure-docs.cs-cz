---
title: "Portál Azure: vytvoření databáze Azure pro PostgreSQL server | Microsoft Docs"
description: "Rychlé spuštění průvodce k vytváření a správě Azure Database pro PostgreSQL server pomocí portálu Azure uživatelské rozhraní."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 08/10/2017
ms.openlocfilehash: 3a76e816f9b1fa484789f548899d7e8e7043febb
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="create-an-azure-database-for-postgresql-server-in-the-azure-portal"></a>Vytvoření databáze Azure pro PostgreSQL server na portálu Azure

Azure databázi PostgreSQL je spravovaná služba, který používáte pro spouštění, spravovat a škálování vysoce dostupné databáze PostgreSQL v cloudu. Tento rychlý start se dozvíte, jak k vytvoření databáze Azure pro PostgreSQL server přibližně pět minut pomocí portálu Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet Azure](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal
Otevřete webový prohlížeč a přejděte na [portál](https://portal.azure.com/). Zadejte přihlašovací údaje pro přihlášení k portálu. Výchozím zobrazením je váš řídicí panel služby.

## <a name="create-an-azure-database-for-postgresql-server"></a>Vytvoření serveru Azure Database for PostgreSQL

Server Azure Database for PostgreSQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md). Server se vytvoří v rámci [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md).

K vytvoření databáze Azure pro PostgreSQL server, proveďte následující kroky:
1. Vyberte tlačítko **Nový** (+) v levém horním rohu portálu.

2. Vyberte **databáze** > **Azure databázi PostgreSQL**.

    ![Možnost "Azure databáze pro PostgreSQL"](./media/quickstart-create-database-portal/1-create-database.png)

3. Vyplňte formulář podrobností nového serveru pomocí následujících informací, jak je vidět na předchozím obrázku:

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    Název serveru |*mypgserver-20170401*|Jedinečný název, který identifikuje vaši databázi Azure pro PostgreSQL server. Název domény *postgres.database.azure.com* se připojí k názvu serveru, který zadáte. Server může obsahovat jenom malá písmena, číslice a pomlčky (-). Musí obsahovat aspoň 3 až 63 znaků.
    Předplatné|Vaše předplatné|Předplatné Azure, který chcete použít pro server. Pokud máte více předplatných, vyberte předplatné, ve kterém se fakturuje pro prostředek.
    Skupina prostředků|*myresourcegroup*| Nový název skupiny prostředků nebo stávající ze svého předplatného.
    Přihlašovací jméno správce serveru |*mylogin*| Vlastní přihlašovací účet se má použít při připojení k serveru. Přihlašovací jméno správce nemůže být **azure_superuser,** **azure_pg_admin,** **správce,** **správce** **kořenové,** **hosta,** nebo **veřejné.** Nelze spustit s **pg_**.
    Heslo |Nějaké si zvolte | Nové heslo pro účet správce serveru. Musí se skládat z 8 až 128 znaků. Heslo musí obsahovat znaky ze tří z následujících kategorií: velká písmena, malá písmena anglické abecedy, čísla (0 až 9) a nealfanumerické znaky (!, $, #, %, atd.).
    Umístění|Oblasti nejbližší uživatelům.| Umístění, které je nejblíže k uživatelům.
    PostgreSQL verze|Nejnovější verzi| Nejnovější verzi, pokud nemáte konkrétní požadavky.
    Cenová úroveň | **Basic**, **50 výpočetních jednotek**, **50 GB** | Úroveň služby a výkonu nové databáze. Vyberte **cenová úroveň**. Potom vyberte **základní** kartě. Potom vyberte levého konce **výpočetní jednotky** posuvníku upravíte hodnota, která má nejnižší možné k dispozici pro tento rychlý start. Chcete-li uložit výběr cenové úrovně, vyberte **OK**. Další informace najdete na následujícím snímku obrazovky. 
    Připnutí na řídicí panel | Zaškrtnout | Umožňuje snadné sledování serveru na stránce front řídicí panel portálu.

    > [!IMPORTANT]
    > Přihlašovací jméno správce serveru a heslo, které zde určíte vyžadovaných pro přihlášení k serveru a její databáze později v tento rychlý start. Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.

    ![V podokně "Cenová úroveň"](./media/quickstart-create-database-portal/2-service-tier.png)

4. Vyberte **Vytvořit**, aby se server zřídil. Zřizování může trvat až 20 minut.

5. Na panelu nástrojů vyberte **oznámení** symbol ke sledování procesu nasazení.

    ![V podokně "Oznámení"](./media/quickstart-create-database-portal/3-notifications.png)
   
  Ve výchozím nastavení **postgres** databáze byla vytvořena v rámci vašeho serveru. [Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) databáze je výchozí databázi, která jsou určené výhradně pro uživatele, nástroje a aplikace třetích stran. 

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurace pravidla brány firewall na úrovni serveru

Azure databázi PostgreSQL vytvoří brána firewall na úrovni serveru. Zabrání externí aplikace a nástroje pro připojení k serveru a všechny databáze na serveru, pokud nevytvoříte pravidlo pro otevření brány firewall pro konkrétní IP adresy. 

1. Po dokončení nasazení najděte váš server. V případě potřeby ho můžete vyhledat. Například v nabídce na levé straně vyberte **všechny prostředky**. Zadejte název serveru, jako je například příkladu **mypgserver 20170401**, vyhledejte nově vytvořenou serveru. Vyberte název serveru ze seznamu výsledků hledání. Otevře se stránka **Přehled** vašeho serveru a poskytne vám možnosti další konfigurace.
 
    ![Vyhledávání názvu serveru](./media/quickstart-create-database-portal/4-locate.png)

2. Na stránce serveru vyberte **Zabezpečení připojení**.

    ![Nastavení "Zabezpečení připojení"](./media/quickstart-create-database-portal/5-firewall-2.png)

3. V části **pravidla brány Firewall** v záhlaví **název pravidla** sloupce, vyberte text, který prázdné pole a vytvořte pravidlo brány firewall. 

    Tento rychlý start můžeme povolit všechny IP adresy do serveru. Zadejte do textového pole v každém sloupci s následujícími hodnotami:

    Název pravidla | Počáteční IP adresa | Koncová IP adresa 
    ---|---|---
    AllowAllIps | 0.0.0.0 | 255.255.255.255

4. Na horním panelu nástrojů na stránce **Zabezpečení připojení** vyberte **Uložit**. Počkejte, až se zobrazí oznámení, s informacemi o tom, že aktualizace zabezpečení nástroje připojení byl úspěšně dokončen, než budete pokračovat.

    > [!NOTE]
    > Připojení k serveru Azure Database for PostgreSQL komunikují přes port 5432. Pokud se pokusíte připojit z podnikové sítě, odchozí provoz přes port 5432 nemusí mít povolený bránou firewall vaší sítě. Pokud ano, se nemůžete připojit k serveru Pokud vaše IT oddělení otevře port 5432.
    >

## <a name="get-the-connection-information"></a>Získání informací o připojení

Při vytváření databáze Azure pro PostgreSQL server výchozí databáze s názvem **postgres** je vytvořena. Pro připojení k databázovému serveru, musíte své celého serveru název a správce přihlašovací údaje. Pravděpodobně jste si tyto hodnoty poznamenali v dřívější části tohoto článku Rychlý start. Pokud se tak nestalo, budete moci snadno najít server název a přihlašovací informace na serveru **přehled** na portálu.

Otevřete stránku **Přehled** vašeho serveru. Poznamenejte si **název serveru** a **přihlašovací jméno pro Server správce**. Ukazatele myši každé pole a symbol kopie se zobrazí vpravo od textu. Podle potřeby ke zkopírování hodnot, vyberte symbol kopírování.

 ![Na stránce server "Přehled"](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-the-postgresql-database-by-using-psql-in-cloud-shell"></a>Připojení k databázi PostgreSQL pomocí psql v prostředí cloudu

Existuje řada aplikací, které můžete použít pro připojení k vašemu serveru Azure Database for PostgreSQL. Nejprve si ukážeme, jak se k serveru připojit pomocí nástroje příkazového řádku psql. Můžete webový prohlížeč a prostředí cloudu Azure podle postupu popsaného tady, aniž by bylo nutné instalovat žádný další software. Pokud máte nástroj psql nainstalovaný místně na svém počítači, můžete se připojit i z něj.

1. V horním navigačním podokně vyberte terminálu symbol otevřete prostředí cloudu.

   ![Azure terminálu symbol cloudové prostředí](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Otevře se v prohlížeči, kde můžete zadat Bash příkazy prostředí cloudové prostředí.

   ![Cloudové prostředí Bash řádku](./media/quickstart-create-database-portal/8-bash.png)

3. Do příkazového řádku prostředí cloudu připojte k databázi v databázi Azure pro PostgreSQL server zadáním psql příkazového řádku.

    Pro připojení k databázi Azure pro server PostgreSQL s [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) nástroj, použijte následující formát:
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    Například následující příkaz se připojí k ukázkovému serveru:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    Parametr psql |Navrhovaná hodnota|Popis
    ---|---|---
    --host | Název serveru | Zadaná hodnota názvu serveru, který jste použili při vytvoření databáze Azure pro server PostgreSQL dříve. Naše Server příklad ukazuje **mypgserver 20170401.postgres.database.azure.com.** Použití s plně kvalifikovaným názvem domény (**\*. postgres.database.azure.com**) jak je znázorněno v příkladu. Pokud si název vašeho serveru nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. 
    --port | 5432 | Port, který se má použít při připojení k databázi Azure pro PostgreSQL server. 
    --username | Přihlašovací jméno správce serveru |Server přihlašovací uživatelské jméno správce, který jste zadali při vytváření databáze Azure pro server PostgreSQL dříve. Pokud jste zapomněli svoje uživatelské jméno, postupujte podle kroků v předchozí části, chcete-li získat informace o připojení. Formát je *username@servername*.
    --dbname | *postgres* | Výchozí název databáze generované systémem, který byl vytvořen pro první připojení. Později můžete vytvořit vlastní databázi.

    Po spuštění příkazu psql s vlastními hodnotami parametrů se zobrazí výzva k zadání hesla správce serveru. Toto heslo je stejný jako ten, který jste zadali při vytváření serveru. 

    Parametr psql |Navrhovaná hodnota|Popis
    ---|---|---
    heslo | Heslo správce | Znaky typu heslo nejsou zobrazeny na řádku bash. Když zadáte všechny znaky, vyberte **Enter** klíč k ověření a připojení.

    Po připojení, zobrazí nástroj psql postgres řádku, kde zadáte příkazy sql. Ve výstupu počáteční připojení může se zobrazit upozornění protože psql v prostředí cloudu může být jinou verzi než databáze Azure pro verzi serveru PostgreSQL. 
    
    Příklad výstupu nástroje psql:
    ```bash
    psql (9.5.7, server 9.6.2)
    WARNING: psql major version 9.5, server major version 9.6.
        Some psql features might not work.
    SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
    Type "help" for help.
   
    postgres=> 
    ```

    > [!TIP]
    > Pokud brána firewall není konfigurována na povolení IP adresu cloudové prostředí, došlo k následující chybě:
    > 
    > "psql: závažná chyba: databáze žádný záznam pg_hba.conf pro hostitele"138.91.195.82"uživatel"mylogin","postgres", protokol SSL u závažná chyba: je nutné připojení SSL. Určení možností protokolu SSL a akci opakujte.
    > 
    > Opravte případné chyby, zkontrolujte konfiguraci serveru odpovídá kroky v části "Konfigurace pravidla brány firewall na úrovni serveru" tohoto článku.

4. Vytvořte prázdnou databázi zadáním následujícího příkazu na příkazovém řádku:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```
    Příkaz může trvat několik minut na dokončení. 

5. Do příkazového řádku spusťte následující příkaz k přepínači připojení k databázi nově vytvořený **mypgsqldb**:
    ```bash
    \c mypgsqldb
    ```

6. Typ `\q`a pak vyberte **Enter** klíč ukončete psql. Jakmile budete hotovi, můžete zavřít cloudové prostředí.

Nyní jste připojení k databázi Azure pro PostgreSQL server a vytvoříte prázdnou uživatelské databáze. Pokračujte v další části pro připojení s použitím jiného běžné nástroje pgAdmin.

## <a name="connect-to-the-postgresql-database-by-using-pgadmin"></a>Připojení k databázi PostgreSQL pomocí pgAdmin

Pro připojení k serveru Azure PostgreSQL s použitím pgAdmin nástroj grafického uživatelského rozhraní:
1. Otevřete aplikaci pgAdmin na klientském počítači. Můžete nainstalovat pgAdmin z [pgAdmin webu](http://www.pgadmin.org/).

2. Na stránce řídicího panelu v části **rychlé odkazy** vyberte **přidejte nový Server** symbol.

3. V **vytvoření - serveru** v dialogovém **Obecné** zadejte jedinečný popisný název serveru, jako například **Azure PostgreSQL Server**.

    ![Karta "Obecné"](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)

4. V **vytvoření - serveru** v dialogovém **připojení** , použijte nastavení uvedeného a pak vyberte **Uložit**.

   ![Na kartě "Připojení"](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)

    Parametr pgAdmin |Navrhovaná hodnota|Popis
    ---|---|---
    Název nebo adresa hostitele | Název serveru | Zadaná hodnota názvu serveru, který jste použili při vytvoření databáze Azure pro server PostgreSQL dříve. Náš příklad server je **mypgserver 20170401.postgres.database.azure.com.** Použití s plně kvalifikovaným názvem domény (**\*. postgres.database.azure.com**) jak je znázorněno v příkladu. Pokud si název vašeho serveru nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. 
    Port | 5432 | Port, který se má použít při připojení k databázi Azure pro PostgreSQL server. 
    Údržby databáze | *postgres* | Výchozí název databáze generované systémem.
    Uživatelské jméno | Přihlašovací jméno správce serveru | Server přihlašovací uživatelské jméno správce, který jste zadali při vytváření databáze Azure pro server PostgreSQL dříve. Pokud si uživatelské jméno nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. Formát je *username@servername*.
    Heslo | Heslo správce | Heslo jste zvolili při vytváření serveru dříve v této rychlý start.
    Role | Ponechte prázdné | Není nutné k zadání názvu role v tomto okamžiku. Ponechte toto pole prázdné.
    Režimu protokolu SSL | Požaduje se | Ve výchozím nastavení jsou všechny servery Azure PostgreSQL vytvoří pomocí protokolu SSL vynucování zapnutý. Chcete-li vypnout vynucování SSL, najdete v části [vynutit SSL](./concepts-ssl-connection-security.md).
    
5. Vyberte **Uložit**.

6. V **prohlížeče** podokna na levé straně, rozbalte **servery** uzlu. Vyberte server, například **Azure PostgreSQL Server**. Kliknutím na tlačítko Připojit se k němu.

7. Rozbalte uzel serveru a pod ním pak rozbalte **Databáze**. V seznamu by měla obsahovat vaše stávající *postgres* databáze a všechny nově vytvořeného uživatele databáze, jako například **mypgsqldb**, které jsme vytvořili v předchozí části. Všimněte si, že můžete vytvořit více databází na serveru s databází Azure pro PostgreSQL.

8. Klikněte pravým tlačítkem na **databáze**, vyberte **vytvořit** nabídce a potom vyberte **databáze**.

9. Zadejte název databáze zvoleného v **databáze** pole, jako třeba **mypgsqldb**, jak je znázorněno v příkladu.

10. Vyberte **vlastníka** databáze z pole se seznamem. Vyberte přihlašovací jméno správce, jako je například server **mylogin**.

11. Vyberte **Uložit** k vytvoření nové prázdné databáze.

12. V **prohlížeče** podokně, najdete v databázi, kterou jste vytvořili v seznamu databází v části název serveru.

    ![V podokně "Browser"](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Vyčištění prostředků
Můžete vyčistit prostředky, které jste vytvořili v rychlý start v jednom ze dvou způsobů. Můžete odstranit [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md), což zahrnuje odstranění všech prostředků v této skupině prostředků. Pokud chcete zachovat další prostředky, odstraňte pouze jeden server prostředku.

> [!TIP]
> Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. Pokud chcete pokračovat v práci s dalšími rychlými starty, neodstraňujte prostředky, které jste vytvořili v rámci tohoto rychlého startu. Pokud nemáte v úmyslu pokračovat, postupujte podle těchto kroků odstranit prostředky, které byly vytvořeny tento rychlý start na portálu.

Chcete-li odstranit skupinu celý prostředků, včetně nově vytvořený serveru:
1. Vyhledejte vaší skupiny prostředků na portálu. V nabídce na levé straně vyberte **skupiny prostředků**. Potom vyberte název vaší skupiny prostředků, jako je například **myresourcegroup**.

2. Na stránce vaší skupiny prostředků vyberte **Odstranit**. Zadejte název vaší skupiny prostředků, jako je například **myresourcegroup**, do textového pole pro potvrzení odstranění. Vyberte **Odstranit**.

Chcete-li odstranit jenom nově vytvořený server:
1. Vyhledejte serverem na portálu, pokud nemáte ho otevřete. V nabídce na levé straně vyberte **všechny prostředky**. Pak vyhledejte server, který jste vytvořili.

2. Na stránce **Přehled** vyberte **Odstranit**.

    ![Tlačítko Odstranit.](./media/quickstart-create-database-portal/12-delete.png)

3. Zkontrolujte název serveru, který chcete odstranit a zobrazit databáze ve které se vztahuje. Zadejte název serveru do textového pole, jako je například **mypgserver 20170401**. Vyberte **Odstranit**.

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)
