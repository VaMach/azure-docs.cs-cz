Teď, když jste přidali aktivační událost, doba jejího něco udělat zajímavé s daty, která je generován aktivační událost. Postupujte podle těchto kroků přidejte **SharePoint Online – vytvoření souboru** akce. Tato akce vytvoří soubor ve službě SharePoint Online pokaždé, když aktivuje novou aktivační událost položky. 

Ke konfiguraci této akce, budete muset zadat následující informace. Jako zadání těchto informací si všimnete, že je snadno použitelný data generována aktivační událost jako vstup pro některé vlastnosti nového souboru:

| Vlastnost souboru | Popis |
| --- | --- |
| Adresa URL webu |Toto je adresa URL webu služby SharePoint Online, kde chcete vytvořit nový soubor. Vyberte lokalitu ze seznamu uvedené. |
| Cesta ke složce |Toto je složka (na adrese URL webu vybrali v předchozím kroku) umístění nový soubor. Vyhledejte a vyberte složku. |
| Název souboru |Toto je název souboru, který vytváří. |
| Obsah souboru |Obsah, který bude zapsán do souboru. |

1. Vyberte **+ nový krok** přidat akci.  
   ![SharePoint online akce obrázek 1](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Vyberte **přidat akci** odkaz. Otevře se tato, které chcete do vyhledávacího pole, kde můžete vyhledat všechny akce můžete provést. V tomto příkladu jsou SharePoint akce zájmu.    
   ![Obrázek SharePoint online akce 2](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Zadejte *sharepoint* k vyhledání akcí souvisejících s služby SharePoint.
4. Vyberte **SharePoint Online – vytvoření souboru** jako akci provést.   **Poznámka:**: Zobrazí se výzva k autorizaci aplikace logiky pro přístup k účtu služby SharePoint, pokud jste dosud nevytvořili připojení ke službě SharePoint Online dříve.    
   ![Obrázek SharePoint online akce 3](./media/connectors-create-api-sharepointonline/action-3.png)    
5. **Vytvořit soubor** řízení otevře.   
   ![SharePoint online akce obrázek 4](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Vyberte **adresa URL webu** a procházet a najít lokalitu, kde chcete vytvořit soubor.     
   ![SharePoint online akce obrázku 5](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Vyberte **cesta ke složce** a procházet a najít složku, kde budou umístěné nový soubor.  
   ![SharePoint online akce obrázek 6](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Vyberte **název souboru** řízení a zadejte název souboru, kterou chcete vytvořit. Zde můžete zadat název souboru přímo nebo můžete použít některou z vlastností z aktivační událost, kterou jste vytvořili dříve. To uděláte tak, že vyberete ze seznamu vlastnosti **výstupy z při vytvoření nové položky**. Tento seznam je zobrazit pouze po výběru **název souboru** ovládacího prvku. V této walkthough I vybrali ID (ID nové položky seznamu) jako název souboru, jehož vytvoření **SharePoint Online – vytvoření souboru** akce.    
   ![Obrázek SharePoint online akce 7](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Vyberte **souboru obsahu** řízení a zadejte obsah, který bude zapsán do souboru, který bude vytvořen. Obsah souboru Všimněte si, které můžete použít jakékoli vlastnosti z aktivační událost, kterou jste vytvořili dříve. Vlastnosti jednoduše vyberte ze seznamu uvedené. Alternativně můžete zadat **souboru obsahu** text přímo do ovládacího prvku. V tomto příkladu I vybrali některé vlastnosti a přidá mezery a pomlčka mezi každou vlastnost.        
   ![SharePoint online akce obrázek 8](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Uložit změny do pracovního postupu  
11. Blahopřejeme, nyní máte logiku plně funkční aplikaci, která se aktivuje při přidání nové položky v seznamu SharePoint Online. Aplikace pak vytvoří soubor pomocí některé vlastnosti z nové položky seznamu.  Nyní ji můžete otestovat tak, že vytvoříte novou položku v seznamu serveru SharePoint. 

