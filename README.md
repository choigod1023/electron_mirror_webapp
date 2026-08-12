# electron_mirror_webapp

**한국어** · [日本語](README.ja.md) · [English](README.en.md)

> 라즈베리파이로 만든 스마트 미러에 들어갈 UI — Electron.js 기반 시계 · 날씨 대시보드

## 소개

**electron_mirror_webapp**은 라즈베리파이(Raspberry Pi) 기반 스마트 미러 화면에 표시할 UI를 Electron.js로 구현한 프로젝트입니다. 검은 배경에 흰 글씨로 구성된 미러용 대시보드로, 현재 시각과 날짜, 현재 날씨, 이후 시간대별 날씨 예보를 한 화면에 보여줍니다. 창 크기는 미러 디스플레이에 맞춰 1024×600으로 고정되어 있습니다.

## ✨ 주요 기능 (코드 기준)

- **시계 & 날짜**: 12시간제(AM/PM) 시각과 "M월 D일 O요일"(한국어 요일)을 우측에 크게 표시합니다.
- **현재 날씨**: OpenWeatherMap 데이터를 받아 현재 날씨 아이콘, 기온(°), 습도(%)를 표시합니다.
- **시간대별 예보**: 이후 5개 시간대의 시각(오전/오후), 날씨 아이콘, 기온, 습도를 리스트로 보여줍니다.
- **상황별 날씨 아이콘 매핑**: 날씨 상태(Rain/Clouds/Snow/Thunderstorm/Mist·Haze·Fog/Clear)와 구름량·일출/일몰 시각을 조합해 `img/` 폴더의 적절한 아이콘(맑음/흐림/밤/눈/폭풍/안개 등)을 선택합니다. (`return_img()`)
- **미러 친화적 UI**: 검은 배경·흰색 텍스트·Nanum Gothic 폰트로 구성되어 거울에 반사되어 보이기 좋은 디자인입니다.

## 🛠 기술 스택

![Electron](https://img.shields.io/badge/Electron-12-47848F?logo=electron&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![OpenWeatherMap](https://img.shields.io/badge/OpenWeatherMap-API-EB6E4B?logo=openweathermap&logoColor=white)

- **Electron.js** (`^12.0.5`) — 데스크톱 셸 / 브라우저 윈도우
- **JavaScript / HTML / CSS** (Vanilla)
- **request** — OpenWeatherMap One Call API 호출
- **nodemon** (devDependency)

## 🏗 동작 방식 / 아키텍처

- **메인 프로세스** (`main.js`): `app.whenReady()` 후 `BrowserWindow`(1024×600, `nodeIntegration: true`, `contextIsolation: false`)를 생성하고 `index.html`을 로드합니다. macOS 관례에 맞춘 `activate` / `window-all-closed` 핸들링을 포함합니다.
- **렌더러 프로세스** (`index.html` + `renderer.js`): 페이지 로드 시 현재 시각/날짜를 계산해 `#clock`에 렌더링하고, OpenWeatherMap One Call API(`api.openweathermap.org/data/2.5/onecall`)를 호출해 현재 날씨(`#current`)와 시간대별 예보(`#hours`)를 채웁니다.
- **에셋** (`img/`): 날씨 상태별 아이콘 PNG (`sun_clear`, `sun_cloud`, `cloud`, `night_clear`, `night_cloud`, `rainy`, `snow`, `storm`, `fog`).

## 🚀 시작하기

### 사전 요구사항

- [Node.js](https://nodejs.org) (npm 포함)
- [OpenWeatherMap](https://openweathermap.org/api) API 키

### 설치

```bash
git clone https://github.com/choigod1023/electron_mirror_webapp.git
cd electron_mirror_webapp
npm install
```

### 설정 (날씨 API)

날씨 데이터는 `renderer.js`의 OpenWeatherMap One Call API 호출로 가져옵니다. 현재 코드에는 API 키(`appid`)와 위치 좌표(`lat`, `lon`)가 하드코딩되어 있으므로, 사용 전 **본인의 OpenWeatherMap API 키와 원하는 지역의 좌표로 교체**하세요.

```js
// renderer.js
request('https://api.openweathermap.org/data/2.5/onecall?lat=<위도>&lon=<경도>&appid=<본인_API_KEY>&units=metric', ...)
```

> 보안 참고: 저장소에 커밋된 예제 API 키는 즉시 교체(재발급)하는 것을 권장합니다.

### 실행

```bash
npx electron .
```

> `package.json`의 `start` 스크립트는 `electromon .`으로 정의되어 있습니다. 표준 실행은 위와 같이 Electron으로 직접 구동하며, 개발 중 자동 재시작이 필요하면 `nodemon`을 활용할 수 있습니다.

## 📁 구조

```
electron_mirror_webapp/
├── main.js          # Electron 메인 프로세스 (BrowserWindow 1024x600)
├── preload.js       # 프리로드 스크립트 (현재 대부분 주석 처리)
├── index.html       # 미러 UI 레이아웃 · 스타일
├── renderer.js      # 시계 · 날씨 로직 (OpenWeatherMap 호출, 아이콘 매핑)
├── img/             # 날씨 상태별 아이콘 PNG
└── package.json
```

## 📝 라이선스

[CC0 1.0 (Public Domain)](LICENSE.md)

---

## 👤 기여도 & 개발 환경

| 항목 | 내용 |
|---|---|
| **기여 비율** | 본인 커밋 **4건** — `electron-quick-start` 템플릿을 히스토리째 가져와 시작한 저장소라 커밋 비율(4/97 = 4.1%)은 의미가 없습니다 |
| **참여 인원** | 1명 (나머지 커밋은 템플릿 원작자들) |

<sub>집계 기준: origin의 **모든 브랜치**에서 도달 가능한 커밋(머지 커밋·빈 커밋 제외), 커밋 author 이메일 기준이며 동일인의 여러 이메일은 하나로 합산, 봇·자동화 커밋(dependabot 93건)은 제외했습니다.</sub>
