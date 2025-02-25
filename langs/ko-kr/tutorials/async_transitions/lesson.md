`Suspense`를 사용하면 데이터가 로딩 중인 경우 폴백 컨텐츠를 표시 할 수 있습니다. 이는 초기 로딩에는 훌륭하지만, 이어지는 네비게이션에서는 스켈레톤 상태로 폴백하는 것이 사용자 경험에 더 나쁜 경우가 있습니다.

`useTransition`을 활용해 폴백 상태로 돌아가는 것을 방지할 수 있는데, 이는 래퍼와 pending 인디케이터를 제공합니다. 래퍼는 모든 비동기 이벤트가 완료될 때까지 커밋되지 않은 모든 다운스트림 업데이트를 트랜잭션에 넣습니다.

제어 흐름이 서스펜드되면, 다음 오프-스크린을 렌더링하는 동안은 계속 현재 브랜치를 표시하게 됩니다. 현재 경계 내에 있는 Resource를 읽는 경우, 이를 트랜지션에 추가합니다. 하지만, 새로 생성된 중첩된 `Suspense` 컴포넌트는 표시되기 전에 로딩이 완료되지 않은 경우 "폴백"을 표시합니다.

예제에서 탭을 이동하는 경우, 컨텐츠가 loading 플레이스홀더로 되돌아가는 것을 볼 수 있습니다. `App` 컴포넌트에 트랜지션을 추가해 보겠습니다. 먼저 `updateTab` 함수를 다음과 같이 변경합니다:

```js
const [pending, start] = useTransition();
const updateTab = (index) => () => start(() => setTab(index));
```

`useTransition`은 pending Signal 인디케이터와 트랜지션 시작 메서드를 반환합니다.

pending Signal을 사용해 인디케이터를 UI에 제공해야 하는데, 탭 컨테이너 div에 pending 클래스를 추가합니다:

```js
<div class="tab" classList={{ pending: pending() }}>
```

이제 탭 전환이 훨씬 부드러워진 것을 볼 수 있습니다.
