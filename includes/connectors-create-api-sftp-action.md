Teď, když jste přidali aktivační událost, doba jejího něco udělat zajímavé s daty, která je generován aktivační událost. Postupujte podle těchto kroků pro přidání **SFTP - extract složky** akce. Tato akce bude extrahujte obsah souboru, pokud jsou splněny definované podmínky. 

Ke konfiguraci této akce, budete muset zadat následující informace. Si všimnete, že je snadno použitelný data generována aktivační událost jako vstup pro některé vlastnosti nového souboru:

| Pomocí protokolu SFTP - vlastnost složky extrakce | Popis |
| --- | --- |
| Cestu ke zdrojovému souboru archivu |Toto je cesta k souboru se extrahují. Můžete vyberte jednu z tokenů starší akce nebo přejít k vyhledávání cesta k souboru na serveru pomocí protokolu SFTP. |
| Cesta k cílové složce |Jedná se o cestu, kde budou umístěné extrahované soubory. Můžete vybrat jednu z tokenů z dřívějších akce jako cílovou cestu nebo procházet serveru pomocí protokolu SFTP a vyberte cestu. |
| Chcete přepsat? |Označuje, pokud je v cesta k cílové složce najít soubor se stejným názvem jako soubor extrahované Pokud existující soubor by měl být přepsáno, nebo ne. |

Začněme přidáním akce extrahujte soubory, pokud je výsledkem podmínku definovaného dříve *True*. 

1. Vyberte **přidat akci**.        
   ![Obrázek stavu akce SFTP 6](./media/connectors-create-api-sftp/condition-6.png)   
2. Vyberte **SFTP - Extract složky** akce      
   ![Obrázek stavu akce SFTP 7](./media/connectors-create-api-sftp/condition-7.png)   
3. Vyberte **cestu ke zdrojovému souboru archivu**              
   ![Obrázek stavu akce SFTP 9](./media/connectors-create-api-sftp/condition-9.png)   
4. Vyberte **cesta k souboru** tokenu. To znamená, že budete používat jako cestu ke zdrojovému souboru archivu cesta souboru, který má aktivační procedura nalezen.           
   ![Obrázek stavu akce SFTP 10](./media/connectors-create-api-sftp/condition-10.png)   
5. Vyberte **cestu k cílové složce**           
   ![Obrázek stavu akce SFTP 11](./media/connectors-create-api-sftp/condition-11.png)   
6. Vyberte **cesta k souboru** tokenu. To znamená, že budete používat cesta souboru, který aktivační událost nalézt jako cílová cesta pro extrahované soubory.   
7. Zadejte *\ExtractedFile* v **cestu k cílové složce** ovládacího prvku. To lze proveďte pouze po token cesta k souboru v ovládacím prvku cesta cílové složky.         
   ![Obrázek stavu akce SFTP 12](./media/connectors-create-api-sftp/condition-12.png)   
8. Zadejte *True* v **přepsat?* řízení k označení, že má existující soubory přepsány, pokud mají stejný název jako extrahované soubory.      
   ![Obrázek stavu akce SFTP 13](./media/connectors-create-api-sftp/condition-13.png)   
9. Uložit změny do pracovního postupu  

