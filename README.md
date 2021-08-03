# arp-spoof
ARP spoofing Implementation

---

## send-arp 코드리뷰

char* C언어에서 처리하기 까다로움.

Deep Copy 필요

---

## 스위치

L3 스위치 == 라우트 : 아이피 레이어이다. 아이피를 보고 어디로 보낼지 결정함
- ARP 테이블 정보를 가지고 있다. IP-MAC의 매칭 정보를 가지고 있다.
- 하나의 IP에 하나의 MAC이 관리된다.

L2 스위치 : 데이터 링크 레이어이다. 맥 어드레스로 스위칭 해주는 역할.
- L2 스위치는 CAM tABLE 정보를 가지고 있다.
- L2에서는 브로드케스트라고 쓰지 않고 플로딩이라고 말함.
- 패킷이 오는 것을 보고 학슴함
- L2 스위치는 완전히 투명하다
**결론 : L2 스위치는 CAM Table 정보를 가짐. SMAC 학습, DMAC으로 스위칭 한다.**

L3 스위치 ARP Table : Key-IP, Value-Mac

L2 스위치 CAM Table : key-Mac, Value-Port Num(스위치 꼽는 번호)

공유기는 L2, L3 전부 가지고 있다.

---

## Promiscuous Mode

Promiscuous Mode : 모든 패킷을 전부 올리는 것.

Non Promiscuous Mode : 네트워크 카드 / 이더넷 어뎁터가 내가 필요한 패킷인지 선별해주는 것.

NP일 경우 이더넷 헤더의 DMAC을 보고 선별한다.
- 자기 자신의 MAC일 경우 받아들임.
- 브로드 케스트의 MAC일 경우 받아들임.
- 멀티케스트 MAC << 잘 안씀, 방송용으로 만들어진 MAC

---

## ARP Spoofing 용어의 정립

* Attacker : ARP spoofing을 시도하는 호스트
* Sender : ARP spoofing의 패킷 전송자
* Target : ARP spoofing의 패킷 수신자
* ARP spoofing Flow : Sender > Attacker > Target
* Original IP Packet : Sender > Target의 IP 패킷
* Spoofed IP Packet : Sender > Attacker의 IP 패킷
* Relay IP Packet : Attacker > Target의 IP 패킷
* ARP Infect : Attacker가 Sender를 감염시키는 행위
* ARP Recover : Sender의 감염이 원상복귀되는 현상

---

## Attacker가 해야할 일
1. Attackersms Sender에게 ARP Infect 패킷을 주기적, 혹은 비 주기적으로 보내서 감염시켜야한다.
2. Attacker는 Sender로부터 Spoofed IP Packet을 수신하면 Relay IP Packet를 보내야 한다.

---

## IP Packet 흐름도

![image](https://user-images.githubusercontent.com/37138188/127981184-0960ea00-f726-4a01-a790-7ea3ded58264.png)
![image](https://user-images.githubusercontent.com/37138188/127982911-3fe3b70f-64e7-4c41-b3d2-2898c2309d50.png)

---

## 과제 설계

1. 기존 send-arp 코드를 기반으로 작성
2. Sender(Victim)에 Attacker MAC주소 삽입
3. Target(Gateway)에 Attacker MAC주소 삽입
4. 쓰레드 생성해서 지속적 ARP Attack

~~왜 생각보다 별로 할 게 없지?~~

---
