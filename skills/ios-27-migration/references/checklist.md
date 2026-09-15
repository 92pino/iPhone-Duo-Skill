# 호환성 점검표

한국어 | [English](checklist.en.md) | [日本語](checklist.ja.md)

이 표는 실무 점검 범위이며, 각 항목이 iOS 27에서 변경되었다는 뜻은 아니다. 사용하는 기능과 실제 릴리스 노트에 맞게 선택한다.

| 영역 | 점검·수정 기준 |
|---|---|
| 빌드·의존성 | SDK 선언, compiler diagnostic, app/extension target, 패키지 지원 버전, lockfile, 링크 오류 |
| SwiftUI·UIKit | 탭 선택의 유효성, navigation 복원, sheet/popover anchor, safe area·키보드, 상태 유지 |
| 동시성·수명 | actor 격리, UI 갱신 위치, 취소·중복 task, foreground 복귀, observer 정리 |
| 저장·네트워크 | 기존 데이터 읽기/쓰기, 실패 시 복구, 오프라인, 인증 만료, 응답 오류 |
| 권한·미디어 | 권한 거부·제한, 카메라/오디오 interruption, 복귀, 실기기에서 저장 결과 확인 |
| 앱에 있는 시스템 연동 | deep link, 알림, background 작업, widget/extension, 테스트 환경의 결제·복원 |
| 접근성·기존 OS | 큰 글자, VoiceOver, RTL/긴 번역, 최소 지원 OS의 fallback |

SwiftUI를 사용하면 릴리스 노트에서 `TabView`와 `selection`을 찾아 SDK에 연결된 선택 제약을 확인한다. 숨겨지거나 사용할 수 없는 탭을 선택하는 경로가 있는지 조사하고, 실제 대상 버전에서 재현한 뒤 수정한다. 사용하지 않는 API를 이 예시 때문에 도입하지 않는다.

## 검증 조합

| 빌드와 실행 | 목적 |
|---|---|
| 기존 SDK + 기존 OS | 기존 실패와 정상 동작의 기준 |
| 기존 SDK + iOS 27, 기존 artifact가 있을 때 | OS 업데이트만의 영향 |
| iOS 27 SDK + iOS 27 | 재빌드 후 오류와 SDK 연동 동작 |
| iOS 27 SDK + 기존 지원 OS | 이전 OS에서의 회귀와 availability fallback |

실제 확보한 조합만 실행하고 나머지는 미실시로 기록한다. 접근성·카메라·background 동작은 빌드 통과만으로 확인되지 않는다. 개인정보가 포함된 로그는 보고서에서 제거한다.

## 공식 출처

- [iOS & iPadOS 27 릴리스 노트](https://developer.apple.com/documentation/ios-ipados-release-notes/ios-ipados-27-release-notes)
- [Xcode 27 릴리스 노트](https://developer.apple.com/documentation/xcode-release-notes/xcode-27-release-notes)
- [iOS & iPadOS 릴리스 노트 목록](https://developer.apple.com/documentation/ios-ipados-release-notes)

2026-09-15 공식 Markdown 본문에서 확인한 구체적인 변경은 [실제 변경 매핑](change-map.md)에 정리했다. 검색 요약의 beta 표기 대신 본문의 버전·항목 상태를 확인하고, 작업 시 실제 SDK/OS build와 다시 대조한다.
