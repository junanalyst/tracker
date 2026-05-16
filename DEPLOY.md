# 🚀 TRACKER 배포 가이드

## 구조 요약

```
소스코드 (GitHub)          GitHub Actions             실제 배포
tracker.html           →   API 키 자동 주입       →   GitHub Pages
__GEMINI_API_KEY__         (Secret에서 가져옴)         실제 키가 들어간 HTML
.gitignore (키 차단)       .env는 올라가지 않음         https://xxx.github.io/tracker
```

---

## Step 1 — Gemini API 키 발급 (무료)

1. **aistudio.google.com** 접속 (Google 계정으로 로그인)
2. 상단 **"Get API key"** 클릭
3. **"Create API key"** 클릭
4. 생성된 키 복사 (`AIza...` 로 시작)

> 무료 한도: 분당 15회, 하루 1,500회 → 개인 앱으로 충분해요

---

## Step 2 — GitHub 저장소 만들기

1. github.com 접속 → 우상단 **"+"** → **"New repository"**
2. Repository name: `tracker`
3. **Public** 선택 (Pages 무료 사용)
4. **"Create repository"** 클릭

---

## Step 3 — 파일 업로드

아래 파일들을 GitHub에 업로드하세요:

```
✅ 올려야 할 파일          ❌ 절대 올리면 안 되는 파일
tracker.html              .env  (API 키 있음!)
.env.example
.gitignore
.github/workflows/deploy.yml
README.md
```

GitHub 저장소 페이지 → **"uploading an existing file"** 클릭 → 파일 드래그

---

## Step 4 — GitHub Secret 등록 (핵심 보안!)

> API 키는 여기에만 저장됩니다. 소스코드에는 절대 없어요.

1. 저장소 → **Settings** 탭
2. 좌측 메뉴 → **Secrets and variables** → **Actions**
3. **"New repository secret"** 클릭
4. Name: `GEMINI_API_KEY`
5. Secret: 복사해둔 Gemini API 키 붙여넣기
6. **"Add secret"** 클릭

---

## Step 5 — GitHub Pages 활성화

1. 저장소 → **Settings** 탭
2. 좌측 메뉴 → **Pages**
3. Source: **"GitHub Actions"** 선택

---

## Step 6 — 배포 실행

파일을 업로드하면 자동으로 GitHub Actions가 실행돼요.

Actions 탭에서 진행 상황 확인 가능 → 초록색 ✅ 뜨면 완료!

---

## 완성! 🎉

```
내 앱 주소: https://[내GitHub아이디].github.io/tracker
```

이 링크를 카카오톡으로 공유하면 누구나 스마트폰으로 접속 가능해요!

---

## 보안 구조 설명

| 항목 | 위치 | GitHub 공개 여부 |
|------|------|:---:|
| API 키 | GitHub Secret | ❌ 비공개 |
| `.env` | 로컬 컴퓨터만 | ❌ .gitignore로 차단 |
| `tracker.html` | 소스코드 (`__GEMINI_API_KEY__` 플레이스홀더) | ✅ 안전 |
| 빌드된 HTML | GitHub Pages | ✅ 배포됨 (키 주입됨) |

**핵심:** 소스코드에는 키가 없고, 빌드 시에만 Secret에서 주입됩니다.

---

## 문제 해결

**Actions 실패 시** → Actions 탭 → 빨간 ❌ 클릭 → 오류 메시지 확인

**API 오류 시** → aistudio.google.com에서 키 재발급 후 Secret 업데이트

**업데이트 방법** → tracker.html 수정 후 GitHub에 다시 업로드 → 자동 재배포
