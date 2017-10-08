___

## 7. HTTP 통신과 RxJS

- RxJS = Observable, Observer, Subscription

- 하나의 데이터를 발생시키는 RxJS예제코드
```javascript
const subscribeFn = function(observer) {
    observer.next('최초의 RxJS');
}
const myFirstObservable = new Rx.Observable(subscribeFn);

myFirstObservable.subscribe((d) => console.log(d));

```

- 연속해서 데이터를 발생시키는 Observable
``` javascript
const number$ = Rx.Observable.create((observer) => {
    let myCounter = 0;
    const producerFn = () => observer.next(myCounter++);

    const intervalId = setInterval(producerFn, 1000);

    const finishFn = () => {
        clearInterval(intervalId);
        observer.complete();
    }

    setTimeout(finishFn, 10 * 1000);

    return finishFn;
});

const subscription = number$.subscribe((n) => console.log(`streaming... ${n}`))l

setTimeout(() => subscription.unsubscribe(), 5 * 1000);
```

- RxJS 연산자 활용
  + RxJS에서는 Observable이 만들어내는 데이터를 Observer에서 구독하는 과정 사이에 다양한 연산자를 추가하여 데이터를 가공하거나 복잡한 비동기 이벤트 간의 처리를 손쉽게 선언 할 수 있음.

  + RxJS의 map 연산자 활용
  ```javascript
  const num$ = Rx.Observable.from([1,2,3,4,5,6,7,8,9,10]);

  num$
    .map(n => n*2)
    .Subscribe(n => console.log(`num: ${n}`));
  ```
  + RxJS의 filter 연산자 활용
  ```javascript
  const num$ = Rx.Observable.from([1,2,3,4,5,6,7,8,9,10]);

  num$
    .filter(n => n % 2 === 0)
    .subscribe(n => console.log(`num: ${n}`));
  ```
  + RxJS의 reduce 연산자 활용
  ```javascript
  const num$ = Rx.Observable.from([1,2,3,4,5,6,7,8,9,10]);

  num$
    .filter(n => n % 2 === 0)
    .reduce((acc, val) => acc + val, 0)
    .subscribe(res => console.log(`result: ${res}`));
  ```
  + RxJS의 zip 연산자를 활용한 Observable 조합
  ```javascript
  untilFive$ = Rx.Observable.interval(1000).take(5);
  const num$ = Rx.Observable.from([1,2,3,4,5,6,7,8,9,10]).map(n => n * 2);

  Observable.zip(num$, untilFive$, (num, int) => num).subscribe(n => console.log(n));
  ```
___

## 8. 폼 다루기

- HTML5 Form Validation
  + required, pattern, minlength, maxlength
- FORM 요소에 "novalidate" 속성을 추가하게 되면..
  + 브라우저에서 네이티브(HTML5 Form Validation)로 지원하던 검증과 에러 출력을 하지 않음
  + 각 입력 요소의 FormControl에 검증 규칙을 주입
    * FormsModule은 RequiredValidator, PatternValidator, MinLengthValidator, MaxLengthValidator 지시만 기본으로 제공
- 커스텀 Validator 구현
  + FormsModule에 포함된 Validator 인터페이스를 구현 해야함.
  + 예제 코드
  ```typescript
  import { Directive, Input, forwardRef } from '@angular/core';
  import { AbstractControl, Validator } from '@angular/forms';

  export const CUSTOM_VALIDATOR: any = {
    provide: NG_VALIDATORS,
    useExisting: forwardRef(() => customValidator),
    multi: true
  }

  // Directive 어노테이션의 CSS 선택자 문법으로 속성명을 정의하는 일
  // 아래와 같이('[myOwnValidate][ngModel]') 정의를 한다면 'ngModel'과 함께 'myOwnValidate' 속성이 선언된 입력 요소에 CustomValidator를 적용하겠다는 선언을 의미
  @Directive({
    selector: '[myOwnValidate][ngModel]',
    providers: [CUSTOM_VALIDATOR]
  })

  export class CustomValidator implements Validator {
    validate(control: AbstractControl): { [key: string]: any} {
      //control.value의 유효성을 검사하여 값이 유효할 떄는 null, 그렇지 않을 경우에는 에러 정보의 객체를 반환
      return null
    }
  }
  ```
- 템플릿 주도형 폼 / 반응형 폼 / 동적 폼 각각의 차이 확인..추후-_...

___

## 9. 앵귤러 동작 원리

- 부트스트랩
  + 애플리케이션의 최초 진입점
  + 'src/main.ts'
  ```typescript
  import { enableProdMode } from '@anugular/core';
  import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
  import { AppModule } from './app/app.module';
  import { environment } from './environments/environment';

  if (environment.production) {
    enableProdMode();
  }

  platformBrowserDynamic().bootstrapModule(AppModule);
  ```
- Zone.js와 변화 감지
  + 앵귤러를 움직이게 만드는 세 가지 이벤트
    - 사용자 액션
    - Ajax와 같은 외부 호출
    - setTimeout, interval 함수
  + Zone.js를 활용한 비동기 코드 감지
  + 컴포넌트마다 자신만의 [변화 감지기](https://www.slideshare.net/deview/114angularvs-react)를 가지고 있음
