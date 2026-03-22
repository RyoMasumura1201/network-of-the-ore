# Docker Client イメージ

## 概要

このディレクトリには、ネットワーク検証用のクライアントノードとして使用するためのカスタム Docker イメージが含まれています。Ubuntu 22.04 ベースのイメージで、ネットワーク設定と接続性テストに必要な基本的なツールがプリインストールされています。

## イメージ内容

- **ベースイメージ**: Ubuntu 22.04
- **プリインストールツール**:
  - `iproute2`: ネットワークインターフェースの設定と管理
  - `iputils-ping`: ICMP ping コマンドによる接続性テスト

## ビルド方法

```bash
docker build -t network-client .
```

## 使用方法

このイメージは主に ContainerLab 環境で使用することを想定していますが、単体でも以下のように使用できます：

```bash
# インタラクティブモードで起動
docker run -it network-client

# バックグラウンドで起動
docker run -d --name my-client network-client
```

## 事前ビルド済みイメージの取得

ローカルでビルドする代わりに、事前にビルド済みのイメージを取得することもできます：

```bash
docker pull gcr.io/container-lab-491010/ubuntu-client:latest
```

## カスタマイズ

必要に応じて、Dockerfile を編集して追加のパッケージをインストールできます：

```dockerfile
FROM network-client
RUN apt-get update && apt-get install -y curl net-tools
```

## ライセンス

MIT License