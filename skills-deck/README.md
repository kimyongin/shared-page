# skills-deck

코딩 에이전트 스킬 시스템 비교용 카드 덱.

**Live**: https://kimyongin.github.io/shared-page/skills-deck/

## 목적

세 개의 유명 스킬 시스템(Matt Pocock / Superpowers / GStack)을 같은 포맷으로
나열해서, 각 시스템의 철학과 워크플로우를 한 화면에서 비교·탐색할 수 있게 한다.

원문(GitHub repo의 README, skills.md)을 그대로 옮기지 않고 *통일된 스토리 아크*로
재구성한 것이 이 덱의 핵심 가치다.

## 콘텐츠 컨셉

### 스토리 아크 — "문제 → 행동 → 결과"

각 스킬의 `tileSummary`(작은 카드)와 `heroSummary`(상세 카드)는 다음 패턴을 따른다.

> "~ 문제를 해결하기 위해 ~ 을 수행해서 ~ 렇게 해결한다."

스킬이 *무엇을 하는지*가 아니라 *무엇을 해결하는지*를 먼저 말한다.
"이 카드는 X를 한다"는 설명은 피한다.

### 한국어 스타일 가이드

- **`박아`, `때려박아` 같은 표현 금지** — 한국어에서 어색함
- **em-dash(`—`) 사용 금지** — 한국어 문서에 시각적으로 튐. 쉼표·줄바꿈·괄호로 대체
- **카테고리 개념 없음** — 모든 스킬을 단일 톤으로 통일 (이전엔 eng/prod 색 구분이 있었으나 제거)

### 콤보 카드의 출처

콤보(combo) 시나리오는 *원문에 명시된 워크플로우만* 사용한다. 추측·합성 금지.

- **Matt**: README 본문에 명시된 워크플로우 예시 + Iron Law 인용
- **Superpowers**: 공식 7-step 워크플로우 (brainstorming → worktree → planning → ...)
- **GStack**: skills.md의 풀 라이프사이클 파이프라인 (ideation → ... → canary)

각 콤보의 `flow[].why`는 미니 카드에 표시되는 *그 시나리오 맥락에서의 의미*를
적는다. 스킬의 일반 설명을 그대로 복사하지 않는다.

## 기술 컨셉

### 단일 HTML 파일

CSS·JS·데이터 모두 한 파일(`index.html`)에 인라인. 빌드 도구 없음.
GitHub Pages에 그대로 올라간다.

### JSON-driven 데이터

`DATASETS` 객체에 세 개 데이터셋(`matt` / `superpowers` / `gstack`)이 들어 있고,
렌더러는 모두 공유. 콘텐츠 수정은 데이터셋만 건드리면 끝.

각 스킬의 데이터 모델:
```js
{
  cmd, emoji, role, roleEn,
  tileSummary,    // 작은 카드 한 줄
  heroSummary,    // 상세 카드 hero 영역
  when[],         // 📌 언제 쓰나
  howCode[],      // 🎮 어떻게 쓰나 (코드 블록)
  howNote,        // (옵션) 코드 옆 설명
  what[],         // ⚡ 무엇을 하나 (블록 배열)
  output[],       // 🎁 결과물
  combos[],       // 관련 스킬 ID
  meta
}
```

### FLIP 애니메이션

작은 카드를 클릭하면 그 위치·크기에서 자라나서 모달이 된다.
`transform-origin: 0 0` + 비균등 `scale(sx, sy)` + `translate`로 계산.
(WAAPI는 초기 프레임 누락 문제로 폐기, CSS transition + reflow trick 사용)

### Markdown 내보내기

다운로드 아이콘 클릭 시 현재 데이터셋을 Markdown으로 변환해 클립보드에 복사.
`htmlToMd()`가 `<strong>` `<em>` `<code>` `<span class="pop">`을 마크다운 등가물로 변환.

콤보 화살표 규칙: JSON의 `arrows[]`는 *기본값(`→`)이 아닌 특수 화살표만* 명시.
숫자 매김(`1. 2. 3.`)이 기본 흐름을 대신하므로 MD 출력에선 기본 화살표는 출력하지 않는다.

## 수정 가이드

### 새 스킬 추가 시

1. `DATASETS.<dataset>.skills`에 키 추가, 위 데이터 모델 그대로 채우기
2. `tileSummary`/`heroSummary`는 *문제→행동→결과* 아크로
3. 다른 스킬과 톤·길이 비슷하게 (스캔할 때 균형 깨지면 안 됨)

### 새 콤보 추가 시

1. 원문에 명시적 근거가 있는지 먼저 확인. 없으면 추가하지 말 것
2. `flow[].why`는 그 콤보 맥락에서의 의미만 적기
3. 미니 카드 5장 이상이면 `wrap`으로 두 줄 표시됨 — OK

### 새 데이터셋 추가 시

1. 탭에 버튼 추가 (`<button class="tab" data-dataset="<name>">`)
2. `DATASETS.<name>` 키 추가 (`preTitle`/`lead`/`footer` + `skills`/`combos`)
3. 렌더러·모달은 그대로 동작 (이미 추상화됨)

## 데이터 정확도 주의

- Matt의 `Engineering`/`Productivity` 분류는 *공식 README* 기준이었음
- Superpowers의 `Workflow`/`Meta` 분류는 *내가 단순화*한 것 (공식은 4개 카테고리)
- GStack의 분류도 *내가 단순화*한 것 (공식은 11+ 카테고리)

→ 현재는 카테고리 개념을 모두 제거해서 위 차이는 무관하지만, 콘텐츠 정확도를 따질
때는 이 점을 기억할 것.
