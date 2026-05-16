# Ticketing System

> 대규모 트래픽 제어 및 실시간 예매 백엔드 시스템

대규모 트래픽이 일시에 몰리는 티켓 오픈 상황을 가정하여, 서버 리소스 고갈을 방지하고 좌석 예매의 데이터 정합성을 100% 보장하는 백엔드 시스템입니다.

---

## 1. Project Overview

- **개발 기간:** 2026.05.17 ~ (진행 중)
- **개발 인원:** 1인 (백엔드)
- **핵심 목표:**
  - 대기열 시스템을 통한 트래픽 제어 및 서버 안정성 확보
  - 동시성 이슈(Overbooking) 해결을 통한 데이터 정합성 보장

---

## 2. Tech Stack

- **Backend:** Java 17, Spring Boot 3.x
- **Database:** MySQL 8.x, MyBatis
- **Cache & Queue:** Redis
- **Infra & Testing:** Docker Compose, JUnit5, JMeter

---

## 3. Architecture & ERD

*(추후 시스템 아키텍처 다이어그램 및 ERD 이미지 첨부 예정)*

---

## 4. Key Challenges & Troubleshooting

> 시스템 개발 과정에서 겪은 주요 문제와 성능 개선 과정입니다.  
> 상세한 고민의 흔적은 링크된 블로그/노션에서 확인하실 수 있습니다.

### 4.1 Redis 기반 실시간 대기열(Queue) 구축

- **Problem:**  
  1만 명 이상의 유저 동시 접속 시 MySQL 커넥션 풀 고갈 및 DB 락(Lock) 경합으로 인한 서버 다운 위험

- **Solution:**  
  DB 접근 전, Redis의 `Sorted Set`을 활용하여 유저에게 대기열 순번을 부여하고 처리 가능한 인원만 순차적으로 DB 진입 허용

- **Result:**  
  대기열 적용 전 대비 서버 처리량(TPS) 향상 및 병목 현상 해소

- [상세 구현 및 부하 테스트 결과 보기](링크)

---

### 4.2 예매 동시성 제어 (Concurrency Control)

- **Problem:**  
  다수의 유저가 동시에 동일한 좌석(Seat) 예매 요청 시, 중복 예매(Overbooking)가 발생하는 데이터 정합성 문제

- **Solution:**  
  MySQL 비관적 락(Pessimistic Lock)과 Redis 분산 락(Redisson) 방식의 성능 비교 테스트 진행 후, 응답 속도와 서버 부하를 고려하여 최적의 제어 방식 도입

- **Result:**  
  좌석 데이터 정합성 100% 보장 및 동시 처리 속도 최적화

- [동시성 제어 방식 비교 분석 및 트러블슈팅 과정 보기](링크)

---

## 5. API Reference

*(추후 API 문서 링크 첨부 예정)*

---

## 6. How to Run

로컬 환경에서 아래 명령어를 통해 즉시 실행해 볼 수 있습니다.

```bash
예정
