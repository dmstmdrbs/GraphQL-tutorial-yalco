## Apollo Server와 CSV를 이용한 DB 설정

```javascript
const { ApolloServer, gql } = require("apollo-server");
```

- ApolloServer

  - typeDef와 resolver를 인자로 받아 서버 생성

  ```javascript
  const typeDefs = gql`
    type Query {
      teams: [Team]
    }
    type Team {
      id: Int
      manager: String
      office: String
      extension_number: String
      mascot: String
      cleaning_duty: String
      project: String
    }
  `;
  const resolvers = {
    Query: {
      teams: () => database.teams,
    },
  };
  const server = new ApolloServer({ typeDefs, resolvers });
  ```

  - typeDef
    - GraphQL 명세에서 사용될 데이터, 요청의 타입 지정
    - gql(template literal tag)로 생성됨
  - Resolver
    - 서비스의 **액션**들을 함수로 지정
    - 요청에 따라 데이터를 반환, 입력, 수정, 삭제
