# Part2 멘토링 (240208)

## 목차

- part1 코드 돌아보기
- 7주차 위클리 미션 찝어보기
- React 컨벤션 모아보기
- 기타 등등

## part1 코드 돌아보기

### html lang 속성

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>FAQ</title>
  </head>
</html>
```

### 뭔가 이상한데

```html
<div class="external_link">
  <a
    href="https://www.facebook.com/"
    target="_blacnk"
    class="icon facebook"
  ></a>
  <a href="https://twitter.com/" target="_blacnk" class="icon twitter"></a>
  <a href="https://www.youtube.com/" target="_blacnk" class="icon youtube"></a>
  <a
    href="https://www.instagram.com/"
    target="_blacnk"
    class="icon instagram"
  ></a>
</div>
```

### document.querySelector

```js
export const email = document.querySelector('#sign_email');
export const password = document.querySelector('#sign_password');
export const emailError = document.querySelector('#error_email');
export const passwordError = document.querySelector('#error_password');
export const eyeButton = document.querySelector('.eye_button');
```

### is-\*, has-\* prefix

```js
const isValidEmail = ...;
const [isOpen, setIsOpen] = useState(false);
const isAdmin = true;
const hasSpecialCard = false
const 건강보험에_가입하였는가=true
```

### 상수는...

```js
const emailRegex = /.../;
const PASSWORD_REGEX = /.../;

// JavaScript
const REGEX = Object.freeze({
  EMAIL: /.../,
  PASSWORD: /.../,
});
```

```ts
// TypeScript
const REGEX = {
  EMAIL: /.../,
  PASSWORD: /.../,
} as const;
```

### 계층이라는 것

- 계층, 레이어
  - 공장을 생각해보자
- 책임
- 우리는 실제로 분업을 한다
- 레이어드 아키텍처, 클린 아키텍처
- 프론트엔드에서는?
  - UI
  - Business Logic
    - Domain
    - api
    - Serialization

### api를 다룰 때는

- 일반적으로는 try-catch를 사용하자

```js
// try-catch
try {
  const user = await getUser({ id: userId });

  if (user.role === 'student') {
    학생증발급(user);
  }

  if (user.role === 'admin') {
    통과();
  }
} catch (error) {
  팝업_오픈(error);
}

// then
getUser({ id: userId })
  .then((user) => {
    if (user.role === 'student') {
      학생증발급(user);
    }

    if (user.role === 'admin') {
      통과();
    }
  })
  .catch((error) => 팝업_오픈(error));
```

- 상태에 대한 처리가 생각보다 중요하다.
  - pending
  - fulfilled
  - rejected

```js
const [data, setData] = useState([]);
const [isLoading, setIsLoading] = useState(false);
const [error, setError] = useState();

if (isLoading) {
  return <Spinner />;
}

if (error) {
  return <div>{error.message}</div>;
}

return <div>비동기 처리 후 보여지는 데이터</div>;
```

```js
// react-query -> @tanstack/react-query
const {data, isLoading, error} = useQuery(...)

if (isLoading) {
  return <Spinner />;
}

if (error) {
  return <div>{error.message}</div>;
}

```

```jsx
// 선언적으로 다룬다.
return (
  <ErrorBoundary fallback={<ErrorComponent />}>
    <Suspense fallback={<Skeleton>}>
     <Profile />
    </Suspense>
  </ErrorBoundary>
)
```

### 추상화라는 것

- 짧은 함수가 무조건 좋은 건 아니다
- 코드에는 의도가 명확히 드러나야 한다
- 예상할 수 있는 코드를 짜야한다

```js
// 검증을 하는 함수
export function validateEmail() {
  const isEmpty = email.value.trim() === '';

  if (isEmpty) {
    createError(email, emailError, '이메일을 입력해 주세요.');
    return false;
  }
  if (!isValidEmail(email.value)) {
    createError(email, emailError, '올바른 이메일 주소가 아닙니다.');
    return false;
  }
  clearError(email, emailError);
  return true;
}
```

### 그 외 잡다한 것들

- 들여쓰기는 2칸이다
- 개행은 생각보다 중요하다
- localStorage도 모듈화를 해서 쓰는 게 좋다
- short hand property

```js
const userInfo = {
  email: email,
  password: password,
};
```

### 이벤트의 차이점

- focusout vs blur
- React에서는 이벤트를 특이하게 다룬다.
  - React 16은 html
  - React 17은 root Dom container
  - SyntheticEvent
  - [참조](https://handhand.tistory.com/287)

## 7주차 위클리 미션 찝어보기

- Mobile First
- 리액트에 적응하자
- Date 다루기
  - moment
  - dayjs
  - date-fns

```js
/**
카드 컴포넌트에서 createdAt 데이터 기준으로 현재 Date와 차이에 따라 아래와 같은 규칙으로 설정해 주세요.
2분 미만은 “1 minute ago”
59분 이하는 “OO minutes ago”
60분 이상은 “1 hour ago”
23시간 이하는 “OO hours ago”
24시간 이상은 “1 day ago”
30일 이하는 “OO days ago”
31일 이상은 “1 month ago”
11달 이하는 “OO months ago”
12달 이상은 “1 year ago”
OO달 이상은 “{OO/12(소수점 버린 정수)} years ago”
*/
```

- 커스텀 훅은 매우 중요하다
  - 무조건 use- prefix를 사용한다

```js
const [user, setUser] = useState();
const [isLoading, setIsLoading] = useState(false);
const [error, setError] = useState();

useEffect(() => {
  const fetchUser = async () => {
    const res = await getUser(id);

    setUser(res.user);
  };
}, []);

fetchUser();

return (
  <div>
    <div>{user.name}</div>
    <div>{user.age}</div>
  </div>
);
```

```js
const { user, isLoading, error } = useGetUser(id);

return (
  <div>
    <div>{user.name}</div>
    <div>{user.age}</div>
  </div>
);
```

### React 컨벤션 모아보기

- 컴포넌트

```js
function Profile () {
  ...
}

export default Profile
```

```js
export default function Profile() {
  ...
}
```

```js
export function Profile() {}
```

```js
const Profile = () => {};

export default Profile;
```

```js
export const Profile = () => {};
```

- useState

```js
const [user, setUser] = useState();
```

```js
const [showModal, setShowModal] = useState(false);

const [isOpenModal, setIsOpenModal] = useState(false);
const [isModalOpen, setIsModalOpen] = useState(false);
```

- 컴포넌트

  - UserProfile.jsx
  - userProfile.jsx
  - user-profile.jsx
  - UserPage.jsx
  - user.page.jsx
  - page.jsx

- 이벤트 핸들러

  - handle-
  - on-
  - 행위

- 폴더 구조

  - assets
  - apis
  - components
  - hooks
  - pages
  - stores(atoms, recoil등)
  - types
  - styles
  - utils

- props

  - interface vs type

  ```tsx
    interface Props {
      title: string;
    }

    export default function Title({title}:Props) {
      return ...
    }

    interface TitleProps {
      title: string;
    }

    export default function Title({title}:TitleProps) {
      return ...
    }

    type TitleProps = {
      title: string;
    }

    export default function Title({title}:TitleProps) {
      return ...
    }
  ```

  - T-, I- Prefix

  ```tsx
  interface Lion {
    할퀴기: () => void;
    물어뜯기: () => void;
  }

  interface ILion {
    할퀴기: () => void;
    물어뜯기: () => void;
  }

  type TLion =  {
    할퀴기: () => void;
    물어뜯기: () => void;
  }

  const lion:Lion = {...}
  const lion:ILion = {...}
  const lion:TLion = {...}

  ```

  - Destructuring

  ```tsx
  interface Props {
    title: string;
    subTitle: string;
    content: string;
    confirmButtonText: string;
    cancelButtonText: string;
  }

  export default function Modal({title, subTitle, content, confirmButtonText, cancelButtonText}:Props) {
    return ...
  }

  export default function Modal(props:Props) {
    const {title, subTitle, content, confirmButtonText, cancelButtonText} = props

    return ...
  }

  ```

- 함수 선언식 vs 화살표 함수

```js
function commaize() {
  return ...
}

const commaize = () => {
  return ...
}
```

### 기타 등등

- 컴포넌트를 잘 만들기 위해 기존 라이브러리들을 공부하자

  - shadcn/ui
  - radix ui
  - material ui

- 기술에 너무 종속되지 말자

  - 우리는 세상의 문제를 해결하는 사람들이다
  - 마음을 얻지 못하면 아무 의미가 없다
  - Product Engineer
  - Maker

- 모두 외울 필요는 없다
  - 저도 몰라요
  - 근데 찾아서 쓸 줄은 알아요
