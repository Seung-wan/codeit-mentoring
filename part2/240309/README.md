# # Part2 멘토링 (240305)

## 목차

- React Query
- JavaScript 이야기
- React 이야기

## React Query

### 해결하려는 문제

React에서 서버 상태 관리를 간소화해주는 라이브러리인 React-query에 대해 이야기하려고 합니다.  
React를 사용하면서 많은 개발자들이 마주치는 일반적인 문제 중 하나는 웹 애플리케이션과 서버 간의 데이터 교환을 효율적으로 관리하는 것입니다.  
이는 주로 데이터를 가져오고(fetching), 업데이트하고(updating), 캐싱하고(caching), 동기화하는(syncing) 과정을 포함합니다.  
기존의 상태 관리 라이브러리를 사용하면 이러한 작업을 수행할 수는 있지만, 대부분의 라이브러리는 클라이언트 상태 관리에 중점을 두고 있으며, 서버 상태 관리를 위한 명확한 해결책을 제공하지 않습니다.  
이로 인해 데이터 패칭 로직이 반복되며, 성능 최적화와 관련된 복잡한 문제들이 발생하게 됩니다.

### 기존 상태관리 라이브러리와의 비교

React의 상태 관리는 주로 Redux, MobX, Context API와 같은 도구를 사용하여 이루어집니다.
이러한 도구들은 애플리케이션의 상태를 중앙에서 관리하고, 구성 요소 간에 상태를 공유하는 데 매우 효과적입니다.
그러나 서버에서 가져온 데이터를 관리하기 시작하면, 이러한 도구들만으로는 비효율적일 수 있습니다.
데이터를 가져오는 로직이 반복되고, 캐싱, 동기화, 데이터 업데이트 등 추가적인 관리가 필요하게 됩니다.
이러한 과정은 개발자가 직접 관리해야 할 복잡성을 증가시킵니다.

### React-query의 주요 특징 및 장점

React-query는 이러한 문제에 대한 해결책을 제공합니다.
React-query를 사용하면 서버 상태를 쉽게 가져오고, 캐시하며, 동기화할 수 있습니다.
React-query는 다음과 같은 주요 특징을 가지고 있습니다:

- 자동 데이터 캐싱: 데이터는 자동으로 메모리에 캐시되어, 사용자 경험을 개선하고 서버 요청 수를 줄여줍니다.
- 백그라운드 업데이트 및 데이터 동기화: 애플리케이션이 백그라운드에 있거나 새로운 데이터가 필요할 때 자동으로 데이터를 업데이트합니다.
- 효율적인 데이터 패칭: 여러 구성 요소에서 동일한 데이터에 대한 요청을 최적화하여 중복 요청을 방지합니다.
- 쉬운 데이터 업데이트 및 무효화: 데이터를 간편하게 업데이트하고 무효화할 수 있어, 항상 최신 상태의 데이터를 유지할 수 있습니다.

### 기본적인 데이터 가져오기

React-query의 useQuery 훅을 사용하면, 서버에서 데이터를 비동기적으로 가져올 수 있습니다.
이 훅은 쿼리 키(query key)와 데이터를 가져오는 비동기 함수를 인자로 받습니다.
쿼리 키는 해당 쿼리를 유일하게 식별하는 데 사용되며, 캐싱 및 데이터 업데이트에 필수적입니다.

```js
import { useQuery } from 'react-query';

function FetchPosts() {
  const { isLoading, error, data } = useQuery('posts', fetchPosts);

  async function fetchPosts() {
    const response = await fetch('https://example.com/api/posts');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  }

  if (isLoading) return 'Loading...';

  if (error) return 'An error has occurred: ' + error.message;

  return (
    <div>
      {data.map((post) => (
        <p key={post.id}>{post.title}</p>
      ))}
    </div>
  );
}
```

### 데이터 쓰기 (useMutation 훅 사용하기)

useMutation 훅은 비동기 요청(예: 데이터 추가, 수정, 삭제)을 수행하고 그 결과를 처리하는 데 사용됩니다.
useMutation은 실행 함수를 반환하며, 이 함수를 호출하여 실제 비동기 요청을 실행할 수 있습니다.
요청의 결과에 따라 성공, 오류, 로딩 상태를 관리할 수 있습니다.

```js
import { useMutation, useQueryClient } from 'react-query';

function AddPost() {
  const queryClient = useQueryClient();
  const mutation = useMutation(
    (newPost) => {
      return fetch('https://example.com/api/posts', {
        method: 'POST',
        body: JSON.stringify(newPost),
        headers: {
          'Content-type': 'application/json; charset=UTF-8',
        },
      });
    },
    {
      onSuccess: () => {
        // 성공 시, 'posts' 쿼리 무효화 및 재패칭
        // 참고: https://pozafly.github.io/react-query/mutation-after-data-update/
        queryClient.invalidateQueries('posts');
      },
    }
  );

  const handleSubmit = async () => {
    const newPost = { title: 'React Query', content: 'Best practices' };
    mutation.mutate(newPost);
  };

  return (
    <div>
      <button onClick={handleSubmit}>Add Post</button>
      {mutation.isLoading && <p>Adding...</p>}
      {mutation.isError && <p>Error adding the post</p>}
      {mutation.isSuccess && <p>Post added!</p>}
    </div>
  );
}
```

### 데이터 업데이트 및 무효화하기

데이터를 수정하거나 삭제한 후에는 관련된 쿼리를 무효화하여 새로운 데이터로 업데이트해야 할 필요가 있습니다.
React-query의 invalidateQueries 메소드를 사용하면 특정 쿼리 또는 모든 쿼리를 쉽게 무효화할 수 있습니다.
이를 통해 데이터의 일관성을 유지하고, 사용자에게 최신 상태의 UI를 제공할 수 있습니다.

위의 예시에서는 onSuccess 옵션을 사용하여 요청이 성공적으로 완료된 후 posts 쿼리를 무효화하고 있습니다.
이로써 서버에 새로운 데이터가 추가된 후에 자동으로 목록을 업데이트할 수 있게 됩니다.

### Optimistic Updates 구현하기

Optimistic Updates는 서버 응답을 기다리지 않고 UI를 미리 업데이트하는 방식입니다.
사용자에게 더 빠른 피드백을 제공할 수 있어 UX를 개선합니다.
useMutation의 onMutate 옵션을 사용하여 Optimistic Update를 구현할 수 있습니다.

```js
const mutation = useMutation(editPost, {
  onMutate: async (newPost) => {
    await queryClient.cancelQueries('posts');
    const previousPosts = queryClient.getQueryData('posts');
    queryClient.setQueryData('posts', (old) =>
      old.map((post) =>
        post.id === newPost.id ? { ...post, ...newPost } : post
      )
    );
    return { previousPosts };
  },
  onError: (err, newPost, context) => {
    queryClient.setQueryData('posts', context.previousPosts);
  },
  onSettled: () => {
    queryClient.invalidateQueries('posts');
  },
});
```

### Pagination

```js
const { isLoading, isError, error, data } = useQuery(
  ['projects', pageNumber],
  fetchProjects
);
```

### Infinite Scroll

```js
const { data, fetchNextPage, hasNextPage, isFetchingNextPage } =
  useInfiniteQuery('projects', fetchProjects, {
    getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  });
```

### enabled option

```js
const { data: userData } = useQuery(['user', userId], fetchUser);
const { data: projectData } = useQuery(
  ['projects', userData?.projectId],
  fetchProjects,
  {
    enabled: !!userData?.projectId,
  }
);
```

### custom hook

```js
function useGetUsersQuery() {
  return useQuery({
    querykey: ['users'],
    queryFn: userApi.getUsers,
  });
}

function usePostUserMutation({data, onSuccess, onError}) {
  return useMutation({
    mutationFn: () => userApi.postUser(data)
    onSuccess,
    onError
   }
  );
}
```

### 캐싱 전략

리액트 쿼리는 쿼리 키를 기반으로 캐싱을 관리한다.

- refetch... 옵션들
- staleTime
- cacheTime

---

## JavaScript 이야기

### 배열의 마지막 요소에 접근할 때

```js
const arr = [1, 2, 3];

arr[arr.length - 1];

arr.at(-1);

last(arr);
```

### 배열에 접근할때

```js
const 거래시간 = [시작_시간, 종료_시간];

거래시간[0];
거래시간[1];

const 시작_시간 = 거래시간[0];
const 종료_시간 = 거래시간[1];

const [시작_시간, 종료_시간] = 거래시간;
```

### 명시적으로 비교해요

```js
const questionCount = ...;

if(!questionCount) {
  return '질문이 없어요.'
}

if(questionCount === 0) {

}
```

### 리턴값이 많아요

```js
const [questions, next, setQuestions, setNext] = useFetchQuestions(
  id,
  questionCount
);

const { questions, next, setQuestions, setNext } = useFetchQuestions(
  id,
  questionCount
);
```

### is-\*, has-\*, disabled, readonly,

```js
const isOpen = ...;
const hasComputer = ...;
const disabled = ...;
const readonly = ...;
```

### -value, -data

```js
const inputValue = ...;
const userData = ...;
```

### 네이밍 고민하기

```js
const REGEX = {
  SPECIAL_CHARS_REGEX: /[!@#$%^&*()_+=[\]{};':"\\|,.<>/?~]/,
};
```

## React 이야기

### useEffect에 기명 함수를 전달해요

```js
useEffect(() => {
  const options = {
    serverUrl: serverUrl,
    roomId: roomId,
  };
  const connection = createConnection(options);
  connection.connect();
  return () => connection.disconnect();
}, [roomId, serverUrl]);

useEffect(
  function 채팅룸에_연결한다() {
    const options = {
      serverUrl: serverUrl,
      roomId: roomId,
    };

    const connection = createConnection(options);
    connection.connect();

    return () => connection.disconnect();
  },
  [roomId, serverUrl]
);
```

### Props 다루기

```js
<Card title={'에스파짱'} description='짱짱' hasAnimation={true} />

<Card title='에스파짱' description='짱짱' hasAnimation />
```

### displayName

```js
const InputText = forwardRef((props, ref) => {
  return <input type='text' ref={ref}>
})

InputText.displayName = 'InputText';
```
