
Podle těchto pokynů vytvořte novou mobilní službu.

1.  Přihlaste se na [portál Azure Classic](https://manage.windowsazure.com/). Ve spodní části navigačního podokna klikněte na **+ NOVÝ**. Rozbalte položku **Compute** a **Mobilní služba** a klikněte na **Vytvořit**.
    
    ![](./media/mobile-services-dotnet-backend-create-new-service/mobile-create.png)

    Zobrazí se dialogové okno **Create a Mobile Service** (Vytvořit mobilní službu).

2.  Na stránce **Create a Mobile Service** (Vytvořit mobilní službu) vyberte **Create a free 20 MB SQL Database** (Vytvořit zdarma 20MB SQL Database), vyberte modul runtime **.NET** a do textového pole **Adresa URL** zadejte název subdomény nové mobilní služby. Kliknutím na tlačítko se šipkou doprava přejděte na další stránku.
    
    ![](./media/mobile-services-dotnet-backend-create-new-service/mobile-create-page1.png)

    Zobrazí se stránka **Specify database settings** (Zadejte nastavení databáze).

    > [AZURE.NOTE] V rámci tohoto kurzu vytvoříte novou instanci a server služby SQL Database. Tuto novou databázi můžete znovu použít a spravovat ji jako jakoukoli jinou instanci služby SQL Database. Pokud už máte ve stejné oblasti, kterou používá nová mobilní služba, nastavenou databázi, můžete zvolit možnost **Použít stávající databázi** a potom tuto databázi vybrat. Databázi se nedoporučuje používat v jiné oblasti kvůli dalším nákladům na šířku pásma a vyšší latenci.

3.  Do pole **Název** zadejte název nové databáze, potom zadejte **Přihlašovací jméno**, což je přihlašovací jméno správce pro nový server služby SQL Database, zadejte a potvrďte heslo a nakonec dokončete proces kliknutím na tlačítko se symbolem zaškrtnutí.
    ![](./media/mobile-services-dotnet-backend-create-new-service/mobile-create-page2.png)

Teď jste vytvořili novou mobilní službu, kterou můžou používat vaše mobilní aplikace.


<!--HONumber=Sep16_HO3-->


