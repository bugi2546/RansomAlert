# 실시간 랜섬웨어 피해 정보 모니터링 시스템


## 프로젝트 개요

* **기간:** 2025.00 ~ 2025.00 (※ 실제 진행 기간으로 수정해 주세요)
* **팀 구성:** 개인 프로젝트 (또는 팀명)
* **한 줄 소개:** Ransomware.live Open API를 활용하여 전 세계 랜섬웨어 공격 피해 현황 및 해킹 조직 활동을 실시간으로 추적·수집하고, Windows 데스크톱 알림을 제공하는 모니터링 애플리케이션
* **담당 역할:** 
  * Open CTI(Ransomware.live) API 연동 및 데이터 수집·파싱 로직 개발
  * GUI 프리징 방지를 위한 `threading` 기반 비동기 백그라운드 모니터링 구현
  * `Set` 자료구조 활용 중복 탐지 방지 및 신규 공격 선별 필터링 개발
  * `Tkinter` 기반 로그 콘솔 GUI 및 `win10toast` 활용 데스크톱 실시간 알림 연동
  * `PyInstaller Spec` 커스텀을 통한 DLL 의존성 수동 링크 및 독립 실행 파일(`.exe`) 패키징
---

## 기술 스택

| 구분 | 사용 기술 / 라이브러리 |
| :--- | :--- |
| **Language** | Python 3.10 |
| **GUI Framework** | `tkinter`, `tkinter.scrolledtext` |
| **Network / API** | `requests` (Ransomware.live API v2 연동) |
| **Concurrency** | `threading`, `time` |
| **Notification** | `win10toast` (Windows 10/11 Toast Notification) |
| **Packaging** | `PyInstaller` (Spec 파일 커스텀 패키징) |

---

## 주요 기능 및 특징

### 1. 실시간 위협 정보 수집 
* `Ransomware.live` 오픈 CTI 서비스의 API를 연동하여 다크웹 및 공개망에 등록되는 최신 랜섬웨어 피해 사례를 파싱
* 수집 항목: 피해자(기업/기관명), 해킹 조직(그룹), 발견 날짜, 공격 날짜, 피해 국가, 다크웹 주장 URL

### 2. 비동기 멀티스레딩
* 백그라운드 스레드에서 주기적으로 API 통신을 수행하여 데이터 수집 중에도 GUI 화면이 멈추지 않고 매끄럽게 동작하도록 처리

### 3. 중복 데이터 필터링 로직
* 자료구조를 활용해 이미 확인된 고유 식별자를 추적 및 저장
* 프로그램 최초 실행 시에는 최근 피해 3건을 로드하며, 이후 주기적 조회에서는 신규 공격이 감지될 때만 알림을 발생시키도록 구현

### 4. GUI & 윈도우 알림 
* **GUI 화면**: 모니터링 시작/중지 버튼 제어 및 스크롤 가능한 실시간 로그 창 제공
* **데스크톱 알림**: 신규 랜섬웨어 공격 감지 시 Windows 우측 하단 팝업 알림 송출

### 5. 독립 실행 파일 배포 
* `PyInstaller Spec File`을 구성하여 Python 미설치 환경에서도 동작하는 독립 실행 파일로 패키징
* CLI 콘솔 창을 숨긴 Non-Console 모드 및 전용 아이콘을 적용

---


## 파일 구조 

```text
.
├── app.py          # 랜섬웨어 모니터링 메인 GUI 및 로직 소스코드
├── app.spec        # PyInstaller 빌드 및 DLL 링크 설정 파일
└── icon.ico        # 프로그램 전용 실행 아이콘 파일
