CSS의 라이브 방송 녹화 기능을 기반으로 구동되는 타임 시프트 기능을 통해, 시청자는 이전 시점의 비디오 스트림을 되감기 및 재생할 수 있습니다. VOD 재생 도메인에서 타임 시프트가 활성화되면 해당 동영상에 대한 TS(Transport Stream) 세그먼트 URL과 TS 파일이 VOD에 저장되며, 사용자는 타임 시프트 재생 도메인을 통해 시간 매개변수를 전달하여 이전 동영상 콘텐츠를 재생할 수 있습니다.



## 타임 시프트 원리
HLS 스트리밍에서 비디오 스트림은 TS 세그먼트로 분할됩니다. 시청자는 m3u8 파일을 사용하여 TS 세그먼트 URL에 액세스하고 TS 파일을 가져오고 해당 TS 세그먼트에서 시작하는 비디오 콘텐츠를 재생합니다.
>!TS 파일은 영구적으로 저장되지 않으므로 과거 재생 시작 시간에 제한이 있습니다.


## 주의 사항
타임 시프트 기능은 현재 베타 테스트 중입니다. 사용 요금은 **2022년 06월 01일 00:00**부터 과금됩니다. 자세한 내용은 [타임 시프트 기능 유료 전환 공지](https://intl.cloud.tencent.com/document/product/267/46847)를 참고하십시오. 타임 시프트는 녹화 기능에 의존하므로 [라이브 방송 녹화 요금](https://intl.cloud.tencent.com/document/product/267/39605), VOD [스토리지 및 재생 요금](https://intl.cloud.tencent.com/document/product/266/2838)도 과금됩니다.

## 타임 시프트 사용

### 전제 조건

-  [Tencent Cloud 가입](https://intl.cloud.tencent.com/document/product/378/17985) 계정이 있어야 합니다. 
-  CSS를 활성화하고 [푸시 도메인 이름](https://intl.cloud.tencent.com/document/product/267/35970)을 추가했습니다. 

[](id:step1)
### 1단계: VOD 서비스 활성화

1. [VOD 콘솔](https://console.cloud.tencent.com/vod/overview)에 로그인하고 **지금 활성화**를 클릭합니다.
2. 서비스 이용약관에 동의하면 체크박스를 선택하고 **확인**을 클릭하면 VOD가 활성화됩니다.

[](id:step2)
### 2단계: 도메인 이름 추가

타임 시프트를 위한 VOD 도메인 이름을 추가하려면 다음 단계를 따르십시오.

1. **VOD 콘솔로 이동하여 왼쪽 사이드바에서 배포 및 재생>도메인 이름**을 선택합니다.
2. **도메인 추가**를 클릭하고 ICP 비안 번호로 등록된 VOD 도메인 이름을 입력합니다. 자세한 내용은 [배포 및 재생 설정](https://intl.cloud.tencent.com/document/product/266/35572)을 참고하십시오.
3. 도메인에 대한 CNAME 레코드를 추가합니다.

[](id:step3)
### 3단계: 녹화 템플릿 연결

1. CSS 콘솔>기능 설정>[라이브 방송 녹화](https://console.cloud.tencent.com/live/config/record)를 선택합니다.
2. **녹화 템플릿 생성**을 클릭합니다. 녹화 템플릿 생성에 대한 자세한 내용은 [라이브 방송 녹화](https://intl.cloud.tencent.com/document/product/267/34223)를 참고하십시오.
> ! 
> - 녹화 형식으로 **HLS**를 선택합니다.
> - 파일 저장 기간을 설정할 수 있으며, 해당 기간은 [타임 시프트 시간](#step4)보다 짧을 수 없습니다.
3. 사용하려는 푸시 도메인으로 녹화 템플릿을 바인딩합니다. 자세한 방법은 [녹화 설정](https://intl.cloud.tencent.com/document/product/267/34224)을 참고하십시오.

[](id:step4)
### 4단계: 타임 시프트 서비스 활성화

타임 시프트 기능을 활성화하려면 [티켓 제출](https://console.cloud.tencent.com/workorder/category?step=0&source=0)해야 하며, ‘CSS’를 제품으로 선택하고 다음 정보를 제공해야 합니다.

- [2단계](#step2)에서 추가한 VOD **타임 시프트 재생 도메인**입니다.
- [3단계](#step3)에서추가한 녹화 템플릿 ID입니다.
- `timeshift_dur`(타임 시프트 시간)에 대한 사용자 정의 값(초)입니다.
> ? 
> - 타임 시프트 시간은 현재 시간에서 비디오 스트림을 재생할 수 있는 시간을 나타냅니다. 현재 허용되는 가장 긴 타임 시프트 기간은 30일입니다.
> - 구성한 타임 시프트 시간이 실제 타임 시프트 시간과 정확히 일치하지 않을 수 있으므로 실제 필요한 것보다 조금 더 길게 설정하는 것이 좋습니다.
> - 7200(2시간)으로 설정된 경우 2시간 전 또는 그 이후에 생성된 콘텐츠를 요청할 수 있으며 재생 delay 매개변수 지연의 값 범위는 90초 - 2시간입니다. delay가 2시간보다 큰 값으로 설정되면 해당 시점에 라이브 스트리밍 콘텐츠가 있더라도 `HTTP 404`가 반환됩니다. 



## 재생 요청

### 요청 URL 포맷

```
http://[Domain]/timeshift/[AppName]/[StreamName]/timeshift.m3u8?delay=xxx
```

### 매개변수 설명

| 매개변수           | 설명                                                         |
| -------------- | ------------------------------------------------------------ |
| [Domain]       | 타임 시프트를 위해 [2단계](#step2)에서 추가한 VOD 도메인 이름입니다.     |
| timeshift      | 사용자 정의할 수 없는 매개변수입니다.                                           |
| [AppName]      | 애플리케이션 이름으로, 애플리케이션 이름이 `live`인 경우 이 매개변수를 `live`로 설정합니다.           |
| [StreamName]   | 스트림 이름으로, 이 매개변수를 타임 시프트를 활성화하려는 스트림의 이름으로 설정합니다.                                 |
| timeshift.m3u8 | 사용자 정의할 수 없는 매개변수입니다.                                           |
| delay          | 재생 지연 시간(초)으로, 90보다 작은 값을 전달하면 90(최소값)이 사용됩니다.|

### 예시

타임 시프트 도메인 이름이 `testtimeshift.com`이고 애플리케이션 이름이 `live`이며 스트림 이름이 `SLPUrIFzGPE`라고 가정합니다. 5분 전의 동영상 콘텐츠를 재생하려면 다음 요청 URL을 사용해야 합니다.

```
http://testtimeshift.com/timeshift/live/SLPUrIFzGPE/timeshift.m3u8?delay=300
```
