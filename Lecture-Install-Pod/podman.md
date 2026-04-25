윈도우 환경에서 Podman을 사용하는 것은 Docker Desktop의 훌륭한 대안이 될 수 있습니다. Podman은 **데몬(Daemon)** 없이 동작하며, **루트리스(Rootless)** 권한으로 보안에 유리하다는 장점이 있습니다.

---
## winget으로 Podman 설치하기

터미널(PowerShell 또는 CMD)을 **관리자 권한**으로 열고 아래 명령어를 입력하세요.

```PowerShell
winget install -e --id RedHat.Podman
```

- `-e`: 정확한 ID 일치 확인    
- `--id RedHat.Podman`: Podman 공식 패키지 식별자    

### Podman Desktop도 설치하고 싶다면?

GUI 환경이 필요하다면 아래 명령어를 추가로 실행하세요.

```powershell
winget install -e --id RedHat.Podman-Desktop
```


---
## 설치 후 필수 초기 설정 (매우 중요)

설치가 완료되었다고 바로 사용할 수 있는 것은 아닙니다. Podman은 WSL2 기반의 가상 머신(VM)이 필요하기 때문에 초기화 과정이 필수입니다.

1. **컴퓨터 재부팅:** WSL 관련 기능이 활성화되도록 재부팅을 한 번 해주세요.    
2. **머신 초기화 및 시작:**

Podman이 사용할 WSL 배포판 생성

```powershell
podman machine init
```


가상 머신 구동

```powershell
podman machine start
```


---
## 설치 확인 및 테스트

모든 설정이 끝났다면 아래 명령어로 확인해 보세요.

버전 확인

```PowerShell
podman version
```

## ⚠️ 주의사항

- **Docker Desktop과 충돌:** 만약 기존에 Docker Desktop이 설치되어 있다면, 포트 충돌이나 WSL 설정 문제로 오류가 발생할 수 있습니다. 가급적 하나만 활성화해서 사용하는 것이 좋습니다.
    
- **환경 변수:** `winget` 설치 직후에 `podman` 명령어를 찾을 수 없다고 나온다면, 터미널을 껐다가 다시 켜보세요.


---
## 주요 사용법 (Docker와 비교)

Podman의 명령어는 Docker와 거의 **99% 동일**합니다. 사실상 `docker` 대신 `podman`만 입력하면 됩니다.

### 기본 명령어 요약
| 기능            | 명령어                                                   |
| :------------ | :---------------------------------------------------- |
| 이미지 검색        | `podman search [키워드]`                                 |
| 이미지 다운로드      | `podman pull [이미지명]`                                  |
| 컨테이너 실행       | `podman run -d --name [이름] -p [호스트포트]:[컨테이너포트] [이미지]` |
| 실행 중인 컨테이너 확인 | `podman ps`                                           |
| 컨테이너 중지 및 삭제  | `podman stop [ID/이름]`, `podman rm [ID/이름]`            |
| 이미지 삭제        | `podman rmi [이미지ID]`                                  |


---
## Podman만의 핵심 개념: Pod(파드)

Podman은 쿠버네티스(Kubernetes)의 최소 단위인 **Pod** 개념을 로컬에서도 사용할 수 있습니다. 여러 컨테이너를 하나의 네트워크 망(Pod) 안에 묶어 관리할 수 있어 편리합니다.

예를 들어, `docker-compose` 작업을 Podman의 네이티브 방식으로 구현하면 다음과 같습니다.

1.  **Pod 생성:** 포트 포워딩 설정을 Pod 단위에서 한 번에 관리합니다.
```powershell
podman pod create --name my-stack -p 5432:5432 -p 8070:80
```
2.  **컨테이너를 Pod 안에 투입:** `--pod` 옵션을 사용합니다.
```powershell
# PostGIS 실행
podman run -d --pod my-stack --name postgis -e POSTGRES_PASSWORD=postgres postgis/postgis:17-3.5

# pgAdmin 실행
podman run -d --pod my-stack --name pgadmin -e PGADMIN_DEFAULT_EMAIL=admin@admin.com -e PGADMIN_DEFAULT_PASSWORD=admin dpage/pgadmin4:8.14
```


---
끝.