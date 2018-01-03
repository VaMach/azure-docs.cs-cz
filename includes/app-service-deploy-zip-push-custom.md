## <a name="deployment-customization"></a>Vlastní nastavení nasazení

Proces nasazení předpokládá, že souboru .zip, který jste push obsahuje připravené ke spuštění aplikace. Ve výchozím nastavení jsou spuštěny žádné úpravy. Pokud chcete povolit stejné procesy sestavení, které jste získali s průběžnou integraci, přidejte následující nastavení aplikace:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

Pokud použijete nasazení nabízené .zip, toto nastavení je **false** ve výchozím nastavení. Výchozí hodnota je **true** průběžnou integraci nasazení. Pokud nastavíte hodnotu **true**, nastavení týkající se nasazení se používají během nasazení. Tato nastavení můžete nakonfigurovat buď jako nastavení aplikace, nebo v konfiguračním souboru .deployment, který je umístěný v kořenovém souboru .zip. Další informace najdete v tématu [úložiště a nastavení týkající se nasazení](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) v odkaz na nasazení.