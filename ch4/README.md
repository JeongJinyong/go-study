# 복합 타입

---

### 배열
  - 내장함수 `len`은 배열의 원소 수를 반환한다. `== getIndex()`
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
  - 
