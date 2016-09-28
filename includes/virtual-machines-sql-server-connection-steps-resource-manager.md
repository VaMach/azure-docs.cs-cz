### Konfigurace názvu DNS veřejné IP adresy

Pokud se chcete z internetu připojit k databázovému stroji systému SQL Server, nejdřív pro svoji veřejnou IP adresu nakonfigurujte název DNS.

> [AZURE.NOTE] Názvy DNS nejsou nutné, pokud se chcete připojit k instanci systému SQL Server jenom v rámci stejné virtuální sítě nebo jenom místně.

Pokud chcete vytvořit název DNS, nejdřív na portálu vyberte **Virtuální počítače**. Vyberte virtuální počítač se systémem SQL Server, abyste zobrazili jeho vlastnosti.

1. V okně virtuálního počítače vyberte své nastavení **Veřejná IP adresa**.

    ![Veřejná IP adresa](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

2. Ve vlastnostech veřejné IP adresa rozbalte položku **Konfigurace**.

3. Zadejte název DNS. Tento název je záznam A, který se dá použít k přímému připojení k vašemu virtuálnímu počítači se systémem SQL Server pomocí názvu, namísto pomocí IP adresy.

4. Klikněte na tlačítko **Uložit**.

    ![Název DNS](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### Připojení k databázovému stroji z jiného počítače

1. Na počítači připojeném k internetu otevřete aplikaci SSMS (SQL Server Management Studio).

2. V dialogovém okně **Připojit k serveru** nebo **Connect to Database Engine** (Připojit k databázovému stroji) upravte hodnotu **Název serveru**. Zadejte úplný název DNS virtuálního počítače (určený v předchozí úloze).

3. V poli **Ověřování** vyberte **Ověřování serveru SQL Server**.

5. Do pole **Přihlášení** zadejte název platného přihlášení SQL.

6. Do pole **Heslo** zadejte heslo pro přihlášení.

7. Klikněte na **Připojit**.

    ![Připojení přes SSMS](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)


<!--HONumber=Sep16_HO3-->


