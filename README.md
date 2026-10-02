<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0F766E,45:2563EB,100:7C3AED&height=90&section=header&text=&fontSize=0" width="100%"/>

## 안녕하세요, 플랫폼 / 인프라 엔지니어 강동현입니다.

개인 물리 서버에 K3s를 올리고 Nginx, Traefik, Keycloak, Vault, PostgreSQL, 모니터링 스택을 직접 구성해 운영하고 있습니다.
Java / Spring 백엔드로 시작했고, 지금은 애플리케이션이 **배포되고 실행되는 환경**을 주로 다룹니다.

장애는 직접 재현하고, 로그만 보지 않고 실제 상태로 확인합니다.
예를 들어 certbot은 매번 `SUCCESS`를 남겼지만, 클라이언트가 받은 인증서 serial을 확인해 보니 38분 25초 동안 이전 인증서로 응답하고 있었습니다.

---

## Focus

- **요청 경로** — Host Nginx TLS 종료 → loopback NodePort → Traefik → Service, forwarded 헤더 신뢰 범위 제한
- **배포** — GitHub Actions → GHCR → Argo CD, Kustomize base / overlay
- **Secret** — Vault → Vault Secrets Operator → Kubernetes Secret, Terraform으로 Vault 설정 관리
- **인증** — Traefik ForwardAuth + oauth2-proxy + Keycloak, 다중 노드 Keycloak의 세션 동작
- **관측** — Prometheus, Alertmanager(Slack), Loki, Grafana

<p>
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white"/>
  <img src="https://img.shields.io/badge/K3s-FFC61C?style=flat-square&logo=k3s&logoColor=black"/>
  <img src="https://img.shields.io/badge/Helm-0F1689?style=flat-square&logo=helm&logoColor=white"/>
  <img src="https://img.shields.io/badge/Argo%20CD-EF7B4D?style=flat-square&logo=argo&logoColor=white"/>
  <img src="https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white"/>
  <img src="https://img.shields.io/badge/Terraform-844FBA?style=flat-square&logo=terraform&logoColor=white"/>
  <img src="https://img.shields.io/badge/Vault-FFEC6E?style=flat-square&logo=vault&logoColor=black"/>
  <img src="https://img.shields.io/badge/Nginx-009639?style=flat-square&logo=nginx&logoColor=white"/>
  <img src="https://img.shields.io/badge/Traefik-24A1C1?style=flat-square&logo=traefikproxy&logoColor=white"/>
  <img src="https://img.shields.io/badge/Keycloak-4D4D4D?style=flat-square&logo=keycloak&logoColor=white"/>
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Prometheus-E6522C?style=flat-square&logo=prometheus&logoColor=white"/>
  <img src="https://img.shields.io/badge/Grafana-F46800?style=flat-square&logo=grafana&logoColor=white"/>
  <img src="https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black"/>
</p>

---

## 프로젝트

### 인증 서버 배포 환경 (GitHub 공개)

현업 개발자 멘토링(2026.03–09) 과제로, 인증 서버 하나를 K3s dev 클러스터에 배포하는 환경을 만들었습니다.
처음에는 Argo CD GitOps로 배포했고, 이후 같은 서버를 ForwardAuth · VSO 구조로 다시 구성했습니다.

```text
project-auth-server ── CI ──▶ GHCR 이미지 ── tag 갱신 ──▶ project-auth-gitops ── Argo CD ──▶ dev K3s
project-infra       : 같은 서버를 ForwardAuth + Vault Secrets Operator + default-deny 구조로 다시 구성한 실행 환경
```

| Repository                                                                          | 내용                                                                                                                                                                                                                                                                                                                                                  |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**project-auth-gitops**](https://github.com/donghyeon-ka/project-auth-gitops) | Argo CD로 dev 클러스터에 배포하는 GitOps 저장소입니다. 앱 CI가 GHCR에 이미지를 올리면`repository_dispatch`로 overlay의 image tag를 갱신하고, Argo CD가 sync wave 순서(Vault → Keycloak · PostgreSQL → 앱)로 반영합니다. Vault 설정은 Terraform으로 관리하고, 최초 bootstrap(수동 runbook)과 평상시 reconcile(self-hosted runner)을 분리했습니다. |
| [**project-infra**](https://github.com/donghyeon-ka/project-infra)             | 인증 서버 실행 환경을 Kustomize로 다시 구성했습니다. 미인증 요청은 Traefik ForwardAuth → oauth2-proxy → Keycloak에서 먼저 막고, Secret은 Vault → VSO → Kubernetes Secret으로 전달합니다. NetworkPolicy는 default-deny에서 시작하고,`kustomize build` · `kubeconform` · `kube-linter`로 매니페스트를 검증합니다.                           |
| [**project-auth-server**](https://github.com/donghyeon-ka/project-auth-server) | 위 환경에 배포한 Spring Boot 인증 서버입니다. 로그인은 Ingress 계층에 맡기고, Keycloak이 발급한 JWT를 Resource Server로 다시 검증합니다.                                                                                                                                                                                                              |

### 개인 서버 프로젝트

개인 서버에서 구성한 프로젝트입니다. 과정은 [TechLog](https://hyeonworks.com)에 정리하고 있습니다.

| 프로젝트                                                       | 한 일                                                                                                                                                               | 결과                                                                                                                                     |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **Home Platform**데스크톱 1대, 단일 노드 K3s2026.04–09  | Gitea, Keycloak, CloudNativePG, 관측 스택을 한 대에 구성했습니다. Host Nginx가 TLS 종료와 접근 제어를, Traefik이 라우팅을 맡고, Secret은 Vault → VSO로 관리합니다. | LAN에서 NodePort 직접 접근 차단(루프백만 허용)Prometheus 수집 대상 30/30Alertmanager 설정 배포 시 rollback 원인 수정                     |
| **Keycloak 2-node 클러스터**노트북 위 VM 2대, K3s2026.09 | Keycloak 26 두 대 구성에서 재시작, DB 지연, 인증서 갱신, refresh 토큰 동시 갱신을 직접 재현하고 측정했습니다.                                                       | 인증서 반영**38분 25초 → 1–2초**DB 200 ms 지연 시 로그인 **66 → 1,872 ms**rolling restart 후 DB 세션 **151개 유지** |

> 수치는 모두 개인 서버에서 측정한 값입니다.

### 백엔드

- [**feed-postgresql-query-tuning**](https://github.com/donghyeon-ka/feed-postgresql-query-tuning) — PostgreSQL 피드 조회 쿼리를 `EXPLAIN ANALYZE`로 분석해 쿼리 구조와 인덱스를 바꾸고, 실행 시간을 **1.77 s → 7.66 ms**로 줄였습니다. (로컬 단일 인스턴스 기준)

---

## 기술 글

- [TechLog — hyeonworks.com](https://hyeonworks.com) · 직접 운영하는 기술 기록 서비스
- [Velog @donghyeon2](https://velog.io/@donghyeon2)

---

## 연락처

- Email: [donghyeonka68@gmail.com](mailto:donghyeonka68@gmail.com)
- GitHub: [github.com/donghyeon-ka](https://github.com/donghyeon-ka)

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:0F766E,45:2563EB,100:7C3AED&height=34&section=footer&text=&fontSize=0" width="100%"/>
