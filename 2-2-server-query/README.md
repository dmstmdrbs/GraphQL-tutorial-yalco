## Apollo Server에서 Query 및 Type 작성해보기

### Query 구현하기

#### Query 루트 타입

```javascript
  type Query{
    teams: [Team]  /* teams의 반환 Type은 Team*/
  }
```

- **자료 요청**에 사용될 쿼리들을 정의
- 쿼리 명령문마다 **반환될 데이터 형태**를 지정

#### Type

```javascript
  type Team{
    id: Int
    manager: String
    ...
    supplies: [Supply]
  }

  type Supply{
    id: Int
    team: Int
  }
```

- 반환될 데이터의 형태를 지정
- 자료형을 가진 필드들로 구성

#### Resolver

```javascript
const resolvers = {
  Query: {
    teams: () => database.teams,
  },
};
```

- **Query**란 object의 항목들로 데이터를 반환하는 함수 선언
  - 함수에서 filter, map 등을 이용해 조건에 맞는 데이터를 리턴하도록 구현한다.
- 실제 프로젝트에서는 MySQL 조회 코드 등..

### Query 예제

#### equipments를 반환하는 쿼리 만들어 보기

- dbtester.js 생성

```js
const database = require("./database");
console.log(database.equipments);
```

- equipment의 데이터 타입 선언

```js
  type Equipment {
      id: String,
      used_by: String,
      count: Int,
      new_or_used: String
  }
```

- 데이터베이스에서 equipments를 추출하여 반환하는 resolver 선언

```js
Query: {
  // ...
  equipments: () => database.equipments;
}
```

- 쿼리 요청해보기 in http://localhost:4000

```js
  query {
      equipments {
          id
          used_by
          count
          new_or_used
      }
  }
```

### 특정 Team만 받아오기

- **args**로부터 주어진 id에 해당하는 Team만 **필터링**하여 반환

```js
/*
  실제 서버 코드에서는 MySQL 등에서 활용되는 쿼리문이 들어갈 것이지만,
  강의에서는 더미 데이터를 이용하기 때문에 js를 이용하여 Query를 구현
*/
  Query: {
    //...
    team: (parent, args, context, info) => database.teams
        .filter((team) => {
            return team.id === args.id
        })[0],
  }
```

- id를 인자로 받아 하나의 Team데이터를 반환하는 **Query 타입 지정**

```js
type Query {
    ...
    team(id: Int): Team
}
```

- in http://localhost:4000

```js
query {
  team(id: 1) {
      id
      manager
      office
      extension_number
      mascot
      cleaning_duty
      project
  }
}
```

### Team에 supplies추가 및 연결

- Team 목록을 반환 시 해당하는 supplies를 supplies 항목에 추가

```js
/*
  실제 서버 코드에서는 MySQL 등에서 활용되는 쿼리문이 들어갈 것이지만,
  강의에서는 더미 데이터를 이용하기 때문에 js를 이용하여 Query를 구현
*/
Query: {
    // ...
    teams: () => database.teams
    .map((team) => {
        team.supplies = database.supplies
        .filter((supply) => {
            return supply.team === team.id
        })
        return team
    }),
}
```

- Team 타입에 Supply 배열 추가

```js
type Team {
    id: Int
    manager: String
    office: String
    extension_number: String
    mascot: String
    cleaning_duty: String
    project: String
    supplies: [Supply]
}
```
