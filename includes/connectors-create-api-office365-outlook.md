#### <a name="prerequisites"></a>Požadavky
* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Office 365](https://office365.com) účtu  

Před použitím účtu služeb Office 365 v aplikaci logiky, autorizovat aplikaci logiky pro připojení k účtu služeb Office 365. Můžete k tomu snadno v rámci aplikace logiky na portálu Azure.  

Autorizace aplikace logiky pro připojení k účtu služeb Office 365 pomocí následujících kroků:

1. Vytvoření aplikace logiky. V Návrháři Logic Apps vyberte **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu a potom zadejte do vyhledávacího pole "office 365". Vyberte jednu z aktivační události nebo akce:  
    ![Krok vytvoření připojení Office 365](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  
2. Pokud jste dosud nevytvořili všechna připojení k Office 365, zobrazí se výzva k přihlášení pomocí přihlašovacích údajů Office 365:  
    ![Krok vytvoření připojení Office 365](./media/connectors-create-api-office365-outlook/office365-signin.png)  
3. Vyberte **přihlášení**a zadejte uživatelské jméno a heslo. Vyberte **přihlášení**:  
    ![Krok vytvoření připojení Office 365](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)
   
    Tyto přihlašovací údaje se používají k autorizaci aplikace logiky připojit k a přístup k účtu služeb Office 365. 
4. Všimněte si, že existuje připojení. Nyní pokračujte další kroky v aplikaci logiky:   
    ![Krok vytvoření připojení Office 365](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  

