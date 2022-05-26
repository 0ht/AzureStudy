# Azure AD Domain Serviceとは？

そもそもActive Directoryをよくわかってないので、その辺からの理解が必要と思いながらも、ADDSについての概要を調べる。特にWVDでの前提条件となっており、



{
  "code": "DeploymentFailed",
  "message": "少なくとも 1 つのリソースのデプロイ操作に失敗しました。詳細については、デプロイ操作の一覧を表示してください。使用方法の詳細については、https://aka.ms/DeployOperations を参照してください。",
  "details": [
    {
      "code": "Conflict",
      "message": "{\r\n  \"status\": \"Failed\",\r\n  \"error\": {\r\n    \"code\": \"ResourceDeploymentFailure\",\r\n    \"message\": \"リソース操作が完了し、ターミナル プロビジョニング状態は 'Failed' です。\",\r\n    \"details\": [\r\n      {\r\n        \"code\": \"DeploymentFailed\",\r\n        \"message\": \"少なくとも 1 つのリソースのデプロイ操作に失敗しました。詳細については、デプロイ操作の一覧を表示してください。使用方法の詳細については、https://aka.ms/DeployOperations を参照してください。\",\r\n        \"details\": [\r\n          {\r\n            \"code\": \"Conflict\",\r\n            \"message\": \"{\\r\\n  \\\"error\\\": {\\r\\n    \\\"code\\\": \\\"PropertyChangeNotAllowed\\\",\\r\\n    \\\"message\\\": \\\"プロパティ 'adminUsername' は変更できません。\\\",\\r\\n    \\\"target\\\": \\\"adminUsername\\\"\\r\\n  }\\r\\n}\"\r\n          },\r\n          {\r\n            \"code\": \"Conflict\",\r\n            \"message\": \"{\\r\\n  \\\"error\\\": {\\r\\n    \\\"code\\\": \\\"PropertyChangeNotAllowed\\\",\\r\\n    \\\"message\\\": \\\"プロパティ 'adminUsername' は変更できません。\\\",\\r\\n    \\\"target\\\": \\\"adminUsername\\\"\\r\\n  }\\r\\n}\"\r\n          }\r\n        ]\r\n      }\r\n    ]\r\n  }\r\n}"
    }
  ]
}

https://docs.microsoft.com/ja-jp/azure/virtual-desktop/troubleshoot-vm-configuration#vms-are-not-joined-to-the-domain

