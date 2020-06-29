# サイト対サイトのVPNゲートウェイを作成する

元ネタ：https://docs.microsoft.com/ja-jp/learn/modules/connect-on-premises-network-with-vpn-gateway/3-exercise-prepare-azure-and-on-premises-vnets-using-azure-cli-commands

## 前提

- オンプレ環境は無いので、2つのVNetをAzureとオンプレに見立てて構成する。

## 手順

1. Azure 側のリソースを作成する
   1. VNet作成 `az network vnet create`
   2. サブネット作成 `az network vnet subnet creat`
   3. ローカルゲートウェイ作成 `az network local-gateway create`
2. （シミュレートした）オンプレミスの作成
   1. VNet作成 `az network vnet create`
   2. サブネット作成 `az network vnet subnet creat`
   3. ローカルゲートウェイ作成 `az network local-gateway create`
   4. 確認 `az network vnet list` `az network local-gateway list`
3. Azure側VPNゲートウェイ作成
   1. パブリックIPの作成 `az network public-ip create`
   2. 仮想ネットワークゲートウェイの作成
4. （シミュレートされた）オンプレミスのVPNゲートウェイ作成
   1. パブリックIPの作成 `az network public-ip creat`
   2. 仮想ネットワークゲートウェイの作成
5. ローカルゲートウェイのIP参照を更新
   1. Azure側
   2. オンプレ側
6. 接続の作成
   1. Azure側 `az network vpn-connection create `
   2. オンプレ側

## 1.Azure 側のリソースを作成する

### Vnet作成

```sh
az network vnet create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --name Azure-VNet-1 \
    --address-prefix 10.0.0.0/16 \
    --subnet-name Services \
    --subnet-prefix 10.0.0.0/24
```

### サブネット作成

```sh
az network vnet subnet create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --vnet-name Azure-VNet-1 \
    --address-prefix 10.0.255.0/27 \
    --name GatewaySubnet
```

### ローカルゲートウェイ作成

この段階ではまだ大対抗のゲートウェイのIPアドレスがわからないので、後で更新する必要がある。
ここでは仮の値94.X.Y.160を設定しておく。

```sh
az network local-gateway create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --gateway-ip-address 94.X.Y.160 \
    --name LNG-HQ-Network \
    --local-address-prefixes 172.16.0.0/16
```

## 2. （シミュレートした）オンプレミスの作成

### Vnet作成

```sh
az network vnet create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --name HQ-Network \
    --address-prefix 172.16.0.0/16 \
    --subnet-name Applications \
    --subnet-prefix 172.16.0.0/24
```

### サブネット作成

```sh
az network vnet subnet create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --address-prefix 172.16.255.0/27 \
    --name GatewaySubnet \
    --vnet-name HQ-Network
```

### ローカルゲートウェイ作成

この段階ではまだ大対抗のゲートウェイのIPアドレスがわからないので、後で更新する必要がある。
ここでも仮の値94.X.Y.160を設定しておく。

```sh
az network local-gateway create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --gateway-ip-address 94.X.Y.160 \
    --name LNG-Azure-VNet-1 \
    --local-address-prefixes 10.0.0.0/16
```

### 確認

```sh
oh_tomtom@Azure:~$ az network vnet list --output table
Name          ResourceGroup                               Location    NumSubnets    Prefixes       DnsServers    DDOSProtection    VMProtection
------------  ------------------------------------------  ----------  ------------  -------------  ------------  ----------------  --------------
Azure-VNet-1  learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX  westus      2             10.0.0.0/16                  False             False
HQ-Network    learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX  westus      2             172.16.0.0/16                False             False

oh_tomtom@Azure:~$ az network local-gateway list \
>     --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
>     --output table
Name              Location    ResourceGroup                               ProvisioningState    GatewayIpAddress    AddressPrefixes
----------------  ----------  ------------------------------------------  -------------------  ------------------  -----------------
LNG-Azure-VNet-1  westus      learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX  Succeeded            94.X.Y.160        10.0.0.0/16
LNG-HQ-Network    westus      learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX  Succeeded            94.X.Y.160        172.16.0.0/16
```

## 3. Azure側VPNゲートウェイ作成

### パブリックIPの作成

```sh
az network public-ip create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --name PIP-VNG-Azure-VNet-1 \
    --allocation-method Dynamic
```

### 仮想ネットワークゲートウェイの作成

```sh
az network vnet-gateway create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --name VNG-Azure-VNet-1 \
    --public-ip-address PIP-VNG-Azure-VNet-1 \
    --vnet Azure-VNet-1 \
    --gateway-type Vpn \
    --vpn-type RouteBased \
    --sku VpnGw1 \
    --no-wait
```

## 4．（シミュレートされた）オンプレミスのVPNゲートウェイ作成

### パブリックIPの作成

```sh
az network public-ip create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --name PIP-VNG-HQ-Network \
    --allocation-method Dynamic
```

### 仮想ネットワークゲートウェイの作成

```sh
az network vnet-gateway create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --name VNG-HQ-Network \
    --public-ip-address PIP-VNG-HQ-Network \
    --vnet HQ-Network \
    --gateway-type Vpn \
    --vpn-type RouteBased \
    --sku VpnGw1 \
    --no-wait
```


```sh
Every 5.0s: az network vnet-gateway list --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX --ou...  Thu Jun 25 09:34:02 2020

ActiveActive    EnableBgp    EnablePrivateIpAddress    GatewayType    Location    Name              ProvisioningState    ResourceGroup
                               ResourceGuid                          VpnGatewayGeneration    VpnType
--------------  -----------  ------------------------  -------------  ----------  ----------------  -------------------  -------------
-----------------------------  ------------------------------------  ----------------------  ----------
False           False        False                     Vpn            westus      VNG-Azure-VNet-1  Updating             learn-d1be69d
3-cdfb-4940-acda-c18f0799f4a1  12eb87e5-4d5c-4fb3-b053-19e6f0c2157b  Generation1             RouteBased
False           False        False                     Vpn            westus      VNG-HQ-Network    Updating             learn-d1be69d
3-cdfb-4940-acda-c18f0799f4a1  84adfd3f-eb4a-41a0-9edc-b2e70c9f8950  Generation1             RouteBased

Every 5.0s: az network vnet-gateway list --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX --ou...  Thu Jun 25 09:59:59 2020

ActiveActive    EnableBgp    EnablePrivateIpAddress    GatewayType    Location    Name              ProvisioningState    ResourceGroup
                               ResourceGuid                          VpnGatewayGeneration    VpnType
--------------  -----------  ------------------------  -------------  ----------  ----------------  -------------------  -------------
-----------------------------  ------------------------------------  ----------------------  ----------
False           False        False                     Vpn            westus      VNG-Azure-VNet-1  Succeeded            learn-d1be69d
3-cdfb-4940-acda-c18f0799f4a1  12eb87e5-4d5c-4fb3-b053-19e6f0c2157b  Generation1             RouteBased
False           False        False                     Vpn            westus      VNG-HQ-Network    Succeeded            learn-d1be69d
3-cdfb-4940-acda-c18f0799f4a1  84adfd3f-eb4a-41a0-9edc-b2e70c9f8950  Generation1             RouteBased

```

## 5．ローカルゲートウェイのIP参照を更新

### Azure側

```sh
oh_tomtom@Azure:~$ PIPVNGAZUREVNET1=$(az network public-ip show \
>     --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
>     --name PIP-VNG-Azure-VNet-1 \
>     --query "[ipAddress]" \
>     --output tsv)
oh_tomtom@Azure:~$ az network local-gateway update \
>     --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
>     --name LNG-Azure-VNet-1 \
>     --gateway-ip-address $PIPVNGAZUREVNET1
{
  "bgpSettings": null,
  "etag": "W/\"1e404bc7-3851-4245-be6c-36d68626c32b\"",
  "fqdn": null,
  "gatewayIpAddress": "104.42.193.119",
  "id": "/subscriptions/538be7b9-15c9-4be0-a2ee-8d948e33ad6a/resourceGroups/learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX/providers/Microsoft.Network/localNetworkGateways/LNG-Azure-VNet-1",
  "localNetworkAddressSpace": {
    "addressPrefixes": [
      "10.0.0.0/16"
    ]
  },
  "location": "westus",
  "name": "LNG-Azure-VNet-1",
  "provisioningState": "Succeeded",
  "resourceGroup": "learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX",
  "resourceGuid": "e9309b44-b125-486d-a1fc-d156d4c7f942",
  "tags": null,
  "type": "Microsoft.Network/localNetworkGateways"
}
```

### オンプレ側

```sh
oh_tomtom@Azure:~$ PIPVNGHQNETWORK=$(az network public-ip show \
>     --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
>     --name PIP-VNG-HQ-Network \
>     --query "[ipAddress]" \
>     --output tsv)
oh_tomtom@Azure:~$ az network local-gateway update \
>     --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
>     --name LNG-HQ-Network \
>     --gateway-ip-address $PIPVNGHQNETWORK
{
  "bgpSettings": null,
  "etag": "W/\"130b6860-a758-40c0-b0fe-9a22e68290d3\"",
  "fqdn": null,
  "gatewayIpAddress": "23.100.35.211",
  "id": "/subscriptions/538be7b9-15c9-4be0-a2ee-8d948e33ad6a/resourceGroups/learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX/providers/Microsoft.Network/localNetworkGateways/LNG-HQ-Network",
  "localNetworkAddressSpace": {
    "addressPrefixes": [
      "172.16.0.0/16"
    ]
  },
  "location": "westus",
  "name": "LNG-HQ-Network",
  "provisioningState": "Succeeded",
  "resourceGroup": "learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX",
  "resourceGuid": "022ba615-50f8-462f-b637-c5d82f9ae7c7",
  "tags": null,
  "type": "Microsoft.Network/localNetworkGateways"
}
```

## 6．接続の作成

### 共有キーを作成

```sh
oh_tomtom@Azure:~$ SHAREDKEY=XXXXXXXXXXXXXXXX
oh_tomtom@Azure:~$
```

### Azure側

```sh
az network vpn-connection create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --name Azure-VNet-1-To-HQ-Network \
    --vnet-gateway1 VNG-Azure-VNet-1 \
    --shared-key $SHAREDKEY \
    --local-gateway2 LNG-HQ-Network
```

### オンプレ側

```sh
az network vpn-connection create \
    --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
    --name HQ-Network-To-Azure-VNet-1  \
    --vnet-gateway1 VNG-HQ-Network \
    --shared-key $SHAREDKEY \
    --local-gateway2 LNG-Azure-VNet-1
```

## 7. 接続確認

```sh
oh_tomtom@Azure:~$ az network vpn-connection show \
>     --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
>     --name Azure-VNet-1-To-HQ-Network  \
>     --output table \
>     --query '{Name:name,ConnectionStatus:connectionStatus}'
Name                        ConnectionStatus
--------------------------  ------------------
Azure-VNet-1-To-HQ-Network  Connected
oh_tomtom@Azure:~$ az network vpn-connection show \
>     --resource-group learn-d1be69d3-cdfb-4940-acda-XXXXXXXXXXXX \
>     --name HQ-Network-To-Azure-VNet-1  \
>     --output table \
>     --query '{Name:name,ConnectionStatus:connectionStatus}'
Name                        ConnectionStatus
--------------------------  ------------------
HQ-Network-To-Azure-VNet-1  Connected
```