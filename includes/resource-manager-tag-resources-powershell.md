Verze 3.0 modulu AzureRm.Resources zavedla podstatné změny při práci se značkami. Než budete pokračovat, zkontrolujte svoji verzi:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Pokud výsledky ukazují verzi 3.0 nebo novější, příklady v tomto tématu budou s vaším prostředím fungovat. Pokud nemáte verzi 3.0 nebo novější, než budete v tomto tématu pokračovat, [aktualizujte verzi](/powershell/azureps-cmdlets-docs/) pomocí Galerie prostředí PowerShell nebo Instalačního programu webové platformy.

```powershell
Version
-------
3.5.0
```

Pokaždé, když použijete značky na prostředek nebo skupinu prostředků, přepíšete pro daný prostředek nebo skupinu prostředků existující značky. Proto je nutné použít jiný přístup na základě toho, jestli prostředek nebo skupina prostředků má existující značky, které chcete zachovat. Přidání značek pro:

* Skupina prostředků bez existujících značek

  ```powershell
  Set-AzureRmResourceGroup -Name TagTestGroup -Tag @{ Dept="IT"; Environment="Test" }
  ```

* Skupina prostředků s existujícími značkami

  ```powershell
  $tags = (Get-AzureRmResourceGroup -Name TagTestGroup).Tags
  $tags += @{Status="Approved"}
  Set-AzureRmResourceGroup -Tag $tags -Name TagTestGroup
  ```

* Prostředek bez existujících značek

  ```powershell
  Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName storageexample -ResourceGroupName TagTestGroup -ResourceType Microsoft.Storage/storageAccounts
  ```

* Prostředek s existujícími značkami

  ```powershell
  $tags = (Get-AzureRmResource -ResourceName storageexample -ResourceGroupName TagTestGroup).Tags
  $tags += @{Status="Approved"}
  Set-AzureRmResource -Tag $tags -ResourceName storageexample -ResourceGroupName TagTestGroup -ResourceType Microsoft.Storage/storageAccounts
  ```

Pokud chcete použít všechny značky ze skupiny prostředků na prostředky a **nezachovat existující značky u prostředků**, použijte tento skript:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

Pokud chcete použít všechny značky ze skupiny prostředků na prostředky a **zachovat existující značky u prostředků, které nejsou duplikáty**, použijte tento skript:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    if ($g.Tags -ne $null) {
        $resources = Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName 
        foreach ($r in $resources)
        {
            $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
            foreach ($key in $g.Tags.Keys)
            {
                if ($resourcetags.ContainsKey($key)) { $resourcetags.Remove($key) }
            }
            $resourcetags += $g.Tags
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Chcete-li odebrat všechny značky, předejte prázdné asociativní pole.

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name TagTestGgroup
```

Chcete-li získat skupiny prostředků s konkrétní značkou, použijte rutinu `Find-AzureRmResourceGroup`.

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

Chcete-li získat všechny prostředky s určitou značkou a hodnotou, použijte rutinu `Find-AzureRmResource`.

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```



<!--HONumber=Feb17_HO3-->


