---
layout: post
title: インターネットネットワーク
image: 13.jpg
date: 2021-09-20 22:20:00 +0200
tags: [Internet, IP, TCP, UDP, PORT, DNS]
categories: internet
---

# インターネットネットワーク
## IP（Internet Protocol）
### インターネットプロトコルの役割
- 指定したIPアドレスにデーターを送信
- Packetという通信単位でデーターを送信

ネットワークが通信をする為にはIPパッケ情報が必要<br>
Strat IP, EndPoint ID, 送信データETC

IPパッケ情報の通信流れ<br>
end Pointに着いたら、逆方で返信を送信する<br>
Start Point server (Client packet) → Internet(Node) → End point server<br>
(IP:100.100.100.1)　　　　　　　　　　　　　　　(IP:200.200.200.2)<br>

### IP Protocolの限界

- 非連携
  - Packetを受ける対象が無い又はサービスが不能の状態の時もpacket送信するので、通信ができない問題発生
- 非信頼性
  - 中間のパッケが消えたら？ 
  - パッケが順次に来なければ
- プログラム区分
    - 同じIPを使用するサーバーに通信するアプリケーションが二つ以上だったら？
  
## TCP (Transmission Control Protocol)
- 連結指向　ー　TCP 3 way handshake　（仮想連結）<br>
  ３段階に分けてデーター送信を行う。　syn → syn+ack → ack
- データー送信保証<br>
  データーを受信した場合、応答する
- 順次保証<br>
  順次にパッケが来なかった場合、順次に送るよう再度要請する
- 信頼できるprotocol<br>
- 現在はほとんどがTCPを利用している<br>

## UDP (User Datagram Protocol)
- 連結指向　ー　TCP 3 way handshake　（仮想連結）X<br>
- データー送信保証X<br>
- 順次保証X<br>
- データー送信及び順番が保証されないが、早い<br>
IPとほぼ一緒。＋PORT＋checksomeぐらい追加<br>
  APPで追加作業が必要

## PORT
マンションの号室的な概念
マンション（PC）来たネットワークを各正しい部屋（PORT）送る機能<br>
0~1023は良く知られているため使わない方がいい
- FTP- 20,21
- TELNET - 23
- HTTP - 30
- HTTPS - 443

## DNS (Domain Name System)
- 電話帳的な
- ドメイン名をIPアドレスに変換

# URLとウェブリクエストの流れ
## URI(Uniform Resource Identifier)
URIはLocator,Name又は二つとも追加で分類される<br>
URL：Uniform Resource　Locator　ほとんどこちらを利用<br>
リソースがある位置を指定<br>
URN：Uniform Resource　Name<br>
リソースに名前をつける

## URL
scheme://[userInfo@]host[:port][/path]?[?query][#fragment]
- scheme 
  - protocol(https)
  - protocol : どんな方式でリソースにアクセスするかを決めている規則　（http,https,ftp）
  - http port : 80 , https : 443を主に利用、port省略可能
  - httpsはhttpにセキュリティを追加(HTTP Secure)
  
- userinfo　（あまり利用しない）
  - URLにユーザー情報を含め認証　
- host name(www.google.com)
  - ドメイン名又はIPアドレスを直接入力可能
- port(443)
- path(/search)
  - リソースの経路、Nested Structure
- query parameter(q=hello&hi=ko)
  - key=value
  - ?でスタート、＆で追加機能
  - query parameter, query stringで呼ばれる。ウェブサーバーに提供するパラメーター、文字形

# HTTP
## HTTPメッセージに全てが送信可能
- HTML, TEXT
- IMAGE, 音声、映像、ファイル
- Json、XML（API）
- サーバー間送信
  
### HTTP　version
- HTTP/1.1 最も広く使われている
- HTTP/2 2015年性能改善
- HTTP/3 TCPの代わりにUDP使用、性能改善

### HTTPの特徴
- クライアントサーバー構造
  - Request, Response構造
  - クライアントはサーバーにリクエストし、レスポンスを待つ
  - サーバーがレスポンスを作成し応答
- Stateless、非連結性
  - サーバーがクライアントの状態を保存しない
  - 長所：サーバーの拡張性が高い(scale out)
  - 短所：クライアントが追加データー送信
- HTTPメッセージ
- Simple,拡張可能
  
### 非連結性
- HTTPは基本が連結を維持しないモデル
- 一般的に秒単位以下の早い速度で応答
- １時間に数千人がサービスを利用しても、サーバーで同時に処理するリクエストは少ない
- サーバーリソースを効率よく使える

非連結性の限界と克服
- TCP/IPを毎回しなきゃいけない　3 way handshake時間の発生
- webサイトの場合、HTMLだけではなくJS、CSS,IMAGEなど数多いリソースを一緒にダウンロードする為、時間が長くなる
- 現在は、HTTP　Persistent Connectionsで問題を解決
- HTTP/2,HTTP/3でもっと最適化している

## HTTP メソッド
### GET
- リソース照会
- サーバーに送信するデーターはQueryを利用して送信
- メッセージボディーでもデーター送信が可能だが、支援するどころが少ないため、おすすめしない
### POST
- 要請デーだー処理
- メッセージボディーを利用してサーバーへ要請データー送信
- サーバーは要請データーを処理
- 主に送信されたデーターで新規リソース登録、プロセス処理に利用する
曖昧な場合はほとんどPOSTを利用すれば良い
### PUT
- リソースを完全代替、上書きされる
- クライアントがリソースを識別
### FATCH
- リソースの部分上書き
### DELETE
- リソース削除

## HTTPメソッドの属性
- 安全(Safe Methods)
  - リソースが変わらない
- 冪等(Idempotent Methods)
  - 何度呼び出ししても結果は一緒
  -　GET,PUT、DELETEが当てはまる
  - 活用：自動復旧メカニズム
- キャッシュ可能(Cacheable Methods)
  - 主にGET,HEADがキャッシュで利用される
  
## HTTP State Code
### 2xx (Successful)
クライアントのリクエストを正常に処理　
- 200 OK
- 201 Created
- 202 Accepted　バッチ処理など
- 204 No Content
### 3xx (Redirection)
永久的Redirection
- 301 Moved Permanent GETでリダイレクト
- 308 Permanent Redirect　POSTで本文維持

temp Redirection
- 302 Found　Getメソッドに変換可能、ボディー削除可能性ある　メインは302
- 307 Temporary Redirect　302と一緒　ただし、メソッドが変わってはいけない
- 303 See Other　GETに変更しなければならない

PRG: POST/Redirect/Get
- Postで注文後Web更新による重複防止
- Postで注文後に注文結果画面をGETメソッドでリダイレクト
- 更新しても結果画面GETで照会
- 重複注文代わりに結果画面だけGETでリクエスト

- 304 Not Modified
  - キャッシュを目的に使用
  - クライアントにリソースが修正されてない事を知らす。したがって、クライアントはローカルPCにSaveされたキャッシュを利用する
  - 304　レスポンスにはメッセージbodyを含めてはいけない
  - 条件付きGET,HEADリクエスト時利用
  
### 4xx (Client Error)
- クライアントリクエストが文法などでサーバーがリクエストを処理できない時
- クライアントはリクエストを再度確認し、リクエストする必要がある。
- パラメーターが間違えた時、APISPECが合わない時

- 401 Unauthorized 認証されない
- 403 Forbidden サーバーがリクエストを理解したが、承認を拒む
- 404 Not Found
  
### 5xx (Server Error)
- 500 server error
- 503 Service Unavailable

## HTTP HEADER
### Content-type
표현 데이터의 형식 설명
- 미디어 타입, 문자 인코딩
  - text/html; charset=utf-8
  - application/json
  - image/jpg
  
### Content-Encoding
표현 데이터 인코딩
- 표현 데이터를 압축하기 위해 사용
- 데이터는 전달하는 곳에서 압축 후 인코딩 헤더 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해체
- 예)
  - gzip
  - deflate
  - identify
  
### Content-Language
표현 데이터의 자연 언어
- 표현 데이터의 자연 언어를 표현
- 예)
  - ko
  - jp
  - en

### Content-Length
표현 데이터의 길이
- 바이트 단위
- Transfer-Encoding(전송 코딩)을 사용하면 content-Length을 사용하면 안됨.

### 협상(content-Negotiation)
클라이언트가 선호하는 표현 요청
- Accept : 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset : 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
- Accept-Language : 클라이언트가 선호하는 자연 언어
- 협상 헤더는 요청시에만 사용 가능

### 전송 방식
- 단순 전송
  - Content-Length 컨텐츠 길이를 알고 있을 때 
- 압축 전송
  - Content-Encoding 압축정보
- 분할 전송
  - Transfer-Encoding: chunked
  - 용량이 큰것을 나누어서 전송하는 법
  - 이 때는 content-length을 넣으면 안됨.
- 범위 전송
  - Range, Content-Range
  - 범위를 지정해서 요청 가능함. 중단 된 시점부터 다시 받을 수 있
  
### 일반 정보
- From : 유저 에이전트의 이메일 정보
  - 일반적으로 잘 사용되지 않음.
  - 검색 엔진 같은 곳에서 주로 사용
  - 요청에서 사용
  
- Referer 이전 웹 페이지 주소
  - 현재 요청된 페이지의 이전 웹 페이지 주소
  - A -> B로 이동하는 경우 B를 요청할 때 Referer:A를 포함해서 요청
  - Referer를 사용해서 유입 경로 분석 가능 예) 데이터 분석
  - 요청에서 사용가능 
  - referer는 단어 referrer 오타
  
- User-Agent 유저 에이전트 애플리케이션 정보
  - 내 웹브라우저 혹은 클라이언트 애플리케이션 정보
  - 통계 정보 
  - 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
  - 요청에서 사용
  
- Server 요청을 처리하는 Origin 서버의 소프트 웨어 정보
  - Server : Apache/2.2.22(Denian)
  - server:nginx
  - 응답에서 사용
  
- Date 메세지가 발생한 날짜와 시간
  - 응답에서 사용
  
### 특별한 정보 
- Host 요청한 호스트 정보 (도메인) 중요 필수값.
  - 요청에서 사용
  - 필수
  - 하나의 서버가 여러 도메인을 처리해야할 때
    - ㅁ
  - 하나의 IP 주소에 여러 도메인이 적용되어 있을 때

- Location: page redirection

- Allow : 허용 가능한 HTTP 메서드

- Retry-After : 유저 에이전트가 다음 요청을 하기까지 기다려야하는 시간
  