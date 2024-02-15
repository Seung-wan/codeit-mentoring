# Part2 멘토링 (240216)

## 목차

- 라이브 코딩하기
  - custom hook 만들기
  - httpClient 만들기
  - 환경 변수 만들기
- 코드 리뷰 같이 보기
- 개발자로서의 마음가짐

### custom hook 만들기

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

### httpClient 만들기
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
