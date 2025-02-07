>?플러그 인 마켓플레이스는 여러 클라우드 기능을 통합하여 더 많은 IM 구현을 가능하게 합니다. 다양한 기능을 갖춘 제품을 빠르게 구축하기 위해 필요에 따라 플러그 인을 선택할 수 있습니다.

플러그 인 마켓플레이스는 현재 카나리아 릴리스 중입니다. 스위치를 찾을 수 없으면 [여기](https://console.cloud.tencent.com/im/plugin)에 액세스하십시오.

## 현재 사용 가능한 플러그 인

### 음성/영상 통화 플러그 인

- [Flutter 버전](https://pub.dev/packages/tim_ui_kit_calling_plugin)

### 벤더 오프라인 푸시 플러그 인

- [Flutter 버전](https://pub.dev/packages/tim_ui_kit_push_plugin)

## 사용 방법

IM 콘솔의 플러그 인 마켓플레이스에서 대상 플러그 인을 찾아 설치를 클릭합니다. 이 작업은 2분 후에 적용됩니다.

- [플러그 인 마켓플레이스 - 중국 사이트](https://console.cloud.tencent.com/im/plugin)
- [플러그 인 마켓플레이스 - 글로벌 사이트](https://console.tencentcloud.com/im/plugin)

>?클라이언트 SDK에서 플러그 인 세부 정보 페이지의 지침에 따라 해당 플러그 인 SDK를 통합합니다.

## 결제

1. 음성/영상 통화 플러그 인 및 오프라인 푸시 플러그 인은 콘솔에서 활성화 후 무료로 사용할 수 있습니다.
2. 음성/영상 통화 플러그 인의 IM 및 TRTC가 충전됩니다.
3. 오프라인 푸시 플러그 인의 청구 및 호출 빈도에 대한 자세한 내용은 다른 벤더의 오프라인 푸시 통합 문서를 참고하십시오.

## 오류 코드
| 오류 코드 | 오류 메시지 | 설명 |
|---------|---------|---------|
| 70130 | the configuration to use this plugin was not obtained | 현재 sdkappid가 있는 애플리케이션에는 플러그 인 사용 권한이 없습니다. 콘솔에서 활성화 하십시오. |
