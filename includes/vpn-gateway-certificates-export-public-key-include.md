Připojení point-to-Site vyžadují soubor .cer veřejného klíče certifikátu (ne privátní klíč) k odeslání do Azure. Následující postup můžete exportovat soubor .cer pro podepsané svým držitelem kořenový certifikát:

1. Chcete-li získat soubor .cer z certifikátu, otevřete **správu uživatelských certifikátů**. Vyhledejte kořenový certifikát podepsaný svým držitelem, obvykle v Certificates - Current User\Personal\Certificates, a klikněte pravým tlačítkem myši. Klikněte na **Všechny úlohy** a potom klikněte na **Exportovat**. Otevře se **Průvodce exportem certifikátu**.
2. V průvodci klikněte na **Další**. Vyberte **Ne, neexportovat privátní klíč** a klikněte na **Další**.
3. Na stránce **Formát souboru pro export** vyberte **X.509, kódování Base-64 (CER)** a klikněte na **Další**. 
4. V části **Soubor pro export** **přejděte** do umístění, do kterého chcete certifikát vyexportovat. V části **Název souboru** zadejte název souboru. Pak klikněte na **Další**.
5. Certifikát vyexportujte kliknutím na **Dokončit**. Zobrazí **úspěšném exportu**. Kliknutím na **OK** zavřete průvodce.