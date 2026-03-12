# 문서 작성 가이드

MkDocs Material 테마 + GitHub Pages 환경에서 발견된 주의사항 모음.

---

## 1. 리스트 렌더링

### 문제
`**소제목**` 바로 아래에 리스트(`-`, `1.`)를 붙이면 리스트로 렌더링되지 않는다.

```markdown
<!-- Bad -->
**애자일 4가지 가치**
1. 개인과 상호작용
2. 동작하는 소프트웨어

<!-- Good -->
#### 애자일 4가지 가치

1. 개인과 상호작용
2. 동작하는 소프트웨어
```

### 해결책
- `**소제목**` 대신 `####` 헤딩 사용
- 헤딩과 리스트 사이에 반드시 빈 줄 한 줄 추가

---

## 2. 표 내 O/X 표시

### 문제
마크다운 bold(`**O**`)는 `<span>` 태그 안에서 동작하지 않는다.

```markdown
<!-- Bad: bold 적용 안 됨 -->
<span style="color:green">**O**</span>

<!-- Good: CSS 인라인 스타일 사용 -->
<span style="color:green;font-weight:bold">O</span>
<span style="color:red;font-weight:bold">X</span>
```

---

## 3. 헤딩 내 링크

### 문제
MkDocs에서 헤딩(`###`) 안에 마크다운 링크를 넣으면 앵커(#)로 변환되어 클릭 시 동작하지 않는다.

```markdown
<!-- Bad: 클릭해도 이동 안 됨 -->
### [알고리즘](./01_알고리즘.md)

<!-- Good: 헤딩과 링크 분리 -->
### 알고리즘 · [문서 보기](./01_알고리즘.md)
```

---

## 4. HTML 렌더링

MkDocs Material은 마크다운 파일 내 HTML 인라인 태그를 지원한다.
단, 마크다운과 HTML을 혼용할 때는 아래 규칙을 따른다.

- `<span>` 안에서는 마크다운 문법(`**`, `_` 등)이 동작하지 않으므로 HTML 속성으로 처리
- 색상: `style="color:green"` / `style="color:red"`
- 굵기: `style="font-weight:bold"`
- 복합: `style="color:green;font-weight:bold"`

---

## 5. 이모지 사용

GitHub Pages 렌더링에서 이모지는 지원되지만 가독성을 해칠 수 있으므로 사용하지 않는다.

- 헤딩, 리스트, 링크 텍스트에 이모지 금지
- 시험 포인트 섹션에 별표(`⭐`) 등 금지

---

## 6. 파일/폴더 구조 규칙

```
docs/
└── TOPCIT/
    └── 01_소프트웨어개발/
        ├── README.md          # 영역 목차 (체크박스 포함)
        ├── 01_알고리즘.md
        ├── 02_자료구조.md
        └── ...
```

- 파일명은 `번호_한글명.md` 형식
- 각 영역 폴더에 `README.md`로 목차 작성
- `mkdocs.yml`의 `nav`에 새 파일 추가 필요

---

## 7. 새 파일 추가 시 체크리스트

1. `docs/TOPCIT/영역폴더/` 에 `번호_파일명.md` 생성
2. 해당 영역의 `README.md` 링크 추가
3. `mkdocs.yml`의 `nav` 섹션에 등록
4. commit & push → GitHub Actions 자동 배포
