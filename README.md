# ourdays-travel-webview

Our Days 앱의 "여행" 탭 → "우리의 여행 웹 보기" 버튼이 불러오는 정적 페이지 저장소입니다.
GitHub Pages로 서빙되며, 앱은 재설치 없이 이 저장소의 최신 파일을 그대로 보여줍니다.

> **Private 전환 불가**: GitHub Free 플랜에서는 비공개 저장소로 GitHub Pages를 서빙할 수 없습니다(Pro 이상 필요). 그래서 이 저장소는 계속 public으로 유지합니다.

> ⚠️ **무조건 `main` 브랜치에 바로 push 하세요.** GitHub Pages는 `main` 브랜치만 서빙합니다. 별도 작업 브랜치(`claude/...` 등)나 PR을 거치면 페이지에 **반영되지 않습니다.** 항상 `main`에서 직접 커밋하거나(또는 작업 브랜치를 만들었다면 반드시 `main`으로 병합·push까지) 마쳐야 실제 배포 주소에 바로 반영됩니다.

## 갱신 이력

| 날짜 | 여행 내용 | 비고 |
|---|---|---|
| 2026-09-13 | 토담한방사우나 & 계곡놀이 (정발산역 출발, 당일치기) | 최초 실제 여행 콘텐츠 |
| 2026-09-22 | 대전·논산 데이트 드라이브 코스 (대전역 렌터카 픽업 ~ 강경구락부, 당일치기) | 동쪽→서쪽 한 방향 타임라인, 체크리스트 없는 코스 소개형 페이지 |

새 여행으로 내용을 바꿀 때마다 위 표에 한 줄씩 날짜와 여행 내용을 추가해주세요. 언제 어떤 여행 페이지가 푸시됐는지 여기만 보면 알 수 있습니다.

## 파일 구성 및 저장 규칙

- **`now_usingPage.html`** — 앱이 실제로 불러오는 **현재 여행** 페이지(고정 경로). "지금 사용 중인 페이지"라는 뜻의 이름으로, 한 번 정해진 뒤로는 **절대 바꾸지 않습니다**(앱 쪽 `TRAVEL_WEBVIEW_URL`이 이 경로로 고정돼 있어, 파일명을 바꾸면 앱도 다시 빌드·재설치해야 합니다). 여행 내용이 바뀔 때는 이 파일의 **내용만** 덮어씁니다.
- **`index.html`** — 지금까지의 모든 여행을 최신순으로 모아보는 **메인(목록) 페이지**. `data/trips.json`을 읽어서 카드 목록을 렌더링합니다. 앱이 불러오는 고정 경로는 아니고, 사람이 직접 열어보는 용도예요.
- `data/trips.json` — `index.html`이 읽는 여행 목록 데이터. 여행마다 `{ id, title, subtitle, date, emoji, url }`을 담은 배열이며, `date`(YYYY-MM-DD) 기준 최신순으로 정렬돼 표시됩니다. **새 여행으로 교체할 때마다 이 배열에 새 항목을 하나 추가하세요.** 현재 여행의 `url`은 `now_usingPage.html`을, 지나간 여행은 `archive/...` 경로를 가리킵니다.
- `data/meta.json` — `updatedAt`(오늘 날짜)과 `title`을 담고 있으며, 앱의 "우리의 여행 웹 보기" 버튼에 최근 갱신일로 표시됩니다. `now_usingPage.html`을 바꿀 때 반드시 함께 갱신하세요.
- `archive/` — 이전에 `now_usingPage.html`이었던 내용을 **덮어쓰기 전에** 그대로 복사해 보관하는 폴더. git 커밋 이력에도 남지만, 실제 파일로도 과거 여행 페이지를 바로 열어볼 수 있도록 별도 보관합니다.
  - 파일명 규칙: `honeyyang_trip_YYYYMMDD.html` (예: `honeyyang_trip_20260913.html`)
  - `YYYYMMDD`는 그 여행 페이지를 올린(갱신한) 날짜입니다.

## 새 여행으로 교체하는 절차

1. 현재 `now_usingPage.html`을 `archive/honeyyang_trip_YYYYMMDD.html`(오늘 날짜)로 복사해 보관합니다.
2. `now_usingPage.html`을 새 여행 내용으로 덮어씁니다. **파일명은 절대 바꾸지 않습니다.**
3. `data/meta.json`의 `updatedAt`(과 필요하면 `title`)을 오늘 날짜로 갱신합니다.
4. `data/trips.json`에 **직전까지 `now_usingPage.html`이었던 여행**을 `archive/...` 경로로 가리키는 새 항목으로 추가하고(1번에서 복사한 파일), **기존에 있던 "현재 여행" 항목**(url이 `now_usingPage.html`인 항목)의 `title`/`subtitle`/`date`/`emoji`를 오늘의 새 여행 내용으로 덮어씁니다. (즉, `now_usingPage.html`을 가리키는 항목은 항상 1개만 유지)
5. 이 README의 "갱신 이력" 표에 오늘 날짜 + 여행 내용을 한 줄 추가합니다.
6. 체크리스트를 쓰는 여행이라면 `now_usingPage.html` 안의 `TRIP_ID` 상수도 새 여행에 맞는 값으로 바꿉니다(아래 "준비물 체크리스트" 참고).
7. **반드시 `main` 브랜치에 바로 push합니다.** (다른 브랜치에서 작업했다면 이 단계에서 `main`으로 병합 후 push까지 마쳐야 합니다.) `main`에 반영되지 않으면 GitHub Pages와 앱에 절대 표시되지 않습니다. 앱은 다음에 웹뷰를 열 때(캐시 방지를 위해 매번 타임스탬프를 붙여 요청) 새 내용을 바로 받아옵니다.

## 배포 주소

- 현재 여행 페이지(앱이 불러오는 고정 경로): https://thkim0515.github.io/ourdays-travel-webview/now_usingPage.html
- **여행 모아보기(메인 페이지)**: https://thkim0515.github.io/ourdays-travel-webview/index.html
- 메타(최근 갱신일): https://thkim0515.github.io/ourdays-travel-webview/data/meta.json

## 준비물 체크리스트 — 체크 저장 / 항목 추가·삭제

`now_usingPage.html` 안 체크박스는 정적이지 않고, Firebase JS SDK(CDN)로 Our Days 앱과 **같은 Firestore 프로젝트**(`our-days-f8384`)에 실시간으로 저장됩니다.

- 저장 위치: `travelWebviewPublic/checklist_{TRIP_ID}` 문서 하나 (`TRIP_ID`는 `now_usingPage.html` 상단 스크립트의 `TRIP_ID` 상수, 예: `toodam-sauna-20260913`). 새 여행으로 내용을 바꿀 때 이 상수도 그 여행에 맞는 값으로 바꿔주면 체크리스트가 이전 여행 것과 섞이지 않습니다.
- 체크 토글·항목 추가·항목 삭제 모두 즉시 Firestore에 저장되고(`onSnapshot`으로 실시간 반영), 커플 두 사람이 동시에 열어도 서로의 체크가 실시간으로 보입니다.
- **보안 범위**: 이 페이지는 로그인이 없는 완전 공개 페이지라 어떤 방문자인지 구분할 방법이 없습니다. 그래서 앱 저장소(`D:\Claude_Dev\app`)의 `firestore.rules`에 `travelWebviewPublic/{document=**}` 네임스페이스 **하나만** `allow read, write: if true`로 열어뒀습니다 — 이 경로는 커플의 다른 개인 데이터(일기·일정·펫 등)와 완전히 격리돼 있어, 이 체크리스트 URL이 알려지더라도 노출되는 건 이 네임스페이스 안 데이터뿐입니다. `{document=**}`는 재귀 와일드카드라, 앞으로 웹뷰에 다른 기능(방명록·투표 등)을 추가해도 이 네임스페이스 아래에 문서만 두면 Firestore 규칙을 다시 배포할 필요가 없습니다(문서 id 앞에 `checklist_`처럼 기능명을 붙여 서로 구분). 그래도 민감한 내용(주소·전화번호 등)은 항목으로 적지 않는 걸 권장합니다.
- Firestore 문서가 아직 없을 때는 `now_usingPage.html`에 하드코딩된 기본 목록으로 자동 초기화됩니다. 이후로는 Firestore 쪽 데이터가 항상 기준입니다.
