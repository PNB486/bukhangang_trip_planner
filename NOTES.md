# 북한강 당일치기 빠지 플래너

## 개요
별내동 출발 당일치기 · 빠지 + 식사 코스 플래너  
GitHub Pages 정적 사이트 + Firebase Firestore (투표/제안)

## 일정
| 시간 | 내용 |
|---|---|
| 09:00 | 별내동 출발 |
| 10:00~13:30 | 빠지 (액티비티) |
| 14:00~15:00 | 식사 |
| 16:00 | 별내동 귀가 |

## 로컬 편집 파일
`C:\Users\S\Desktop\bukhangang_mockup.html`

## 배포
```powershell
Copy-Item "C:\Users\S\Desktop\bukhangang_mockup.html" "C:\Users\S\AppData\Local\Temp\bk_repo\index.html" -Force
cd C:\Users\S\AppData\Local\Temp\bk_repo
git add index.html
git commit -m "변경 내용"
git push origin master
```

## Firebase
- 프로젝트: `bukhangang-planner`
- Firestore collections:
  - `votes/{placeId}` — `{ count: number }`
  - `suggestions/{autoId}` — `{ name, cat, region, note, votes, createdAt }`
- placeId: `badge_0`~`badge_4`, `food_0`~`food_5`

## ⚠️ Firestore 규칙 (콘솔에서 직접 설정 필요)
Firebase Console → Firestore → 규칙 탭:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /votes/{id} {
      allow read, write: if true;
    }
    match /suggestions/{id} {
      allow read, create: if true;
      allow update: if request.resource.data.diff(resource.data).affectedKeys().hasOnly(['votes']);
    }
  }
}
```

## 구현된 기능
- [x] Slate Dark + Gold 다크 테마 (Georgia serif 헤더, grain overlay)
- [x] 빠지/식사 탭 전환 (master-detail 레이아웃)
- [x] 상세 패널: 가격대, 이동시간, 운영시간, 시설, OSM 지도 embed
- [x] Firebase 👍 투표 (실시간, 취소 가능, 1인1표 localStorage)
- [x] 장소 제안 모달 + 제안 목록 패널 (제안에도 투표 가능)

## 데이터 목록
### 빠지 (5)
1. 가평빠지 클럽비발디 — 가평, ★4.8
2. 가평빠지 캠프통포레스트 — 가평 서락면, ★4.3
3. 가평빠지 레저친구 — 춘천 남산면, ★4.8 (통합형)
4. 가평빠지 리버패리스 — 춘천 남산면, ★4.7
5. 가평빠지 블랙수상레저 — 가평, ★5.0

### 식사 (6)
1. 춘천 명동 닭갈비 골목 — 춘천 명동
2. 유포리 막국수 — 춘천 신북읍
3. 샘밭 막국수 — 춘천 신북읍
4. 가평 잣두부마을 — 가평읍
5. 북한강 민물매운탕 — 가평 북한강변
6. 가평 경강막국수 — 가평읍
