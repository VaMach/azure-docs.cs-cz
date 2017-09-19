

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. Vyberte **Nový** > **Web a mobilní zařízení** > **Centrum oznámení**.
   
      ![Azure Portal – vytvoření centra oznámení](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. Do pole **Centrum oznámení** zadejte jedinečný název. Vyberte svou **Oblast**, **Předplatné** a **Skupinu prostředků** (pokud už nějakou máte). 
   
    Pokud už máte obor názvů služby Service Bus, ve kterém chcete centrum vytvořit, postupujte následovně:

    a. V poli **Obor názvů** vyberte odkaz **Vybrat existující**. 
   
    b. Vyberte **Vytvořit**.

    Pokud ještě obor názvů služby Service Bus nemáte, můžete použít výchozí název vytvořený na základě názvu centra (pokud je název oboru názvů dostupný).
   
      ![Azure Portal – nastavení vlastností centra oznámení](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

    Po vytvoření oboru názvů a centra oznámení se otevře Azure Portal. 
   
      ![Azure Portal – stránka portálu centra oznámení](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-page.png)

4. Vyberte **Nastavení** > **Zásady přístupu**. Poznamenejte si dva připojovací řetězce, které máte k dispozici. Budete je potřebovat později pro zpracování nabízených oznámení.
   
      ![Azure Portal – připojovací řetězce centra oznámení](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

