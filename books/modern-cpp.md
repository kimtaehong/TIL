___
## chapter 1. 형식영역

### Item 1. 템플릿 형식 연역 규칙을 숙지하라.

+ 템플릿 함수 예제코드
  ```c++
  // 함수 템플릿의 선언은 대체로 다음과 같은 모습이다.
  template<typename T>
  void f(ParamType parm);
  ...
  // 그리고 이를 호출하는 코드는 대체로 이런 모습이다.
  f(expr); // 어떤 표현식으로 f를 호출
  ```

+ 컴파일 도중 컴파일러는 "expr"을 이용해서 두 가지 형식을 연역하는데, \
  하나는 T에 대한 형식이고 또 하나는 "ParamType"에 대한 형식이다.
+ 두 형식이 다른 경우가 많은데, 이는 ParamType에 흔히 const나 \
  참조 한정사(ex. & or &&)  같은 수식어들이 붙기 때문이다.
  - 예제 코드
  ```c++
  template<typename T>
  void f(const T& param); //paramType : const T&
  ...
  int x = 0;
  f(x);
  ```
+ 위의 예제를 보면 T가 "expr(int x)"의 형식(int)이라고 생각을 할 것이다. \
  하지만, 항상 전달 되는 함수에 전달된 인수의 형식과 동일 하지 않다.
+ T에 대해 연역된 형식은 "expr"의 형식에 의존할 뿐만 아니라 "ParamType"의 형태 에도 의존 한다. 이러한 경우는 다음과 같다.
  - case 1) "ParamType"이 포인터 또는 참조 형식이지만 보편 참조(universal reference)는 아닌 경우.
  - case 2) "ParamType"이 보편 참조인 경우
  - case 3) "ParamType"이 포인터도 아니고 참조도 아닌 경우

#### case 1) ParamType이 포인터 또는 참조 형식이지만 보편 참조는 아님
+ "ParamType" 포인터 형식이나 참조 형식이지만 보편 참조는 아닌 경우는 다음과 같이 진행 된다.

  - step-1) 만일 expr이 참조 형식이면 참조 부분을 무시한다.
  - step-2) expr의 형식을 ParamType에 대해 패턴 부합(pattern-matching) \
    방식으로 대응시켜서 T의 형식을 결정
    *
    ```c++
    template<typename T>
    void f(T& param);
    ...
    int x = 27; // x는 int
    const int cx = x; // cx는 const int
    const int& rx = x; // rx는 const int인 x에 대한 참조
    ...
    f(x); // T는 int, param의 형식은 int&
    f(cx); // T는 const int,
           // param의 형식은 const int&
    f(rx); // T는 const int,
           // param의 형식은 const int&
    ```

#### case 2) ParamType이 보편 참조임
+ 만일 expr이 왼값이면, T와 ParamType 둘 다 왼값 참조로 연역된다.
