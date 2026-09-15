# SDK와 API 확인

한국어 | [English](en/compatibility.md) | [日本語](ja/compatibility.md)

2026-09-15 확인 시 Apple의 [Duo 안내](https://developer.apple.com/iphone-duo/)는 Xcode 27.1 beta를 이달 말 제공 예정으로 표시했다. 이 날짜는 기록이며, 실행 시점에 공식 안내와 설치된 SDK를 다시 확인한다.

## 세 가지를 따로 확인

| 확인 대상 | 근거 | 처리 |
|---|---|---|
| 컴파일 가능 여부 | 선택한 SDK의 header/Swift interface, 실제 빌드 | 선언이 없으면 해당 심벌을 빌드 소스에 추가하지 않는다 |
| 실행 OS 지원 여부 | 선언의 availability, 배포 대상 | 확인한 버전으로 `#available` 또는 `@available`와 fallback을 구성한다 |
| 현재 장치·scene의 기능 | runtime capability, 실제 연결 상태 | 값이 바뀌거나 지원하지 않는 경우를 처리한다 |

`if #available(iOS 27.1, *)`만으로 구 SDK에 없는 타입의 컴파일 오류를 막을 수 없다. `#if canImport(SwiftUI)`도 SwiftUI 내부의 새 타입 존재 여부를 검사하지 않는다. Swift 컴파일러 버전을 iOS SDK 버전 대신 사용하지 않는다.

공식 문서 → 공식 세션 코드 → 로컬 SDK 선언 → 최소 빌드 순으로 필요한 근거를 모은다. 검색 실패는 API가 없다는 증거가 아니다. 반대로 세션에 이름이 있다고 설치된 SDK가 이를 제공하는 것도 아니다. 이름·모듈·인자·availability를 추측하지 않는다.

## 미확인 심벌의 처리

`ArrangementView`, `UIArrangementViewController`, `reservedRegions`, `onHingeChange`, `UIHingeInteraction`, `axisBehavior`, `toolbarVerticalBehavior`, `CameraCaptureAccessory`, `AVCaptureDeviceDirectionCoordinator` 등은 원본과 발표 자료에서 다루는 **확인 대상**이다. 이 목록을 컴파일 가능한 선언 목록으로 취급하지 않는다.

SDK가 부족하면 기존 컨테이너의 리사이즈, safe area, 상태 유지 개선을 먼저 완료한다. 미확인 신기능은 설계 메모에 남기고 unresolved symbol, 가짜 stub, private API로 채우지 않는다. SDK 갱신이 필요한 경우 필요한 버전과 재검증 항목을 보고한다.

빌드는 프로젝트의 기존 명령을 따른다. Tuist 프로젝트는 생성된 프로젝트보다 원본 manifest를 수정한다. 실제 scheme과 destination을 조회해서 사용하고 서명·계정 설정을 임의 변경하지 않는다.
