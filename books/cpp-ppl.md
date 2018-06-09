## PPL
+ PPL(Paralled Patterns Library)
  - 스레드 대신 병렬 작업을 추상화 시킨 `task` 개념을 사용
+ task를 추상화환 `task_handle` 클래스
  - task_handle 클래스는 매우 단순한 구조의 템플릿 클래스로, 실행할 작업을 함수 포인터, 함수 객체, 람다 표현식 등의 형태로 전달받아 객체를 생성
+ task group
  - `structured_task_group`
    * PPL에서 말하는 태스크 그룹은 일반적으로 `structured_task_group` 클래스를 의미
  - `task_group` class
    * `structed_task_group` 과의 차이점
      * 메소드의 스레드 세이프 여부, 이는 `structured_task_group` 클래스와 `task_group` 클래스를 구분하는 가장 큰 `차이점`이다. structed_task_group 클래스는 스레드 세이프하지 않기 때문에 객체를 생성한 스레드 안에서만 메소드를 호출해야 한다는 제약이 따른다. 스레드 세이프 규칙에는 한가지 예외 사항이 있는데, `cancel` 메소드와 `is_canceling` 메소드는 두 클래스 모두 다른 스레드에서 호출해도 안전하다는 점이다. 이러한 예외 규칙 덕분에 `structured_task_group`객체가 실행하는 task 내부에서도 cancel 메소드를 호출하여 task group을 취소 할 수 있다.
      * 두 번째 차이점은 wait 메소드를 호출하여 task 종료를 대기하는 도중에 다른 task를 task group에 추가할 수 있는지 여부다.
      * 세 번째 차이점은 run 메소드의 파리미터 타입이다.
