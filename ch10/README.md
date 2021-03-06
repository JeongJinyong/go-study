# 패키지와 GO도구

---

### 임포트 경로
 - 각 패키지는 임포트 경로라는 고유한 문자열로 식별된다. 임포트 경로는 `import`선언에 표시되는 문자열이다
### 패키지 선언
 - 모든 Go 소스 파일은 package 선언을 시작한다. 이 선언의 주 목적은 다른 패키지에 의해 임포트될 때의 기본 식별자(패키지 이름이라 함)를 결정하는 것이다.
 ```go
 package main
 impot (
    "fmt"
     "math/rand"
 )
 
 func main() {
    fmt.Println(rand.Int())
 }
 ```
 - 패키지명은 관행적으로 임포트 경로의 "마지막 부분"이며, 이로 인해 임포트 경로가 다른 별개의 두 패키지가 같은 이름을 가질 수 있다. 
 ```
 "마지막 부분"이라는 관행에는 세 가지 예외가 있다.

  - 명령(Go 실행 프로그램)을 정의하는 패키지는 패키지의 임포트 경로와 무관하게 패키지명으로 항상 main을 사용한다. 이는 go build에게 실행 파일을 만들기 위해 링커를 호출해야 한다고 알리는 신호다.
    - ex) main, text/main, test/simple/main

  - 디렉토리 안의 파일명이 _test.go로 끝나는 일부 파일들은 패키지명 뒤에 _test가 붙을 수 있다. 이런 디렉토리는 두 가지 패키지인 일반 패키지 및 외부 테스트 패키지를 정의할 수 있다. _test 접미사는 go test에 두 패키지를 모두 빌드해야 한다고 알리며, 파일들이 각각 어떤 패키지에 속하는지를 나타낸다. 외부 테스트 패키지는 테스트 의존성으로 인한 임포트 그래프의 순환을 방지하기 위해 사용된다.

  - 의존성을 관리하는 일부 도구에서 패키지 임포트 경로에 "gopkg.in/yaml.v2"와 같이 버전을 접미사로 추가하는 것이다. 패키지 이름은 접미사를 제외하므로 이 경우에는 그냥 yaml이 될 것이다.
 ```
 ### import 선언
   - 기본적으로 선언하는건 제외
   - math/rand와 crypto/rand처럼 이름이 같은 두 패키지를 세 번째 패키지에 임포트할 때는 충돌을 피하기 위해 import 선언 중 적어도 하나에 대체 이름을 지정해야 한다. 이것을 리네임 임포트(rename import)라 한다.
  ```go
  import ( 
  	"crypto/rand"
  	mrand "math/rand" 
  )
  대체 이름은 임포트하는 파일에서만 유효하다.
  ```
  ### 공백 임포트
    - 패키지를 파일로 임포트하고 정의된 패키지명을 파일 안에서 참조하지 않으면 오류가 발생한다.
    - 그러나 떄로는 패키지 수준 변수의 초기화 표현식을 평가하고 init함수를 실행하는 부수효과가 필요해서 패키지를 임포트하는 경우도 있다. << 요경우가 자주있는가 예를들면 뭐가있을까여
    ```go
    import _ "image/png"
    이름 바꾸기 임포트를 사용하여 빈 식별자인 _ 로 대체 이름을 지정
    ```
### 패키지 이름짓기
  - 패키지 이름은 짧은 이름이 좋지만 의미를 알 수 없을 정도로 짧아서는 안된다.
  - 서술적이고 모호하지 않게하라 util 대신 imageutil, ioutil과 같이 구체적이고 간결한 이름을 사용하라
  - 지역 변수에서 자주 사용하는 이름은 패키지명으로 사용하지 않아야 하며 그렇지 않으면 패키지 사용자가 path패키지에서처럼 반강제적으로 패키지 경로의 이름을 바꿔야 할 것이다.
## GO 도구
  ### 작업 공간 구조
  - 대부분의 사용자에게 필요한 유일한 설저은 작업 공간의 루트를 지정하는 GOPATH 환경 변수다.
  - GOROOT 환경 변수는 표준 라이브러리의 모든 패키지를 제공하는 Go 배포본의 루트 디렉토리를 지정한다.
  - go env로 환경 정보를 볼 수 있다.
  ### 패키지 다운로드
  - go 도구를 사용할 때 패키지의 임포트 경로는 로컬 작업 공간 외에 인터넷상의 위치도 나타내기 때문에 go get을 사용해 가져오고 갱신할 수 있다.
  - 패키지를 다운로드 한 후에는 라이브러리나 명령들을 빌드하고 설치한다.
  - go get 은 github, bitbucket, launchpad 와 같은 인기있는 사이트를 지원한다.
  ### 패키지 빌드
  - go build 명령은 인수로 주어진 각 패키지를 컴파일한다.
  - 패키지가 라이브러리이면 결과는 폐기된다. 실행파일이 없다는 말인듯
  - go install 명령은 go build와 매우 유사하지만 각 패키지의 컴파일된 코드와 명령을 폐기하지 않는다 컴파일된 패키지는 소스가 있는 src 디렉토리와 일치하는 $GOPATH/pkg 디렉토리 아래에 저장되며 실행 파일은 $GOPATH/bin 디렉토리에 저장된다. -> 하지만 난 이해하지 못하였다 컴파일된 코드와 명령을 폐기하지 않는다는게 정확히 무슨뜻? 
  ### 패키지 문서화
  - Go는 패키지 API의 문서화를 매우 권장한다.
  - 익스포트된 패키지 멤버와 패키지 선언 자체에는 그 목적과 사용법을 설명하는 주석이 선행돼야 한다.
  - Go의 문서 주석은 항상 완전한 문자이며, 첫 번째 문장은 보통 선언된 이름으로 시작하는 요약문이다. 함수 파라미터와 기타 식별자는 인용표시나 마크업 없이 언급된다.
  - godoc은 웹 브라우저에서 볼 수 있는 패키지 정보들이 표시가 된다. $ godoc -http :8000 http://localhost:8000/pkg
  ### 내부 패키지
  - 익스포트되지 않은 식별자는 같은 패키지 내에서만 볼수 있으며, 익스포트된 식별자는 어디서 나 볼수 있다. 
  - go build 도구는 패키지의 임포트 경로에 internal 이라는 부분이 있으면 특별하게 취급한다. 
  - 새 패키지의 API를 조급하게 커밋하지 않고 일부 사용자에게 "시험 중"으로 배포해 실험할 수 있다.
  - 내부 패키지는 internal 디렉토리의 상위 디레고리 안에 있는 다른 패키지에서만 임포트할 수 있다.
  ```
  net/http
  net/http/internal/chunked
  net/http/httputil
  net/url
  net/http/internal/chunked는 net/http와 net/http/httputil에서는 임포트가 가능하지만, net/url에서는 불가능하다.
  ```
  ### 패키지 조회
  - go list 도구는 사용 가능한 패키지에 대한 정보를 보고한다.
  - go list 명령은 패키지 경로만이 아닌 완전한 메타데이터를 수집하고 다양한 포맷으로 사용할 수 있게 한다. ex) -json
  - go list 명령은 일회성 대화형 쿼리와 빌드 및 테스트 자동화 스크립트에 모두 유용하다. ㅡ  이건 뭔소리임 11장에서 설명해준다한다.
