# AADInternals
## 1.AADInternalsとは
- Azure Active Directory（AAD）及びMicrosoft365を管理するためのPowerShellモジュール
- 主に管理者によって利用される
- 一方、悪意のある攻撃者にも悪用される可能性がある
- 基本的にAADやMicrosoft365のAPIを利用する際は認証情報が必要だが、一部のAPIは認証不要で利用することができるものもあり、それらのAPIを利用してターゲットの情報を収集するという動作をする  
### 補足
- Azure Active Directory(AAD)とは
  - Azure版のADのことで、従来のADと同様な機能を持ちながら、クラウド環境に特化したアイデンティティ及びアクセス管理システムのこと 

## 2.主な機能
- **アカウント探索と列挙**
  - AADユーザの列挙
  - AADグループの列挙
- **デバイス管理**
  - AADへのデバイス登録
- **クラウドサービス探索**
  - Office365やSharePointインスタンス、OpenID設定などの情報収集
- **アカウント操作**
  - 新規AADユーザの作成
- **データ収集**
  - ユーザのOneDriveからファイル収集
- **ドメインおよびテナントポリシーの変更**
  - フェデレーションドメインへの変換によるバックドアの作成
  - DesktopSSO情報の変更
- **認証プロセスの走査**
  - SAMLトークンの作成
  - Kerberosチケットの偽造
- **情報収集**
  - ユーザのメールアドレスの存在確認
  - テナントのドメイン情報収集 
- **認証情報の収集**
  - AADConnectなどのサービスの認証情報収集
  - ADSyncやADFederatedServicesサーバーからの暗号化キーの収集
- **レジストリ操作**
  - パススルー認証エージェントの設定の一部としてのレジストリキーの変更  

## 3.収集できる情報
- 概 要
  - 項目2ではAADInternalの主な機能を上げたが、その機能の中で収集できる情報のみ抽出してより詳細にして下記に整理する
  - ここで収集できる情報は認証不要なAPIや認証済みのAPIを通じて取得することができる
    - しかしながら認証不要APIで取得できる情報は限定的であるため、より詳細な情報を取得するには、適切な認証と権限が必要となる 
  - 一部の情報は、管理者権限でなくても一般ユーザアカウントからでも収集可能

- **ユーザとグループの列挙**
  - AADのユーザアカウント情報
  - グループ情報と所属メンバー
- **テナント情報**
  - テナント名とテナントID
  - 登録されているドメイン名とその種類
  - シームレスシングルサインオン（デスクトップSSO）の状態
- **ライセンス情報**
  - テナントで使用されているライセンスの種類と数
- **メールボックス情報**
  - ユーザのメールボックスの設定や容量
- **ディレクトリ同期の状態**
  - オンプレミスADとAADの同期状態
- **デバイス情報**
  - AADに登録されているデバイスの情報
- **アプリケーション情報**
  - テナントに登録されているアプリケーションの一覧
- **条件付きアクセスポリシー**      
  - 設定されているポリシーの概要
- **多要素認証（MFA）の状態**
  - MFAが有効になっているユーザの一覧
- **フェデレーション設定**
  - ADFSなどのフェデレーション設定情報    

## 5.使い方
- **SETUP**
  - AADInternalはPowerShellもモジュールなので、使用するにはAADinternalのインストールとモジュールインポートが必要
```powerShell
# モジュールのインストール
Install-Module AADInternals

# モジュールのインポート
Import-Module AADInternals
```

- **認証なしで使えるコマンド**
  - 認証不要API（いわゆる公開API）のみを使って取得できる情報とその取得の方法を整理する
```powerShell
# Invoke-AADIntTeconAsOutsider
## テナントの基本情報を収集
Invoke-AADIntReconAsOutsider -DomainName example.com
```
```powerShell
# Get-AADIntLoginInformation
## テナントのログイン情報を取得する
Get-AADIntLoginInformation -Domain example.com
```
```powerShell
# Get-AADIntTenantID
## テナントIDを取得する
Get-AADIntTenantID -Domain example.com
```
```powerShell
# Get-AADIntTenantDomains
## テナントに関連付けられたドメインを取得する
Get-AADIntTenantDomains -Domain example.com
```
```powerShell
# Get-AADIntOpenIDConfiguration
## OpenID設定情報を取得する
Get-AADIntOpenIDConfiguration -Domain example.com
```