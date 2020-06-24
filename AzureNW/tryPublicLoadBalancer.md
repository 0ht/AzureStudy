# Azure Load Balancer をパブリックに構成する

基本、MS Learnのシナリオをそのまま実行してみる。

前提
- 1つの可用性セット内に2台の仮想マシン

手順は以下の通り

1. (準備) VM・アプリのデプロイ
1. Load Balancerを作成
   1. パブリックIP作成
   2. Load Balancer作成
   3. プローブ作成
   4. ルール作成
   5. バックエンドプールの構成

##  (準備) VM・アプリのデプロイ

以下のGithubにサンプルシェルを実行して、割り振り先VMを構成する。
（ARMテンプレートかと思いきやAzure CLI・・・）

```sh
git clone https://github.com/MicrosoftDocs/mslearn-improve-app-scalability-resiliency-with-load-balancer.git
cd mslearn-improve-app-scalability-resiliency-with-load-balancer

bash create-high-availability-vm-with-sets.sh learn-2a118604-18ff-47d4-9f6a-dee183edd593
```

以下の様に出来上がる。

```sh
oh_tomtom@Azure:~/mslearn-improve-app-scalability-resiliency-with-load-balancer$ az vm list --output tsv
None            None    None    None            None    /subscriptions/6fed9ee8-9478-4abb-9141-f53f6943f1e6/resourceGroups/LEARN-2A118604-18FF-47D4-9F6A-DEE183EDD593/providers/Microsoft.Compute/virtualMachines/webVM1      None    None    None    westus  webVM1       None     None    Succeeded       None    LEARN-2A118604-18FF-47D4-9F6A-DEE183EDD593      None                    Microsoft.Compute/virtualMachines     None    055dd782-82ed-44da-9ad3-a6f3c4fd4f2f    None
None            None    None    None            None    /subscriptions/6fed9ee8-9478-4abb-9141-f53f6943f1e6/resourceGroups/LEARN-2A118604-18FF-47D4-9F6A-DEE183EDD593/providers/Microsoft.Compute/virtualMachines/webVM2      None    None    None    westus  webVM2       None     None    Succeeded       None    LEARN-2A118604-18FF-47D4-9F6A-DEE183EDD593      None                    Microsoft.Compute/virtualMachines     None    d00964b5-1e00-48fc-bcb6-e9dd5202c208    None
oh_tomtom@Azure:~/mslearn-improve-app-scalability-resiliency-with-load-balancer$

oh_tomtom@Azure:~/mslearn-improve-app-scalability-resiliency-with-load-balancer$ az vm list --output table
Name    ResourceGroup                               Location    Zones
------  ------------------------------------------  ----------  -------
webVM1  LEARN-2A118604-18FF-47D4-9F6A-DEE183EDD593  westus
webVM2  LEARN-2A118604-18FF-47D4-9F6A-DEE183EDD593  westus
```

## Load Balancer作成

### パブリックIP作成

myPublicIPという名前でパブリックIPを作成する。

```sh
az network public-ip create \
  --resource-group learn-2a118604-18ff-47d4-9f6a-dee183edd593 \
  --allocation-method Static \
  --name myPublicIP
```

### Load Balancer作成

前ステップで作成した

```sh
az network lb create \
  --resource-group learn-2a118604-18ff-47d4-9f6a-dee183edd593 \
  --name myLoadBalancer \
  --public-ip-address myPublicIP \
  --frontend-ip-name myFrontEndPool \
  --backend-pool-name myBackEndPool
```