# ourdays-travel-webview

Our Days 앱의 "여행" 탭 안 웹뷰 버튼이 불러오는 정적 페이지 저장소입니다.
GitHub Pages로 서빙되며, 앱은 재설치 없이 이 저장소의 최신 파일을 그대로 보여줍니다.

## 사용 방법

1. `test.html` 내용을 새 여행 계획 내용으로 덮어씁니다.
2. `data/meta.json`의 `updatedAt` 값을 오늘 날짜(`YYYY-MM-DD`)로 바꿉니다. 앱의 "웹뷰 보기" 버튼에 이 날짜가 최근 갱신일로 표시됩니다.
3. `main` 브랜치에 push하면 GitHub Pages가 자동으로 재배포하고, 앱은 다음에 웹뷰를 열 때 새 내용을 받아옵니다(캐시 방지를 위해 앱이 매번 쿼리스트링에 타임스탬프를 붙여 요청합니다).

## 배포 주소

- 페이지: https://thkim0515.github.io/ourdays-travel-webview/test.html
- 메타(최근 갱신일): https://thkim0515.github.io/ourdays-travel-webview/data/meta.json
