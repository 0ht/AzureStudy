# タグで管理を楽にする

https://docs.microsoft.com/ja-jp/azure/azure-resource-manager/management/tag-resources#code-try-25

- タグはテキスト データの名前と値を組み合わせたものであり、これをリソースやリソース グループに付けることが可能
- 1 つのリソースには最大 50 個のタグを設定可能
- タグ名は512 文字に制限
  - ストレージ アカウントの場合、128 文字制限
- タグ値は256 文字制限
- タグは親リソースから継承されない
- タグはCase Censitive

## タグの付与方法

### Azure CLI

#### タグを上書き

```sh :AzureCLI
az resource tag 
    --tags Department=Finance \
    --resource-group msftlearn-core-infrastructure-rg \
    --name msftlearn-vnet1 \
    --resource-type "Microsoft.Network/virtualNetworks"

az resource tag 
    --tags 'Dept=IT' 'Environment=Test' 
    -g examplegroup 
    -n examplevnet 
    --resource-type "Microsoft.Network/virtualNetworks"
```

#### タグを追加

```sh
az resource update 
    --set tags.'Status'='Approved' 
    -g examplegroup 
    -n examplevnet 
    --resource-type "Microsoft.Network/virtualNetworks"
```


### PowerShell

#### 新規で追加する場合

`New-AzTag`を使用

```sh :PowerShell
$tags = @{"Dept"="Finance"; "Status"="Normal"}
$resource = Get-AzResource -Name demoStorage -ResourceGroup demoGroup
New-AzTag -ResourceId $resource.id -Tag $tags
```

#### 追加する場合

`Update-AzTag`を使用、
`-Operation Merge`を指定する

```sh
$tags = @{"Dept"="Finance"; "Status"="Normal"}
Update-AzTag -ResourceId $resource.id -Tag $tags -Operation Merge
```

