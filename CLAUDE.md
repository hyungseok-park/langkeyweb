# web-langkey — CLAUDE.md

## 프로젝트 개요

LangKey macOS 앱의 Cloudflare Pages 배포용 단일 파일 랜딩 페이지.
파일 구성: `index.html` (전체 페이지), `_headers` (Cloudflare 보안/캐시 헤더).

## 다국어(i18n) 구조

### 지원 언어 목록

| 키 | 언어 | 네비 표시 |
|----|------|-----------|
| `en` | English | 🇺🇸 EN |
| `ko` | 한국어 | 🇰🇷 한국어 |
| `ja` | 日本語 | 🇯🇵 日本語 |
| `zh-hans` | 简体中文 | 🇨🇳 简体 |
| `zh-hant` | 繁體中文 | 🇨🇳 繁體 |

### 언어 추가 시 체크리스트

새 언어를 추가할 때 반드시 수정해야 할 위치:

1. **`langMeta` 객체** — `{ flag, code, html }` 항목 추가
2. **`i18n` 객체** — 해당 언어 키로 모든 문자열 번역 추가 (기존 `en` 항목을 복사 후 번역)
3. **네비게이션 드롭다운** — `<div class="lang-option" data-lang="..." onclick="setLang(...)">` 항목 추가
4. **`detectLang()` 함수** — `navigator.language` 감지 규칙 추가
5. **`mockupData` 객체** — 히어로 목업 매핑 추가 (아래 규칙 참고)

### 히어로 목업 언어 매핑 (`mockupData`)

목업은 "자국 언어가 Right Shift에 오는" 규칙을 따른다.
Left Shift는 항상 🇺🇸 English(ABC) 고정.

현재 매핑:

| 언어 선택 | Right Shift | Left Control | Right Control |
|-----------|-------------|--------------|---------------|
| en / ko   | 🇰🇷 한국어 / 두벌식 | 🇯🇵 日本語 / Romaji | 🇨🇳 中文 / Pinyin |
| ja        | 🇯🇵 日本語 / Romaji | 🇰🇷 한국어 / 두벌식 | 🇨🇳 中文 / Pinyin |
| zh-hans   | 🇨🇳 中文 / Pinyin   | 🇯🇵 日本語 / Romaji | 🇰🇷 한국어 / 두벌식 |
| zh-hant   | 🇨🇳 繁體中文 / Zhuyin | 🇯🇵 日本語 / Romaji | 🇰🇷 한국어 / 두벌식 |

**입력 방식 서브텍스트 기준 — macOS가 자국어로 표기하는 이름 사용:**
- 한국어: 두벌식
- 日本語: ローマ字 (PC 사용자 대다수, かな 직접 입력은 소수)
- 中文 간체: 拼音
- 繁體中文 번체: 注音 (대만 표준 주음부호)

**새 언어 추가 규칙:**
- `rs`: 자국 언어를 Right Shift에 배치
- `lc` / `rc`: 나머지 언어들을 Left Control / Right Control에 배치 (순서는 기존 패턴 참고)
- `current` / `menubar` / `overlay`: 해당 자국 언어 표시

`mockupData` 항목 구조:
```js
'언어키': {
  current: '🏳️ 언어명',       // 팝업 헤더 현재 언어
  menubar: '🏳️ ⌨️',           // 메뉴바 아이콘
  rs: ['🏳️', '언어명', '서브텍스트'],  // Right Shift
  lc: ['🏳️', '언어명', '서브텍스트'],  // Left Control
  rc: ['🏳️', '언어명', '서브텍스트'],  // Right Control
  overlay: ['🏳️', '언어명'],   // 플로팅 오버레이 배지
  sampleText: '문장 예시',      // How It Works Step 3 샘플 텍스트
  addLang: '언어 추가...',      // 팝업 하단 버튼 (Localizable.xcstrings에서 가져올 것)
  quit: 'LangKey 종료',         // 팝업 하단 버튼 (Localizable.xcstrings에서 가져올 것)
}
```

### Problem Demo 언어별 예제 (`demoExamples`)

언어를 변경하면 Problem Demo 애니메이션도 해당 언어의 잘못 입력 예제로 바뀐다.

| 언어 | 잘못 입력된 것 (wrong) | 원래 의도 (right) | 비고 |
|------|----------------------|-------------------|------|
| en / ko | `sksms` | `나는` | 한글 자모가 영문 키에 대응 |
| ja | `watashi` | `わたし` | 일본어 IME는 로마자 입력 → 히라가나 변환. 영어 모드에선 로마자가 그대로 남음 |
| zh-hans | `wo shi` | `我是` | 병음(pinyin) 입력 → 간체 한자 변환. 영어 모드에선 병음이 그대로 남음 |
| zh-hant | `xue xi` | `學習` | 병음 입력 → 번체 한자 변환. 학(學 vs 学)으로 간·번체 차이가 명확히 드러남 |

**새 언어 추가 시** `demoExamples` 객체에 항목 추가:
```js
'언어키': { wrong: '잘못입력', right: '올바른문자', flag: '🏳️', name: '언어명', intended: '올바른문자' }
```
- `wrong`: 영어 모드에서 실수로 입력되는 텍스트 (로마자/자모 등)
- `right`: 올바른 언어로 입력했을 때의 결과
- `intended` = `right`와 같은 값 (왼쪽 패널 "wanted this" 표시용)

## 출시 전 TODO

- [ ] `hero_cta` Download Free 버튼 복원 (현재 Coming Soon으로 대체됨)
- [ ] 가격 섹션 복원 (현재 Coming Soon 카드로 대체됨)
- [ ] `LicenseConfig.swift`의 `checkoutURL` 확정 후 "Buy License" href 업데이트
- [ ] 앱 다운로드 링크 확정 후 Download CTA href 업데이트

## 배포

```bash
npx wrangler pages deploy .
```

또는 GitHub 저장소를 Cloudflare Pages 대시보드에 연결해 자동 배포.
