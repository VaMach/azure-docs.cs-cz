#### <a name="prerequisites"></a>Požadavky
* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* A [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) účtu 

Než účtem Onedrivu můžete použít v aplikaci logiky, autorizovat aplikaci logiky se připojit ke svému účtu OneDrive.  Můžete k tomu snadno v rámci aplikace logiky na portálu Azure. 

Autorizace aplikace logiky pro připojení k účtu OneDrive pomocí následujících kroků:

1. Vytvoření aplikace logiky. V Návrháři Logic Apps vyberte **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu a potom zadejte do vyhledávacího pole "onedrive". Vyberte jednu z aktivační události nebo akce:  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Pokud jste dosud nevytvořili všechna připojení k Onedrivu, budete vyzváni k přihlášení pomocí přihlašovacích údajů OneDrive:  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Vyberte **přihlášení**a zadejte uživatelské jméno a heslo. Vyberte **přihlášení**:  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Tyto přihlašovací údaje se používají k autorizaci aplikace logiky pro připojení k a přístupu k datům v účtu OneDrive. 
4. Vyberte **Ano** autorizovat aplikaci logiky, aby používala účtu OneDrive:  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Všimněte si, že existuje připojení. Nyní pokračujte další kroky v aplikaci logiky:  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)

