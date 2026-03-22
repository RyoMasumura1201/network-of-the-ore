# network-of-the-ore

## プロジェクト概要

network-of-the-ore は、ContainerLab を使用して構築されたネットワーク検証環境です。このプロジェクトでは、Open vSwitch ブリッジを介して接続された複数のコンテナベースのネットワークトポロジーを構成・検証することができます。

## プロジェクト構造

```
network-of-the-ore/
├── containerlab-l2-switch/     # L2 スイッチング環境
├── docker-images/              # カスタム Docker イメージ
│   └── client/                 # クライアントノード用イメージ
└── l3-router/                  # （予定）L3 ルーティング環境
```

## コンポーネント詳細

### 1. ContainerLab L2 スイッチ環境

Open vSwitch ブリッジを介して接続された2つの Ubuntu コンテナから構成されるシンプルな L2 ネットワークトポロジーです。

#### 主要特徴
- Open vSwitch ブリッジを使用したコンテナ間接続
- ContainerLab による簡単なデプロイメント
- ネットワーク検証用の基本的な構成

#### 詳細情報
詳細については [containerlab-l2-switch/README.md](containerlab-l2-switch/README.md) を参照してください。

### 2. Docker クライアントイメージ

ネットワーク検証用に最適化された Ubuntu ベースの Docker イメージです。

#### イメージ内容
- Ubuntu 22.04 ベース
- iproute2 (ネットワーク設定用)
- iputils-ping (接続性テスト用)

#### 詳細情報
Dockerfile については [docker-images/client/Dockerfile](docker-images/client/Dockerfile) を参照してください。

## 始め方

1. 各コンポーネントの README を参照して必要な前提条件を満たしてください
2. ContainerLab 環境をセットアップしてください
3. 各トポロジーをデプロイしてネットワークを検証してください

## 注意事項

- Open vSwitch はホストシステムに事前にインストールされている必要があります
- 特権モードでの実行が必要な場合があります
- ContainerLab のバージョンによっては互換性がない可能性があります

## ライセンス

MIT License
