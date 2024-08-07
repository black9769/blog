---
title: "[Network] TCP VS. UDP"
published: 2024-07-03
description: ''
image: ''
tags: ['Network','CS','TCP','UDP']
category: 'Network'
draft: false 
---
# 개요
TCP (Transmission Control Protocol)와 UDP (User Datagram Protocol)는 인터넷에서 데이터 전송에 사용되는 두 가지 주요 프로토콜입니다. 두 프로토콜은 서로 다른 특성과 용도를 가지고 있어, 각각의 특성에 따라 적절한 서비스가 다릅니다.

## 연결성
- TCP: 연결 기반 프로토콜로, 데이터 전송 전에 송신자와 수신자 간에 연결을 설정합니다. 이 연결을 통해 데이터가 전달되며, 데이터의 순서와 무결성을 보장합니다.

<details>
<summary>💡 TCP 3-Way Handshake</summary>

## 💡 TCP 3-Way Handshake
1. SYN (Synchronize) - 첫 번째 단계
- **클라이언트가 서버에 연결을 요청**:
    - 클라이언트가 서버에 TCP 연결을 요청하는 `SYN` 패킷을 보냅니다.
    - 이 패킷에는 클라이언트의 초기 시퀀스 번호(`Seq=J`)가 포함되어 있습니다.
2. SYN-ACK (Synchronize-Acknowledge) - 두 번째 단계
- **서버가 클라이언트의 요청에 응답**:
    - 서버는 클라이언트의 `SYN` 패킷을 받으면 `SYN-ACK` 패킷을 클라이언트로 보냅니다.
    - 이 패킷에는 서버의 초기 시퀀스 번호(`Seq=K`)와 클라이언트의 시퀀스 번호에 대한 확인 응답(`Ack=J+1`)이 포함되어 있습니다.
3. ACK (Acknowledge) - 세 번째 단계
- **클라이언트가 서버의 응답을 확인**:
    - 클라이언트는 서버의 `SYN-ACK` 패킷을 받은 후, `ACK` 패킷을 서버로 보냅니다.
    - 이 패킷에는 서버의 시퀀스 번호에 대한 확인 응답(`Ack=K+1`)이 포함되어 있습니다.
    - 이 단계가 완료되면 양쪽은 데이터를 주고받을 준비가 된 상태입니다.
```css
클라이언트                      서버
 |                              |
 |--------SYN, Seq=J----------->|
 |                              |
 |<---SYN-ACK, Seq=K, Ack=J+1---|
 |                              |
 |------ACK, Ack=K+1----------->|
 |                              |
```
</details>
<details>
<summary>💡 TCP 4-Way Handshake</summary>

## 💡 TCP 4-Way Handshake
1. 첫 번째 단계: FIN
- **연결 종료 요청**:
    - 클라이언트나 서버 중 하나가 연결을 종료하고자 할 때 `FIN` 패킷을 보냅니다.
    - 이 패킷은 "더 이상 데이터 전송이 없을 것"이라는 의사를 표시합니다.
    - 예: `FIN, Seq=M`
2. 두 번째 단계: ACK
- **종료 요청에 대한 확인**:
    - 상대방은 `FIN` 패킷을 받고, 이를 확인하는 `ACK` 패킷을 보냅니다.
    - 이 단계에서 상대방은 여전히 데이터를 전송할 수 있습니다.
    - 예: `ACK, Seq=N, Ack=M+1`
3. 세 번째 단계: FIN
- **상대방의 종료 요청**:
    - 상대방도 이제 데이터 전송을 완료하고 연결을 종료하려는 `FIN` 패킷을 보냅니다.
    - 예: `FIN, Seq=P`
4. 네 번째 단계: ACK
- **종료 요청에 대한 확인**:
    - 원래 연결 종료를 요청한 측은 상대방의 `FIN` 패킷을 받고, 이를 확인하는 `ACK` 패킷을 보냅니다.
    - 이 단계가 완료되면 연결이 완전히 종료됩니다.
    - 예: `ACK, Ack=P+1`
```css
클라이언트                   서버
|                          |
|----FIN, Seq=M----------->|
|                          |
|<---ACK, Seq=N, Ack=M+1---|
|                          |
|                          |
|<---FIN, Seq=P------------|
|                          |
|----ACK, Ack=P+1--------->|
                                                                     |                          |
```
</details>

- UDP: 비연결 기반 프로토콜로, 데이터 전송 전에 연결 설정이 필요하지 않습니다. 각 패킷은 독립적으로 전송되며, 데이터의 순서와 무결성을 보장하지 않습니다.


## 신뢰성
- TCP: 신뢰성을 보장합니다. 패킷이 손실되면 재전송하고, 패킷의 순서를 보장합니다. 또한, 전송된 데이터의 확인 응답(ACK)을 받습니다.
- UDP: 신뢰성을 보장하지 않습니다. 패킷 손실, 중복, 순서 바뀜에 대해 처리하지 않으며, 데이터 수신에 대한 확인 응답도 없습니다.

## 흐름 제어와 혼잡 제어
- TCP: 흐름 제어와 혼잡 제어를 수행하여 네트워크의 상태에 따라 데이터 전송 속도를 조절합니다.
- UDP: 이러한 제어 기능이 없습니다. 데이터 전송 속도는 애플리케이션이 관리합니다.

## 전송 단위
- TCP: 스트림 전송을 사용하여 데이터가 바이트 스트림으로 전달됩니다. 데이터가 필요한 경우만큼만 나누어 전송됩니다.
- UDP: 메시지 전송을 사용하여 데이터가 독립된 데이터그램으로 전송됩니다. 각 데이터그램은 독립적입니다.

## 헤더 크기
- TCP: 상대적으로 큰 헤더(20바이트 이상)를 가지고 있습니다. 이는 다양한 제어 정보(순서, 확인 응답 등)를 포함하기 때문입니다.
- UDP: 작은 헤더(8바이트)를 가지고 있습니다. 간단한 체크섬과 포트 정보만을 포함합니다.

> 💡 데이터 그램 방식
> 
> 데이터그램은 개별적으로 처리되고 독립적인 패킷으로 전송됩니다.


## 활용되는 서비스 

### TCP

1. 웹 브라우징 (HTTP/HTTPS): TCP는 데이터의 신뢰성과 순서를 보장하므로, 웹 페이지를 로드할 때 중요한 HTML, CSS, JavaScript 파일이 손상 없이 전송됩니다.
2. 파일 전송 (FTP, SFTP): 파일이 손상 없이 정확하게 전송되어야 하므로 TCP의 신뢰성이 필요합니다.
3. 이메일 (SMTP, IMAP, POP3): 이메일 데이터가 손실 없이 도착해야 하므로 TCP가 적합합니다.
4. 데이터베이스 연결 (MySQL, PostgreSQL): 클라이언트와 데이터베이스 간의 신뢰할 수 있는 연결이 필요합니다.

### UDP

1. 영상 및 음성 스트리밍 (VoIP, IPTV): 실시간 전송이 중요하며, 일부 패킷 손실을 허용할 수 있으므로 지연이 적은 UDP가 적합합니다.
2. 온라인 게임: 지연 시간이 중요한 게임에서는 빠른 패킷 전송이 필요하며, 일부 패킷 손실을 허용할 수 있어 UDP를 사용합니다.
3. DNS (Domain Name System): 빠른 응답이 요구되며, 단일 요청과 응답이 일치하므로 UDP가 적합합니다.
4. SNMP (Simple Network Management Protocol): 네트워크 관리에서 상태 정보를 빠르게 전송하는 데 사용됩니다.

> ⭐ DNS가 UPD인 이유
> 
> UDP는 비연결 기반이기 때문에 패킷 전송 전에 연결을 설정할 필요가 없습니다. 이는 DNS 요청과 응답의 지연 시간을 줄이는 데 중요한 역할을 합니다. 연결 설정이 필요 없는 UDP는 TCP보다 빠른 응답을 제공할 수 있습니다.
> 또한, DNS 요청과 응답은 일반적으로 간단한 쿼리-응답 패턴을 따릅니다. 대부분의 DNS 응답은 작은 크기이기 때문에 UDP 패킷 하나에 충분히 담을 수 있습니다. 이 단순한 트래픽 패턴은 UDP의 사용에 적합합니다. 특징중하나인 1:N방식으로 통신이 가능하다.
> 

## Sumary - TCP와 UDP 비교

| **특성**                | **TCP**                                     | **UDP**                                 |
|-------------------------|---------------------------------------------|-----------------------------------------|
| **연결 방식**           | 연결 기반                                   | 비연결 기반                             |
| **신뢰성**              | 신뢰성 보장 (패킷 재전송, 순서 보장)         | 신뢰성 보장 안 됨 (패킷 손실 가능)      |
| **흐름 제어 및 혼잡 제어** | 있음                                        | 없음                                    |
| **전송 단위**           | 바이트 스트림                                | 메시지 데이터그램                        |
| **헤더 크기**           | 크다 (20바이트 이상)                        | 작다 (8바이트)                          |
| **적합한 서비스**       | 웹 브라우징 (HTTP/HTTPS)<br>파일 전송 (FTP, SFTP)<br>이메일 (SMTP, IMAP, POP3)<br>데이터베이스 연결 (MySQL, PostgreSQL) | 영상 및 음성 스트리밍 (VoIP, IPTV)<br>온라인 게임<br>DNS (Domain Name System)<br>SNMP (Simple Network Management Protocol) |

## FINAL
TCP와 UDP의 차이에 대해서 알아보았다. HTTP/3에서는 UDP를 기반으로 QUIC이라는 프로토콜이 개발이 되었다고 한다. 이번 포스팅과는 조금 다른 내용이긴하지만,
`UDP는 신뢰성이 없다.` 라고 암기하는 것보다는 `UDP는 신뢰성을 판단할 근거가 없다`라는 말이 더 올바른 표현이라고 한다. TCP의 Header는 오래되었기 때문에 아주 그득그득 차있다.
우리가 과거의 코드를 보는것처럼 저것도하고 있것도 하고 뭐가 많다. 하지만 UDP의 헤더를 보면 뽀얀 도화지와 같다. 즉, 커스터마이징하기 좋다는 말이다. 공부하면서 너무 잘 설명된 글을 발견해서
첨부한다.
[http/3 란?](https://evan-moon.github.io/2019/10/08/what-is-http3/)