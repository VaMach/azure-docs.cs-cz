Funkce Mobile Apps služby Azure App Service používá [Azure Notification Hubs] k odesílání nabízených oznámení, tak se konfigurace centra oznámení pro mobilní aplikace.

1. V [portál Azure], přejděte na **App Services**a potom klikněte na vaši aplikaci back-end. V části **nastavení**, klikněte na tlačítko **Push**.
2. Klikněte na tlačítko **Connect** přidat prostředek centra oznámení do aplikace. Můžete buď vytvořit rozbočovač nebo připojení k existující.

    ![](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Nyní jste se připojili centra oznámení projektu back-end mobilní aplikace. Později nakonfigurujete toto centrum oznámení pro připojení k systému oznámení platformy (PNS) k zařízení.

[portál Azure]: https://portal.azure.com/
[Azure Notification Hubs]: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-push-notification-overview/
