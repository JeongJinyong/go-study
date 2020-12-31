# 복합 타입

---

### 배열
  - 내장함수 `len`은 배열의 원소 수를 반환한다. `== getSize()`
  - 내가 알고있는 언어와 다르게 데이터타입 앞에 `[]`배열이 들어간다.
  ```go
  var q [3]int = [3]int{1,2,3}
  
  w := [...]int{1,2,3} // 배열크기를 자동으로
  
  var a [3]int
  a[0] = 1
  
  var a1 = [3]int{1, 2, 3}
  
  // 다차원 배열
  var multiArray [3][4][5]int  // 정의
  multiArray[0][1][2] = 10     // 사용
  
  var a = [2][3]int{
        {1, 2, 3},
        {4, 5, 6},  //끝에 콤마 추가 <<-- 이거 왜 추가하시는지 아시는분???? http://golang.site/go/article/12-Go-%EC%BB%AC%EB%A0%89%EC%85%98---%EB%B0%B0%EC%97%B4
        // 중괄호가 한줄인경우는 안붙여도 되지만 여러줄인경우는 붙여야한다고합니다. 맵 찾아보다가 알았음 이유는 못찾음 ㅠ 
        //Need trailing comma before newline in composite literal (복합 리터럴에서 줄 바꿈 앞에 후행 쉼표가 필요합니다.) https://golang.org/ref/spec#Semicolons
    }
  ```
### 슬라이스
  - 모든 원소가 같은 타입인 가변 길이 시퀸스
  - 크기가 없는 배열 타입처럼 보인다.
  - 구성요소로 포인터, 길이, 용량이 있고 포인터는 n번째 원소를 가리키며 내장함수 len으로 길이를 cap으로 용량을 확인할수있다.
  - append를 사용하면 [가변은 진심 자동이다.](https://github.com/adonovan/gopl.io/blob/master/ch4/append/main.go) 복잡도의 선형 증가를 막기위해 두 배로 증가시킨다.
  ```go
  var a []int        //슬라이스 변수 선언
  a = []int{1, 2, 3} //슬라이스에 리터럴값 지정
  s := make([]int, 5, 10)

  슬라이스에 별도의 길이와 용량을 지정하지 않으면, 기본적으로 길이와 용량이 0 인 슬라이스를 만드는데, 이를 Nil Slice 라 하고, nil 과 비교하면 참을 리턴한다.
  var s []int

  if s == nil {
      println("Nil Slice")
  }
  println(len(s), cap(s)) // 모두 0
  ```
### 맵
  - 해당 변수에 없는 키를 넣으면 추가, 해당 변수에 이미 있는 키값을 넣으면 값 갱신이 됩니다.
  ```go
  ages := make(map[stirng]int)
  ages := map[string]int{
    "alice": 31,
    "charlie": 34,
  }
  
  delete(ages, "alice") // 삭제
  
  ages["alice"]++ // 해당 형태로도 int값 +1 가능
  
  // 해당 키값의 원소가 존재하는지 확인
  val,ok := args["boo"]
  ```
### 구조체
  ```go
  // struct 정의
  type person struct {
      name string
      age  int
  }

  func main() {
      // person 객체 생성
      p := person{}

      // 필드값 설정
      p.name = "Lee"
      p.age = 10

      fmt.Println(p)
  }
  
  
  //구조체 익명필드
  type Admin struct {
      Username, Password string
  }

  type User struct {
      ID uint64
      FullName, Email string
      Admin // embedded struct
  }
  fmt.Println(user.Username)
  fmt.Println(user.Password)
  //위와 같이 admin 구조체 바로 접근하여 사용가능
  ```
  
  ### [Json](https://github.com/adonovan/gopl.io/blob/master/ch4/movie/main.go)
    - 데이터 구조를 Json으로 변환하는것은 마샬링이라한다.
    - 데이터 구조에서 export된 필드만 마샬링되며 익스포트를 위해선 필드명이 대문자로 시작하여야 한다.
    
 ### 텍스트와 HTML 템플릿 능력부족으로 리뷰할게 없습니다. 뭘해야할지 감이 안잡히네... 
