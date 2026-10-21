# 모아 플레이어 · iPhone

Mac 없이 GitHub의 macOS 환경에서 빌드하고, 개인 Apple 계정으로 서명해 설치하는 iOS 앱 소스입니다. iOS 17 이상을 대상으로 합니다.

## 구현된 기능

- 유튜브 검색, 검색 기록 드롭다운, 개별/전체 기록 삭제
- 검색 영상 클릭 시 온라인 재생, 별도 음원 M4A / 영상 MP4 다운로드
- URL 입력으로 음원/영상 선택 저장
- 아이폰 Documents에 보관한 파일의 오프라인 재생, 파일/폴더 가져오기, 삭제
- 재생/일시정지, 이전/다음, 10초 이동, 반복, 위치 이동
- 영상 화면 탭으로 재생/일시정지, 전체화면, 목록과 재생 화면 전환
- 백그라운드 오디오 및 잠금 화면 재생 제어
- 10가지 테마 및 선택값 유지

## 현재 검증 상태

Windows에서 Swift 구문, YAML/JSON, yt-dlp와 EJS 버전 일치 여부를 검사했습니다. **Xcode 빌드 성공과 실제 아이폰 동작은 별도 확인이 필요합니다.** GitHub Actions 실행 결과가 IPA 생성 성공 여부의 기준입니다.

YouTube의 제공 형식과 접근 제한에 따라 일부 영상은 처리되지 않을 수 있습니다. 영상은 아이폰에서 바로 재생 가능한 H.264 MP4 단일 스트림, 음원은 M4A를 선택합니다. 별도 영상/음원 트랙 병합은 포함하지 않습니다. 다운로드는 진행 중 앱을 화면에 유지하는 것이 좋습니다. 저장이 완료된 파일의 오프라인 재생에는 서버나 PC가 필요하지 않습니다.

## 저장소 구성

`MoaPlayer-iOS-source.zip`은 전체 소스, 고정된 라이브러리 참조, 런타임 리소스, 라이선스를 포함합니다. 브라우저로 업로드하면서 폴더 구조를 유지하기 위한 소스 묶음이며 실행파일이 아닙니다. 빌드 작업은 이 파일을 풀어 프로젝트를 생성합니다. 사용자 음악, 검색 기록, PC 설정과 비밀키는 포함하지 않습니다.

## 빌드

GitHub Actions → **Build iPhone IPA** → **Run workflow**. 성공하면 **MoaPlayer-iPhone** 첨부 결과에서 `MoaPlayer-iPhone-unsigned.ipa`를 받습니다. 파일이 생성되기 전에는 설치 파일이 준비되었다고 볼 수 없습니다.

로컬 Mac에서는 소스 압축을 푼 뒤 Xcode 26 이상과 XcodeGen으로:

```sh
xcodegen generate
xcodebuild -project Moa.xcodeproj -scheme Moa -configuration Release -sdk iphoneos -destination 'generic/platform=iOS' -derivedDataPath build CODE_SIGNING_ALLOWED=NO build
```

## 무료 설치

IPA는 아이폰에서 파일을 누르는 것만으로 설치되지 않습니다. 무료 Apple 개인 계정과 SideStore 같은 서명 도구가 필요합니다.

1. Windows에서 SideStore 공식 설치 문서에 따라 최초 USB 연결, iPhone 신뢰, iloader로 SideStore 설치를 진행합니다.
2. 아이폰 개발자 모드, 개발자 앱 신뢰, LocalDevVPN 및 SideStore를 설정합니다.
3. SideStore에서 빌드된 IPA를 선택해 서명·설치합니다.
4. 무료 개인 계정의 서명은 7일마다 갱신해야 합니다. 갱신에는 인터넷과 Wi-Fi가 필요합니다. iOS 업데이트 등으로 페어링이 풀리면 PC 재연결이 필요할 수 있습니다.

설치 완료 후 매번 PC와 연결할 필요는 없지만, 무료 서명을 인터넷 없이 영구 유지할 수는 없습니다. Apple 계정 비밀번호나 인증 코드를 채팅 또는 이 저장소에 넣지 마세요.

공식 안내: [SideStore 설치 조건](https://docs.sidestore.io/docs/installation/prerequisites), [설치](https://docs.sidestore.io/docs/installation/install), [Apple 계정](https://developer.apple.com/help/account/basics/about-your-developer-account).

## 오픈소스

JavaScriptCore 브리지 3개 파일과 EJS 리소스는 [FreeTube](https://github.com/leshkodev/freetube) 커밋 `8fc6d3b7be9571e3ffcb179de3d1de2dfe7012e7`에서 가져왔으며 MIT 고지를 유지했습니다. 나머지 모아 화면, 저장 및 재생 코드는 이 앱용으로 작성했습니다.

- [YouTubeKit](https://github.com/b5i/YouTubeKit) — 검색
- [Python-iOS](https://github.com/kewlbear/Python-iOS) / [PythonKit](https://github.com/pvieito/PythonKit) — 아이폰 내 Python 실행
- [yt-dlp](https://github.com/yt-dlp/yt-dlp), nightly `2026.10.10.233141` — 메타데이터와 스트림 해석
- [yt-dlp/ejs](https://github.com/yt-dlp/ejs), 0.8.0 — JavaScript 해석 리소스
- [certifi](https://github.com/certifi/python-certifi) — HTTPS 루트 인증서

각 라이선스는 `Licenses/`에 포함됩니다. 내려받은 콘텐츠의 권리는 원 권리자에게 있으며, 본인 소유 또는 다운로드가 허용된 콘텐츠를 이용하세요.
