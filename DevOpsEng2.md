# 分野 2: 構成管理および Infrastructure as Code  
   * 2.1 展開ニーズに基づいて展開サービスを決める。  
   * 2.2 業務ニーズに基づいて、アプリケーションとインフラストラクチャの展開モデルを決める。  
   * 2.3 リソースプロビジョニングの自動化においてセキュリティ概念を適用する。  
   * 2.4 展開にライフサイクルフックを実装する方法を決める。  
   * 2.5 AWS の構成管理ツールおよび構成管理サービスを使用してシステムを管理するために必要な、概念を適用する。  

## 分野 2の問題
DevOpsのエンジニアがAWS Lambda関数を作成し、それをAWS CloudFormationテンプレートスニペット（下記参照）で定義し、それをAmazon S3バケットに格納しました。  

```rb
MyLambdaFunctionV1:
  Type: “AWS::Lambda::Function”
    Properties:
      Handler: “index.handler”
      Role: “arn:aws:iam::515290864834:role/AccountScanner”
      Code:
        S3Bucket: “johndoe-com-lambda-source”
        S3Key: “AccountScanner.zip”
    Runtime: “dotnetcore2.1”
    Timeout: 60
```
CloudFormationスタックが作成され、Lambda関数は予想通りに機能しています。 
エンジニアは新しいバージョンの関数コードを入手したので、スタックの更新直後にこの新しいバージョンが実行されるようにしたいと考えています。  
どのデプロイ手順でこれを達成できますか？（3つ選択）  

A. CloudFormationテンプレートのLambda関数の論理名をMyLambdaFunctionV1からMyLambdaFunctionV2に更新してから、CloudFormationスタックの更新を実行します。  
B. 既存のS3バケットでバージョン管理を有効にします。新しいコードを既存のS3バケットにアップロードします。
CloudFormationテンプレートのLambda関数のS3ObjectVersionプロパティでS3オブジェクトのバージョンIDを指定してから、
CloudFormationスタックの更新を実行します。  
C. AWS SAMを使用して、CloudFormationテンプレートにsam deployコマンドを発行して、Lambda関数のバージョンアップデートを実行します。  
D. CloudFormationテンプレートのLambda関数のS3バケットプロパティを更新して、別のバケットの場所を指すようにします。
新しいコードを新しいS3バケットの場所にアップロードしてから、CloudFormationスタックの更新を実行します。  
E. CloudFormationテンプレートのLambda関数のS3Keyプロパティを更新して、.zipファイルの場所と名前を変更します。
.zipファイルの場所と名前の変更に注意して新しいコードをS3バケットにアップロードしてから、CloudFormationスタックの更新を実行します。  
F. サーバーレスフレームワークを使用して、 “serverless deploy function -f MyLambdaFunctionV1“コマンドを発行して既存のLambda機能を更新します。  

解説：
この問題では、CloudFormation で既存スタックを更新する場合の知識が問われています。
CloudFormation でスタックを更新する場合は、何らかの方法でテンプレートのコードが前回と変わっている必要性がありますので、
元のコードから変更が発生する選択肢が正解ということになります。
A は、そもそも既存のLambda 関数の更新ですので、論理名を変更してはいけません。これは間違いです。
B は、S3 のバージョニングを有効化しますので、新しい関数のデプロイコードにはバージョンID の指定を追加します。
前回コードと内容が変わりますので正解となります。
C は、AWS SAM を利用するという点ですが、今回のテンプレートはSAM 形式ではありません。SAM はCloudFormation の拡張フォーマットになりますのでSAM に対応するには大幅な変更が必要となります。よって、C は選択肢とはなりません。
D はS3 バケット名やキーが元のコードから変わりますので正解です。
E はコードのKey 部分が変わりますので正解です。
F はCと同様です。
よって、この問題の正解は、B、D、E となります。
