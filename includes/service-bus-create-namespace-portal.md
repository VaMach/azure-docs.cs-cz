1. Přihlaste se na portál Azure Classic.

2. V levém navigačním podokně portálu klikněte na **Service Bus**.

3. V dolním podokně portálu klikněte na **Vytvořit**.

    ![Výběr možnosti Vytvořit][select-create]
   
4. V dialogovém okně **Přidat nový obor názvů** zadejte název oboru názvů. Systém okamžitě kontroluje, jestli je název dostupný.

    ![Název oboru názvů][namespace-name]
  
5. Až se ujistíte, že je název oboru názvů dostupný, zvolte zemi nebo oblast, kde se má obor názvů hostovat.

6. V ostatních polích dialogového okna nechte výchozí hodnoty (**Zasílání zpráv** a **Úroveň Standard**) a potom klikněte na značku zaškrtnutí OK. Systém teď vytvoří obor názvů a povolí ho. Pravděpodobně budete muset několik minut počkat, než systém zřídí prostředky pro váš účet.
 
    ![Úspěšné vytvoření][created-successfully]

###Získání přihlašovacích údajů
1. V levém navigačním podokně klikněte na uzel **Service Bus** a zobrazte seznam dostupných oborů názvů:
 
    ![Výběr služby Service Bus][select-service-bus]
  
2. V zobrazeném seznamu vyberte právě vytvořený obor názvů:
 
    ![Výběr oboru názvů][select-namespace]
 
3. Klikněte na **Informace o připojení**.

    ![Informace o připojení][connection-information]
  
4. V podokně **Informace o přístupovém připojení** najděte připojovací řetězec, který obsahuje klíč SAS a název klíče.

    ![Informace o přístupovém připojení][access-connection-information]
  
5. Klíč si poznamenejte, nebo ho zkopírujte do schránky.

<!--Image references-->

[select-create]: ./media/service-bus-create-namespace-portal/select-create.png
[namespace-name]: ./media/service-bus-create-namespace-portal/namespace-name.png
[created-successfully]: ./media/service-bus-create-namespace-portal/created-successfully.png
[select-service-bus]: ./media/service-bus-create-namespace-portal/select-service-bus.png
[select-namespace]: ./media/service-bus-create-namespace-portal/select-namespace.png
[connection-information]: ./media/service-bus-create-namespace-portal/connection-information.png
[access-connection-information]: ./media/service-bus-create-namespace-portal/access-connection-information.png


<!--Reference style links - using these makes the source content way more readable than using inline links-->
[classic-portal]: https://manage.windowsazure.com


<!--HONumber=Aug16_HO4-->


