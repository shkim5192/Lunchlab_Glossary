# 사내 용어집 — 설정 가이드

## 파일 구조
```
/
├── index.html      ← 메인 앱 (SHEET_ID만 수정)
├── manifest.json   ← PWA 설정
├── CNAME           ← 커스텀 도메인
└── README.md
```

---

## 1. Google Sheets 준비

### 스프레드시트 구조
```
[_categories]  ← 카테고리 목록 전용 탭 (앱이 여기서 탭 목록을 읽음)
[경영]         ← 카테고리별 탭
[마케팅]
[개발]
...
```

### _categories 탭 구조
| A열 |
|-----|
| 카테고리 (헤더 — 건너뜀) |
| 경영 |
| 마케팅 |
| 개발 |
| HR |

- 카테고리 **추가**: A열에 행 추가 → 새로고침으로 즉시 반영 ✅
- 카테고리 **삭제**: 해당 행만 제거 → 즉시 반영 ✅
- **순서 변경**: 행 순서 = 사이드바 순서 ✅
- **GitHub 재배포 불필요** ✅

### 용어 시트 구조 (카테고리 탭마다 동일)
| A열 (용어명) | B열 (영문명) | C열 (용어설명) | D열 (사용 예시) |
|------------|------------|--------------|--------------|
| KPI | Key Performance Indicator | 핵심 성과 지표... | 이번 분기 KPI는... |

- **1행은 헤더** (자동 건너뜀)
- **2행부터** 실 데이터

### 공개 배포 설정
1. 스프레드시트 메뉴 → **파일 → 공유 → 웹에 게시**
2. "전체 문서" + "쉼표로 구분된 값(.csv)" → **게시**
3. URL에서 `/d/` 뒤 긴 문자열 = **SHEET_ID**

---

## 2. index.html 수정 (딱 1곳)

```js
const SHEET_ID = 'YOUR_SPREADSHEET_ID';  // ← 실제 ID로 교체
```

카테고리 목록은 `_categories` 시트에서 자동으로 읽어옵니다.

---

## 3. GitHub Pages 배포

```bash
git init
git add .
git commit -m "init"
git remote add origin https://github.com/YOUR_ORG/glossary.git
git push -u origin main
# Settings → Pages → Branch: main / (root) → Save
```

---

## 4. Route 53 커스텀 도메인

### CNAME 파일 수정
```
glossary.yourcompany.com
```

### Route 53 레코드
| 필드 | 값 |
|------|-----|
| 유형 | CNAME |
| 이름 | glossary |
| 값 | `YOUR_ORG.github.io` |
| TTL | 300 |

### GitHub Pages 설정
Settings → Pages → Custom domain → `glossary.yourcompany.com` → Enforce HTTPS 체크

---

## 5. 운영 치트시트

| 작업 | 방법 | 재배포 필요? |
|------|------|------------|
| 용어 추가/수정/삭제 | 해당 시트 탭에서 편집 | ❌ 새로고침만 |
| 카테고리 추가 | `_categories` 탭에 행 추가 + 새 시트 탭 생성 | ❌ 새로고침만 |
| 카테고리 삭제 | `_categories` 탭에서 행 제거 | ❌ 새로고침만 |
| 카테고리 순서 변경 | `_categories` 탭에서 행 순서 변경 | ❌ 새로고침만 |
| 도메인 변경 | CNAME 파일 수정 | ✅ push 필요 |

---

## 6. PWA 홈화면 설치

- **Android**: Chrome → 주소창 메뉴 → "앱 설치"
- **iOS**: Safari → 공유 버튼 → "홈 화면에 추가"
- **PC**: Chrome 주소창 우측 설치 아이콘
