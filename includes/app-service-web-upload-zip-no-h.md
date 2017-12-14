V [portál Azure](https://portal.azure.com), klikněte na tlačítko **skupiny prostředků** > **cloudu-prostředí-úložiště -\<your_region >**  >   **\<storage_account_name >**.

![Najít účet úložiště pro cloudové prostředí](../articles/app-service/media/app-service-deploy-zip/upload-choose-storage-account.png)

V **přehled** stránky účtu úložiště, vyberte **soubory**.

Vyberte automaticky generované sdílené složky a vyberte **nahrát**. Tuto sdílenou složku připojené v prostředí cloudu jako `clouddrive`.

![Najít tlačítko odeslání](../articles/app-service/media/app-service-deploy-zip/upload-select-button.png)

Klikněte na tlačítko modulu pro výběr souborů a souboru ZIP a potom klikněte na tlačítko **nahrát**. 

V prostředí cloudu, použijte `ls` ověřit, zda se zobrazí nahrávaný soubor ZIP ve výchozí `clouddrive` sdílet.

```azurecli-interactive
ls clouddrive
```
