# mikopbx-tkytel-quickstart

東京広域電話網用の Docker コンテナ構成例です。

このクイックスタートでは、東京広域電話網に接続するための MikoPBX + Tailscale を構築することができます。

このクイックスタートが向いているのは、次のような人です。

- [いまさら VoIP 網](https://zenn.dev/kusaremkn/articles/abd760f9f2f450) で用いられるような LXC コンテナの動作が有効なホストを持っていない人
    - ホストに Tailscale をインストールし、それを共有すると、共存している他のサービスのポートまでアクセスされてしまうかもしれない、これを避けたいと考える人

このクイックスタートが向いていないのは、次のような人です。

- [いまさら VoIP 網](https://zenn.dev/kusaremkn/articles/abd760f9f2f450) で動作させられる人

## Tailscale 側の設定

1. Get Auth Keys from [https://login.tailscale.com/admin/settings/keys](https://login.tailscale.com/admin/settings/keys).
    - Enable "Reusable"
    - Enable "Tag"

2. Set environmental Keys to `.env`.

```ini
TS_AUTH_KEY=TS-AUTH-YOUR_TS_AUTH_KEY
```

3. Access `:3000`

## MikoPBX

Web コンソールから、[通常のステップ](https://zenn.dev/kusaremkn/articles/abd760f9f2f450) 通りの設定を済ませてください。

## ホストに紐づけるポートについて

VoIP ルータへ MikoPBX / Asterisk を接続したいとき、LAN 端子を増やし、ホストコンピュータ側でポートフォワーディングしたい需要があります。

私の環境 & このリポジトリの `compose.yaml` では、以下のような形です。

<img src="/docs/network-example.png" width="400px" />

デフォルトでは以下のように紐づける設定です。

```yaml
    ports:
      - "3000:3000"
      - "5060:5060/tcp"
      - "5060:5060/udp"
      - "5061:5061/tcp"
      - "10000-10800:10000-10800/udp"
```

これは、MikoPBX の「システム」→「一般設定」→「一口」の以下の設定に基づきます。

<img src="/docs/network-port.png" width="400px" />

あるいは、ここの説明がよくわからなかった場合は、

<img src="/docs/network-port-2.png" width="400px" />

から意図を理解することができます。

## 免責事項

このリポジトリは [東京広域電話網](https://tkytel.github.io/) とは何ら関係がありません。

## 関連項目

- [Org: 東京広域電話網 (tkytel)](https://github.com/tkytel)
- [Web: 東京広域電話網](https://tkytel.github.io/)
- [Zenn: いまさら電話網](https://zenn.dev/kusaremkn/articles/abd760f9f2f450)

## コメント

これを機に、私とも相互接続してくださると嬉しいです。
