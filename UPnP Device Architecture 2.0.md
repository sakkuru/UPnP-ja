 http://upnp.org/specs/arch/UPnP-arch-DeviceArchitecture-v2.0.pdf
 の適当翻訳。

# UPnP Device Architecture 2.0

Document Revision Date: September 1, 2014

# UPnP技術とは？

UPnP技術は、あらゆる形状のインテリジェントな電気機器、無線デバイス、PCなどの広く普及したP2Pネットワーク接続のためのアーキテクチャを定義する。
これは家庭や中小企業、公共スペースか、インターネットに接続されているにかかわらず、使いやすく、フレキシブルで、標準に基づいたアドホックもしくは制御のいらないネットワークへの接続をもたらすようデザインされている。

UPnP技術は、シームレス近接ネットワーク、さらに制御とネットワーク機器間のデータ転送を可能にするため、TCP/IPとWeb技術を利用する、分散されたオープンネットワークアーキテクチャを提供する。

UPnP Device Architecture (UDA) は、 プラグアンドプレイ周辺機器モデルのただの単純な拡張
を超えるものである。
それは、zero-configuration("invisible"ネットワーク）や、広範なベンダーの幅広いデバイスカテゴリのための自動検出をサポートするよう設計されている。
これはデバイスは動的にネットワークにjoinし、IPアドレスを取得し、自身の機能を伝え、他のデバイスの存在と機能を学ぶことができることを意味する。
最後にデバイスは、不必要な状態を残さずにネットワークをスムーズにかつ自動的に去る。


UPnPアーキテクチャの影響下にある技術は、IP、TCP、UDP、HTTPやXMLなどのインターネットプロトコルを含む。
インターネットのように、協定は宣言的でXMLで表現され、HTTPをつかって通信する有線プロトコルを元にしている。
異なる物理メディアへ広がり、実世界のマルチベンダの相互運用を可能にし、インターネットと多くの家やオフィスのイントラネットの相乗効果を得る、その証明された能力ゆえ、インターネットプロトコルの使用は、UDAのための強力な選択である。
UPnPアーキテクチャはそれらの環境に適用するよう明確に設計されていた。
さらに、コスト、技術、レガシーが、メディアやデバイスを現行ののIPに接続することを妨害しているとき、ブリッジングを通して、UDAはnon-IPプロトコルで動いているメディアに適用する。

UPnP技術についての"universal"とはなんだろうか。
No device drivers; 一般的なプロトコルが代わりに使われる。
UPnPデバイスはいくつものプログラミング言語やいくつものOS上でで実装されるだろう。
UPnP技術はアプリケーションのためのAPIの設計を仕様に含めたり、強要したりしない;
OSベンダーはカスタマーのニーズに合わせてAPIを作ることができる。



## UPnPフォーラム

UPnPフォーラムは多くの異なるベンダーのスタンドアロンデバイスやPC間の、簡単で強固な接続を可能にするよう設計されたindustry initiativeである。
UPnPフォーラムは、device-to-deviceの相互運用性をスケーラブルにネットワークで繋がった環境で可能にするため、XMLベースのデバイススキーマとデバイスプロトコルを表すための標準を開発しようとしている。

UPnPフォーラムは多くの産業を横断したメンバ企業からなっている。


## In this document

ここで書かれているUPnPデバイスアーキテクチャ（かつてはDCPフレームワークで知られた）は、コントローラやコントロールポイント、デバイス間の通信のためのプロトコルを定義する。
discovery、description、control、eventingやpresentationのため、UPnPデバイスアーキテクチャは下記のプロトコルスタックを使用する（表示された色やスタイルは本ドキュメント中でそれぞれのプロトコル要素が定義されている場所を示している）。


<table>
<tr><td colspan="4">UPnP vendor</td></tr>
<tr><td colspan="4">UPnP Forum</td></tr>
<tr><td colspan="4">UPnP Device Architecture</td></tr>
<tr><td rowspan="2">SSDP</td><td rowspan="2">Multicast events</td><td>SOAP</td><td>GENA</td></tr>
<tr><td>HTTP</td><td>HTTP</td></tr>
<tr><td colspan="2">UDP</td><td colspan="2">TCP</td></tr>
<tr><td colspan="4">IP</td></tr>
</table>

最上位層では、


## Glossary

### action

### argument

### control point

### device

### device description

### device type

### event

### GENA

### publisher

### root device

### service

### service description

### service type

### SOAP

### SSDP

### state variable

### subscriber

### UPnP Device Template

### UPnP Service Template

### UPnP Service Template

### UPnP Device Schema

### Vendor Domain Name

## 0 Adressing

### 0.1 Auto-IPを使うべきどうかの決定

### 0.2 アドレス選択

### 0.3 アドレスのテスト

### 0.4 ルールの転送


### 0.5 動的アドレスが使用可能か定期的なチェック

### 0.6 デバイスネーミングとDNSインタラクション

### 0.7 IPアドレス解決の名前

### 0.8 リファレンス

## 1 ディスカバリ

### 1.1 SSDPメッセージフォーマット

#### 1.1.1 SSDP スタートライン

SSDPメッセージそれぞれは、1つのスタートラインを持つものとする。
すべてのSSDPメッセージのこうをの定義のために、1.2節"Advertisement"と、1.3節"Search"を参照。
スタートラインはRFC 2616 節5.1か、節6.1で定義されている。
さらに、スタートラインは以下の3行の1つであるとする。

NOTIFY * HTTP/1.1\r\n
M-SEARCH * HTTP/1.1\r\n
HTTP/1.1 200 OK\r\n

明確化のため、スタートラインが"HTTP/1.1"を含むとはいえ、これはSSDPがすべてHTTP 1.1にもとづいていることを示しているわけではない。
このスタートライン要素は後方互換性のためだけに含まれる。

#### 1.1.2 SSDPメッセージヘッダフィールド

SSDPメッセージのメッセージヘッダフィールドはRFC 2615の4.2節に従ってフォーマットされることとする。
これは、コロンが続く大文字小文字を区別しないフィールドネームからなるそれぞれのメッセージフィールドを指定し、大文字小文字を区別しない値が続く。
SSDP許可されるフィールド値を制限する。

SSDPヘッダの例:

HOST: 239.255.255.250:1900

#### 1.1.3 SSDPヘッダフィールド拡張

UPnPワーキング委員会とUPnPベンダは、追加のSSDPヘッダフィールドによるSSDPメッセージの拡張を許可されている。
追加のメッセージヘッダフィールはまた、UPnP Forum Technical committeeによって定義されることもある。

ヘッダフィールド定義の名前衝突（2つのパーティが偶然、異なる意味論を持つ同じヘッダフィールド名を定義する）を防ぐため、ベンダー定義のヘッダフィールドネームは以下のフォーマットを持つこととする:

field-name = token “.” domain-name

 domain-nameはベンダードメイン名と


 ベンダー定義のSSDPヘッダフィールド:

myheader.philips.com: “some value”
myheader.sony.com: “other value”


#### 1.1.4 UUIDフォーマットと推奨される生成アルゴリズム