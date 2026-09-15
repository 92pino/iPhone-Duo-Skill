# 내비게이션과 바

한국어 | [English](en/bars.md) | [日本語](ja/bars.md)

SwiftUI는 navigation container의 toolbar, UIKit은 navigation/tab controller를 우선한다. 바 자체를 수동 회전하거나 고정된 측면 여백을 덧붙여 시스템 동작을 흉내 내지 않는다.

## 항목별 판단

- 버튼에는 의미 있는 제목과 아이콘을 제공한다. SwiftUI의 `Label`처럼 표현을 시스템이 선택할 수 있게 한다.
- 금액·진행 상태처럼 텍스트 자체가 정보라면 아이콘만 남기지 않는다. 단순 건수는 배지로 표현할 수 있지만 접근성 이름에도 의미를 전달한다.
- 뒤로/닫기, 주요 작업, 보조 작업의 순서를 살핀다. 크기가 바뀌어도 동일한 동작을 제공하고 필요하면 overflow에서 접근할 수 있게 한다.
- 키보드가 나타날 때, 긴 번역 문자열일 때, 글자가 클 때 주요 동작이 사라지지 않는지 확인한다. 키보드 accessory는 키보드와 함께 배치한다.

원본의 축 제어, 표시 우선순위, overflow 통합, 세로 바 비활성화 API는 [SDK 확인](compatibility.md) 후 적용한다. 기존 custom bar를 표준으로 바꾸는 것이 제품 디자인을 깨뜨리면 먼저 크기 적응과 접근성을 고친다. 단일 버튼 시트 등에서 세로 바를 끄는 선택은 실제 가용 공간을 비교한 뒤 결정한다.

검증은 수평/수직 전환, 좁은 높이, RTL, VoiceOver 이름·순서, Reduce Transparency를 포함한다. 시스템 바의 물리적 위치를 RTL이라고 수동 반전하지 않는다.

출처: [Raise the bar with iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111462/).
