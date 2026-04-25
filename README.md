# 🌍 eduOSGeoSeoul: 오픈소스 공간정보 교육 가이드

**eduOSGeoSeoul**은 [OSGeo(Open Source Geospatial Foundation) 한국지부](https://www.osgeo.kr)에서 제공하는 오픈소스 공간정보(OSSGIS) 기술 교육을 위한 실습 기반 지무 교육 저장소입니다.

이 과정은 전문가들이 실무에서 즉시 활용할 수 있도록 **PostGIS**를 이용한 데이터 관리와 **GeoServer**를 활용한 웹 서비스 발행의 핵심 기술을 다룹니다.

---

## 📚 강의 구성

저장소는 크게 세 가지 핵심 모듈로 나뉘어져 있습니다.

### 🐳 [Module 0] 컨테이너 및 실행 환경 구성
실습을 진행하기 위한 OS 및 컨테이너 런타임 환경 구성 가이드를 제공합니다.

- **관련 문서**: 
  - [wsl.md](./Lecture-Install-Pod/wsl.md): Windows Subsystem for Linux (WSL) 설정 안내
  - [podman.md](./Lecture-Install-Pod/podman.md): Podman 컨테이너 런타임 설치 안내
  - [osgeo-pod.md](./Lecture-Install-Pod/osgeo-pod.md): OSGeo 실습용 Pod/컨테이너 실행 가이드

### 🐘 [Module 1] PostGIS를 이용한 공간 데이터베이스
공간 데이터를 일반 DBMS(PostgreSQL)에 넣어 효과적으로 관리하고, 공간 SQL을 통해 강력한 분석을 수행하는 방법을 배웁니다.

- **강의 문서**: [Lecture-PostGIS.md](./Lecture-PostGIS/Lecture-PostGIS.md)
- **주요 학습 내용**:
  - `Docker`를 이용한 PostGIS & pgAdmin 환경 구축
  - 한국 좌표계(EPSG:5186 등) 정정 및 관리
  - QGIS 및 GDAL(`ogr2ogr`)을 이용한 대량 데이터 임포트
  - 실무 공간 SQL 기초 및 공간 분석 기법

### 🛰️ [Module 2] GeoServer를 활용한 공간정보 서비스
인터넷상에 공간정보를 서비스하기 위한 표준 인터페이스(WMS, WFS 등)와 스타일링 기술을 배웁니다.

- **강의 문서**: [Lecture-GeoServer.md](./Lecture-GeoServer/Lecture-GeoServer.md)
- **주요 학습 내용**:
  - `GeoServer` 설치 및 레이어 발행 (Shapefile, Raster, PostGIS)
  - OGC 표준 서비스(WMS, WFS, WCS)의 이해와 활용
  - `SLD` 스타일링 및 `uDig`을 이용한 시각 최적화
  - `GeoWebCache(GWC)`를 이용한 타일 캐시 성능 가속화

---

## 🛠️ 준비 사항

학습을 시작하기 전에 아래 도구들이 설치되어 있어야 합니다. (강의 문서 내 가이드 포함)

- **QGIS**: 공간 데이터 확인 및 가공
- **Code Editor**: 마크다운 열람 및 스타일 수정 (VS Code 추천)

---


이 교육 자료가 오픈소스 공간정보 기술을 배우는 데 큰 도움이 되기를 바랍니다! 🚀
