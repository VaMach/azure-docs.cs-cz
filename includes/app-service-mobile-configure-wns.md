
1. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **Procházet vše** > **App Services**a klikněte na vaše mobilní aplikace back-end. V části **nastavení**, klikněte na tlačítko **aplikace služby Push**a potom klikněte na název vašeho centra oznámení.
2. Přejděte na **Windows (WNS)**, zadejte **klíč zabezpečení** (tajný klíč klienta) a **SID balíčku** můžete získat z webu služby Live, a pak klikněte na **uložit** .

    ![Nastavení klíče WNS v portálu](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

Back-end vašeho je nyní nakonfigurována používat k odesílání nabízených oznámení WNS.
