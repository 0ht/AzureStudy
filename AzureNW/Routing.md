# ルーティング機能を知る・試す

## 概要

- 通常、Azureネットワークのトラフィックは、規定で割り当てられるシステムルートによって制御されるので、特に意識的にルートを切る必要はない
- しかし、NVA（Network Virtual Appliance）の導入やパートナー、FWなどを追加して独自のルート制御を行いたいという場合がある
- そのような場合にはカスタムルートを追加して制御を行うことができる。

## カスタムルート実装のオプション

カスタムルートの設定には、2つのオプションがある

### ユーザー定義ルート

システムルートをオーバーライドする。設定時には、次のホップの種類を指定することになる。

- **仮想アプライアンス**
  - 通常、仮想アプライアンスはファイアウォール デバイスであり、ネットワークを出入りするトラフィックを分析またはフィルター処理するために使われます。 仮想マシンに接続されている NIC のプライベート IP アドレスを指定して、IP 転送を有効にすることができます。 または、内部ロード バランサーのプライベート IP アドレスを指定することができます。
- **仮想ネットワーク ゲートウェイ**
  - 特定のアドレスのルートで、仮想ネットワーク ゲートウェイにルーティングするタイミングを示すために使用します。 仮想ネットワーク ゲートウェイは、次ホップの種類に対する VPN として指定されます。
- **仮想ネットワーク**
  - 仮想ネットワーク内の既定のシステム ルートをオーバーライドするために使用します。
- **インターネット**
  - インターネットにルーティングされる指定されたアドレス プレフィックスにトラフィックをルーティングするために使用します。
- **なし**
  - 指定されたアドレス プレフィックスに送信されたトラフィックを削除するために使用します。

ユーザー定義ルートでは、次ホップの種類として VirtualNetworkServiceEndpoint を指定することはできません。これは、仮想ネットワーク ピアリングを示します。

### BGPによるネットワーク間のルート交換

- オンプレミス ネットワーク内のネットワーク ゲートウェイでは、BGP を使って、Azure 内の仮想ネットワーク ゲートウェイとルートを交換
- Azure ExpressRoute 経由で Azure データセンターに接続している場合は、通常、BGP を使用してオンプレミス ルートを Azure にアドバタイズ
- VPN サイト間接続を使用して Azure 仮想ネットワークに接続する場合も、BGP を構成することができる

次の図は、Azure VPN Gateway とオンプレミス ネットワークの間でデータを渡すことができるパスを含むトポロジを示しています。

![picture 27](images/f709fa2ddd9255ae178ddbe52ce6aaf0bcf148fd06f64fd2fe59764191298af4.png)  

Border Gateway Protocol の使用例を示す図

BGP では、接続パスがダウンした場合、ルーターは接続を迅速に変更してパケットを送信できるため、ネットワークの安定性が提供されます。

## 試してみる

例によってCloud Shellで確認する。
最終的には、以下のような構成・ルートを定義する。

![picture 28](images/31144ab5d5cd35ccced0b19fd69d3e2c5fb26f9135eb8e3ec3be8b9f2b876eec.png)  

nva ネットワーク アプライアンスを dmzsubnet サブネットにデプロイします。 その後、publicsubnet からのトラフィックと、カスタム ルートを使用するトラフィックが privatesubnet サブネットに送信されるように、IP 転送を有効にします。

### 1．ルートテーブルとカスタムルートの作成

まずリソースグループを作成。
```
az group create \
    --name CustomRoute-rg \
    --location japaneast
{
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg",
  "location": "japaneast",
  "managedBy": null,
  "name": "CustomRoute-rg",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```

ルートテーブルを作成、そのまんま、route-table create。実際のルート定義を入れる入れ物みたいなもの。

```
az network route-table create \
    --name publictable \
    --resource-group CustomRoute-rg \
    --disable-bgp-route-propagation false
{- Finished ..
  "disableBgpRoutePropagation": false,
  "etag": "W/\"55ca8069-42c5-40d4-b4ff-684dcc74b278\"",
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/routeTables/publictable",
  "location": "japaneast",
  "name": "publictable",
  "provisioningState": "Succeeded",
  "resourceGroup": "CustomRoute-rg",
  "routes": [],
  "subnets": null,
  "tags": null,
  "type": "Microsoft.Network/routeTables"
}
```

カスタムルートを作成

```
az network route-table route create \
    --route-table-name publictable \
    --resource-group CustomRoute-rg \
    --name productionsubnet \
    --address-prefix 10.0.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 10.0.2.4
{- Finished ..
  "addressPrefix": "10.0.1.0/24",
  "etag": "W/\"964b81d4-54c2-403a-948d-ae92f3c086f2\"",
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/routeTables/publictable/routes/productionsubnet",
  "name": "productionsubnet",
  "nextHopIpAddress": "10.0.2.4",
  "nextHopType": "VirtualAppliance",
  "provisioningState": "Succeeded",
  "resourceGroup": "CustomRoute-rg",
  "type": "Microsoft.Network/routeTables/routes"
}
```

ルートテーブルを確認してみると、テーブル情報が追加されている。

```
az network route-table show     --name publictable     --resource-group CustomRoute-rg
{
  "disableBgpRoutePropagation": false,
  "etag": "W/\"964b81d4-54c2-403a-948d-ae92f3c086f2\"",
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/routeTables/publictable",
  "location": "japaneast",
  "name": "publictable",
  "provisioningState": "Succeeded",
  "resourceGroup": "CustomRoute-rg",
  "routes": [
    {
      "addressPrefix": "10.0.1.0/24",
      "etag": "W/\"964b81d4-54c2-403a-948d-ae92f3c086f2\"",
      "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/routeTables/publictable/routes/productionsubnet",
      "name": "productionsubnet",
      "nextHopIpAddress": "10.0.2.4",
      "nextHopType": "VirtualAppliance",
      "provisioningState": "Succeeded",
      "resourceGroup": "CustomRoute-rg",
      "type": "Microsoft.Network/routeTables/routes"
    }
  ],
  "subnets": null,
  "tags": null,
  "type": "Microsoft.Network/routeTables"
}
```

### 2. 仮想ネットワークとサブネットを作成する

仮想ネットワーク作成

```
az network vnet create \
    --name vnet \
    --resource-group CustomRoute-rg \
    --address-prefix 10.0.0.0/16 \
    --subnet-name publicsubnet \
    --subnet-prefix 10.0.0.0/24
```

```
az network vnet subnet create \
    --name privatesubnet \
    --vnet-name vnet \
    --resource-group CustomRoute-rg \
    --address-prefix 10.0.1.0/24
```

```
az network vnet subnet create \
    --name dmzsubnet \
    --vnet-name vnet \
    --resource-group CustomRoute-rg \
    --address-prefix 10.0.2.0/24
```

```
az network vnet subnet list \
    --resource-group CustomRoute-rg \
    --vnet-name vnet \
    --output table
    AddressPrefix    Name           PrivateEndpointNetworkPolicies    PrivateLinkServiceNetworkPolicies    ProvisioningState    ResourceGroup
---------------  -------------  --------------------------------  -----------------------------------  -------------------  ---------------
10.0.0.0/24      publicsubnet   Enabled                           Enabled                              Succeeded            CustomRoute-rg
10.0.1.0/24      privatesubnet  Enabled                           Enabled                              Succeeded            CustomRoute-rg
10.0.2.0/24      dmzsubnet      Enabled                           Enabled                              Succeeded            CustomRoute-rg
```

### 3．ルート テーブルをパブリック サブネットに関連付ける

```
az network vnet subnet update \
    --name publicsubnet \
    --vnet-name vnet \
    --resource-group CustomRoute-rg \
    --route-table publictable

{
  "addressPrefix": "10.0.0.0/24",
  "addressPrefixes": null,
  "delegations": [],
  "etag": "W/\"18101b4c-3d15-4e37-b116-13fe946bdf40\"",
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/virtualNetworks/vnet/subnets/publicsubnet",
  "ipAllocations": null,
  "ipConfigurationProfiles": null,
  "ipConfigurations": null,
  "name": "publicsubnet",
  "natGateway": null,
  "networkSecurityGroup": null,
  "privateEndpointNetworkPolicies": "Enabled",
  "privateEndpoints": null,
  "privateLinkServiceNetworkPolicies": "Enabled",
  "provisioningState": "Succeeded",
  "purpose": null,
  "resourceGroup": "CustomRoute-rg",
  "resourceNavigationLinks": null,
  "routeTable": {
    "disableBgpRoutePropagation": null,
    "etag": null,
    "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/routeTables/publictable",
    "location": null,
    "name": null,
    "provisioningState": null,
    "resourceGroup": "CustomRoute-rg",
    "routes": null,
    "subnets": null,
    "tags": null,
    "type": null
  },
  "serviceAssociationLinks": null,
  "serviceEndpointPolicies": null,
  "serviceEndpoints": null,
  "type": "Microsoft.Network/virtualNetworks/subnets"
}

```

### 4. NVAをデプロイする

便宜的に？NVAとしてubuntuのVMをデプロイする。後にこのVMでIP転送を行うように構成してルーティングを確認する。

```
az vm create \
    --resource-group CustomRoute-rg \
    --name nva \
    --vnet-name vnet \
    --subnet dmzsubnet \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password Zaq12wsxcde#
{- Finished ..
  "fqdns": "",
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Compute/virtualMachines/nva",
  "location": "japaneast",
  "macAddress": "00-22-48-66-B7-75",
  "powerState": "VM running",
  "privateIpAddress": "10.0.2.4",
  "publicIpAddress": "20.48.50.151",
  "resourceGroup": "CustomRoute-rg",
  "zones": ""
}

NVAIP="$(az vm list-ip-addresses \
    --resource-group CustomRoute-rg \
    --name nva \
    --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
    --output tsv)"

echo $NVAIP
20.48.50.151
```

### 5. Azure ネットワーク インターフェイスの IP 転送を有効にする

```
NICID=$(az vm nic list \
    --resource-group CustomRoute-rg\
    --vm-name nva \
    --query "[].{id:id}" --output tsv)

echo $NICID
/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/networkInterfaces/nvaVMNic
```

```
NICNAME=$(az vm nic show \
    --resource-group CustomRoute-rg \
    --vm-name nva \
    --nic $NICID \
    --query "{name:name}" --output tsv)

echo $NICNAME
nvaVMNic
```

```
az network nic update --name $NICNAME \
    --resource-group CustomRoute-rg \
    --ip-forwarding true
    {
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": [],
    "internalDnsNameLabel": null,
    "internalDomainNameSuffix": "bgcfolouqfmebltify2m0uxunh.lx.internal.cloudapp.net",
    "internalFqdn": null
  },
  "enableAcceleratedNetworking": false,
  "enableIpForwarding": true,
  "etag": "W/\"fb64235c-9377-4684-aced-279ec4dfbdd5\"",
  "hostedWorkloads": [],
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/networkInterfaces/nvaVMNic",
  "ipConfigurations": [
    {
      "applicationGatewayBackendAddressPools": null,
      "applicationSecurityGroups": null,
      "etag": "W/\"fb64235c-9377-4684-aced-279ec4dfbdd5\"",
      "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/networkInterfaces/nvaVMNic/ipConfigurations/ipconfignva",
      "loadBalancerBackendAddressPools": null,
      "loadBalancerInboundNatRules": null,
      "name": "ipconfignva",
      "primary": true,
      "privateIpAddress": "10.0.2.4",
      "privateIpAddressVersion": "IPv4",
      "privateIpAllocationMethod": "Dynamic",
      "privateLinkConnectionProperties": null,
      "provisioningState": "Succeeded",
      "publicIpAddress": {
        "ddosSettings": null,
        "dnsSettings": null,
        "etag": null,
        "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/publicIPAddresses/nvaPublicIP",
        "idleTimeoutInMinutes": null,
        "ipAddress": null,
        "ipConfiguration": null,
        "ipTags": null,
        "location": null,
        "name": null,
        "provisioningState": null,
        "publicIpAddressVersion": null,
        "publicIpAllocationMethod": null,
        "publicIpPrefix": null,
        "resourceGroup": "CustomRoute-rg",
        "resourceGuid": null,
        "sku": null,
        "tags": null,
        "type": null,
        "zones": null
      },
      "resourceGroup": "CustomRoute-rg",
      "subnet": {
        "addressPrefix": null,
        "addressPrefixes": null,
        "delegations": null,
        "etag": null,
        "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/virtualNetworks/vnet/subnets/dmzsubnet",
        "ipAllocations": null,
        "ipConfigurationProfiles": null,
        "ipConfigurations": null,
        "name": null,
        "natGateway": null,
        "networkSecurityGroup": null,
        "privateEndpointNetworkPolicies": null,
        "privateEndpoints": null,
        "privateLinkServiceNetworkPolicies": null,
        "provisioningState": null,
        "purpose": null,
        "resourceGroup": "CustomRoute-rg",
        "resourceNavigationLinks": null,
        "routeTable": null,
        "serviceAssociationLinks": null,
        "serviceEndpointPolicies": null,
        "serviceEndpoints": null
      },
      "type": "Microsoft.Network/networkInterfaces/ipConfigurations",
      "virtualNetworkTaps": null
    }
  ],
  "location": "japaneast",
  "macAddress": "00-22-48-66-B7-75",
  "name": "nvaVMNic",
  "networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "flowLogs": null,
    "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Network/networkSecurityGroups/nvaNSG",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "CustomRoute-rg",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  },
  "primary": true,
  "privateEndpoint": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "CustomRoute-rg",
  "resourceGuid": "fa2c2ba3-12c2-416f-a2a4-b42155c0953d",
  "tags": {},
  "tapConfigurations": [],
  "type": "Microsoft.Network/networkInterfaces",
  "virtualMachine": {
    "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/CustomRoute-rg/providers/Microsoft.Compute/virtualMachines/nva",
    "resourceGroup": "CustomRoute-rg"
  }
}
```

```
ssh -t -o StrictHostKeyChecking=no azureuser@$NVAIP 'sudo sysctl -w net.ipv4.ip_forward=1; exit;'
Warning: Permanently added '20.48.50.151' (ECDSA) to the list of known hosts.
azureuser@20.48.50.151's password:
net.ipv4.ip_forward = 1
Connection to 20.48.50.151 closed.
```

### 5．仮想マシンを作成

cloud-init.txt を作成しておく。
中身
```
#cloud-config
package_upgrade: true
packages:
   - inetutils-traceroute
```

```
az vm create \
    --resource-group CustomRoute-rg \
    --name public \
    --vnet-name vnet \
    --subnet publicsubnet \
    --image UbuntuLTS \
    --admin-username azureuser \
    --no-wait \
    --custom-data cloud-init.txt \
    --admin-password Zaq12wsxcde#
```

```
az vm create \
    --resource-group CustomRoute-rg  \
    --name private \
    --vnet-name vnet \
    --subnet privatesubnet \
    --image UbuntuLTS \
    --admin-username azureuser \
    --no-wait \
    --custom-data cloud-init.txt \
    --admin-password Zaq12wsxcde#
```

作成を確認
```
    watch -d -n 5 "az vm list \
    --resource-group CustomRoute-rg \
    --show-details \
    --query '[*].{Name:name, ProvisioningState:provisioningState, PowerState:powerState}' \
    --output table"

    Every 5.0s: az vm list     --resource-group CustomRoute-rg     --show-details     --query '[*].{Name:name, ProvisioningState:provisioningState, PowerState:powerState}'     --output table                                     Tue Jul 28 02:25:28 2020

Name     ProvisioningState    PowerState
-------  -------------------  ------------
nva      Succeeded            VM running
private  Succeeded            VM running
public   Succeeded            VM running
```

```
PUBLICIP="$(az vm list-ip-addresses \
    --resource-group CustomRoute-rg  \
    --name public \
    --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
    --output tsv)"

echo $PUBLICIP
20.48.40.41
```

```
PRIVATEIP="$(az vm list-ip-addresses \
    --resource-group CustomRoute-rg  \
    --name private \
    --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
    --output tsv)"

echo $PRIVATEIP
20.48.40.242
```

### 6. いざ確認

パブリックからプライベートへのルートをトレースする。
![picture 29](images/b4d9ca0f0a77307a4fe2bfd5fb60b8a7ef09b0ea6f939bd5d9becfa79240f82d.png)  

```
ssh -t -o StrictHostKeyChecking=no azureuser@$PUBLICIP 'traceroute private --type=icmp; exit'

Warning: Permanently added '20.48.40.41' (ECDSA) to the list of known hosts.
azureuser@20.48.40.41's password:
traceroute to private.bgcfolouqfmebltify2m0uxunh.lx.internal.cloudapp.net (10.0.1.4), 64 hops max
  1   10.0.2.4  1.479ms  0.540ms  0.328ms
  2   10.0.1.4  0.930ms  1.621ms  0.916ms
Connection to 20.48.40.41 closed.
```

最初のステップで、publicサブネットにルート定義しているので、privateあてであるにもかかわらず、10.0.2.4のNVA宛に転送されているのがわかる。

次に、プライベートからパブリックのルートを確認する。
![picture 30](images/1c3a3a3a94625e5cc7ae08f740dc56a7c29c5f0f66940378a660144c42f5209a.png)  

```
ssh -t -o StrictHostKeyChecking=no azureuser@$PRIVATEIP 'traceroute public --type=icmp; exit'
Warning: Permanently added '20.48.40.242' (ECDSA) to the list of known hosts.
azureuser@20.48.40.242's password:
traceroute to public.bgcfolouqfmebltify2m0uxunh.lx.internal.cloudapp.net (10.0.0.4), 64 hops max
  1   10.0.0.4  1.553ms  1.652ms  1.242ms
Connection to 20.48.40.242 closed.
```

ここはカスタムのルート定義がないので、そのままストレートにアクセスされている。





