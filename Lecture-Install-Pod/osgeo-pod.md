윈도우 환경에서 Podman **네이티브 방식(Pod)**을 사용하여 PostGIS, pgAdmin, GeoServer 환경을 구축하는 전체 가이드를 폴더 구조와 함께 최종 정리해 드립니다.


---
## Podman 네이티브 구성 (osgeo-pod)

모든 명령어는 반드시 **`\osgeo-pod` 폴더 내에서** 실행해 주세요.

### 단계 1: Pod(파드) 생성

파드 이름을 `osgeo-pod`로 지정하고 필요한 모든 포트를 오픈합니다.

```powershell
podman pod create --name osgeo-pod -p 5432:5432 -p 8070:8070 -p 8080:8080
```

### 단계 2: PostGIS 실행

```powershell
podman run -d --pod osgeo-pod --name postgis `
  -e POSTGRES_USER=postgres `
  -e POSTGRES_PASSWORD=postgres `
  -e POSTGRES_DB=gisdb `
  -v postgis_db:/var/lib/postgresql/data:Z `
  postgis/postgis:17-3.5
```

### 단계 3: pgAdmin 실행

```powershell
podman run -d --pod osgeo-pod --name pgadmin `
  -e PGADMIN_DEFAULT_EMAIL=admin@admin.com `
  -e PGADMIN_DEFAULT_PASSWORD=admin `
  -e PGADMIN_LISTEN_PORT=8070 `
  dpage/pgadmin4:8.14
```

### 단계 4: GeoServer (OSGeo 공식 이미지) 실행

```powershell
podman run -d --pod osgeo-pod --name geoserver `
  -e GEOSERVER_DATA_DIR=/opt/geoserver_data `
  -v geoserver_data:/opt/geoserver_data:Z `
  docker.osgeo.org/geoserver:2.28.0
```

모니터링:

```bash
podman stats --no-stream --format "table {{.Name}} {{.CPUPerc}} {{.MemPerc}}"
```

---

## 서비스 접속 및 내부 연결 정보

컴포즈와 다르게 파드(네이티브) 방식의 **모든 서비스가 `localhost`를 통해 통신**한다는 점입니다.

모든 컨테이너가 같은 Pod 안에 있으므로, **서로를 찾을 때 IP나 서비스명 대신 `localhost`를 사용**합니다.

| **서비스**       | **접속 주소**                         | **초기 계정 / 비밀번호**        |
| ------------- | --------------------------------- | ----------------------- |
| **pgAdmin**   | `http://localhost:8070`           | admin@admin.com / admin |
| **GeoServer** | `http://localhost:8080/geoserver` | admin / geoserver       |
| **PostGIS**   | `localhost:5432` (외부 접속용)         | postgres / postgres     |

### 💡 연결 팁 (중요)

1. **pgAdmin에서 DB 등록 시:** Host에 `localhost`를 적으세요.
    
2. **GeoServer에서 PostGIS 저장소 등록 시:** Host에 `localhost`를 적으세요.



---

## 관리 및 자동화 가이드

### 리부팅 후 다시 시작하기

윈도우를 다시 켰을 때는 아래 두 줄만 기억하세요.

```powershell
podman machine start
```

```powershell
podman pod start osgeo-pod
```

### 포트 변경 등 설정 수정을 위해 삭제할 때

데이터는 별도 볼륨에 안전하게 저장되어 있으니 안심하고 파드를 삭제 후 재생성하세요.

```powershell
podman pod rm -f osgeo-pod
```


위 다이어그램처럼 호스트의 물리적 폴더(`pgdata`, `geodata`)가 컨테이너 내부의 핵심 경로와 직접 연결(Volume Mount)됩니다. 이 덕분에 컨테이너라는 "껍데기"는 언제든 갈아치울 수 있고, 소중한 "데이터"는 윈도우 폴더에 안전하게 남게 되는 것입니다.


### (선택) 부팅 시 일괄 실행 (Batch 파일 예시)

`\manage-gis.bat` 파일을 만들고 아래 내용을 저장해두면 편리합니다.

코드 스니펫

```
@echo off
podman machine start
timeout /t 5
podman pod start osgeo-pod
echo OSGeo GIS Stack is starting...
pause
```

### 파드 관리 명령어 요약

- **파드 중지:** `podman pod stop osgeo-pod`
    
- **파드 시작:** `podman pod start osgeo-pod`
    
- **상태 확인:** `podman ps --pod`
    
- **완전 삭제 (재설정 시):** `podman pod rm -f osgeo-pod`
    



---