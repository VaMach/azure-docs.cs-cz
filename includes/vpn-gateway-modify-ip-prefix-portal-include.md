### <a name="noconnection"></a>Úprava předpon IP adres místní síťové brány – žádné připojení brány

#### <a name="to-add-additional-address-prefixes"></a>Přidání dalších předpon adres:

1. Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **konfigurace**.
2. Přidejte adresní prostor IP adres v *přidat další rozsah adres* pole.
3. Klikněte na tlačítko **Uložit** uložte nastavení.

#### <a name="to-remove-address-prefixes"></a>Odebrání předpon adres:

1. Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **konfigurace**.
2. Klikněte **"..."** na řádek obsahující předpona, kterou chcete odebrat.
3. Klikněte na tlačítko **odebrat**.
4. Klikněte na tlačítko **Uložit** uložte nastavení.

### <a name="withconnection"></a>Úprava předpon IP adres místní síťové brány – existující připojení brány

Pokud máte připojení k bráně a chcete přidat nebo odebrat předpony IP adres obsažené v bráně místní sítě, musíte v uvedeném pořadí provést následující kroky. Způsobí to určitý výpadek připojení VPN. Při upravování předpon IP adres není potřeba odstraňovat bránu VPN. Stačí jenom odebrat připojení.

#### <a name="1-remove-the-connection"></a>1. Odeberte připojení.

1. Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **připojení**.
2. Klikněte **...**  na řádek pro každé připojení klikněte **odstranit**.
3. Klikněte na tlačítko **Uložit** uložte nastavení.

#### <a name="2-modify-the-address-prefixes"></a>2. Úprava předpon adres.

Přidání dalších předpon adres:

1. Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **konfigurace**.
2. Přidejte adresní prostor IP adres.
3. Klikněte na tlačítko **Uložit** uložte nastavení.

Odebrání předpon adres:

1. Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **konfigurace**.
2. Klikněte **...**  na řádek obsahující předpona, kterou chcete odebrat.
3. Klikněte na tlačítko **odebrat**.
4. Klikněte na tlačítko **Uložit** uložte nastavení.

#### <a name="3-recreate-the-connection"></a>3. Znovu vytvořte připojení.

1. Přejděte k bráně virtuální sítě pro virtuální síť. (Nikoli brány místní sítě.)
2. Na bráně virtuální sítě v **nastavení** klikněte na tlačítko **připojení**.
3. Klikněte **+ přidat** otevřete **přidat připojení** okno.
4. Znovu vytvořte připojení.
5. Klikněte na tlačítko **OK** k vytvoření připojení.