# 의존성 호환성 점검

한국어 | [English](dependencies.en.md) | [日本語](dependencies.ja.md)

## 목록과 재현

1. `Package.swift`, `Package.resolved`, `Podfile`, `Podfile.lock`, `Cartfile.resolved`, Tuist manifest, 수동 framework/XCFramework 연결을 실제 프로젝트에서 찾는다. 없는 관리 도구는 도입하지 않는다.
2. 직접·간접 의존성별 현재 고정 버전, 소비 target, 소스/바이너리/매크로·plugin 구분, 요구 toolchain·최소 OS, 공급자의 공식 release note URL을 기록한다. 지원 선언과 로컬 검증 결과는 별도 칸으로 둔다.
3. 고정 버전을 유지한 기준 빌드로 첫 원인 diagnostic을 확보한다. package 다운로드/인증 실패는 컴파일 불호환이 아니다. 네트워크 실패 때문에 버전이나 lockfile을 변경하지 않는다.

## 유형별 확인

| 유형 | 확인·수정 |
|---|---|
| SPM 소스 | 선언된 tools version, platform 조건, product/target 연결, 실제 resolved 버전. 기존 resolve 절차를 사용하고 일괄 update하지 않는다 |
| CocoaPods | lockfile과 설치 상태, 앱이 사용하는 workspace, generated xcconfig 연결. 기존 Bundler 구성이면 그 환경에서 실행하고 Pod 업데이트는 원인 패키지로 한정 |
| 바이너리 XCFramework | device/simulator platform variant와 architecture를 함께 확인. 같은 arm64여도 platform이 다를 수 있다. Swift module/interface의 toolchain 호환, embed/link, transitive framework, resource bundle을 확인 |
| macro/build plugin | 호스트 macOS·toolchain·실행 권한과 앱 target 오류를 구분. plugin의 실제 diagnostic과 공급자의 지원 버전을 확인하고 검증 절차를 일괄 비활성화하지 않는다 |
| 중복 모듈 | [변경 매핑](change-map.md)의 136303612 항목에 따라 SPM/Pods/수동 연결과 header search path 중복을 확인 |

## 최소 변경과 검증

문제 재현 → 공급자 공식 수정 근거 → 최소 호환 버전 선택 → manifest/lockfile 차이 검토 → 소비 target 빌드 → 해당 기능 실행 순으로 진행한다. 전이 의존성 변경도 보고한다. 실제 지원 여부가 불명확하면 추정으로 “지원” 표시하지 않는다.

로컬 패키지 캐시·Pods 생성 파일을 수정해 끝내지 않는다. 업데이트가 최소 OS를 높이거나 큰 API 변경을 요구하면 기존 OS를 유지할 수 있는 버전·작은 adapter를 검토한다. 해결이 불가능하면 필요한 선택과 영향을 보고하고 다른 독립적인 수정은 완료한다.

simulator 빌드 성공을 실기기 링크·archive 성공으로 간주하지 않는다. binary 또는 embed 변경이면 가능한 device build/archive도 확인한다. 기존 서명·계정 설정을 바꾸지 않고 실행 못 한 항목은 남긴다. 전체 캐시 삭제는 첫 조치로 삼지 말고 필요하면 격리된 build directory로 캐시 영향부터 구별한다.

## 보고 양식

`패키지/기존→변경 버전 | 소비 target | 원인 diagnostic | 공급자 근거 | lockfile/전이 변경 | simulator/device/기능 결과 | 남은 제약`

이 절차는 일반적인 의존성 검증이다. iOS 27에 대한 공급자별 지원 현황은 실제 사용하는 버전을 조사해 채운다.
