# biz-arena

풀면서 배우는 경제·경영 사이트 (biz-arena.org). Arena 패밀리 사이트로, sci-arena와 같은 레슨 구조를 씁니다.

- 레벨1: 고교 「경제」·「금융과 경제생활」, 특성화고 상업·경영 전문교과 수준 8과목 (c1~c8), 공개: c1 경제, c2 금융생활, c3 경영 기초, c4 회계 원리, c5 회계 실무
- 레벨2~3: 대학 원론과 전공 트랙 독학 가이드 (준비 중)

## 구조
- `index.html` 첫 화면, `browse.html` 코스 목록, `player.html` 레슨 플레이어
- `content/level1/courses.json` 과목 목록 (`available`로 공개 여부 관리)
- `content/level1/c{N}/ch{NN}/` 챕터별 레슨(l01~l04), 복습(review.json), chapter.json
- `content/rules.json` 바뀔 수 있는 법정·제도 수치(세율, 최저임금 등)를 한곳에서 관리
- `tools/` 빌드·점검 도구 (사이트 동작에는 필요 없음)

## 빌드와 점검
```
python3 tools/build_c1_ch01.py content      # 챕터 빌드 (ch01~ch08)
python3 tools/make_course_index.py content   # course.json 갱신
python3 tools/count.py tools/build_c1_ch01.py  # 빌드 전 레슨 분량 확인
python3 -m http.server 8765 &                # 로컬 서버
python3 tools/test_l1.py c1                  # 브라우저 점검: 모든 문항에 정답 입력 후 채점 확인
python3 tools/audit.py                       # 규정값 점검 기한 확인
```

## 규정값 관리
레슨에 법정 수치를 쓸 때는 숫자를 직접 적지 않고 `tools/rules.py`의 `rv("키")`로 불러옵니다.
그 레슨에는 `"rules": ["키"]`를 적어 두면 레슨 파일에 점검 기한이 기록되고, `audit.py`가 기한이 다가온 항목과 해당 레슨을 보여 줍니다.
값이 바뀌면 `rules.json`만 고친 뒤 해당 챕터를 다시 빌드합니다.
