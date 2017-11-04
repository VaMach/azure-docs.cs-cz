### <a name="prerequisites"></a>Požadavky
* [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol) účtu  

Než v aplikaci logiky můžete použít váš účet pomocí protokolu SFTP, musíte je nejdříve autorizovat aplikaci logiky pro připojení k účtu pomocí protokolu SFTP. Naštěstí můžete k tomu snadno z v rámci aplikace logiky na portálu Azure.  

Tady jsou kroky k autorizaci aplikace logiky pro připojení k účtu pomocí protokolu SFTP:  

1. Chcete-li vytvořit připojení k protokolu SFTP, v návrháři aplikace logiky, vyberte **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu zadejte *SFTP* do vyhledávacího pole. Vyberte **SFTP – Pokud je soubor přidat ani upravit** aktivační události:  
   ![Obrázek online připojení pomocí protokolu SFTP 1](./media/connectors-create-api-sftp/sftp-1.png)  
2. Pokud jste nevytvořili žádné připojení k protokolu SFTP před, budete získat zobrazí výzva k zadání přihlašovacích údajů vaší SFTP. Tyto přihlašovací údaje se použije k autorizaci aplikace logiky pro připojení k a přístup k datům pomocí protokolu SFTP účtu:  
   ![Obrázek online připojení pomocí protokolu SFTP 2](./media/connectors-create-api-sftp/sftp-2.png)  
3. Všimněte si vytvořil připojení a je nyní můžete pokračovat v dalších krocích v aplikaci logiky:   
   ![Obrázek online připojení pomocí protokolu SFTP 3](./media/connectors-create-api-sftp/sftp-3.png) 

