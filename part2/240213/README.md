# Part2 멘토링 (240213)

## 목차

- React 컨벤션 조금만 더 보기
- 코드 리뷰 같이 보기

## React 컨벤션 조금만 더 보기

### import도 순서가 있어요

- builtin
- internal
- external
- [eslint-plugin-import](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/order.md)
- [참조](https://pozafly.github.io/environment/putting-rules-into-import-syntax-with-eslint/)

```js
import { useState } from 'react';
import axios from 'axios';

import Folder from '../components';

import { ROUTES_PATHS } from '../constants';

import { formatDate } from '../utils';
```

```js
import { useState } from 'react';
import axios from 'axios';

import Folder from '../components';
import { ROUTES_PATHS } from '../constants';
import { formatDate } from '../utils';
```

### Styled-Components

- 컴포넌트와 같이 두기

```js
export default function Header() {
  return (
    <Container>
      <Wrapper>
        <Left>Logo</Left>
        <Right>로그인</Right>
      </Wrapper>
    </Container>
  );
}

const Container = styled.header`
...
`;

const Wrapper = styled.div`
...
`;

const Left = styled.div`
...
`;

const Right = styled.div`
...
`;
```

- 별도의 파일로 분리하기
- Header.styled.ts

```js
export const Container = styled.header`
...
`;

export const Wrapper = styled.div`
...
`;

export const Left = styled.div`
...
`;

export const Right = styled.div`
...
`;
```

### 라우트 경로도 상수로 관리해요

- 휴먼 에러 방지

```js
export const ROUTE_PATHS = {
  HOME: '/home',
  PRODUCT: '/product',
  FOLDER: '/folder',
};
```

### 반복되는 라우트도 상수로 관리해요

```js
const ROUTES = [
  {
    path: ROUTE_PATHS.HOME,
    element: <Home />,
  },
  {
    path: ROUTE_PATHS.PRODUCT,
    element: <Product />,
  },
];

return (
  <Routes>
    {ROUTES.map({path, element} => <Route key={path} path={path} element={element} />)}
  </Routes>
)
```
