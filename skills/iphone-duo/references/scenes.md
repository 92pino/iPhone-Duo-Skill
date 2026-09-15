# Scene, 힌지, 상태 유지

한국어 | [English](en/scenes.md) | [日本語](ja/scenes.md)

힌지는 상호작용이나 효과에 사용하고 레이아웃은 가용 크기와 시스템 레이아웃 정보로 결정한다. 힌지 없는 기기의 기본 동작을 유지하고 연속 이벤트로 무거운 작업을 매번 실행하지 않는다.

## 상태 경계

- 문서·계정·저장 데이터와 창별 navigation path·선택·focus를 구분한다.
- 크기 전환마다 모델을 재생성하거나 `.id`를 바꿔 편집 내용을 잃지 않게 한다.
- background/foreground 및 scene 재연결 시 구독을 중복 등록하지 않는다. 화면이 사라지면 해당 observer/task를 정리한다.
- 창이 여러 개일 때 현재 scene을 사용한다. `connectedScenes.first`를 사용자의 현재 창으로 가정하지 않는다.

다중 창 지원은 기존 앱 설정과 동적인 가능 여부를 모두 확인한다. 새 창 요청은 실패할 수 있으므로 실패 이유를 처리하고 현재 작업을 보존한다. Duo 대응만을 위해 앱에 불필요한 창 모델을 추가하지 않는다.

## Scene accessories

부가 콘텐츠가 실제로 필요할 때만 도입한다. `CameraCaptureAccessory` 등은 [SDK 확인](compatibility.md)을 거친다. 등록 위치, availability 변화, 사라질 때의 리소스 정리를 살핀다. 외부 화면을 잃어도 주 화면의 작업은 계속 가능해야 한다. 공유 데이터에 연결하되 창별 선택 상태를 무조건 전역화하지 않는다.

카메라를 공유하는 화면은 [카메라 참조](camera.md)도 읽는다. 양쪽 화면에서 독립적으로 같은 캡처 자원을 시작하지 않도록 소유권을 정한다.

출처: [Leverage multiple displays and scenes on iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111464/).
