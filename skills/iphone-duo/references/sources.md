# 출처와 보완 범위

한국어 | [English](en/sources.md) | [日本語](ja/sources.md)

확인일: 2026-09-15. 버전·API는 작업 시점에 다시 확인한다.

## 기반 저장소

[d-date/iphone-duo-skill](https://github.com/d-date/iphone-duo-skill), commit `6f5374935c76d1a4be149cbded04571a9ac694b7`.
주제 구분과 마이그레이션 관점을 기반으로 한국어·영어·일본어로 재구성했다. 원본의 저작권·MIT 고지는 [LICENSE.upstream](../LICENSE.upstream)에 보존한다.

## Apple 1차 자료

- [Duo 개발자 안내 및 SDK 배포 상태](https://developer.apple.com/iphone-duo/)
- [Prepare your app for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111461/)
- [Raise the bar with iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111462/)
- [Strike a pose with adaptive layouts on iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111463/)
- [Leverage multiple displays and scenes on iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111464/)
- [Build a great camera experience for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111465/)
- [Design for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111466/)
- [Designing for iPhone Duo — HIG](https://developer.apple.com/design/human-interface-guidelines/designing-for-iphone-duo)

공식 안내와 세션 페이지의 존재·주제를 확인했다. HIG는 링크만 확인했으며 본문 검증은 하지 못했다. 모든 API 시그니처와 Duo runtime 동작을 검증했다는 의미는 아니다. 설치된 Xcode 26.4.1 / iOS SDK 26.4에서는 Duo 신기능 컴파일·실행 검증을 수행하지 않았다.

## 이 배포판의 추가 기준

SDK 선언과 runtime availability 구분, 상태 보존, 접근성·지역화, 카메라 오류 처리, 구 OS fallback, 검증 결과 등급은 실무용 보완이다. 이를 Apple의 필수 요구사항으로 인용하지 않는다.

원본의 일괄 규칙도 조건부 판단으로 바꿨다. 콘텐츠 기반 breakpoint는 허용하고, 짝수 그리드 열이나 센서 보정 비활성화는 해당 화면·출력에 필요하고 검증된 경우만 적용한다.
