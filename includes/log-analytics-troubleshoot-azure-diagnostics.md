### <a name="troubleshoot-azure-diagnostics"></a>Odstraňování potíží Diagnostiky Azure

Pokud se zobrazí následující chybová zpráva, poskytovatel prostředků Microsoft.insights není registrovaný:

`Failed to update diagnostics for 'resource'. {"code":"Forbidden","message":"Please register the subscription 'subscription id' with Microsoft.Insights."}`

Pokud chcete poskytovatele prostředků registrovat, proveďte na webu Azure Portal následující kroky:

1.  V navigačním podokně na levé straně klikněte na *Předplatná*.
2.  Vyberte předplatné identifikované v chybové zprávě.
3.  Klikněte na *Poskytovatelé prostředků*.
4.  Vyhledejte poskytovatele *Microsoft.insights*.
5.  Klikněte na odkaz *Zaregistrovat*.

![Zaregistrujte poskytovatele prostředků Microsoft.insights.](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

Jakmile bude poskytovatel prostředků *Microsoft.insights* zaregistrovaný, zopakujte konfiguraci diagnostiky.


<!--HONumber=Feb17_HO2-->


