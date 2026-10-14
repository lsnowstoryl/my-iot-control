# 🌐 SMART_SEAT Vercel 배포 & Supabase 클라우드 연동 가이드

더 이상 복잡한 **`.bat` 파일 실행, ngrok 터널링, 포트포워딩, PC IP 변경 문제 없이**, Vercel을 통해 전 세계 어디서든 24시간 접속 가능한 웹페이지를 배포하는 방법입니다.

---

## 🚀 1분 만에 Vercel에 무료 배포하기 (가장 쉬운 방법)

### 방법 A: Vercel 대시보드에서 드래그 앤 드롭 (가장 추천)
1. 브라우저에서 [vercel.com](https://vercel.com/) 에 접속하여 로그인합니다 (GitHub 계정 등으로 간편 로그인).
2. 대시보드 우측 상단의 **`Add New...`** ➔ **`Project`** 를 클릭합니다.
3. 내 컴퓨터의 `C:\Users\USER\Documents\unity\SV` 폴더를 통째로 Vercel 브라우저 화면의 **`Drop files here to deploy`** 영역으로 드래그 앤 드롭합니다.
4. **Deploy** 버튼을 누르면 약 10~20초 후 나만의 영구 HTTPS 주소가 발급됩니다!
   - 예: `https://smartseat-control.vercel.app/`
5. 발급된 주소로 스마트폰이나 PC에서 접속하면 끝!
   - 제어판: `https://[내-프로젝트].vercel.app/`
   - 승객용 예매 앱: `https://[내-프로젝트].vercel.app/app`

---

### 방법 B: GitHub 연동 자동 배포
1. GitHub에 새 저장소(Repository)를 만듭니다 (예: `smartseat-web`).
2. `unity/SV` 폴더의 파일들을 해당 저장소에 업로드합니다.
3. Vercel에서 **`Import Git Repository`**를 누르고 방금 만든 저장소를 선택한 뒤 **Deploy**를 클릭합니다.

---

## 🗄️ Supabase 데이터베이스 정보
본 웹앱과 Unity / ESP32는 동일한 Supabase DB를 공유합니다:
- **프로젝트 URL**: `https://daqmyuwjmqhrjwlomizo.supabase.co`
- **공개 키 (anon)**: `sb_publishable_Rt5hF5FjGJ9WAhm3rGKmBg_f7_T6yoG`
- **테이블**: `device_control` (ID: 1, `status`: "ON" / "OFF")

---

## 🎮 Unity 연동 확인 방법
1. Unity에서 `unity/SMART_SEAT` 프로젝트를 엽니다.
2. 유니티 상단 메뉴: **`Smart Seat` -> `Setup Monitoring Scene (FHD 중앙 확인 시스템 자동 구성)`** 클릭
3. 씬의 `SmartSeat_System` 게임오브젝트에 **`SupabaseBridge`** 컴포넌트가 자동으로 부착되어 있습니다.
4. 상단 **Play(▶)**를 누르면 유니티가 1초마다 Supabase의 상태를 감지하여:
   - 웹에서 **ON** 클릭 ➔ 유니티 좌석 잠금 해제 (초록색 점등)!
   - 웹에서 **OFF** 클릭 ➔ 유니티 좌석 잠금 복귀!

---

## 🤖 ESP32 펌웨어 연동 (선택 사항)
ESP32 아두이노 코드에서도 기존의 로컬 PC IP(`172.31.119.192`) 대신 Supabase REST API(`https://daqmyuwjmqhrjwlomizo.supabase.co/rest/v1/device_control?id=eq.1`)로 HTTPS GET 요청을 보내면, PC가 꺼져 있어도 보드가 직접 클라우드 상태를 읽어와 LED와 솔레노이드를 제어할 수 있습니다.
