# 기본세팅

## graphql, Apollo-server

## 세팅

```
npm install apollo-server graphql
```

- server.js 파일 생성 및 apollo 세팅

```js
const { ApolloServer, gql } = require('apollo-server');

// The GraphQL schema
const typeDefs = gql`
  type Query {
    "A simple type for getting started!"
    hello: String
  }
`;

// A map of functions which return data for the schema.
const resolvers = {
  Query: {
    hello: () => 'world World@@',
  },
};

const server = new ApolloServer({
  typeDefs,
  resolvers,
});

server.listen().then(({ url }) => {
  console.log(`🚀 Server ready at ${url}`);
});
```

## prisma (ORM)

### 설치

```
npm install prisma -D
npx prisma init
```

### postgresql 연결

- 맥: postgresapp, postico 설치
- 윈도우: pgadmin
- 데이터베이스 연결 URL 예제
- https://www.prisma.io/docs/reference/database-reference/connection-urls
- 로컬호스트에서는 비밀번호를 사용하지 않아도 된다.
- postgresql db 생성: 앱에서 postgres 더블클릭으로 터미널 실행 -> CREATE DATABASE {db명}

#### prisma 세팅

- schema.prisma 에 model 작성

```prisma
model Movie {
  id         Int      @id @default(autoincrement())
  title      String
  year       Int
  createdAt  DateTime @default(now())
  updatedAt  DateTime @updatedAt
}
```

- migration 적용

```prisma
npx prisma migrate dev
npx prisma migrate dev --name init // name을 init 으로 설정
```

- 해당명령어를 package.json 의 스크립트에 등록하여 사용한다.

```json
"scripts": {
  "dev": "nodemon --exec babel-node server.js",
  "migrate": "npx prisma migrate dev"
},
```

- prisma studio: phpMyAdmin 과 동일

```
npx prisma studio
```

### 파일 구조 새로 짜기

- queries, typeDefs, mutations 은 꼭 export defualt 를 해준다. 그래야지 graphql-tools 이 merge 할 수 있음!

client.js
schema.js
movies/movies.queries.js
movies/movies.typeDefs.js
movies/movies.mutations.js

### graphql-tools: 스키마 머지

```
npm i @graphql-tools/schema @graphql-tools/load-files @graphql-tools/merge
```

## PostgreSQL

### 명령어

- 모든 테이블 조회: SELECT \* FROM PG_TABLES;
