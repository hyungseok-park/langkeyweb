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
ASSET_DIR=/private/tmp/langkeyweb-assets
rm -rf "$ASSET_DIR"
mkdir -p "$ASSET_DIR"
cp index.html _headers apple-touch-icon.png favicon-16x16.png favicon-32x32.png "$ASSET_DIR"/
npx wrangler deploy --name langkeyweb --assets "$ASSET_DIR" --compatibility-date 2026-05-17 --old-asset-ttl 0
```

repo 루트를 `--assets .`로 직접 배포하지 않는다. `.git/`나 `.wrangler/` 같은 로컬 상태 파일이 asset으로 올라갈 수 있다.

## 관련 저장소

- [app-langkey](https://github.com/hyungseok-park/langkey) — macOS 앱 본체
