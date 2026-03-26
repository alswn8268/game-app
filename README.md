# Tap Timing

줄어드는 링을 정확한 타이밍에 탭하는 하이퍼캐주얼 모바일 게임.

---

## 게임 방법

1. 화면 가운데 원(타깃)을 향해 바깥에서 링이 줄어듭니다.
2. 링이 **초록 구간(GOOD)** 에 들어올 때 탭 → 50점
3. 링이 **하얀 구간(PERFECT)** 까지 기다렸다가 탭 → 100점
4. 타이밍을 놓치면 목숨 1개 차감 (총 3개)
5. 콤보 3개 이상 → ×2 배율, 5개 이상 → ×3 배율

---

## 레벨 진행

| 레벨 | 속도 | 타깃 크기 | PERFECT 판정 | 페이크 확률 |
|------|------|-----------|-------------|------------|
| 1–3  | 기본 | 110 px   | ±18 px      | 없음       |
| 4–6  | +10% | 104 px   | ±16 px      | 없음       |
| 5–7  | +20% | 104 px   | ±16 px      | ~8–23%     |
| 8–10 | +35% | 98 px    | ±14 px      | ~28–43%    |
| 13+  | +65% | 86 px    | ±10 px      | 최대 55%   |

- **레벨 5+**: 링이 중간에 멈추고 흔들리는 **페이크 멈춤** 등장
- **레벨 8+**: 링 시작 크기가 매 판마다 랜덤 (예측 불가)

---

## 주요 기능

### 게임플레이
- `PERFECT` / `GOOD` / `MISS` / `EARLY` / `CLOSE!` 5단계 판정
- `CLOSE!` — GOOD 구간 직전 ±20 px에서 놓쳤을 때 별도 피드백
- 실시간 최고 점수 갭 표시 (`+50` / `-120`)
- 목숨 1개 남으면 심장박동 애니메이션 + 빨간 위험 테두리
- 레벨업 시 배너 텍스트 오버레이

### 결과 화면
- PERFECT / GOOD / MISS 횟수
- **최대 콤보** / **정확도 %** / **도달 레벨**
- `최고 점수까지 -X점` — 재도전 동기 유발

### 오디오 (Web Audio API, 외부 파일 없음)
| 상황 | 효과음 |
|------|--------|
| PERFECT | 3음 상승 아르페지오 |
| GOOD | 깔끔한 단음 |
| CLOSE! / MISS | 저음 버즈 |
| EARLY | 경고 하강음 |
| 레벨업 | 4음 팡파레 |
| 게임오버 | 3음 슬픈 하강 |
| 콤보 | 콤보 수에 비례한 피치 |
| **배경음악** | A단조 루핑 일렉트로닉, 레벨마다 BPM 증가 (132→168) |

---

## 기술 스택

```
HTML5 + CSS3 + Vanilla JavaScript (ES Module)
Capacitor v6          — 웹앱 → Android 네이티브 패키징
@capacitor-community/admob — 배너 / 전면 광고
Web Audio API         — 모든 효과음 & 배경음악 (파일 없음, 프로시저럴)
```

---

## 프로젝트 구조

```
game-app/
├── www/
│   └── index.html        # 전체 게임 (HTML + CSS + JS 단일 파일)
├── capacitor.config.json # Capacitor & AdMob 설정
├── package.json
├── guide.html            # 개발/배포 전체 가이드
├── store-description.txt # 플레이스토어 등록 문구
└── icon.svg              # 앱 아이콘
```

---

## 빠른 시작 (로컬 테스트)

```bash
# 브라우저에서 바로 열기 (AdMob 없이 폴백 UI로 동작)
open www/index.html

# 또는 간단한 서버
npx serve www
```

## Android 빌드

```bash
npm install
npx cap sync
npx cap open android   # Android Studio에서 빌드
```

> `capacitor.config.json`의 `ca-app-pub-XXXXXXXX` 값을 실제 AdMob ID로 교체 필요.

---

## 광고 설정

`www/index.html` 내 다음 세 곳의 `ca-app-pub-XXXXXXXXXXXXXXXX/XXXXXXXXXX`를 실제 ID로 변경:

```javascript
// 배너
adId: 'ca-app-pub-XXXXXXXXXXXXXXXX/XXXXXXXXXX'
// 전면 (2곳)
adId: 'ca-app-pub-XXXXXXXXXXXXXXXX/XXXXXXXXXX'
```

---

## 라이선스

MIT
