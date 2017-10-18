## <a name="deploy-template-from-cloud-shell"></a>Nasazení šablony ze služby Cloud Shell

K nasazení šablony můžete použít [Cloud Shell](../articles/cloud-shell/overview.md). Nejprve je však nutné šablonu nahrát do sdílené složky pro vaši službu Cloud Shell. Pokud jste ještě službu Cloud Shell nepoužívali, přečtěte si téma [Přehled služby Azure Cloud Shell](../articles/cloud-shell/overview.md), kde najdete informace o jejím nastavení.

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

1. Vyberte vaši skupinu prostředků služby Cloud Shell. Vzor názvů je `cloud-shell-storage-<region>`.

   ![Výběr skupiny prostředků](./media/resource-manager-cloud-shell-deploy/select-cs-resource-group.png)

1. Vyberte účet úložiště pro službu Cloud Shell.

   ![Výběr účtu úložiště](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. Vyberte **Soubory**.

   ![Výběr souborů](./media/resource-manager-cloud-shell-deploy/select-files.png)

1. Vyberte sdílenou složku pro službu Cloud Shell. Vzor názvů je `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Výběr sdílené složky](./media/resource-manager-cloud-shell-deploy/select-file-share.png)

1. Vyberte **Přidat adresář**.

   ![Přidání adresáře](./media/resource-manager-cloud-shell-deploy/select-add-directory.png)

1. Pojmenujte ho **templates** a vyberte **OK**.

   ![Pojmenování adresáře](./media/resource-manager-cloud-shell-deploy/name-templates.png)

1. Vyberte nový adresář.

   ![Výběr adresáře](./media/resource-manager-cloud-shell-deploy/select-templates.png)

1. Vyberte **Nahrát**.

   ![Výběr nahrání](./media/resource-manager-cloud-shell-deploy/select-upload.png)

1. Vyhledejte a nahrajte vaši šablonu.

   ![Nahrání souboru](./media/resource-manager-cloud-shell-deploy/upload-files.png)

1. Otevřete příkazový řádek.

   ![Otevření služby Cloud Shell](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
