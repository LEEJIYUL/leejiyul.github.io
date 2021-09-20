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



## PORT

## DNS

