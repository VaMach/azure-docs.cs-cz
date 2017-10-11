## <a name="connect-to-outlookcom"></a>Připojení k Outlook.com
### <a name="prerequisites"></a>Požadavky
* Účet Outlook.com

Než v aplikaci logiky můžete použít váš účet Outlook.com, musíte je nejdříve autorizovat aplikaci logiky se připojit ke svému účtu Outlook.com. Naštěstí můžete k tomu snadno z v rámci aplikace logiky na portálu Azure. 

Tady jsou kroky k autorizaci aplikace logiky pro připojení k účtu Outlook.com:

1. Všechny aplikace logiky muset spustit aktivační událost, takže po vytvoření aplikace logiky návrháře otevře a spustí zobrazí seznam, který můžete použít ke spuštění aplikace logiky:
   
   ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. Do vyhledávacího pole zadejte "outlook". Všimněte si, že že seznam je filtrovaný seznam všechny aktivační události s "Outlook" v názvu:![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Vyberte **Office 365 Outlook - na nové e-mailu**.   
   Pokud jste nevytvořili žádné připojení k aplikaci Outlook před, budete získat zobrazí výzva k zadání přihlašovacích údajů vaší Outlook.com. Tyto přihlašovací údaje se použije k autorizaci aplikace logiky pro připojení k a přístup k datům účtu Outlook.com:![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Zadejte svoje přihlašovací údaje pro aplikaci Outlook a přihlaste se:![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
   A je to. Nyní jste vytvořili připojení k aplikaci Outlook. Toto připojení bude k dispozici pro použití v jiné aplikaci logiky, který vytvoříte.

