# LangKey — Landing Page

LangKey macOS 앱의 랜딩 페이지. Cloudflare Pages로 배포되는 단일 파일 정적 사이트.

## 구조

```
index.html   — 전체 페이지 (HTML/CSS/JS 단일 파일)
_headers     — Cloudflare 보안/캐시 헤더
```

## 지원 언어

영어(EN) · 한국어 · 日本語 · 简体中文 · 繁體中文  
`navigator.language` 자동 감지, 네비게이션 드롭다운으로 수동 전환 가능.

## 배포

```bash
npx wrangler pages deploy .
```

또는 GitHub 저장소를 Cloudflare Pages 대시보드에 연결해 자동 배포.

## 관련 저장소

- [app-langkey](https://github.com/hyungseok-park/langkey) — macOS 앱 본체
