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
  - 하나의 IP 주소에 여러 도메인이 적용되어 있을 때

- Location: page redirection
  - 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
  - 201 (Created): Location값은 요청에 의해 생성된 리소스 URI
- Allow : 허용 가능한 HTTP 메서드
  - 405 (Method Not Allowed) 에서 응답에 포함해야함.
  - Allow: GET, HEAD , put
- Retry-After : 유저 에이전트가 다음 요청을 하기까지 기다려야하는 시간
  - 503 (Service Unavailable) 서비스가 언제까지 불능인지알려줄 수 있음. 
  

### 인증
- Authorization: 클라이언트 인증 정보를 서버에 전달
  - Authorization: Basic xxxxxxxxx
  
- WWW-Authenticate : 리소스 접근시 필요한 인증 방법 정의
  - 리소스 접근 시 필요한 인증 방법 정의 
  - 401 Unauthorized 응답 과 함께 사용
  
### 쿠키 : Stateless(무상태)의 단점을 보안하기 위함. 모든 요청에 사용자 정보를 포함.
- Set-Cookie : 서버에서 클라이언트로 쿠키 전달(응답)
- Cookie : 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달
  - 모든 요청에 쿠키 정보 자동 포함.
  
- 사용처
  - 사용자 로그인 세션 관리
  - 광고 정보 트래킹
  
- 쿠키 정보는 항상 서버에 전송됨.
  - 네트워크 트래픽 추가 유발
  - 최소한의 정보만 사용 (세션 id, 인증 토큰)
  - 서버에 전송하지 안하고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 참고 
  
주의! 보안에 민감한 데이터는 저장하면 안됨. (주민번호, 신용카드정보) 
#### 쿠키 - 생명주기
- Set-Cookie : expires=Sat, 26-Dec-2020----- 만료일이 되면 쿠키 삭제
- Set-Cookie: max-age=3600 0이나 음수를 지정하면 쿠키 삭제
- 세션 쿠키 : 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
- 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지

#### 쿠키 - 도메인 
- 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함.
  - domain = example.org를 지정해서 쿠키 생성
    - example.org는 물론이고
    - dev.example.org 도 쿠키 접근
  
- 생략: 현재 문서 기준 도메인만 적용
  - example.org 에서 쿠키를 생성하고, domain지정을 생략
    - example.org에서만 쿠키접근
    - dev.example.org는 쿠키 미접근 
  
#### 쿠키 -경로
- 예) path=/home
- 이 경로로 포함한 하위 경로 페이지만 쿠키접근
- 일반적으로 path=/루트로 지정

#### 쿠키 보안
- Secure
  - 쿠키는 http,https를 구분하지 않고 전송
  - Secure를 적요하면 https인 경우만 전송
  
- HttpOnly
  - XSS 공격방지
  - 자바스크립트에서 접근 불가
  - Http 전송에만 사용
  
- SameSite
  - XSRF 공격방지
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송
  
  
### 캐시와 조건부 요청
#### 캐시 기본 동작
- 캐시가 없을 대 
  - 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야한다.
  - 인터넷 네트워크는 매우 느리고 비싸다. 
  - 브라우저 로딩 속도가 느리다.
  - 느린 사용자 경험
  
- 캐시 적용 
  - 캐시 덕분에 캐시 가능 시간동안 네트워크를 사용하지 않아도 된다.
  - 비싼 네트워크 사용량을 줄일 수 있다.
  - 브라우저 로딩 속도가 매우 빠르다.
  - 빠른 사용자 경험
  
- 캐시 시간 초과
  - 캐시 유효 시간이 초과하면 서버를 통해 데이터를 다시 조회하고 캐시를 갱신한다.
  - 이때 다시 네트워크 다운로드가 발생한다.
  
#### 검증 헤더와 조건부 요청
- 캐시 시간 초과
  - 캐시 유효 시간이 초과해서 서버에 다시 요청하면 다음 2가지 상황이 나타난다. 
    - 1 서버에서 기존 데이터를 변경함.
    - 2 서버에서 기존 데이터를 변경하지 않음.
  - 캐시 만료후에도 서버에서 데이터를 변경하지 않음.
  - 생각해보면 데이터를 전송하는 대신에 저장해 두었던 캐시를 재사용 할 수 있다.
  - 단 클라이언트의 데이터와 서버의 데이터가 같다는 사실을 확일할 수 있는 방법이 필요
  
- 캐시 유효 시간이 초과해도, 서버의 데이터가 갱신되지 않으면
- 304 Not Modified + 헤더메타 정보만 응답 (바디x)
- 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타정보 갱신
- 클라이언트는 캐시에 저장되어있는 데이터 재활용
- 결과적으로 네트워크 다운로드가 발생하지만, 용량이 적은 헤더 정보만 다운로드
- 매우 실용적인 해결책.

- 검증헤더
  - 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
  - Last-Modified, Etag
  
- 조건부 요청 헤더
  - 검증 헤더로 조건에 따른 분기
  - If-Modified-Since: Last-Modified 사용
  - If-None-MatchL ETag사용
  - 조건이 만족하면 200 oK
  - 조건이 만족하지 않으면 304 Not Moidified
  
- If-Modified-Since: 이후에 데이터가 수정되었으면
  - 데이터 미변경
    - 304 Not Moidified 헤더 데이터만 전송 (BODY 미포함)
  - 데이터 변경
    - 200 OK 모든 데이터 전송
  
- Last-Modified, If-Modified-Since 단점
  - 1초 미안 단위로 캐시 조정이 불가능
  - 날짜 기반의 로직사용
  - 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과 똑같은 경우
  - 서버에서 별도의 캐시 로직을 관리하고 싶은경 
    - 예) 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시는 유지하고 싶은 경우
  
- ETag, If-None-Match
  - ETag(Entity Tag)
  - 캐시용 데이터에 임의 고유한 버전 이름을 달아둠
    - ETag:"v1.0", ETag:"a2asda"
  - 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시생성)
  - 진짜 단순하게 ETag만 보내서 같으면 유지 다르면 다시받기
  - 캐시 제어 로직을 서버에서 완전히 관리
  - 클라이언트는 단순히 이 값을 서버에 제공
    - 예)
    - 서버는 배타 오픈 기간인 3일 동안 파일이 변경되엇 ETag를 동일하게 유지
    - 애플리케이션 배포 주기에 맞추어 Etag 모두 갱신
  
- Cache-COntrol 캐시 지시어(directives)
  - Cach-Control: max-age
    - 캐시 유효 시간, 초단위
  
- Cache-Control:no-cache
  - 데이터는 캐시해도 되지만, 항상 원(origin) 서버에 검증하고 사용
  
- Cache-Control:no-store
  - 데이터에 민감한 정보가 있으므로 저장하면 안됨. (메모리에서 사용하고 최대한 빨리 삭제)
  
#### 프록시 캐시
- Cache-Control 캐시지시어 기타
  - Cache-Control:public
    - 응답이 public 캐시에 저장되어도 됨.
  - Cache-Control:private
    - 응답이 해당 사용자만을 위한 것임. private 캐시에 저장해야함. 기본값
  - Cache-Control:s-maxage
    - 프록시 캐시에만 적용되는 max-age
  - Age: 60(HTTP 헤더)
    - 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)

#### 캐시 무효화 확실한 캐시 무효화 응답
- Cache-Control:no-cache
  - 데이터는 캐시해도 되지만, 항상 원 서버에 검증하고 사용 이름에 주의
  
- Cache-Control:no-store
  - 데이터에 민감한 정보가 있으므로 저장하면 안됨.
  
- Cache-Control:must-revalidate
  - 캐시 만료 후 최초 조회시 원 서버에 검증해야함.
  - 원서버 접근 실패시 반드시 오류가 발생해야함.
  - must-revalidate는 캐시 유효 시간이라면 캐시를 사용함.
