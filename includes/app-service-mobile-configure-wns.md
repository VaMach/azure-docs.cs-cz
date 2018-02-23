
1. V [portál Azure](https://portal.azure.com/), vyberte **Procházet vše** > **App Services**. Potom vyberte Mobile Apps back-end. V části **nastavení**, vyberte **aplikace služby Push**. Pak vyberte vaším jménem centra oznámení.
2. Přejděte na **systému Windows (WNS)**. Poté zadejte **klíč zabezpečení** (tajný klíč klienta) a **SID balíčku** který jste získali z web služeb Live Services. Potom vyberte **Uložit**.

    ![Nastavení klíče WNS v portálu](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

Back-end vašeho je nyní nakonfigurována používat k odesílání nabízených oznámení WNS.
