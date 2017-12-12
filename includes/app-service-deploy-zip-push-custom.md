## <a name="deployment-customization"></a>Vlastní nastavení nasazení

Proces nasazení předpokládá, že soubor .zip, který nabízené obsahuje připravené ke spuštění aplikace. Ve výchozím nastavení jsou spuštěny žádné úpravy. Můžete povolit stejné procesy sestavení, kterou poskytuje průběžnou integraci vložením následujícího textu pro nastavení aplikace:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

Pokud používáte nasazení nabízené .zip, toto nastavení je **false** ve výchozím nastavení. Výchozí hodnota je **true** průběžnou integraci nasazení. Pokud nastavíte hodnotu **true**, nastavení týkající se nasazení se používají během nasazení. Tato nastavení lze nastavit jako nastavení aplikace nebo v `.deployment` konfigurační soubor umístěný v kořenovém souboru zip. Další informace najdete v tématu [úložiště a nastavení týkající se nasazení](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) v odkaz na nasazení.