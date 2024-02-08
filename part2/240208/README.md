# Part2 멘토링 (240208)

## 목차

- part1 코드 돌아보기
- 7주차 위클리 미션 찝어보기
- React 컨벤션 모아보기
- 기타 등등

## part1 코드 돌아보기

### 뭔가 이상한데

- typo
- 예측 불가능하게 움직여라

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

- 왠만하면 try-catch를 사용하자

```js
const user = await getUser({ id: userId });
```
