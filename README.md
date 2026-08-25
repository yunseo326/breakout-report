# breakout-report

추세 이탈 이벤트 분석 리포트를 GitHub Pages로 발행하는 저장소입니다.

**→ https://yunseo326.github.io/breakout-report/**

## 이 저장소에 대해

여기 있는 HTML은 전부 **생성물**입니다. 직접 편집하지 마세요.
분석 코드와 원천 데이터는 별도의 private 저장소(`yunseo326/2trading`)에 있고,
`publish_report.py`가 빌드 결과만 이쪽으로 복사·커밋합니다.

| 경로 | 설명 |
|---|---|
| `index.html` | `reports.json`에서 매번 새로 생성되는 랜딩 페이지 |
| `reports.json` | 발행 목록 매니페스트 |
| `reports/*.html` | 자체 완결형 리포트 (데이터 내장) |
| `.github/workflows/pages.yml` | main 푸시 시 Pages 배포 |

## 면책

연구용 자료입니다. 투자 조언이 아니며, 과거 성과가 미래 수익을 보장하지 않습니다.
