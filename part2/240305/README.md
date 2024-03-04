# Part2 멘토링 (240305)

## 목차

- Mutation
- 코드 같이 보기

## Mutation

### 다시 조회하기

```js
const addPost = async (post) => {
  await postApis.addPost(post);

  const response = await postApis.getPosts();
  setPosts(response.data);
};
```

### response 이용하기

```js
const addPost = async (post) => {
  const response = await postApis.addPost(post);

  setPosts((prev) => [response, ...prev]);
};
```

### 직접 가공하기

```js
const addPost = async (post) => {
  await postApis.addPost(post);

  setPosts((prev) => [post, ...prev]);
};
```

### 낙관적 업데이트(optimistic update)

```js
const likePost = async (id) => {
  const prevState = liked;

  setLiked((prev) => !prev);

  try {
    await postApis.likePost(id);
  } catch (error) {
    setLiked(prevState);
  }
};
```

## 코드 같이 보기

### props 고민하기

```js
function FloatingButton({ isMobile, onClick }) {
  return (
    <Button onClick={onClick}>
      <Text>{isMobile ? '질문 작성' : '질문 작성하기'}</Text>
    </Button>
  );
}

export default FloatingButton;
```

### 스타일드 컴포넌트

```js
const Temp = styled.div`
  position: absolute;
  top: 25px;
`;

// 왜 안될까
// const DropdownMenu = styled(EditDropdownMenu)`
//   position: absolute;
//   top: 25px;
// `;
```

### Production

```js
function FeedCard({ questionId, answer, content, createdAt, like, subjectId, state = "sent" }) {
  const [isEditMenuVisible, setEditMenuVisible] = useState(false);

  const handleClick = () => {
    console.log(isEditMenuVisible);
    setEditMenuVisible(!isEditMenuVisible);
  };

  return (
    ...
  )
}
```
