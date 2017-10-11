Přidejme aktivační událost.

1. Zadejte *sftp* do vyhledávacího pole v designeru aplikace logiky zvolte **SFTP – Pokud je soubor přidat ani upravit** aktivační události   
   ![Obrázek aktivační události pomocí protokolu SFTP 1](./media/connectors-create-api-sftp/trigger-1.png)  
2. **Je přidána nebo upravena souboru** otevře se ovládací prvek  
   ![Obrázek aktivační události pomocí protokolu SFTP 2](./media/connectors-create-api-sftp/trigger-2.png)  
3. Vyberte **...**  nachází na pravé straně ovládacího prvku. Otevře se ovládací prvek pro výběr složky  
   ![Obrázek aktivační události pomocí protokolu SFTP 3](./media/connectors-create-api-sftp/action-1.png)  
4. Vyberte **SFTP** vyberte kořenové složky jako složka ke sledování pro nové nebo upravení soubory. Všimněte si kořenové složce se nyní zobrazí v **složky** ovládacího prvku.  
   ![Obrázek aktivační události pomocí protokolu SFTP 4](./media/connectors-create-api-sftp/action-2.png)   

Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která zahájí spuštění ostatních triggery a akce v pracovním postupu po upravit nebo vytvořit ve složce konkrétní SFTP souboru. 

> [!NOTE]
> Pro aplikace logiky být funkční musí obsahovat nejméně jedna aktivační událost a jedna akce. Postupujte podle kroků v další části a přidejte akci.  
> 
> 

