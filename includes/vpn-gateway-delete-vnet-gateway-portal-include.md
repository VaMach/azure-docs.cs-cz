### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>Krok 1: Přejděte na bránu virtuální sítě

1. V [portál Azure](https://portal.azure.com), přejděte na **všechny prostředky**. 
2. Otevřete okno brány virtuální sítě, přejděte k bráně virtuální sítě, který chcete odstranit a klikněte na něj.

### <a name="step-2-delete-connections"></a>Krok 2: Odstranění připojení

1. V okně pro bránu virtuální sítě, klikněte na **připojení** zobrazíte všechna připojení k bráně.
2. Klikněte na tlačítko **"..."** na řádku název připojení, potom vyberte **odstranit** z rozevíracího seznamu.
3. Klikněte na tlačítko **Ano** potvrďte, že chcete odstranit připojení. Pokud máte více připojení, odstranění každé připojení.

### <a name="step-3-delete-the-virtual-network-gateway"></a>Krok 3: Odstranit bránu virtuální sítě

Upozorňujeme, že pokud máte kromě konfiguraci S2S P2S konfigurace do této virtuální sítě, odstraňuje se Brána virtuální sítě automaticky odpojí všichni klienti P2S bez upozornění.

1. V okně brány virtuální sítě klikněte na **přehled**.
2. Na **přehled** okně klikněte na tlačítko **odstranit** odstranit bránu.
