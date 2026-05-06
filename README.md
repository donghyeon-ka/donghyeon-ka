## 안녕하세요, 백엔드 개발자 강동현입니다.

Java / Spring Boot 기반 백엔드를 중심으로 공부하고 있습니다.  
요즘은 **인증 경계**, **PostgreSQL 실행 계획**, **Kubernetes 위에서 애플리케이션이 실제로 실행되는 방식**에 관심이 많습니다.

작게 구현하더라도 “어디까지가 애플리케이션 책임이고, 어디부터가 인프라 / 데이터베이스 / 인증 시스템의 책임인가”를 분리해서 이해하려고 합니다.

---

## 요즘 집중하는 것

- Spring Security Resource Server와 Keycloak 기반 인증 경계
- PostgreSQL `EXPLAIN ANALYZE` 기반 쿼리 분석과 인덱스 설계
- K3s, Traefik, Vault/VSO를 이용한 dev 실행 환경 구성
- 계층 의존성을 테스트로 검증하는 Clean / Hexagonal Architecture

---

## 대표 프로젝트

### Project-Auth-Server / Project-Infra

Spring Boot 인증 서버와 Kubernetes 기반 실행 환경을 함께 구성한 프로젝트입니다.

- 로그인 / 세션 처리는 Keycloak + oauth2-proxy + Traefik ForwardAuth에 위임
- auth-server는 Spring Security Resource Server로 JWT를 다시 검증
- Vault → VSO → Kubernetes Secret 흐름으로 secret 전달 경계 분리
- Kustomize, NetworkPolicy, PSS Restricted로 dev 환경 실행 구조 정리

[project-auth-server](https://github.com/donghyeon-ka/project-auth-server) · [project-infra](https://github.com/donghyeon-ka/project-infra)

### feed-postgresql-query-tuning

PostgreSQL 피드 조회 쿼리를 실행 계획 기준으로 분석하고 개선한 프로젝트입니다.

- `EXPLAIN ANALYZE`로 JOIN 폭증, 인덱스 미사용, 외부 정렬 병목 확인
- 쿼리 구조와 인덱스 설계를 바꿔 실행 시간 **1.77 s → 7.66 ms** 개선
- Redis 캐시와 Locust 부하 테스트로 개선 경로 검증

모든 수치는 로컬 실험 또는 단일 인스턴스 테스트 기준입니다.

[feed-postgresql-query-tuning](https://github.com/donghyeon-ka/feed-postgresql-query-tuning)

---

## 기술 글

기술적 의사결정과 트러블슈팅 과정을 기록합니다.

- [Velog @donghyeon2](https://velog.io/@donghyeon2)

---

## 연락처

- Email: [donghyeonka68@gmail.com](mailto:donghyeonka68@gmail.com)
- GitHub: [github.com/donghyeon-ka](https://github.com/donghyeon-ka)
