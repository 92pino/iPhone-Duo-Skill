# 실제 변경 사항과 코드 점검

한국어 | [English](change-map.en.md) | [日本語](change-map.ja.md)

2026-09-15 공식 Markdown 본문에서 확인한 항목이다. 전체 변경 목록이 아니며 실행 시 해당 SDK/OS build의 상태를 다시 확인한다. 검색 결과는 후보일 뿐이다. 수정 전에 호출 경로와 실제 영향을 확인한다.

| 항목·공식 분류·번호 | 검색 대상 | 재현과 최소 수정 판단 |
|---|---|---|
| SwiftUI 탭 선택 / New Features / 164516837 | `TabView`, `selection`, `Tab`, `tag`, 저장된 탭 ID | 27 SDK 빌드에서 선택 탭을 로그인·권한·feature flag로 숨기거나 저장된 선택을 복원한다. 존재하고 표시 가능한 탭으로 선택을 정규화하며 화면 변경과 선택 갱신 사이에 유효하지 않은 상태가 노출되지 않게 한다. 딥링크·재시작·로그아웃 회귀 테스트 |
| Foundation URL 인코딩 / Resolved Issues / 161588649 | `URL(string:`, `URLWithString`, `addingPercentEncoding`, `removingPercentEncoding`, `%25` | 유효한 percent escape와 공백·비ASCII가 섞인 URL을 기존/대상 OS에서 비교한다. 기존 이중 인코딩 보정이 이제 잘못된 URL을 만드는지 확인한다. 경로·query 값을 따로 검증하며 전체 URL을 무조건 decode/encode하지 않는다 |
| App Intents 사진 스키마 / Known Issues / 181800016 | `AppEntity`, `.photos.asset` | 기존 conformer를 27 SDK로 빌드한다. 새 필수 프로퍼티는 실제 SDK 선언으로 확인하고 해당 availability 경계 안에서 구현한다. 기존 OS의 연동을 유지하고 새 스키마를 필요 없는 앱에 추가하지 않는다 |
| Swift dependency scanner / New Features / 136303612 | `module.modulemap`, `HEADER_SEARCH_PATHS`, 같은 모듈의 복수 배포 경로 | 같은 scan에 노출되는 Clang 모듈명 중복을 compiler diagnostic으로 확인한다. 하나의 의존성이 SPM/Pods/수동 framework에 중복 연결됐는지 조사한다. 중복 연결 또는 검색 경로를 최소 변경하고 SDK 파일·생성 캐시를 임의 편집하지 않는다 |

앞의 세 항목은 [iOS 27 릴리스 노트](https://developer.apple.com/documentation/ios-ipados-release-notes/ios-ipados-27-release-notes), 마지막은 [Xcode 27 릴리스 노트](https://developer.apple.com/documentation/xcode-release-notes/xcode-27-release-notes)에서 확인했다. 분류가 New Features여도 기존 코드의 호환성에 영향을 줄 수 있다. Resolved Issues는 OS에서 고친 항목이므로 불필요한 workaround를 새로 넣지 않는다.

## 후보 검색

프로젝트의 실제 소스 경로로 범위를 좁혀 실행한다. 첫 검색에서는 vendor/generated 코드를 제외하고, 의존성 원인일 때 해당 패키지를 별도로 읽는다.

```sh
rg -n --glob '*.{swift,m,mm,h}' 'TabView|URL\(string:|URLWithString|addingPercentEncoding|removingPercentEncoding|AppEntity|\.photos\.asset' .
rg --files --hidden -g '!.git/**' -g '*modulemap' -g '*.xcconfig' -g 'Package.resolved' -g 'Podfile.lock'
```

각 후보에 공식 항목 번호, 적용 SDK/OS, 파일·심벌, 재현 결과를 붙인다. 해당 API가 없으면 대상 외, 문서만 확인했으면 미실시로 남긴다. 이 문서의 수정·테스트 제안은 실무 판단이며 Apple의 필수 구현 요구사항 전체를 뜻하지 않는다.
