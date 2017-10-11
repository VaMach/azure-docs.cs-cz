### <a name="prerequisites"></a>Požadavky
* A [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) účtu  

Než v aplikaci logiky můžete použít váš účet SMTP, musíte je nejdříve autorizovat aplikaci logiky se připojit ke svému účtu SMTP. Naštěstí můžete k tomu snadno z v rámci aplikace logiky na portálu Azure.  

Tady jsou kroky k autorizaci aplikace logiky pro připojení k účtu SMTP:  

1. Chcete-li vytvořit připojení k SMTP, v návrháři aplikace logiky, vyberte **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu zadejte *SMTP* do vyhledávacího pole. Vyberte aktivační události nebo akci, kterou budete chtít použít:  
   ![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Pokud jste nevytvořili žádné připojení k SMTP před, budete získat zobrazí výzva k zadání přihlašovacích údajů vaší SMTP. Tyto přihlašovací údaje se použije k autorizaci aplikace logiky pro připojení k a přístup k datům účet SMTP:  
   ![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Všimněte si vytvořil připojení a je nyní můžete pokračovat v dalších krocích v aplikaci logiky:  
   ![](./media/connectors-create-api-smtp/smtp-3.png)  

