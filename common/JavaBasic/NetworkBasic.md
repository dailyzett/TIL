## 네트워크 프로그래밍 기초 용어

> 트랜스포트 레이어의 TCP, UDP

- TCP
  - Transmission Control Protocol 
  - 연결 기반 프로토콜
  - 데이터가 전송된다는 보장을 확실히 받을 수 있지만 내부적으로 처리되는 절차 복잡
- UDP
  - User Datagram Protocol 
  - 비신뢰성 프로토콜
  - 데이터가 전송된다는 보장을 받지 못하지만 TCP 보다 속도가 빠름
- 포트
  - 일반적인 웹 애플리케이션에서는 80이라는 번호의 포트 사용
  - 16비트로 구성되어 65,535 까지 사용, 단 0~1023 까지는 포트번호가 정해져있으므로 제한