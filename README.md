# Svelte Todo

Svelte로 간단한 Todo CRUD 흐름을 연습한 프로젝트입니다.

입력한 Todo를 추가하고, 수정하고, 삭제하는 기본 기능을 구현했습니다.  
복잡한 기능을 많이 넣기보다 Svelte의 데이터 바인딩, 이벤트 처리, 조건문, 반복문, store 사용 흐름을 익히는 데 초점을 둔 예제입니다.

---

## 바로 보기
[`바로가기`](https://heodokyung.github.io/svelte-todo/)

---

## 학습 포인트

- Svelte 컴포넌트 기본 구조
- `bind:value`를 이용한 입력값 처리
- `on:click`, `on:keydown` 이벤트 처리
- `{#each}` 반복문으로 Todo 목록 출력
- `{#if}` 조건문으로 수정 모드 전환
- `writable` store를 이용한 상태 관리
- Rollup 기반 Svelte 프로젝트의 빌드와 배포 흐름

---

## 프로젝트 구조

```txt
svelte-todo/
├─ public/
│  ├─ index.html
│  └─ global.css
├─ src/
│  ├─ App.svelte
│  ├─ Todo.svelte
│  └─ main.js
├─ rollup.config.js
├─ package.json
└─ package-lock.json
```

---

## 실행 방법

의존성을 설치합니다.

```bash
npm install
```

개발 서버를 실행합니다.

```bash
npm run dev
```

브라우저에서 아래 주소로 접속합니다.

```txt
http://localhost:8080
```

---

## 빌드

```bash
npm run build
```

빌드 결과는 `public/build/` 폴더에 생성됩니다.

```txt
public/
├─ index.html
├─ global.css
└─ build/
   ├─ bundle.js
   └─ bundle.css
```

---

## GitHub Pages 배포

이 프로젝트는 예전 Svelte Rollup 템플릿 구조입니다.  
React CRA처럼 `package.json`의 `homepage` 값으로 경로가 자동 조정되지 않습니다.

대신 `public/index.html`에서 정적 파일 경로를 상대경로로 작성했습니다.

```html
<link rel="stylesheet" href="./global.css" />
<link rel="stylesheet" href="./build/bundle.css" />
<script defer src="./build/bundle.js"></script>
```

그래서 GitHub Pages처럼 하위 경로에 배포해도 파일을 안정적으로 찾을 수 있습니다.

배포는 `.github/workflows/deploy-pages.yml`에서 처리합니다.

GitHub 저장소에서 아래 설정을 확인하세요.

```txt
Settings
→ Pages
→ Build and deployment
→ Source: GitHub Actions
```

그다음 `main` 브랜치에 push하면 자동으로 빌드 후 GitHub Pages에 배포됩니다.

---

## 메모

이 프로젝트는 최신 SvelteKit 프로젝트가 아니라 Rollup 기반의 Svelte 학습 예제입니다.  
새 프로젝트를 만든다면 SvelteKit 또는 Vite 기반 Svelte 템플릿을 사용하는 편이 좋지만, 이 저장소는 예전에 학습한 코드 흐름을 보존하는 데 의미를 둡니다.
