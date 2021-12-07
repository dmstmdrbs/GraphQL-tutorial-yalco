## 2-3 Server mutation 구현하기

### Mutation이란?

- *Mutation*은 데이터에 어떠한 수정을 가하는 것
  - *Query*는 요청하는 것
  - GraphQL에 어떠한 기술적 제한이 걸려있는 것은 아니다. Query에서도 쿼리를 어떻게 짰느냐에 따라서 서버 측 데이터에 수정을 가할 수도 있다.
  - 마찬지로 Mutation에서도 데이터를 가져오기만(fetch) 하도록 짤 수도 있다.
  - REST에서 get, post 등을 이용해 할 수 있는 것 처럼 개발자들간의 합의, 명세라고 생각하면 됨.
- 서버 측 데이터를 **생성, 수정, 삭제**를 담당. 즉, CRUD의 **Create, Update, Delete**를 담당
- **Mutation과 짝을 이루는 Resolver가 필요**
- **Resolver** 함수에 **비즈니스 로직**이 들어감.
  <br/>

### Mutation 구현하기

#### 데이터 삭제

---

##### Mutation - 삭제 루트 타입

```js
type Mutation {
  deleteEquipment(id: String): Equipment //Equipment를 삭제하면 삭제한 Equipment를 반환 받도록
}
```

- String타입 *id*를 argument로 받는 deleteEquipment
- deleteEquipment는 *Equipment*를 반환

##### 삭제 Resolver

```js
/* 
  실제 서버 코드에서는 MySQL 등에서 활용되는 쿼리문이 들어갈 것이지만,
  강의에서는 더미 데이터를 이용하기 때문에 js를 이용하여 Query, Mutation을 구현 
*/
Mutation: {
  deleteEquipment: (parent, args, context, info) => {
    const deleted = database.equipments.filter((equipment) => {
      return equipment.id === args.id;
    })[0];
    database.equipments = database.equipments.filter((equipment) => {
      return equipment.id !== args.id;
    });
    return deleted;
  };
}
```

#### 데이터 추가

---

##### Mutation - 추가 루트 타입

```js
type Mutation {
  innerEquipment(
    id: String,
    used_by: String,
    count: Int,
    new_or_used: String,
  ): Equipment //Equipment를 추가하면 추가 된 Equipment를 반환 받도록
}
```

- (id: String, used_by: String, count: Int, new_or_used: String)를 argument로 받는 innerEquipment
- innerEquipment는 *Equipment*를 반환

##### 추가 Resolver

```js
/*
  실제 서버 코드에서는 MySQL 등에서 활용되는 쿼리문이 들어갈 것이지만,
  강의에서는 더미 데이터를 이용하기 때문에 js를 이용하여 Query, Mutation을 구현
*/
Mutation: {
    insertEquipment: (parent, args, context, info) => {
        database.equipments.push(args)
        return args
    },
    //...
}
```

#### 데이터 수정

---

##### Mutation - 수정 루트 타입

```js
type Mutation {
    editEquipment(
        id: String,
        used_by: String,
        count: Int,
        new_or_used: String
    ): Equipment
    ...
}
```

- (id: String, used_by: String, count: Int, new_or_used: String)를 argument로 받는 innerEquipment
- innerEquipment는 *Equipment*를 반환

##### 수정 Resolver

```js
/*
  실제 서버 코드에서는 MySQL 등에서 활용되는 쿼리문이 들어갈 것이지만,
  강의에서는 더미 데이터를 이용하기 때문에 js를 이용하여 Query, Mutation을 구현
*/
Mutation: {
    // ...
    editEquipment: (parent, args, context, info) => {
        return database.equipments.filter((equipment) => {
            return equipment.id === args.id
        }).map((equipment) => {
            Object.assign(equipment, args)
            return equipment
        })[0]
    },
    // ...
}
```

#### 생각 정리

- *Query*는 데이터 `GET`, Mutation은 데이터 _Create, Update, Delete_ `Post`
- RESTful API에서 CRUD작업에 대해서 http 요청 방식이 규칙이 지정되어있는 것 처럼 GraphQL에서도 Query와 Mutation이라는 **규칙**으로 정해두었다고 생각하면 될 것 같다.
