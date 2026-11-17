# Ondap_app_senior

## 폴더 구조

```text
Ondap_app_senior/
├─ android/                       # Kotlin · Jetpack Compose (Gradle 멀티 모듈)
│  ├─ app/                        # 앱 진입·내비게이션·의존성 주입
│  ├─ core/
│  │  ├─ network/                 # Ondap_api 호출 (Ondap_api contracts/openapi에서 생성)
│  │  ├─ data/                    # 저장소·로컬 캐시·로그인 토큰 보관
│  │  ├─ audio/                   # 녹음·재생 (Ondap_api contracts/audio 규격)
│  │  ├─ notifications/           # FCM 푸시 수신
│  │  ├─ designsystem/            # 큰 글씨·고대비·큰 터치 영역 공통 UI
│  │  └─ testing/                 # 테스트 공용 도구·가짜 데이터
│  └─ feature/
│     ├─ onboarding/              # 초대 코드로 가족과 연결·녹음 보관 동의
│     ├─ home/                    # 새 답장 알림·녹음 버튼 (첫 화면)
│     ├─ record/                  # 이야기 녹음·업로드·재시도
│     ├─ replies/                 # 가족 음성 답장 듣기·다시 듣기
│     └─ settings/                # 글씨 크기·알림·연결된 가족 확인
├─ ios/                           # Swift · SwiftUI (Xcode 프로젝트)
│  ├─ OndapSenior/                # 앱 타깃
│  │  ├─ App/                     # 앱 진입·내비게이션·의존성 주입
│  │  ├─ Core/
│  │  │  ├─ Network/              # Ondap_api 호출 (Ondap_api contracts/openapi에서 생성)
│  │  │  ├─ Data/                 # 저장소·로컬 캐시·키체인 토큰 보관
│  │  │  ├─ Audio/                # 녹음·재생 (Ondap_api contracts/audio 규격)
│  │  │  ├─ Notifications/        # APNs 푸시 수신
│  │  │  └─ DesignSystem/         # 큰 글씨·고대비·큰 터치 영역 공통 UI
│  │  ├─ Features/
│  │  │  ├─ Onboarding/           # 초대 코드로 가족과 연결·녹음 보관 동의
│  │  │  ├─ Home/                 # 새 답장 알림·녹음 버튼 (첫 화면)
│  │  │  ├─ Record/               # 이야기 녹음·업로드·재시도
│  │  │  ├─ Replies/              # 가족 음성 답장 듣기·다시 듣기
│  │  │  └─ Settings/             # 글씨 크기·알림·연결된 가족 확인
│  │  └─ Resources/               # 이미지·문구·음성 안내
│  ├─ OndapSeniorTests/           # 단위 테스트
│  └─ OndapSeniorUITests/         # 화면 시나리오 테스트
├─ docs/                          # 화면 설계·접근성 기준·배포 절차
└─ .github/workflows/             # Android·iOS 빌드·검사
```

## 필요해질 때 추가할 것

| 위치 | 내용 | 추가 시점 |
|---|---|---|
| `feature/`·`Features/` 기기 연결 | 스피커 기기와 함께 쓰는 설정 | 기기 파일럿 시작 |

## 온답 저장소 구성

| 저장소 | 역할 |
|---|---|
| [Ondap_app_senior](https://github.com/dimigo-Paradise/Ondap_app_senior) | 어르신 앱 (Android·iOS). 이야기 녹음, 가족 답장 듣기. **MVP의 어르신 쪽 접점** |
| [Ondap_app_carer](https://github.com/dimigo-Paradise/Ondap_app_carer) | 보호자 앱 (Android·iOS). 요약 확인, 답장 녹음·승인 |
| [Ondap_api](https://github.com/dimigo-Paradise/Ondap_api) | HTTP API 서버 (Python·FastAPI). **DB를 소유하는 유일한 저장소** |
| [Ondap_server](https://github.com/dimigo-Paradise/Ondap_server) | 백그라운드 작업 (Python·Celery). STT·마스킹·요약·오디오 변환. DB 직접 접근 없음 |
| [Ondap_embed](https://github.com/dimigo-Paradise/Ondap_embed) | 스피커 기기 펌웨어 (ESP-IDF). MVP 이후 |

```text
senior app ─┐
            ├─ HTTPS ─▶ Ondap_api ─▶ PostgreSQL · S3 · FCM/APNs
carer app  ─┘              │  ▲
                           ▼  │   작업 발행(Redis) ↓ / 결과 보고(내부 API) ↑
                       Ondap_server ─▶ STT · 요약 API
```
