# ContainerLab Open vSwitch ブリッジ トポロジー

この ContainerLab トポロジーは、Open vSwitch ブリッジを介して接続された2つの Ubuntu コンテナを構成します。

## トポロジー概要

```
ubuntu1 ---- ovs-br ---- ubuntu2
```

- **ubuntu1**: テスト用の Ubuntu コンテナ
- **ovs-br**: ContainerLabのovs-bridgeタイプによるOpen vSwitchブリッジ
- **ubuntu2**: テスト用の Ubuntu コンテナ

## 前提条件

- ContainerLab がインストールされていること
- Docker がインストールされていること
- **Open vSwitch がホストシステムにインストールされていること**

### Open vSwitch のインストール

Ubuntu/Debian システムの場合:
```bash
sudo apt update
sudo apt install -y openvswitch-switch
```

インストール後、Open vSwitchサービスが動作していることを確認してください:
```bash
sudo systemctl status openvswitch-switch
```

## 事前準備 - OVSブリッジの作成

ContainerLabはユーザーのためにブリッジを作成してくれないため、事前にOVSブリッジを作成しておく必要があります。

1. OVSブリッジを作成します:
   ```bash
   sudo ovs-vsctl add-br ovs-br
   ```

2. ブリッジが正しく作成されたことを確認します:
   ```bash
   sudo ovs-vsctl show
   ```

## デプロイ手順

### 1. このディレクトリに移動します
```bash
cd containerlab-l2-switch
```

### 2. 事前にOVSブリッジを作成します
```bash
sudo ovs-vsctl add-br ovs-br
```

### 3. トポロジーをデプロイします
```bash
# ローカルからマウントしてmultipassを起動している関係で、権限エラーが出るため、LABDIRをマウント外の箇所に変更している
sudo CLAB_LABDIR_BASE=/home/ubuntu/labs containerlab deploy -t topology.yml
```

ContainerLabのovs-bridgeタイプを使用しているため、Open vSwitchの設定は自動的に処理されます。

## トポロジー定義の詳細

ContainerLabのovs-bridgeタイプを使用する場合、以下の点に注意が必要です:

1. 事前にホスト上に同名のOVSブリッジを作成しておく必要があります
2. トポロジー内のノード名は、ホスト上のOVSブリッジ名と一致させる必要があります
3. リンク定義では、ブリッジ側のエンドポイントに任意のポート名を指定できます

## Ubuntu ノードの設定

### ubuntu1 と ubuntu2 の設定
1. Ubuntu コンテナのいずれかに入ります:
   ```bash
   sudo docker exec -it clab-ubuntu-ovs-switch-ubuntu1 bash
   ```

2. 次のコマンドを実行してネットワークを設定します:
   ```bash
   # ip設定
   ip addr add 10.0.0.1/24 dev eth1
   # インターフェースを起動
   ip link set eth1 up
   ```

3. もう片方のコンテナで同様にネットワーク設定
   ```bash
   # ip設定
   ip addr add 10.0.0.2/24 dev eth1
   # インターフェースを起動
   ip link set eth1 up
   ```

## 接続性のテスト

1. 一方の Ubuntu コンテナに入ります:
   ```bash
    sudo docker exec -it clab-ubuntu-ovs-switch-ubuntu1 bash
   ```

2. もう一方のコンテナとの接続をテストします:
   ```bash
   ping 10.0.0.2
   ```