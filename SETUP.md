# 우리 같이 운동해요 💪 — 배포 & 실시간 공유 가이드

`index.html` 하나면 앱은 완성이에요. 이 문서는 두 가지를 알려줘요.
1. **친구들과 실시간 공유** 켜기 (Firebase, 약 10분)
2. **링크로 공유** 하기 (GitHub Pages)

Firebase config를 안 넣으면 데이터는 이 브라우저에만 저장돼요(혼자 쓰기엔 이대로도 충분).

---

## 1) 실시간 공유 켜기 — Firebase (무료)

### ① 프로젝트 만들기
1. https://console.firebase.google.com 접속 → **프로젝트 추가**
2. 이름 아무거나 (예: `workout-us`) → 애널리틱스는 꺼도 됨 → 생성

### ② 웹 앱 등록 & config 복사
1. 프로젝트 홈에서 **`</>` (웹)** 아이콘 클릭
2. 앱 닉네임 입력 → 등록
3. 화면에 뜨는 `firebaseConfig` 객체 복사 (apiKey, authDomain, projectId … 6줄)

### ③ Firestore 만들기
1. 왼쪽 메뉴 **빌드 → Firestore Database → 데이터베이스 만들기**
2. 위치 아무거나 → **테스트 모드로 시작** 선택 (아래 ⑤에서 규칙 고정)

### ④ config 붙여넣기
`index.html` 을 열고 `FIREBASE_CONFIG` 부분을 복사한 값으로 교체:

```js
const FIREBASE_CONFIG = {
  apiKey: "AIza…",
  authDomain: "workout-us.firebaseapp.com",
  projectId: "workout-us",
  storageBucket: "workout-us.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234:web:abcd"
};
```

> 이 값들은 브라우저에 노출돼도 되는 공개 키예요(Firebase 설계상 정상).

저장하고 새로고침 → 상단에 🟢 **"실시간 공유 중"** 뜨면 성공!

### ⑤ (권장) 보안 규칙
테스트 모드는 30일 뒤 잠겨요. Firestore → **규칙** 탭에 아래를 붙여두면 이 앱 데이터만 읽고 쓰게 열려요:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /tracker/main {
      allow read, write: if true;
    }
  }
}
```

> 친한 셋만 링크로 쓰는 용도라 이 정도면 충분해요. 더 잠그고 싶으면 나중에 로그인(Auth)을 붙이면 됩니다.

---

## 2) 링크로 공유 — GitHub Pages

1. GitHub에 이 폴더를 **public** 저장소로 올려요.
2. 저장소 **Settings → Pages → Branch: main / (root)** 선택 → Save
3. 몇 분 뒤 `https://<아이디>.github.io/<레포>/workout/` 링크 생성
4. 그 링크를 자몽이·클레어·케밥에게 공유 🎉

Firebase config를 넣은 채로 올리면, 셋이 각자 폰에서 링크만 열어도 서로의 인증이 실시간으로 보여요.

---

## 데이터 구조 (참고)
Firestore `tracker/main` 문서에 이렇게 저장돼요:
```
{ days: { "2026-08-04": { "자몽이": true, "케밥": true } , ... } }
```
