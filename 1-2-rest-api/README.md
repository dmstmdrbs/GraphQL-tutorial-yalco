## REST API란?

- 소프트웨어간 정보를 주고 받는 방식
  - GraphQL 이전부터 사용되어 옴
  - GraphQL과는 다른 방식.

* URI와 요청 방식을 함께 나타냄
  - GET
  - POST
  - PUT/PATCH
  - DELETE
* 단점
  - 불필요한 정보까지 모두 받아와야 함. (Overfetching)
  - 내가 원하는 정보가 여러 계층에 걸쳐서 받아와야 한다면 한번에 받아오지 못하고 요청을 여러번 해야한다. (Underfetching)
