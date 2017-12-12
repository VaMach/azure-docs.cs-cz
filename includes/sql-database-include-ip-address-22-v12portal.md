
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, the following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through the new Azure portal
-->


1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

2. V seznamu na levé straně vyberte **Procházet**. 

3. Posuňte zobrazení a vyberte **servery SQL**. 
   
    ![Najít server služby Azure SQL Database na portálu][b21-FindServerInPortal]
4. Pro usnadnění práce minimalizovat **Procházet** okno.

5. Do textového pole Filtr začněte zadávat text název vašeho serveru. Řádek, který jste se zobrazí.

6. Vyberte řádek pro váš server. Zobrazí se okno pro váš server.

7. V okně vaší serveru vyberte **nastavení**. 

8. Vyberte **brány Firewall**. 
   
    ![Vyberte nastavení > brány Firewall][b31-SettingsFirewallNavig]
9. Vyberte **Přidání klienta IP**. První textového pole zadejte název nové pravidlo.

10. Zadejte v vysoké a nízké hodnoty adres IP pro oblast, kterou chcete povolit.
    
    * Může být užitečný tak, aby měl nízkou hodnotu končit **.0** a vysokou hodnotu končit **.255**.
    
    ![Přidat rozsah IP adres umožňuje][b41-AddRange]
11. Vyberte **Uložit**.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
