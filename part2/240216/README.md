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
