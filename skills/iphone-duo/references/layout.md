# 레이아웃

한국어 | [English](en/layout.md) | [日本語](ja/layout.md)

## 기존 코드 점검

프로젝트 소스 범위에서 다음 패턴을 검색하고 호출 문맥을 읽는다. 검색 결과는 결함 판정이 아니다.

```sh
rg -n --glob '*.swift' 'UIScreen\.main|userInterfaceIdiom|UIDevice\.current\.orientation|interfaceOrientation|ignoresSafeArea|safeAreaInsets|GeometryReader|frame\(width:' .
```

- 컨테이너 크기는 전체 디스플레이 크기와 다르다. 다중 창에서도 현재 view 크기로 레이아웃을 계산한다.
- compact/regular 전환에 대응하되, 내부 디스플레이가 항상 regular라고 하드코딩하지 않는다.
- 카드의 최소 읽기 폭이나 본문 최대 폭은 유지할 수 있다. 모든 고정 수치와 breakpoint를 일괄 제거하지 않는다.
- UIKit은 safe area layout guide/Auto Layout을 우선한다. 직접 frame을 계산한다면 `view.bounds.inset(by: view.safeAreaInsets)`처럼 네 변을 반영하고 레이아웃 갱신 시 다시 계산한다.
- SwiftUI의 `ignoresSafeArea`는 목적에 맞는 배경에 적용한다. 버튼·본문까지 무조건 확장하지 않는다. 키보드가 있을 때도 입력 필드와 제출 동작이 보이는지 확인한다.

## 접힘에 적응

원본의 reserved regions는 분할 영역과 가림 영역을 구별한다. 사용할 때 [SDK 확인](compatibility.md)을 거치고 좌표계, 활성 상태, 변경 알림을 확인한다. 안전 영역만으로 접힘 영역을 모두 표현한다고 가정하지 않는다.

주 내용과 보조 내용이 모두 보여야 하면 split, 앞뒤 관계가 있으면 overlay를 검토한다. `ArrangementView`를 도입할 때 공식 세션의 중첩 제약을 확인한다. 원본은 navigation container를 arrangement 바깥에 두고, arrangement를 `List`/`ScrollView` 안에 넣지 않도록 안내한다.

스크롤 중인 글이나 피드를 접힘마다 다른 영역으로 옮기지 않는다. 그리드의 짝수 열은 대칭 분할이 실제로 유용할 때만 고려하며 좁은 폭·큰 글자에서 강제하지 않는다. 힌지 각도만으로 레이아웃을 계산하지 않는다.

대체 레이아웃은 기존 `NavigationStack`/`NavigationSplitView` 및 프로젝트 지원 OS에 맞는 stack/grid로 구성한다. 뷰 구조가 바뀌어도 stable ID와 상태 소유자를 유지한다.

출처: [적응형 레이아웃 세션](https://developer.apple.com/videos/play/tech-talks/111463/), [디자인 세션](https://developer.apple.com/videos/play/tech-talks/111466/). 원본 해석과 추가 실무 기준은 [출처 기록](sources.md) 참고.
