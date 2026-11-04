# 에이전트 지침

## 응답 언어

사용자가 어떤 언어(영어 포함)로 입력하더라도 항상 한국어로 응답합니다.

## 프로젝트

의존성 없는 단일 파일 우주 탐사 퀴즈입니다: `index.html` (HTML + 인라인 `<style>` + 인라인 `<script>`). 빌드, 패키지 매니저, 테스트, 린터가 없습니다. 파일을 직접 열어 실행합니다(`file:///C:/KSW260502/GIT/index.html`). 서버나 외부 의존성/CDN을 추가하지 마세요.

## 아키텍처

- **데이터**: 스크립트 상단의 `quizData` 배열에 `{ question, choices[4], answer }` 형태의 문항 10개가 있으며, `answer`는 `choices`의 0부터 시작하는 인덱스입니다. 결과 이모지 기준은 `quizData.length`를 사용하고, HTML의 카운터 텍스트는 `renderQuestion()`이 덮어씁니다.
- **상태**: 모듈 수준 변수 `current`, `score`, `answered`. 흐름은 `renderQuestion()` → `handleAnswer()` → `nextQuestion()` → `showResult()`이며, `restartQuiz()`가 상태를 초기화하고 다시 렌더링합니다.
- **화면**: `#quizView`와 `#resultView`는 `.hidden` 클래스(`display: none !important`)로 전환합니다.
- **피드백**: 정답/오답 피드백은 CSS가 담당합니다. `.feedback.correct`(초록, `pulse`), `.feedback.wrong`(빨강, `shake`)이며, 선택지 테두리는 `--good` / `--bad`를 사용해 인라인으로 지정합니다.
- **진행률 바**: `#progressBar`의 너비를 인라인으로 설정하며, 렌더링 시(답한 문항 수 기준)와 답을 선택한 직후에 모두 갱신합니다.

## 규칙

- **테마**: 모든 색상은 `:root`의 CSS 변수에서 가져오고, `@media (prefers-color-scheme: dark)` 블록에서 재정의합니다. 새 UI는 이 변수(및 색조에는 `color-mix`)를 사용해 두 테마가 모두 유지되도록 하고, 하드코딩한 글자색은 양쪽 테마에서 확인하세요.
- **타이포그래피/레이아웃**: sans-serif 폰트 스택, 본문 15px, 좁은 단일 중앙 컬럼(`.quiz-shell`, 최대 540px).
- **접근성**: 선택지는 실제 `<button>`입니다. 포커스를 의도적으로 관리합니다. 답을 고르면 `#nextBtn`으로, 새 문제(첫 문제 제외)에서는 첫 번째 선택지로, 결과 화면에서는 `#restartBtn`으로 이동합니다. 흐름을 변경할 때 이를 유지하세요. 장식 요소(헤더 LED와 `MISSION CONTROL` 태그)는 `aria-hidden`이며, `prefers-reduced-motion`에서는 애니메이션을 끕니다.
- **언어**: 사용자에게 보이는 모든 텍스트는 한국어입니다.

## 변경 검증

통합 브라우저에서 페이지를 새로고침하고 전체 흐름을 확인합니다. 정답/오답 선택(점수, 진행률, 피드백), 10문항을 끝까지 진행해 결과 화면 확인, 다시 도전 동작을 점검하세요. 스타일을 수정했다면 라이트/다크 스킴을 모두 확인하세요.
