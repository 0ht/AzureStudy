# Azure Sentinelを使用してセキュリティ分析

## 概要

こんな時に使えるソリューション

- クラウドやオンプレミスの複数の場所に存在する可能性がある、組織全体の詳細なセキュリティ脅威の監視を行う必要がある。
- 複雑で多様なツールに依存しないようにする必要がある。
- 専門家によって構築されたエンタープライズレベルの AI を使用して、組織全体の脅威を特定し、処理する必要がある。

## 利用手順

1. Azure Sentinelリソースを作成
   1. **Log Analyticsワークスペースを作成してSentinelに追加**
   2. ![picture 25](images/f2eb02692ae0e8dfd1f5b6efa9c417a6afbf784072dc37937f75ad4534a19fc4.png)  
2. データソースに接続
   1. ![picture 26](images/9ad69b4e8e148574a284632716cc55f00357655f3ec9b7a53d9db54430186559.png)  
   2. ![picture 27](images/69c32e78d2b350dc0513293fc024050344eabd2dde28b7681e12b9340b313d8c.png)  
3. データの統合が行われる。
   1. SecurityCenterで設定したプレイブックなども統合されている
   2. ![picture 28](images/6b8ff9065b2571c50b026a1021dece9e3bd0cae0336777d970aca9dcb8a2e6cc.png)  
4. インシデントを確認して、詳細化分析を行っていく
   1. ![picture 29](images/8190e557d59554d4a8d147fba48a7dd19ef05782bf8ec443a24ae60edfa04a6c.png)  
   2. インシデントを担当者に割り当て
      1. ![picture 30](images/661c88c1937a9f33b688fb2919bf3c7df36c42861c7371d94d2d7feebe27aa91.png)  
   3. 詳細を詰めていく
      1. ![picture 31](images/08f27327475c120489adecbd581a6dff2ad929b19cb41f479c2c28a8a5a5cb58.png)  
