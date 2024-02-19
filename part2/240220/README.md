# Part2 멘토링 (240220)

## 목차

- 동작하는 코드 만들기
- 코드 리뷰 같이 보기
- 페어프로그래밍 하기

## 동작하는 코드 만들기

### 만약 기업 과제를 한다면

- 요구사항을 모두 만족하는 평범한 코드
- 요구사항을 모두 만족하지는 않지만 클린 코드
- 우리의 선택은?

### 문제를 해결하기 위한 스텝을 매우 작게 나눈다

- 우리가 흔히 생각하는 작게보다 더 작게
- 글로 적으면서 생각을 정리하는게 핵심
- 작은 단위로 나눠야만 일을 하기 좋다

- 디자이너: 팝업 컴포넌트를 만들어주세요~
- A 개발자
  - 팝업 만들어야겠다
  - 대충 컴포넌트 만들고 디자인 보고 작업하고 하면 되겠지~
- B 개발자
  - 팝업을 만들어야 되는구나, 작업을 잘게 쪼개보자
    - 먼저 components/common 폴더 아래에 popup.jsx 파일을 만들자.
    - 파일을 생성하고 기본적인 컴포넌트 틀을 작성하자.
    - 스타일링은 스타일드 컴포넌트로 하면 되겠다.
    - 디자인을 보니까 여기서는 합성 컴포넌트 패턴을 활용하는게 좋겠다.
      - 아래 컴포넌트들을 만들자.
      - Dimmed
      - Title
      - SubTile
      - Description
      - Button
      - Divider
    - 팝업 컴포넌트를 다 만들고 나서는 정상적으로 화면에 보여지는지 테스트해야겠다.
    - 테스트까지 완료되면 PR을 올리고 스토리북은 디자이너분들에게도 공유해서 컨펌받아야겠다.

## 코드 리뷰 같이 보기

### useEffectOnce

```js
import { useEffect } from 'react';

export const useEffectOnce = (callback) => {
  useEffect(() => {
    callback();
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);
};

export default function Product() {
  const [products, setProducts] = useState([]);

  useEffectOnce(() => {
    const fetchProducts = async () => {
      const data = await getProducts();

      setProductList(data);
    };

    fetchProducts();
  });

  return <ProductList products={products} />;
}
```

### 데이터가 유효한지 체크하기

```js
export default function Product() {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    const fetchProducts = async () => {
      const data = await getProducts();

      setProductList(data);
    };

    fetchProducts();
  }, []);

  if (!products) {
    return <div>데이터가 없어요!</div>;
  }

  return <ProductList products={products} />;
}
```

## 페어프로그래밍 하기

### 집단 지성

### 러버덕 디버깅

### 함께 한다는 것
