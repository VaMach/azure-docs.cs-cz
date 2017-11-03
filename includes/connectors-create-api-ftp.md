### <a name="prerequisites"></a>Požadavky
* [FTP](https://wikipedia.org/wiki/File_Transfer_Protocol) účtu  

Než v aplikaci logiky můžete použít účet na serveru FTP, musíte je nejdříve autorizovat aplikaci logiky pro připojení k účtu FTP. Naštěstí můžete k tomu snadno z v rámci aplikace logiky na portálu Azure.  

Tady jsou kroky k autorizaci aplikace logiky pro připojení k účtu FTP:  

1. Chcete-li vytvořit připojení k serveru FTP, v návrháři aplikace logiky, vyberte **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu zadejte *FTP* do vyhledávacího pole. Vyberte aktivační události nebo akci, kterou budete chtít použít:  
   ![Krok vytvoření připojení FTP](./media/connectors-create-api-ftp/ftp-1.png)  
2. Pokud jste nevytvořili žádné připojení k serveru FTP před, budete získat zobrazí výzva k zadání přihlašovacích údajů vaší FTP. Tyto přihlašovací údaje se použije k autorizaci aplikace logiky pro připojení k a přístup k datům účtu FTP:  
   ![Krok vytvoření připojení FTP](./media/connectors-create-api-ftp/ftp-2.png)  
3. Všimněte si vytvořil připojení a je nyní můžete pokračovat v dalších krocích v aplikaci logiky:  
   ![Krok vytvoření připojení FTP](./media/connectors-create-api-ftp/ftp-3.png)  

