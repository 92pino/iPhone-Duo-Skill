# 카메라

한국어 | [English](en/camera.md) | [日本語](ja/camera.md)

## 선택과 전환

먼저 기존 앱이 요구하는 해상도·프레임률·깊이·동시 캡처를 확인한다. 원본이 제시한 virtual front camera로 요구를 만족하면 수동 전환을 추가하지 않는다. 물리 카메라가 필요하면 지원 device와 format을 런타임에서 열거한다. 제품 이름만으로 해상도나 depth 지원을 보장하지 않는다.

`AVCaptureDeviceDirectionCoordinator`와 inner/outer device type은 [SDK 확인](compatibility.md) 후 사용한다. direction은 view 기준이므로 두 화면이 있을 때 하나의 방향 값을 전역으로 재사용하지 않는다.

## 실행과 오류 처리

- 권한과 usage description을 확인하고 거부·제한·카메라 부재 시 대체 UI를 제공한다.
- session의 시작·정지·재구성은 기존 serial executor 또는 카메라 actor에 모은다. UI와 preview 변경은 main actor에서 처리한다. actor라고 해서 blocking 작업이 자동으로 적절한 executor에서 실행되는 것은 아니므로 기존 동시성 설계를 확인한다.
- 전환 전에 입력 추가 가능 여부를 확인하고 재구성 실패 시 기존 입력 복구 또는 명시적 실패 상태를 제공한다. 구성 트랜잭션 도중 start/stop이 끼어들지 않게 직렬화한다.
- 연속 개폐 이벤트의 오래된 전환 결과가 최신 선택을 덮어쓰지 않게 한다. 녹화 중 전환은 출력 지원과 녹화 유지 요구를 따로 확인한다.
- interruption, background 진입, 복귀, runtime error를 처리한다. 화면 해제 후 카메라가 계속 실행되지 않도록 한다.

## 프리뷰와 결과물

rotation coordinator의 변경을 관찰하고 preview와 capture의 각도를 각각 적용한다. 지원하는 회전 각도인지 확인한다. 미러링과 저장 결과의 방향은 별도 검증한다. aspect fill의 잘림과 aspect fit의 여백은 제품 의도에 맞게 선택한다.

원본의 센서 방향 보정 비활성화나 dynamic aspect ratio 설정은 일반 규칙으로 적용하지 않는다. SDK 선언과 사용하는 출력의 요구를 확인하고 실제 저장 이미지·영상까지 검증한 뒤 적용한다.

시뮬레이터 결과는 카메라 전환·센서·녹화 품질의 실기기 증거가 아니다. 장치가 없으면 검증 미실시로 남긴다.

출처: [Build a great camera experience for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111465/).
