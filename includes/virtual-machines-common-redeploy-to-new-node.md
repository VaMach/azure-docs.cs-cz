## <a name="use-the-azure-portal"></a>Použití webu Azure Portal
1. Vyberte virtuální počítač, který chcete znovu zavést a pak vyberte *znovu nasaďte* v tlačítko *nastavení* okno. Budete muset posuňte se dolů a najdete **podpory a řešení potíží** oddíl, který obsahuje tlačítko: znovu nasaďte, jako v následujícím příkladu:
   
    ![Okno Azure virtuálních počítačů](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. Pokud chcete potvrdit operaci, vyberte *znovu nasaďte* tlačítko:
   
    ![Znovu nasaďte okno virtuálních počítačů](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. **Stav** virtuálního počítače se změní na *aktualizace* jako virtuální počítač připraví se znovu nasadit, jak je znázorněno v následujícím příkladu:
   
    ![Aktualizace virtuálního počítače](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. **Stav** poté změní na *počáteční* jako virtuální počítač se spustí na nového hostitele Azure, jak je znázorněno v následujícím příkladu:
   
    ![Spuštění virtuálního počítače](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. Po dokončení procesu spouštění, virtuální počítač **stav** vrátí do *systémem*, označující virtuální počítač má byla znovu úspěšně nasadil:
   
    ![Spuštění virtuálního počítače](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

