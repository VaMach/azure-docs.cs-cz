### <a name="prerequisites"></a>Požadavky
* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Účtu úložiště objektů Blob Azure](../articles/storage/common/storage-create-storage-account.md) včetně název účtu úložiště a jeho přístupový klíč. Tyto informace je uvedena v okně vlastností účtu úložiště na portálu Azure. Další informace o [Azure Storage](../articles/storage/common/storage-introduction.md).

Před použitím svého účtu úložiště objektů Blob Azure v aplikaci logiky se připojte k účtu úložiště objektů Blob Azure. Můžete k tomu snadno v rámci aplikace logiky na portálu Azure.  

Připojte k účtu Azure Blob Storage pomocí následujících kroků:  

1. Vytvoření aplikace logiky. V Návrháři Logic Apps přidejte aktivační událost a potom přidat akci. Vyberte **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu a potom zadejte do vyhledávacího pole "blob". Vyberte jednu z akcí:  
   
    ![Azure Blob Storage – krok vytvoření připojení](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  
2. Pokud jste dosud nevytvořili žádné připojení k úložišti Azure, budete vyzváni k podrobnosti připojení:   
   
    ![Azure Blob Storage – krok vytvoření připojení](./media/connectors-create-api-azureblobstorage/connection-details.png)  
3. Zadejte podrobnosti o účtu úložiště. Vyžadují se vlastnosti s hvězdičkou.
   
   | Vlastnost | Podrobnosti |
   | --- | --- |
   | Název připojení * |Zadejte libovolný název pro připojení. |
   | Název účtu úložiště Azure * |Zadejte název účtu úložiště. Název účtu úložiště se zobrazí ve vlastnostech úložiště na portálu Azure. |
   | Přístupový klíč účtu úložiště Azure * |Zadejte klíč účtu úložiště. Přístupové klíče se zobrazí ve vlastnostech úložiště na portálu Azure. |
   
    Tyto přihlašovací údaje se používají k autorizaci aplikace logiky připojení a přístup k datům. 
4. Vyberte **Vytvořit**.
5. Všimněte si, že existuje připojení. Nyní pokračujte další kroky v aplikaci logiky: 
   
    ![Azure Blob Storage – krok vytvoření připojení](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  

