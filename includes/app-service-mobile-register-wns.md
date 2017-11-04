
1. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt aplikace Windows Store a klikněte na tlačítko **úložiště** > **přidružit aplikaci ve Store**.

    ![Přidružit aplikace Windows Store](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. V průvodci klikněte na tlačítko **Další**a přihlaste se pomocí účtu Microsoft. Zadejte název aplikace v rámci **rezervovat nový název aplikace**a potom klikněte na **rezervy**.
3. Po registraci aplikace je úspěšně vytvořen, vyberte nový název aplikace, klikněte na **Další**a potom klikněte na **přidružit**. Tento postup přidá požadované informace o registraci Windows Store do manifestu aplikace.
4. Opakujte kroky 1 a 3 pro projekt aplikace Windows Phone Store pomocí stejné registrace, který jste dříve vytvořili pro aplikace pro Windows Store.  
5. Vyhledejte [Centrum vývojářů pro Windows](https://dev.windows.com/en-us/overview)a přihlaste se pomocí účtu Microsoft. Klikněte na novou registraci aplikace v **Moje aplikace**a potom rozbalte **služby** > **nabízená oznámení**.
6. Na **nabízená oznámení** klikněte na tlačítko **Web služeb Live Services** v části **nabízených oznámení Windows služby (WNS) a Microsoft Azure Mobile Apps**. Poznamenejte si hodnoty **SID balíčku** a *aktuální* hodnotu **tajný klíč aplikace**. 

    ![Nastavení aplikace v Centru pro vývojáře](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Tajný klíč aplikace a SID balíčku jsou důležité přihlašovací údaje zabezpečení. Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.
   >
   >
