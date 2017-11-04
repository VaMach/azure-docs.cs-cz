#### <a name="prerequisites"></a>Požadavky
* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* A [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) účtu 

Před použitím účtu Dynamics v aplikaci logiky, autorizovat aplikaci logiky pro připojení k účtu CRM Online. Můžete k tomu snadno v rámci aplikace logiky na portálu Azure. 

Autorizace aplikace logiky pro připojení k účtu CRM Online, pomocí následujících kroků:

1. Vytvoření aplikace logiky. V Návrháři Logic Apps vyberte **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu a potom zadejte do vyhledávacího pole "dynamics". Vyberte jednu z aktivační události nebo akce:  
   ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Pokud jste dosud nevytvořili všechna připojení k aplikaci Dynamics, zobrazí se výzva k přihlášení pomocí přihlašovacích údajů Dynamics:  
   ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Vyberte **přihlášení**a zadejte uživatelské jméno a heslo. Vyberte **přihlášení**. 
   
    Tyto přihlašovací údaje se používají k autorizaci aplikace logiky pro připojení k a přístupu k datům v účtu Dynamics. 
4. Všimněte si, že existuje připojení. Nyní pokračujte další kroky v aplikaci logiky:  
   ![](./media/connectors-create-api-crmonline/dynamics-properties.png)

