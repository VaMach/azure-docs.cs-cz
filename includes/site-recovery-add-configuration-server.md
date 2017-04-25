1. Spusťte instalační soubor sjednocené instalace.
2. Na stránce **Než začnete** vyberte **Nainstalovat konfigurační server a procesový server**.
    ![Než začnete](./media/site-recovery-add-configuration-server/combined-wiz1.png)
3. Na stránce **Licence k softwaru jiného výrobce** vyberte **Souhlasím** pro stažení a instalaci MySQL.

    ![Software jiných výrobců](./media/site-recovery-add-configuration-server/combined-wiz2.png)
4. Na stránce **Registrace** vyberte registrační klíč, který jste si stáhli z trezoru.

    ![Registrace](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. Na stránce **Nastavení internetu** určete, jak se zprostředkovatel, který běží na konfiguračním serveru, připojí k Azure Site Recovery přes internet.

   * Pokud chcete nastavit připojení s proxy serverem, který je aktuálně nastavený na počítači, vyberte **Připojit se s existujícím nastavením proxy serveru**.
   * Pokud chcete, aby se zprostředkovatel připojil přímo, vyberte **Připojit přímo bez proxy serveru**.
   * Pokud stávající proxy server vyžaduje ověření nebo pokud chcete pro připojení zprostředkovatele používat vlastní proxy server, vyberte **Připojit se s vlastním nastavením proxy serveru**.

     * Pokud použijete vlastní proxy server, budete muset zadat adresu, port a přihlašovací údaje.
     * Pokud používáte proxy server, měli byste už mít povolené adresy URL popisované v [požadavcích](#prerequisites).

     ![Brána firewall](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. Na stránce **Kontrola předpokladů** instalační program provede kontrolu a ověří, že lze spustit instalaci. Pokud se zobrazí varování u položky **Kontrola synchronizace globálního času**, ověřte, že čas na systémových hodinách (nastavení **Datum a čas**) je stejný jako časové pásmo.

    ![Požadavky](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. Na stránce **Konfigurace MySQL** vytvořte přihlašovací údaje pro přihlašování k nainstalované instanci serveru MySQL.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. Na stránce **Podrobnosti o prostředí** vyberte, zda se chystáte replikovat virtuální počítače VMware. Pokud ano, instalační program zkontroluje, že je nainstalované rozhraní PowerCLI 6.0.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz7.png)

9. Na stránce **Umístění instalace** vyberte, kam chcete nainstalovat binární soubory a ukládat mezipaměť. Vybraná jednotka musí mít minimálně 5 GB dostupného místa na disku, ale pro mezipaměť doporučujeme jednotku alespoň s 600 GB volného místa.

    ![Umístění instalace](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. Na stránce **Výběr sítě** zadejte naslouchací proces (síťový adaptér a port SSL), na kterém konfigurační server odesílá a přijímá data replikace. Výchozím portem pro odesílání a příjem přenosů replikace je port 9443, ale toto číslo portu můžete změnit podle potřeb vašeho prostředí. Kromě portu 9443 otevíráme také port 443, který používá webový server k orchestraci operací replikace. Nepoužívejte port 443 pro odesílání nebo příjem provozu replikace.

    ![Výběr sítě](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. Na stránce **Souhrn** zkontrolujte informace a klikněte na **Nainstalovat**. Po dokončení instalace se vygeneruje heslo. Budete ho potřebovat k povolení replikace, proto si ho zkopírujte a uložte na bezpečném místě.

    ![Souhrn](./media/site-recovery-add-configuration-server/combined-wiz10.png)

Po dokončení registrace se server zobrazí v okně **Nastavení** > **Servery** v trezoru.
