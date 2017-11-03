Teď, když jste přidali podmínku, doba jejího něco udělat zajímavé s daty, která je generován aktivační událost. Postupujte podle těchto kroků přidejte **Salesforce - Get objekt** akce. Tato akce se získat data pokaždé, když se vytvoří nové zájemce. Pokud přidáte druhou akci, který bude používat data ze služby Salesforce - Get akce objektu k odesílání e-mailu pomocí konektor Office 365.  

Ke konfiguraci této akce, budete muset zadat následující informace. Si všimnete, že je snadno použitelný data generována aktivační událost jako vstup pro některé vlastnosti nového souboru:

| Vlastnost souboru | Popis |
| --- | --- |
| Typ objektu |Toto je typ objektu služby Salesforce, které vás zajímají. Příklady jsou realizace, účet, atd. |
| ID objektu |Reprezentuje identifikátor objektu. |

1. Vyberte **přidat akci** odkaz. Otevře se tato, které chcete do vyhledávacího pole, kde můžete vyhledat všechny akce můžete provést. V tomto příkladu jsou Salesforce akce zájmu.      
   ![Obrázek akce Salesforce 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Zadejte *salesforce* k vyhledání akcí souvisejících s salesforce.
3. Vyberte **Salesforce - Get objekt** jako akci provést.   **Poznámka:**: Zobrazí se výzva k autorizaci aplikace logiky k přístupu k svému účtu Salesforce, pokud jste tak dosud neučinili dříve.    
   ![Obrázek akce Salesforce 2](./media/connectors-create-api-salesforce/action-2.png)    
4. **Objekt Get** řízení otevře.  
5. Vyberte *vést* jako typ objektu.
6. Vyberte **ID objektu** ovládacího prvku.
7. Vyberte **...**  rozbalte seznam tokeny, které lze použít jako vstup pro akce.       
   ![Obrázek akce Salesforce 3](./media/connectors-create-api-salesforce/action-3.png)    
8. Vyberte **vést ID** řízení otevře.   
   ![Obrázek akce Salesforce 4](./media/connectors-create-api-salesforce/action-4.png)     
9. Všimněte si, že token vést ID je nyní v ovládacím prvku ID objektu indikující akce objekt Get bude hledat realizace s ID, které se rovná realizace ID realizace, která aktivuje tuto aplikaci logiky.  
   ![Obrázek akce Salesforce 5](./media/connectors-create-api-salesforce/action-5.png)  
10. Uložte svou práci. Je to, jste přidali do aplikace logiky akce objekt Get. Ovládací prvek objekt Get by měl vypadat takto:    
    ![Obrázek akce Salesforce 6](./media/connectors-create-api-salesforce/action-6.png)  

Teď, když jste přidali akci, kterou chcete získat zájemce, můžete provést něčeho zajímavého s nově vytvořený realizace. V podniku můžete odeslat e-mail s oznámením distribuční seznam vytvořený nové zájemce. Odeslat e-mail s některé důležité informace z nového objektu realizace v Salesforce použijeme konektor Office 365.  

1. Vyberte **přidat akci** zadejte *e-mailu* v ovládacím prvku vyhledávání. Tím se odfiltrují akce na ty, které se vztahují k odesílání a příjmu e-mailu.  
2. Vyberte **Office 365 Outlook - e-mailu** položky seznamu. Pokud jste ještě nevytvořili *připojení* ke svému účtu služeb Office 365, zobrazí se výzva k zadání pověření Office 365 ji vytvořit. Po dokončení **e-mailovou zprávu** řízení otevře.        
   ![Obrázek akce Salesforce 7](./media/connectors-create-api-salesforce/action-7.png)  
3. Zadejte e-mailovou adresu, která chcete poslat e-mailu v **k** ovládacího prvku.
4. V **subjektu** řídit, zadejte *vytvořit nové vést* – vyberte *společnosti* tokenu. Bude se zobrazovat *společnosti* pole z nové zájemce vytvořené v Salesforce.  
5. V **textu** ovládací prvek, můžete vybrat libovolný tokenů z nového objektu realizace a také můžete zadat jakýkoli text, který se má zobrazit v textu e-mailu. Tady je příklad:  
   ![Obrázek akce Salesforce 8](./media/connectors-create-api-salesforce/action-8.png)   
6. Uložení pracovního postupu.  

A je to. Aplikace logiky je nyní dokončen.  

Nyní můžete testovat svou aplikaci logiky: v Salesforce, vytvořte nové zájemce, který splňuje podmínky, které jste vytvořili.  Pokud jste postupovali plně podle tohoto návodu, vytvořte s e-mailovou adresu, která obsahuje jenom zájemce *amazon.com* v ní. Za několik sekund by měl být spuštěn svou aplikaci logiky a výsledky může vypadat podobně jako tento:  
![Obrázek akce Salesforce 9](./media/connectors-create-api-salesforce/action-9.png)  

