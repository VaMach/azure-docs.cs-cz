V příkladech v tomto článku vyžadují verze 3.0 nebo novější prostředí Azure PowerShell. Pokud nemáte verze 3.0 nebo novější, [aktualizujte verzi](/powershell/azureps-cmdlets-docs/) pomocí Galerie prostředí PowerShell nebo webové platformy.

Pokud chcete zobrazit existující značky pro *skupinu prostředků*, použijte:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Výstup tohoto skriptu bude v následujícím formátu:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

Pokud chcete zobrazit existující značky pro *prostředek s konkrétním ID prostředku*, použijte:

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

Nebo, pokud chcete zobrazit existující značky pro *prostředek se zadaným názvem a skupinou prostředků*, použijte:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

Pokud chcete získat *skupiny prostředků s konkrétní značkou*, použijte:

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name
```

Pokud chcete získat *prostředky s konkrétní značkou*, použijte:

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

Pokaždé, když použijete značky na prostředek nebo skupinu prostředků, přepíšete pro daný prostředek nebo skupinu prostředků existující značky. Proto je nutné použít jiný přístup na základě toho, jestli prostředek nebo skupina prostředků má existující značky.

Pokud chcete přidat značky ke *skupině prostředků bez existujících značek*, použijte:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

Pokud chcete přidat značky ke *skupině prostředků s existujícími značkami*, načtěte existující značky, přidejte novou značku a znovu tyto značky použijte:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

Pokud chcete přidat značky k *prostředku bez existujících značek*, použijte:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId $r.ResourceId -Force
```

Pokud chcete přidat značky k *prostředku s existujícími značkami*, použijte:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
$r.tags += @{Status="Approved"}
Set-AzureRmResource -Tag $r.Tags -ResourceId $r.ResourceId -Force
```

Pokud chcete použít všechny značky ze skupiny prostředků na prostředky a *nezachovat existující značky u prostředků*, použijte tento skript:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups)
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
}
```

Pokud chcete použít všechny značky ze skupiny prostředků na prostředky a *zachovat existující značky u prostředků, které nejsou duplikáty*, použijte tento skript:

```powershell
$group = Get-AzureRmResourceGroup "examplegroup"
if ($group.Tags -ne $null) {
    $resources = $group | Find-AzureRmResource
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        foreach ($key in $group.Tags.Keys)
        {
            if (($resourcetags) -AND ($resourcetags.ContainsKey($key))) { $resourcetags.Remove($key) }
        }
        $resourcetags += $group.Tags
        Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
    }
}
```

Pokud chcete odebrat všechny značky, předejte prázdnou zatřiďovací tabulku:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```
