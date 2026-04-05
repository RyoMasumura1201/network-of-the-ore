# L3 ルータ検証環境

Linuxコンテナをルータとして使い、異なるサブネットに属する2つのクライアントがL3ルーティングで通信できる環境です。

## ネットワーク構成

```
client1 (10.0.1.2/24) ──── eth1 [router] eth2 ──── client2 (10.0.2.2/24)
                            10.0.1.1/24  10.0.2.1/24
```

| ノード | サブネット | IPアドレス | 役割 |
|--------|-----------|-----------|------|
| client1 | 10.0.1.0/24 | 10.0.1.2 | クライアント |
| client2 | 10.0.2.0/24 | 10.0.2.2 | クライアント |
| router | 10.0.1.0/24, 10.0.2.0/24 | 10.0.1.1, 10.0.2.1 | ルータ (IP forwarding) |

## 前提条件

- ContainerLabがインストール済みであること
- `ore-client` イメージがビルド済みであること
- `ore-router` イメージがビルド済みであること

## ルータ用Dockerイメージのビルド

```bash
cd docker-images/router
docker build -t ore-router .
```

## デプロイ

```bash
cd containerlab-l3-router
sudo containerlab deploy -t topology.yml
```

デプロイ時にトポロジファイルの `exec` により、各ノードのIPアドレス設定・ルーティング設定・IP forwarding有効化が自動で行われます。

## 疎通確認

client1 から client2 へ ping:

```bash
sudo docker exec -it clab-l3-router-client1 ping 10.0.2.2
```

client2 から client1 へ ping:

```bash
sudo docker exec -it clab-l3-router-client2 ping 10.0.1.2
```

## ルータの状態確認

```bash
# IP forwarding の確認
sudo docker exec -it clab-l3-router-router sysctl net.ipv4.ip_forward

# ルーティングテーブルの確認
sudo docker exec -it clab-l3-router-router ip route

# インターフェースの確認
sudo docker exec -it clab-l3-router-router ip addr
```

## 破棄

```bash
cd containerlab-l3-router
sudo containerlab destroy -t topology.yml
```
