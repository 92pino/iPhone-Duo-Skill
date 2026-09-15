---
name: iphone-duo
description: iPhone Duo 대응을 위한 SwiftUI·UIKit 앱의 적응형 레이아웃, 세로 바, 힌지, 다중 scene 및 카메라 전환을 점검하고 구현한다. Duo 대응 요청이나 접고 펼칠 때 발생하는 UI·상태·카메라 문제에 사용한다. 일반적인 iOS 작업에 단순히 화면 회전이 언급된 것만으로 적용하지 않는다.
---

# iPhone Duo

한국어 | [English](SKILL.en.md) | [日本語](SKILL.ja.md)

기존 앱의 구조와 지원 OS를 존중하면서 크기·자세·디스플레이 변화에 적응시킨다. 설명과 결과는 사용자의 언어로 작성하고 API 식별자는 원문을 유지한다.

가이드는 한 언어만 읽고 해당 언어의 필요한 참조만 추가로 읽는다. 세 언어는 함께 갱신하며 번역본을 별도 스킬로 등록하지 않는다.

For English instructions, read [SKILL.en.md](SKILL.en.md). 日本語の手順は [SKILL.ja.md](SKILL.ja.md) を参照する。

## 작업 시작

1. 프로젝트 지침과 변경 상태를 읽고 SwiftUI/UIKit, 배포 대상, 빌드 방식, 관련 화면과 상태 소유자를 파악한다. 검토 요청이면 발견 사항을 보고하고, 구현 요청이면 수정과 검증까지 진행한다.
2. `xcodebuild -version`, `xcodebuild -showsdks`로 실제 도구를 확인한다. 시뮬레이터 검증이 필요하면 `xcrun simctl list devices available`로 대상을 찾는다. 존재하지 않는 Duo runtime이나 destination을 가정하지 않는다.
3. 새 API를 넣기 전에 [SDK 확인 절차](references/compatibility.md)를 읽는다. 발표 자료에 등장하는 API와 현재 SDK에서 컴파일되는 API를 구분한다.
4. 요청한 기능에 필요한 참조만 읽는다. 카메라나 다중 창을 사용하지 않는 앱에 해당 기능을 새로 추가하지 않는다.

| 작업 | 참조 |
|---|---|
| 화면 폭, safe area, 접힘 영역, split/overlay | [레이아웃](references/layout.md) |
| 내비게이션, 세로 바, overflow | [바](references/bars.md) |
| 힌지 효과, 상태 유지, 창·디스플레이 | [Scene](references/scenes.md) |
| 카메라 선택·전환·프리뷰 | [카메라](references/camera.md) |
| 기존 앱 마이그레이션 및 완료 판정 | [검증 체크리스트](references/checklist.md) |
| 원본 버전, 공식 링크, 확인 범위 | [출처](references/sources.md) |

## 구현 원칙

- 기기 모델명이나 `.phone`/`.pad`로 공간을 추정하지 않는다. 현재 컨테이너의 크기와 size class를 사용한다. size class만으로 부족한 최소 콘텐츠 폭은 실제 가용 폭으로 판단한다.
- `UIScreen.main.bounds`로 UI 크기를 계산하는 코드는 해당 view/scene 기준으로 바꾼다. 화면이 꼭 필요한 경우 연결된 window scene에서 얻는다.
- safe area와 margins의 네 변을 각각 취급한다. 배경 확장과 조작 요소의 배치를 분리한다.
- 표준 navigation container와 시스템 바를 우선 사용한다. 기존 커스텀 디자인의 목적을 확인하고 필요한 부분만 조정한다.
- 크기 변화로 navigation path, 선택 항목, 편집 중인 내용, 재생 위치를 초기화하지 않는다. 공유 도메인 데이터와 창별 UI 상태를 구분한다.
- 기존 배포 대상을 임의로 올리지 않는다. 이용할 수 없는 API는 기존 레이아웃으로 대체하고, 새로운 기능은 SDK에서 확인한 뒤 통합한다.

## 완료 보고

변경한 화면/파일과 사용자에게 달라지는 동작, 실행한 빌드·테스트 및 결과, 확인하지 못한 장치별 동작을 요약한다. 정적 검토나 일반 iPhone 시뮬레이터 테스트를 Duo 검증 통과로 표현하지 않는다. 구현 요청에서는 가능한 수정은 마치고, 환경 때문에 보류한 부분만 구체적으로 남긴다.
