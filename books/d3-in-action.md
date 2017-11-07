___
#### CHAPTER 1
##### D3 개요
+ D3는 Data Driven Document의 약자이다.
+ D3는 셀렉션과 바인딩이 핵심이다.
+ 셀렉션 예제 코드(데이터 x)
```javascript
d3.selectAll("circle.a").style("fill", "red").attr("cx", 100);
```
위의 코드는 웹 페이지에서 **a** 클래스에 속한 원을 모두 선택해 빨간색으로 채우고 원의 중점을 **svg** 영역의 왼쪽에서 100px 떨어진 곳에 위치 시킨다.
+ **d3.selectAll()** 은 셀렉션 코드로서, D3를 이해하는데 필요한 핵심 코드이다.
  - D3에서 사용 할 수 있는 셀렉션 함수는 **select()** , **selectAll()** 두 함수가 존재 한다. select() 함수는 찾아낸 첫 번째 요소를 선택하고, selectAll() 함수는 여러 요소를 선택 할 때 사용 한다.

다음 코드의 셀렉션은 [1, 5, 11, 3] 배열에 있는 요소를 market 클래스의 div 요소에 바인딩 하는 예제이다.
```javascript
d3.selectAll("div.market").data([1, 5, 11, 3])
```
+ D3를 사용 하는 웹 페이지의 전형적인 처리 과정
  - 웹 페이지 로딩 -> 요소 선택 -> 데이터 바인딩 -> 요소 생성/갱신/제거 -> 사용자입력
+ D3는 코드에 **utf-8** 문자를 사용하므로 다음 세가지 문자셋 지정 방법 중 하나를 따라야 한다.
```html
<!-- 방법 1. meta charset 이용 -->
<!DOCTYPE html><meta charset="utf-8">
<!-- 방법 2. 스크립트의 문자셋 이용-->
<script charset="utf-8" src="d3.js"></script>
<!-- 방법 3. 또는 UTF-8 문자를 포함하지 않은 압축된 스크립트를 사용 -->
<script src="d3.min.js">
```
+ DOM은 화면에서 요소가 그려지는 순서를 결정한다. 자식 요소는 부모 요소가 그려진 후에 그 안에 그려진다. 전통적인 HTML에서는 **z-index** 로 요소를 앞이나 뒤에 그릴 수 있지만 SVG에서는 **z-index** 를 사용 할 수 없다.(SVG 요소는 **render-order** 속성으로 어느 정도 앞뒤를 결정 할 수 있다.)
##### SVG
+ SVG를 이용하면 그림을 간단한 수식으로 표현해 확대하고 애니메이션하기에 편하다.
+ path 요소
  - path는 **d 속성** 이 결정하는 도형의 영역
  - 도형은 열리거나 닫힐 수 있는데, 종점이 시작점에 연결되면 닫힌 도형, 그렇지 않으면 열린 도형
  - **d 속성** 은 주로 다음 세가지 방법 중 하나로 생성한다.
    * 원, 직사각형, 다각형 등 기본 도형을 이용한다.
    * 어도비 일러스트레이터나 잉크스케이프 등의 벡터 그래픽 편집기로 그린다.
    * 직접 만든 생성자나 D3에 내장된 생성자에 파라미터를 입력해 그린다.
##### 자바스크립트
+ 메서드 체이닝
```javascript
d3.selectAll("div").data(someData).enter().append("div").html("Wow")
  .append("span").html("Even More Wow").style("font-weight", "900")
```
___
