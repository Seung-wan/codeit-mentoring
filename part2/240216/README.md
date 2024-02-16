# Part2 멘토링 (240216)

## 목차

- 라이브 코딩하기
  - custom hook 만들기
  - httpClient 만들기
  - 환경 변수 만들기
- 코드 리뷰 같이 보기
- 개발자로서의 마음가짐

## custom hook 만들기

- 커스텀훅이라는 용어가 낯설지만... part1에서도 우리는 해본적이 있다.
  - 자바스크립트 모듈화
  - 함수로 분리하는데, 그 분리하는 대상이 React Hook인 것
- 사실 핵심은 로직을 어떤 기준으로 분리할지 선택하는 것

```js
import { useState, useEffect } from 'react';

export default function StatusBar() {
  const [isOnline, setIsOnline] = useState(true);

  useEffect(() => {
    function handleOnline() {
      setIsOnline(true);
    }

    function handleOffline() {
      setIsOnline(false);
    }

    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);

    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);

  return <h1>{isOnline ? '✅ Online' : '❌ Disconnected'}</h1>;
}
```

```js
function StatusBar() {
  const isOnline = useOnlineStatus();

  return <h1>{isOnline ? '✅ Online' : '❌ Disconnected'}</h1>;
}
```

## httpClient 만들기

- http 통신을 도와주는 매개체
- Next13에서는 fetch를 쓰지만...
- axios, ky

```js
const result1 = httpClient.get('/users');
const result2 = httpClient.post('/users', { id, email });
const result3 = httpClient.put('/users/1', { description });
const result4 = httpClient.delete('/users/1');
```

- 왜 axios를 그대로 사용하지 않을까?
  - 구체적인 구현에 의존하지 않게 만든다
  - Inversion of Control
  - Dependency Inversion Principle

## 환경변수(Environment Variables) 만들기

- 환경변수란?
- local, development, production
- 프로젝트나 번들러마다 다르다

```env
REACT_APP_API_BASE_URL=https://domain.com
...
```

## 코드 리뷰 같이 보기

### 에러처리는 어떻게?

- 에러가 발생하면 어떻게 해야할까
  - retry
  - error message(popup, toast, text)
  - error page

```js
const getUser = async () => {
  const response = await fetch('');

  if (response.ok === false) {
    throw new Error('...');
  }

  const res = await response.json();
  return res;
};

function Component() {
  useEffect(() => {
    const fetchUser = async () => {
      try {
        const user = await getUser();
      } catch (error) {
        // TODO: Error Handling
        // ...
      }
    };
  }, []);
}
```

### 로딩 처리는 어떻게?

- query
  - Spinner
  - Skeleton
  - null
- mutation
  - Spinner
  - null

### let 줄이기
- const, const, const
  
```js
const handleLoadProfile = async () => {
  let result;
  try {
    setNoneProfile(false);
    result = await getProfile();
  } catch (error) {
    setNoneProfile(true);
    return;
  }
  const { email, profileImageSource } = result;
  setUserEmail(email);
  setUserProfileImgSrc(profileImageSource);
};
```

## 개발자로서의 마음가짐

### 우리는 프로다

- 겸손 / 정리 / 메모 / 객관화
- 원칙(TDD / OOP / FP / Pair / CI) + 행동(도전 / Agile)
- 품질
- 스케쥴링 / 시간관리
- 독서
- 오픈소스 기여

### 팀, 사람

- 큰일은 작은 일을 하는 작은 팀들의 협업으로 이루어진다
- 팀 = 사람
- 외부 방해는 당연한다 (All at once / Slack / 도움 요구)
- 개발에 영웅은 없다
