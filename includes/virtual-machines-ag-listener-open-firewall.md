V tomto kroku vytvoříte pravidlo brány firewall otevřít port testu pro koncový bod Vyrovnávání zatížení sítě (59999, jak je uvedeno výše) a jiné pravidlo otevřít port naslouchacího procesu skupiny dostupnosti. Vzhledem k tomu, že jste vytvořili koncový bod Vyrovnávání zatížení na virtuálních počítačích, které obsahují replik skupin dostupnosti, budete muset otevřít port testu a port naslouchacího procesu na příslušných virtuálních počítačích.

1. Na virtuálních počítačích, které jsou hostiteli repliky, spusťte **brány Windows Firewall s pokročilým zabezpečením**.

2. Klikněte pravým tlačítkem na **příchozí pravidla**a potom klikněte na **nové pravidlo**.

3. Na **typ pravidla** vyberte **Port**a potom klikněte na **Další**.

4. Na **protokol a porty** vyberte **TCP**, typ **59999** v **určité místní porty** pole a pak klikněte na tlačítko  **Další**.

5. Na **akce** ponechte **povolit připojení** vybrané a potom klikněte na **Další**.

6. Na **profil** stránky, přijměte výchozí nastavení a pak klikněte na tlačítko **Další**.

7. Na **název** stránky v **název** text zadejte název pravidla, jako například **vždy na Port naslouchacího procesu testu**a potom klikněte na **Dokončit**.

8. Opakujte předchozí kroky pro port naslouchacího procesu skupiny dostupnosti (uvedený dříve v parametru $EndpointPort skriptu) a pak zadejte název příslušné pravidlo, jako například **vždy na Port naslouchacího procesu**.

