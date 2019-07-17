---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: IBM CIS connection, CIS network connection, Origin web server, troubleshooting

subcollection: cis

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# CIS ネットワーク接続のトラブルシューティング
{:#troubleshooting-your-cis-network-connection}

## データが IBM CIS 接続を経由しているかどうかを知る方法
{:#how-do-i-know-if-my-data-is-passing-through-my-cis-connection}

IBM Cloud Internet Services (CIS) では、読み取り、追加、および変更が可能な HTTP ヘッダーが使用されます。 このヘッダーを使用すると、CF-Ray 番号を使用して、要求がどのように経路指定されたかをトレースできます。 CF-Ray 番号は、`curl` コマンドまたは「Claire」と呼ばれる Google Chrome プラグインを使用して見つけることができます。

データが IBM CIS を経由したかどうかを知るには、各パケット上にある `Ray ID` を見つけます。

**Unix コマンド・ライン・ツールは以下のとおりです。**

 * curl (HTTP 用): `$ curl -vso /dev/null http://example.com`

 * dig (DNS 用): `$ dig www.example.com`

 * traceroute (ネットワーク用): `$ traceroute example.com`

**例えば次のようにします。**

端末コマンド:  `curl -svo /dev/null YOUR_URL_HERE. -L`

結果: `CF-RAY: 1ca349b6c1300da3-SJC`

## CF-Ray ヘッダーの追加

ネットワークを介した Web サイトへの要求をトレースするのに役立つ CF-RAY ヘッダーが追加されます。サポートと連携して接続性に関連する問題をトラブルシューティングする際に、これを使用します。Apache と nginx の構成ファイルをいくらか編集することによって、ログ内の「レイ ID」を明示できます。

### Apache
{:#troubleshooting-cis-apache}

```
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-agent}i\" %{CF-Ray}i" cf_custom

CustomLog log/access_log cf_custom
```

### nginx
{:#troubleshooting-cis-nginx}

```
log_format cf_custom '$remote_addr - $remote_user [$time_local]  '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent" '
                    '$http_cf_ray';

access_log  /var/log/nginx/access.log cf_custom;
```

## 経路のトレース方法
{:#how-do-i-trace-a-route}

経路が IBM CIS パスを経由しているかどうかを確認するには、「ターミナル」ウィンドウで「dig」を実行する (Mac または Linux の場合) か、Windows コマンド・プロンプトで `nslookup` を使用します (Windows の場合)。

パケットに CF-Ray 値がある場合は、CIS を経由しています。

`traceroute` コマンドでは、IP 要求で使用されたパス全体が表示されます。

サポート・チームは、これらのコマンドを使用してユーザーを支援します。

## プライバシーに関する警告が表示された場合
{:#troubleshooting-cis-privacy-warning}

IBM CIS 発行の証明書は、ルート・ドメイン (`example.com`) と 1 つのレベルのサブドメイン (`*.example.com`) を対象としています。 第 2 レベルのサブドメイン (`*.*.example.com`) にアクセスしようとすると、ブラウザーにプライバシーに関する警告が表示されます。これは、これらのホスト名が SAN に追加されていないためです。

また、いずれかのパートナー認証局 (CA) が新規証明書を発行するまで、最大 15 分待ってください。 新規証明書が未発行の場合、ブラウザーにプライバシーに関する警告が表示されます。

## DDoS 攻撃のターゲットになった場合の対応方法
{:#troubleshooting-cis-ddos-attack}

 * **ステップ 1:** ダッシュボードから「防御モード (Defense Mode)」をオンにします。
 * **ステップ 2:** セキュリティーが最大になるように DNS レコードを設定します。
 * **ステップ 3:** IBM CIS からレート制限要求やスロットル要求を行わないでください。
 
「防御モード (Defense Mode)」になっている場合、各新規訪問者に対して「Captcha」によるユーザー確認が行われます。ユーザー確認が行われていないアクセスに対して Cookie が与えられる前に、訪問者はこれに合格しなければなりません。 この方法により、「防御モード (Defense Mode)」がオフになるまで、ボットネット・トラフィックがブロックされます。 ユーザー確認に不合格であった訪問者は、(不正な) IP レピュテーション・データベースに追加されます。

## 発生する可能性があるその他の問題
{:#troubleshooting-cis-other-problems}

以下に、ユーザーまたはサポート・チームが受け取る可能性のある一般的なエラー・メッセージをいくつか示します。

| エラー・コード    | 理由 |
| ------------- | ------------- |
| 1001  | DNS 解決エラー。 最近サインアップした顧客の DNS 情報がまだ伝搬されていないか、DNS を管理しているユーザーに障害が発生しています。 |
| 521  | 起点 Web サーバーが CIS からの接続を拒否しました。 起点 Web サーバーが実行されていないか、何かが IBM CIS IP アドレスをブロックしています。 |
| 522  | 起点サーバーに対する接続タイムアウト (デフォルトで 30 秒)。 CIS がレート制限されているか、Web サーバーがすべてのリソースを消費している (共有サーバー) か、Web サーバーと IBM CIS との間でネットワーク接続の問題が発生している可能性があります。 |
| 523  | 起点サーバーにアクセスできません。 DNS レコードの起点 IP アドレスが CIS DNS の「設定」ページに表示されているものと同じであることを確認してください。 |
| 524  | IBM CIS は TCP 接続できましたが、Web サーバーから応答がありませんでした。 長時間実行されているアプリケーションやデータベース照会によって妨害されています。 |

### ネットワーク・トラフィックが表示されない
{:#troubleshooting-cis-network-traffic}

トラフィックが表示されない場合に、CNAME を使用しているときは、トラフィックがルート・ドメインに経路指定されないように、適切にリダイレクトされていることを確認してください。 場合によっては、DNS 伝搬が完了するまで最大 48 時間かかることに注意してください。

### Web サイトがオフライン
{:#troubleshooting-cis-website-offline}

次のように表示されることがあります。

`IBM CIS cannot connect to the origin server (error 521, 522, 523)`

**Web サイトがオフライン - キャッシュされたバージョンがない**

1. サーバーはオンラインですが、IBM CIS 要求をブロックしています。
2. 起点サーバーがオフラインで、IBM CIS にバックアップ Web サイト・イメージがありません。 

可能な処置は以下のとおりです。

* IBM CIS IP アドレスがホワイトリストされていることを確認します。
* IBM CIS IP がレート制限されていないことを確認します。
* 以下に、[ホワイトリストする IP](/docs/infrastructure/cis?topic=cis-ibm-cloud-cis-whitelisted-ip-addresses) のリストを示します。

### 502 エラー「恐怖の 502」
{:#troubleshooting-cis-502-error}

これは、ユーザーに表示されることがある最も一般的なエラーの 1 つです。 これは通常、ネットワークの一部が使用できない (例えば、DDoS 攻撃が開始された) 場合に発生します。 特定のデータ・センターが一時的に使用できない場合があります。 トラフィックは転送されます。 traceroute を実行してください。 

次のように表示されることがあります。`Error 502 - bad gateway error`

発生内容は以下のとおりです。

* IBM CIS ネットワークの一部に問題があります。
* 通常、この問題は、1 つのデータ・センター内の 1 つのサーバーに限定されます。
* この影響を受けるのは、サイト訪問者の一部のみです。
* IBM CIS のテクニカル・オペレーション・チームがこれらに対応します。

可能な処置は以下のとおりです。

* チケットで `www.YOUR_DOMAIN.com/cdn-cgi/trace` から[サポート](/docs/get-support?topic=get-support-getting-customer-support)に結果を送信します。
* 一時的に DNS レコードをオフに切り替えます (プロキシーなし)。
