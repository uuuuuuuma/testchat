# P2P チャット (テスト用サンプル)

端末間でメッセージが実際に届くかを確認するための、最小構成のチャットアプリです。
[PeerJS](https://peerjs.com/)（WebRTCのラッパー）を使い、ブラウザ同士が直接データをやり取りします。サーバー側でメッセージを保存・中継する処理は一切ないため、**静的ファイルだけで GitHub Pages にそのまま公開できます**。

## 仕組み

- 各端末がページを開くと、PeerJS の無料シグナリングサーバー（`0.peerjs.com`）経由で自分専用のID(ランダムなUUID)を取得します。
- 片方の端末で相手のIDを入力して「接続」すると、WebRTCの直接通信(P2P)が確立します。
- 以降のメッセージ送受信は、シグナリングサーバーを経由せず端末同士で直接行われます。
- メッセージはどこにも保存されません(ページを閉じると消えます)。

## ローカルでの動作確認

```bash
cd p2p-chat-sample
python3 -m http.server 8765
```

ブラウザで `http://localhost:8765` を2つのタブ(またはPCとスマホ)で開き、片方に表示されたIDをもう片方の入力欄に貼り付けて「接続」→ メッセージを送信して確認してください。

## GitHub Pages への公開手順

1. GitHubで新しいリポジトリを作成します(例: `p2p-chat-sample`)。Public / Privateどちらでも構いません(Privateの場合はGitHub Pages自体が有料プラン限定になるので、無料で使うならPublic推奨)。

2. このフォルダの中身をリポジトリにpushします。

   ```bash
   cd p2p-chat-sample
   git init
   git add index.html README.md
   git commit -m "Add P2P chat sample"
   git branch -M main
   git remote add origin https://github.com/<あなたのユーザー名>/p2p-chat-sample.git
   git push -u origin main
   ```

3. GitHubのリポジトリページで **Settings → Pages** を開きます。

4. "Build and deployment" の **Source** を `Deploy from a branch` にし、**Branch** を `main` / `/(root)` に設定して **Save** します。

5. 数十秒〜数分待つと、`https://<あなたのユーザー名>.github.io/p2p-chat-sample/` でアクセスできるようになります。

## 使い方(公開後)

1. 端末Aでページを開き、表示された「自分のID」をコピー、または「招待リンクをコピー」でリンクを取得します。
2. そのIDまたはリンクを端末B(別のスマホ・PCなど)に共有します。
   - リンクを使った場合、端末Bでリンクを開くと相手のIDが自動入力されるので「接続」を押すだけです。
3. 接続が成立すると「接続済み」と表示され、メッセージ入力欄が使えるようになります。
4. お互いにメッセージを送って、実際に端末間で届くか確認してください。

## 補足・注意点

- PeerJSの無料シグナリングサーバーはテスト・学習用途向けです。本番利用や大量アクセスには適していません。
- WebRTCの直接接続は、双方のネットワーク環境(厳しいファイアウォール/対称型NATなど)によっては確立できない場合があります。その場合は自宅Wi-Fiとモバイル回線など、別々のネットワークで試すか、TURNサーバーの追加設定が必要になることがあります。
- HTTPS配信(GitHub Pagesは標準でHTTPS)が必要です。`file://` で直接開いた場合は動作しないブラウザがあります。
