<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0F766E,45:2563EB,100:7C3AED&height=90&section=header&text=&fontSize=0" width="100%"/>

## 안녕하세요, 백엔드 개발자 강동현입니다.

Java / Spring Boot 기반 백엔드를 중심으로 공부하고 있습니다.  
요즘은 **인증 경계**, **PostgreSQL 실행 계획**, **Kubernetes 위에서 애플리케이션이 실제로 실행되는 방식**에 관심이 많습니다.

작게 구현하더라도 “어디까지가 애플리케이션 책임이고, 어디부터가 인프라 / 데이터베이스 / 인증 시스템의 책임인가”를 분리해서 이해하려고 합니다.

---

## Focus

- Spring Security Resource Server와 Keycloak 기반 인증 경계
- PostgreSQL `EXPLAIN ANALYZE` 기반 쿼리 분석과 인덱스 설계
- K3s, Traefik, Vault/VSO를 이용한 dev 실행 환경 구성
- 계층 의존성을 테스트로 검증하는 Clean / Hexagonal Architecture

<p>
  <img src="https://img.shields.io/badge/Java-007396?style=flat-square&logo=openjdk&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white"/>
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white"/>
  <img src="https://img.shields.io/badge/K3s-FFC61C?style=flat-square&logo=k3s&logoColor=black"/>
  <img src="https://img.shields.io/badge/Traefik-24A1C1?style=flat-square&logo=traefikproxy&logoColor=white"/>
  <img src="https://img.shields.io/badge/Vault-FFEC6E?style=flat-square&logo=vault&logoColor=black"/>
  <img src="https://img.shields.io/badge/Keycloak-4D4D4D?style=flat-square&logo=keycloak&logoColor=white"/>
</p>

---

## 대표 프로젝트

| Project | What I worked on | Links |
|---|---|---|
| **Project-Auth-Server / Project-Infra** | Spring Boot 인증 서버와 K3s 실행 환경을 함께 구성했습니다. 로그인 / 세션은 Keycloak + oauth2-proxy + Traefik ForwardAuth에 위임하고, auth-server는 Resource Server로 JWT를 다시 검증합니다. Vault → VSO → Kubernetes Secret 흐름으로 secret 전달 경계도 분리했습니다. | [auth-server](https://github.com/donghyeon-ka/project-auth-server) · [infra](https://github.com/donghyeon-ka/project-infra) |
| **feed-postgresql-query-tuning** | PostgreSQL 피드 조회 쿼리를 `EXPLAIN ANALYZE`로 분석했습니다. JOIN 폭증, 인덱스 미사용, 외부 정렬 병목을 확인하고 쿼리 구조와 인덱스 설계를 바꿔 실행 시간을 **1.77 s → 7.66 ms**로 줄였습니다. | [repo](https://github.com/donghyeon-ka/feed-postgresql-query-tuning) |

> 성능 수치는 로컬 실험 또는 단일 인스턴스 테스트 기준입니다.

---

## 기술 글

기술적 의사결정과 트러블슈팅 과정을 기록합니다.

- [Velog @donghyeon2](https://velog.io/@donghyeon2)

---

## 연락처

- Email: [donghyeonka68@gmail.com](mailto:donghyeonka68@gmail.com)
- GitHub: [github.com/donghyeon-ka](https://github.com/donghyeon-ka)

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:0F766E,45:2563EB,100:7C3AED&height=34&section=footer&text=&fontSize=0" width="100%"/>
