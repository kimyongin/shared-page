# shared-page

GitHub Pages로 호스팅하는 정적 페이지 모음. 라우팅 인덱스에서 각 페이지로 이동.

**Live**: https://kimyongin.github.io/shared-page/

## 구조

```
shared-page/
├── index.html              # 라우팅 인덱스 (페이지 목록)
├── skills-deck/
│   └── index.html          # → /shared-page/skills-deck/
└── <새-페이지>/
    └── index.html          # → /shared-page/<새-페이지>/
```

각 페이지는 자기 폴더 안의 `index.html`로 자체 완결된 단일 HTML. URL 끝에 `.html`이 붙지 않아 깔끔.

## 페이지 추가하기

1. 새 폴더 생성 후 `index.html` 작성
   ```
   shared-page/<페이지-이름>/index.html
   ```

2. 루트 `index.html`의 `.grid` 안에 카드 추가
   ```html
   <a class="card" href="./<페이지-이름>/">
     <div class="card-emoji">🔧</div>
     <div class="card-title">제목</div>
     <div class="card-desc">설명</div>
     <div class="card-meta">메타정보</div>
   </a>
   ```

3. 커밋 & 푸시
   ```bash
   git add -A
   git commit -m "Add <페이지-이름>"
   git push
   ```

푸시 후 약 1분 내 자동 재배포.

## 배포 설정

GitHub Pages가 `main` 브랜치 루트에서 서빙하도록 설정됨 (Settings → Pages).
별도 빌드 단계 없음 — 정적 HTML/CSS/JS만 사용.
