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

## TCP, UDP

### TCP (Transmission Control Protocol)
- 連結指向　ー　TCP 3 way handshake　（仮想連結）<br>
  ３段階に分けてデーター送信を行う。　syn → syn+ack → ack
- データー送信保証<br>
  データーを受信した場合、応答する
- 順次保証<br>
  順次にパッケが来なかった場合、順次に送るよう再度要請する
- 信頼できるprotocol<br>
- 現在はほとんどがTCPを利用している<br>

### UDP (User Datagram Protocol)
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


