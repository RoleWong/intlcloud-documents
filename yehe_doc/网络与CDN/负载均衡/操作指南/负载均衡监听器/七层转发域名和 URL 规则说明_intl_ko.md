## 프로세스 흐름
레이어 7 및 레이어 4 CLB(이전 ‘애플리케이션 CLB’)의 프로세스 흐름은 다음과 같습니다.
![](https://main.qcloudimg.com/raw/9c30c679e6a5fd70f9dd9273f8784d6d.jpg)
레이어 7 CLB를 사용하여 HTTP/HTTPS 프로토콜을 포워딩하는 경우 CLB 리스너에서 포워딩 규칙을 생성할 때 해당 도메인 이름을 추가할 수 있습니다.
- 하나의 포워딩 규칙만 생성된 경우 VIP + URL을 통해 해당 포워딩 규칙 및 서비스에 액세스할 수 있습니다.
- 여러 포워딩 규칙이 생성된 경우 VIP + URL을 사용한다고 해서 지정된 도메인 이름 + URL에 대한 액세스가 보장되지는 않습니다. 포워딩 규칙이 적용되었는지 확인하려면 도메인 이름 + URL에 직접 액세스해야 합니다. 즉, 여러 포워딩 규칙을 구성할 때 VIP는 여러 도메인 이름에 해당할 수 있습니다. 이 경우 VIP + URL이 아닌 지정된 도메인 이름 + URL을 통해 서비스에 액세스하는 것을 권장합니다.

## 레이어 7 포워딩 구성
### 도메인 포워딩 구성
레이어 7 CLB는 다른 도메인 이름과 URL의 요청을 다른 서버로 포워딩할 수 있습니다. 레이어 7 리스너는 여러 도메인 이름으로 구성할 수 있으며, 각 도메인 이름은 여러 포워딩 경로로 구성할 수 있습니다.
- 포워딩된 도메인 이름의 길이 제한: 1 - 80자.
- `_`로 시작할 수 없습니다.
- `www.example.com`과 같은 정확한 도메인이 지원됩니다.
- 와일드카드 도메인 이름이 지원되지만 현재는 `*.example.com` 또는 `www.example.*` 형식의 도메인 이름만 지원됩니다. 즉, 와일드카드 도메인 이름은 `*`로 시작하거나 끝나며 한 번만 나타납니다.
- 비정규식 포워딩 도메인 이름의 경우 유효한 문자 집합에는 `a-z` `0-9` `.` `-` `_`가 포함됩니다.
- 포워딩된 도메인 이름은 정규식을 지원합니다. 정규식 도메인 이름:
  - 지원되는 문자 집합은: `a-z` `0-9` `.` `-` `?` `=` `~` `_` `-` `+` <code>\\</code> `^` `*` `!` `$` `&` `|` `(` `)` `[` `]`입니다.
  - `~`로 시작해야 하며, 한 번만 나타날 수 있습니다.
  - CLB에서 지원하는 정규식 도메인 이름의 예시는: `~^www\d+\.example\.com$`입니다.

### 포워딩된 도메인 이름 매칭
#### 일반 매칭 정책
1. 포워딩 규칙에 도메인 이름 대신 IP 주소를 입력하고 포워딩 그룹에 여러 URL을 구성하면 VIP + URL을 사용하여 서비스에 액세스합니다.
2. 포워딩 규칙에서 전체 도메인 이름을 구성하고 포워딩 그룹에서 여러 URL을 구성하면 도메인 이름 + URL을 사용하여 서비스에 액세스합니다.
3. 포워딩 규칙에 와일드카드 도메인 이름을 설정하고 포워딩 그룹에 여러 개의 URL을 설정하면 요청한 도메인 이름과 URL의 매칭을 통해 서비스에 액세스하게 됩니다. 다른 도메인 이름이 동일한 URL을 가리키도록 하려면 이 방법을 구성에 사용할 수 있습니다. `example.qcould.com`을 예로 들면 형식은 다음과 같습니다.
   - 정확히 매칭: 입력한 도메인 `example.qcloud.com`과 완전히 일치하는 도메인 이름 `example.qcloud.com`을 매칭합니다.
   - 접두사 와일드카드: `*.qcloud.com`과 같이 지정된 두 번째 및 최상위 레벨 도메인이 있는 모든 도메인 이름과 매칭합니다.
   - 접미사 와일드카드: `example.qcloud.*`와 같이 지정된 세 번째 및 두 번째 레벨 도메인이 있는 모든 도메인 이름과 매칭합니다.
   - 정규식 일치: `~^www\d+\.example\.com$`.
   - 우선 순위: 정확히 일치 > 접두사 와일드카드 > 접미사 와일드카드 > 정규식 일치. 여러 매칭 규칙이 활성화되지 않도록 보다 정확한 도메인 이름을 사용하는 것이 좋습니다. 그렇지 않으면 같은 레벨의 여러 도메인 이름이 한 번에 히트될 때 부정확한 매칭 결과가 나올 수 있습니다.
4. 포워딩 규칙에서 도메인 이름을 구성하고 포워딩 그룹에서 퍼지 매칭을 위한 URL을 구성하는 경우 접두사 일치 항목을 사용하고 접미사가 붙은 와일드카드 $를 추가하여 전체 매칭을 시작할 수 있습니다.
예를 들어 `URL ~*.(gif|jpg|bmp)$`를 입력하면 모든 `gif`, `jpg` 및 `bmp` 파일과 일치합니다.

#### 기본 도메인 이름 정책[](id:default)
요청된 도메인 이름이 규칙과 일치하지 않으면 CLB는 요청을 기본 도메인 이름(Default Server)으로 포워딩합니다. 하나의 리스너는 기본 도메인 이름을 하나만 가질 수 있습니다.
예를 들어, CLB1의 `HTTP:80` 리스너는 `www.test1.com` 및 `www.test2.com`이라는 두 개의 도메인 이름으로 구성되며, 여기서 `www.test1.com`은 기본 도메인 이름입니다. 사용자가 `www.example.com`을 방문하면 일치하는 도메인 이름이 없으므로 CLB는 요청을 기본 도메인 이름인 `www.test1.com`으로 포워딩합니다.

>?
>- 2020년 5월 18일 이전에는 레이어 7 리스너에 기본 도메인 이름 구성 여부는 선택 사항이며 기본 도메인 이름 구성 여부를 선택할 수 있습니다.
>    - 레이어 7 리스너에 기본 도메인 이름이 구성된 경우 다른 규칙과 일치하지 않는 클라이언트 요청은 기본 도메인 이름으로 포워딩됩니다.
>    - 레이어 7 리스너에 기본 도메인 이름이 구성되어 있지 않은 경우 다른 규칙과 일치하지 않는 클라이언트 요청은 CLB에서 로드한 첫 번째 도메인 이름으로 포워딩됩니다(로드 순서는 콘솔에서 구성된 순서와 다를 수 있으므로 콘솔에서 구성된 첫 번째 순서가 아닐 수 있습니다). 
>- 2020년 5월 18일부터:
>    - 모든 새 레이어 7 리스너에는 기본 도메인 이름이 있어야 합니다. 레이어 7 리스너의 첫 번째 규칙이 기본 도메인 이름으로 설정됩니다. API를 통해 레이어 7 규칙을 생성하면 `DefaultServer` 필드가 true로 설정됩니다.
>    - 기본 도메인 이름이 구성된 모든 리스너의 경우 기존 기본 도메인 이름을 수정하거나 삭제할 때 새 기본 도메인 이름을 지정해야 합니다. 콘솔에서 작업을 수행할 때 새 기본 도메인 이름을 지정해야 합니다. API를 호출하여 작업을 수행할 때 새 기본 도메인 이름을 설정하지 않으면 CLB는 나머지 도메인 이름 중 가장 먼저 생성된 이름을 새 기본 도메인 이름으로 설정합니다.
>    - 기본 도메인 이름이 없는 기존 규칙의 경우 아래 ‘[작업4](#step4)’의 지침에 따라 비즈니스 요구 사항에 따라 기본 도메인 이름을 직접 구성할 수 있습니다. 그렇게 하지 않으면 Tencent Cloud는 CLB에서 로드한 첫 번째 도메인 이름을 기본 도메인 이름으로 설정합니다. 기존 리스너는 모두 2020년 6월 19일 이전에 처리됩니다.
>
>상기 정책은 2020년 5월 18일부터 점진적으로 시행 중이며, 각 사례별 시행일자는 다소 상이할 수 있습니다. 2020년 6월 20일부터 도메인 이름이 포워딩된 모든 레이어 7 리스너는 기본 도메인 이름을 갖게 됩니다.

기본 도메인 이름에 대해 다음 네 가지 작업을 수행할 수 있습니다.
- **작업1**: 레이어 7 리스너에 대한 첫 번째 포워딩 규칙을 구성할 때 기본 도메인 이름은 활성화됨 상태여야 합니다.
- **작업2**: 현재 기본 도메인 이름을 비활성화합니다.
   - 리스너 아래에 여러 도메인 이름이 있는 경우 현재 기본 도메인 이름을 비활성화할 때 새 기본 도메인 이름을 지정해야 합니다.
   - 리스너에 도메인 이름이 하나만 있고 도메인 이름이 기본 도메인 이름인 경우 비활성화할 수 없습니다.
- **작업3**: 기본 도메인 이름을 삭제합니다.
   - 리스너 아래에 여러 도메인 이름이 있는 경우 기본 도메인 이름 아래의 규칙을 삭제:
     - 규칙이 기본 도메인 이름의 마지막 규칙이 아닌 경우 직접 삭제할 수 있습니다.
     - 규칙이 기본 도메인 이름의 마지막 규칙인 경우 새 기본 도메인 이름을 설정해야 합니다.
   - 리스너 아래에 도메인 이름이 하나만 있는 경우 새 기본 도메인 이름을 설정하지 않고 모든 규칙을 직접 삭제할 수 있습니다.
- <span id = "step4"></span>**작업4**: 리스너 목록에서 기본 도메인 이름을 빠르게 수정할 수 있습니다.

### 포워딩된 URL 경로 구성 규칙
레이어 7 CLB는 처리를 위해 다른 URL의 요청을 다른 서버로 포워딩할 수 있으며 단일 도메인 이름에 대해 여러 포워딩된 URL 경로를 구성할 수 있습니다.
- 포워딩된 URL의 길이 제한: 1–200자.
- 비정규식 포워딩 URL은 `a-z` `A-Z` `0-9` `.` `-` `_` `/` `=` `?` `:`를 포함한 유효한 문자 집합과 함께 `/`로 시작해야 합니다.
- 포워딩된 URL은 정규식을 지원합니다.
  - 정규식 URL은 `~`로 시작해야 하며, 한 번만 나타날 수 있습니다.
  - 정규식 URL의 경우 유효한 문자 집합: `a-z` `A-Z` `0-9` `.` `-` `_` `/` `=` `?` `~` `^` `*` `$` `:` `(` `)` `[` `]` `+`  `|`.
  - 정규식 URL의 예는 `~* .png$`일 수 있습니다.
- 포워딩된 URL에 대한 매칭 규칙은 다음과 같습니다.
   - `=`로 시작하는 것은 정확히 일치함을 나타냅니다.
   - ^~로 시작하는 URL은 정규 문자열로 시작하고 정규식 일치를 위한 것이 아님을 나타냅니다.
   - `~`로 시작하는 것은 대소문자를 구분하는 정규식 일치를 나타냅니다.
   - `~*`로 시작하는 것은 대소문자를 구분하지 않는 정규식 일치를 나타냅니다.
   - `/`는 다른 일치 항목이 없는 경우 모든 요청이 일치하는 일반 일치를 나타냅니다.

### 포워딩된 URL 경로 일치 설명
![](https://main.qcloudimg.com/raw/6399b39845c9f23adccb90089e10bade.jpg)

1. 매칭 규칙: 가장 긴 접두어 일치를 기반으로 정확한 일치가 먼저 수행되고 퍼지 일치가 수행됩니다.
예를 들어 위와 같이 포워딩 규칙 및 포워딩 그룹을 구성한 후 다음 요청이 다른 포워딩 규칙과 순서대로 일치합니다.
   1. `example.qloud.com/test1/image/index1.html`은 포워딩 규칙1에 의해 구성한 URL 규칙과 정확히 일치하므로, 요청은 포워딩 규칙1과 연결된 실제 서버, 즉 그림에서 CVM1 및 CVM2의 포트 80으로 포워딩됩니다.
   2. `example.qloud.com/test1/image/hello.html`에는 정확히 일치하는 항목이 없으므로 가장 긴 접두어 일치를 기준으로 포워딩 규칙2와 일치합니다. 따라서 요청은 포워딩 규칙2와 연결된 실제 서버, 즉 그림에서 CVM2 및 CVM3의 포트 81로 전달됩니다.
   3. `example.qloud.com/test2/video/mp4/`에는 정확히 일치하는 항목이 없으므로 가장 긴 접두어 일치를 기반으로 포워딩 규칙3과 일치합니다. 따라서 요청은 포워딩 규칙3과 관련된 실제 서버, 즉 그림에서 CVM4의 포트 90으로 전달됩니다. 
   4. `example.qloud.com/test3/hello/index.html`에는 정확히 일치하는 항목이 없기 때문에 루트 디렉터리의 Default URL인 `example.qloud.com/`과 가장 긴 접두어 일치로 일치합니다. 이 경우 Nginx는 요청을 FastCGI(php) 또는 Tomcat(jsp)과 같은 실제 서버로 전달하고 Nginx는 역방향 프록시 서버로 존재합니다.
   5. `example.qloud.com/test2/`에는 정확히 일치하는 항목이 없기 때문에 루트 디렉터리의 Default URL인 `example.qloud.com/`과 가장 긴 접두어 일치로 일치합니다.

2. 설정된 URL 규칙에서 서비스가 제대로 작동하지 않을 경우, 매칭 성공 후 다른 페이지로 리디렉션되지 않습니다.
예를 들어 클라이언트가 `example.qloud.com/test1/image/index1.html`을 요청하여 포워딩 규칙1과 일치시킵니다. 하지만 포워딩 규칙1의 실제 서버는 예외가 있고 404 오류 페이지가 나타납니다. 404 오류 페이지가 표시되지만 다른 페이지로 리디렉션되지는 않습니다.
3. Default URL을 안정적인 페이지(예: 정적 페이지 또는 홈페이지)로 지정하고 모든 실제 서버에 바인딩하는 것이 좋습니다. 일치하는 규칙이 없으면 시스템은 요청을 Default URL 페이지로 지정합니다. 그렇지 않으면 404 오류가 발생할 수 있습니다.
4. Default URL을 설정하지 않고, 일치하는 포워딩 규칙이 없으면, 서비스에 액세스할 때 404 오류가 반환됩니다.
5. 레이어 7 URL 경로 끝에 있는 슬래시 참고: 설정한 URL이 `/`로 끝나지만 클라이언트의 액세스 요청에 `/`가 포함되지 않은 경우 요청은 `/`로 끝나는 규칙으로 리디렉션됩니다(301 리디렉션).
예를 들어 `HTTP:80` 리스너에서 구성된 도메인 이름은 `www.test.com`입니다.
   1. 이 도메인 이름 아래에 설정된 URL이 `/abc/`인 경우:
       - 클라이언트가 `www.test.com/abc`에 액세스하면 `www.test.com/abc`로 리디렉션됩니다.
       - 클라이언트가 `www.test.com/abc/`에 액세스하면 `www.test.com/abc/`와 일치합니다.
   2. 이 도메인 이름 아래에 설정된 URL이 `/abc`인 경우:
       - 클라이언트가 `www.test.com/abc`에 액세스하면 `www.test.com/abc`와 일치합니다.
       - 클라이언트가 `www.test.com/abc/`에 액세스하면 `www.test.com/abc/`와도 일치합니다.

## 레이어 7 상태 확인 구성 설명
### 상태 확인 도메인 이름 구성 규칙
상태 확인 도메인 이름은 레이어 7 CLB에서 실제 서버의 상태를 감지하는 데 사용하는 도메인 이름입니다.
- 길이 제한: 1 - 80자.
- 기본값: 포워딩된 도메인 이름.
- 정규식은 지원되지 않습니다. 포워딩된 도메인 이름이 와일드카드 도메인 이름인 경우 고정된 이름(비정규식)을 지정해야 합니다.
- 유효한 문자 집합에는 `a-z` `0-9` `.` `-` `_`가 포함됩니다. 예시: `www.example.qcould.com`.

### 상태 확인 경로 구성 규칙
상태 확인 경로는 레이어 7 CLB가 실제 서버의 상태를 감지하는 데 사용하는 URL 경로입니다.
- 길이 제한: 1 - 200자.
- 기본: `/`. `/`로 시작하는 사용자 정의 경로를 입력할 수 있습니다.
- 정규식은 지원되지 않습니다. 상태 확인을 위해 고정 URL(정적 페이지)을 지정하는 것이 좋습니다.
- 유효한 문자 집합에는 `a-z` `A-Z` `0-9` `.` `-` `_` `/` `=` `?` `:`가 있습니다. 예시 `/index`.
