## CSS 마스터 클래스

### 1강. flexbox

#### 1.2 First Rule of Flexbox

- 기존의 방식으로는 박스를 inline-block 으로 생성 후 margin 을 이용해서 정렬했지만, 이 방식으로는 디스플레이의 크기를 제대로 반영하질 못했습니다.
- 이 단점을 보완하기 위해 나온 것이 Flexbox 입니다. 방식은 간단합니다. 박스 위의 상위 div 에 `display: flex`를 주면 됩니다. 이 div 를 컨테이너라 부릅니다.

#### 1.3 Main Axis and Cross Axis

- flexbox css 에는 행 row, 열 column 이 있습니다. flex container 의 `flex-direction` 기본값은 row 입니다.
  - 여기서 `justify-content` 를 통해 수평축(row 방향)의 flex children 위치를 변경합니다. 만약 j`ustify-content: center` 를 한다면, 가로방향으로 정 가운데로 위치하게 됩니다. space-between 은 박스들 사이에 공간을, space-around 는 박스 사이에 같은 빈 공간을 계산해줍니다.
  - 기본값 `flex-direction:row` 일 때, 수평 축을 main axis 라고 부릅니다. `justify-content` 를 통해 주 축 방향으로 item 을 움직일 수 있습니다.
- 주 축이 수평일 때, 자연스럽게 수직이 cross axis 이 됩니다. 보조 축 방향으로 item 을 움직이려면 `align-items` 를 이용합니다.
  - `align-items` 는 center, flex-end, flex-start, strecth, initial, inherit, unset, baseline 등이 있습니다.
  - 만약 `align-items:center` 를 했는데 바뀌질 않는다면, 부모 컨테이너를 보면 됩니다. 부모 컨테이너의 높이가 하위 박스들과 같다면 수직 정렬이 안될 것입니다. 컨테이너에 높이를 주면, 아이템들의 보조 축 방향의 가운데를 제대로 설정할 수 있습니다.
  - strech 는 컨테이너의 높이 끝까지 채웁니다.
  - flex-end 는 컨테이너의 끝에서 시작하고, flex-start 는 기본값(처음 위치)입니다.

#### 1.4 Column and Row

- `flex-direction:column`으로 바꿔보겠습니다. 이제 주 축이 수직으로, 보조 축이 수평으로 바뀝니다.
  - `justify-content`, `align-items`의 역할도 다시 바뀝니다.
- 정리
  - `flex-direction`이 row 냐, column 이냐에 따라 주 축과 보조 축이 바뀝니다.
  - `justify-content`, `align-items`를 제대로 사용하려면, **컨테이너에 높이를 설정해야합니다.**

#### 1.5 align-self and order

- 지금까지는 컨테이너가 어떻게 자식들을 옮기는 지를 다뤘습니다. 이번에는 하위 박스에서 설정하는 법을 다룹니다.
- `align-self`
  - `align-items`와 같은 방식으로 작동합니다. 다만 특정 박스의 보조 축만을 수정합니다.
- `order`
  - 하위 박스의 순서를 변경합니다. HTML 의 순서를 바꾸기 어려울 때 사용합니다.

```css
/* 예시 */
.child:nth-child(1) {
  order: 2;
}
.child:nth-child(2) {
  order: 3;
}
/* nth-child(3) order 는 기본값 0 이므로,
  박스 위치는 3 1 2 가 됩니다.
*/
```

#### 1.6 wrap, no-wrap, reverse, align-content

##### 1.6.1 flex-wrap

- 만약 박스를 3개에서 6개로 늘린다면? 늘린만큼 박스의 너비가 줄어들게 됩니다. flexbox 는 기본적으로 하위 항목들이 모두 같은 줄에 있도록 유지합니다. (너비를 직접 입력했음에도 불구하고, 임의로 바꿔서 유지합니다.) 즉 flexbox 는 같은 줄에 있기만 한다면 너비를 상관하지 않습니다.
- `flex-wrap`은 부모 컨테이너에서 설정합니다. 기본값은 nowrap 입니다. nowrap 은 위의 경우처럼 어떻게 해서든 박스들을 같은 줄에 넣습니다.
- `flex-wrap:wrap`은 박스들의 너비를 그대로 유지하라고 명령합니다.
- flex-direction: row-reverse, flex-direction: column-reverse
  - 주 축을 기반으로, 박스들의 위치를 역순으로 정렬합니다. (1,2,3 => 3,2,1)
  - row-reverse 는 가로, column-reverse 는 세로 방향에서 역순으로 정렬한다는 차이점이 있습니다.(주 축이 다르므로)
- flex-wrap: wrap-reverse
  - 위의 reverse 와 비슷하지만, 박스들의 너비를 유지한 채로 작동합니다.

##### 1.6.2 align-content

- 주 축을 wrap 으로 정렬했을 때 생기는 보조 축의 빈 공간(줄 나눔)을 다룹니다.
- `flex-start`를 하면 보조 축의 줄 나눔이 사라집니다.
- 기본값은 space-around 입니다.

#### 1.7 flex-grow, flex-shrink

두 속성 모두 자식에게 주어지는 속성입니다.

##### 1.7.1 flex-shrink

- flexbox 의 크기가 줄어들 때의 element 행동을 정의합니다. (flex 의 기본값 no-wrap 은 박스의 너비에 상관없이 한 줄에 맞추기 위해 박스를 줄이는 것을 생각합시다) 굉장히 유용한 속성입니다.
- 기본값은 1입니다. 다른 도형들과 마찬가지로 1:1 의 비율로 줄어든다는 뜻입니다.
- 2로 한다면, 브라우저를 줄여 공간을 없앨 때마다 해당 박스만 2배로 줄어들게 됩니다.

##### 1.7.2 flex-grow

- `flex-shrink`와 정반대로 작동합니다. 남아있는 박스 옆의 빈 공간들을 가져와 박스의 크기를 키웁니다. (빈 공간이 남아있을 경우에만)
- 사용 예시: 모바일에서는 같은 비율이지만, pc 브라우저에서는 다른 비율을 가지는 사이드바, 메인 콘텐츠, 우측 광고 바같이 사용가능합니다. 더이상 픽셀로 너비를 쓸 필요가 없어집니다.
- 기본값은 0입니다.
- 만약 브라우저를 줄여 여분의 공간이 없다면, `flex-grow`는 작동하지 않습니다. 오직 여분의 공간이 있을 때만 `flex-grow`를 준 박스를 키울 수 있습니다. 또한, 이미 공간을 다 차지한 상황이라면 아무리 값을 2, 3, 4 로 키워도 작동하지 않습니다.
  - 다른 박스에도 값을 줄 때, 박스끼리 여분의 공간을 계산해 나눠갖게 됩니다.

```css
/* 2번이 2/3, 3번이 1/3 을 가져갑니다. */
.child:nth-child(2) {
  flex-grow: 2;
}
.child:nth-child(3) {
  flex-grow: 3;
}
```

#### 1.8 flex-basis

- 자식에게 줄 수 있는 속성으로, `flex-basis`는 element 에게 첫 크기를 제공합니다.
  - 줄어들거나(shrink), 늘어나기(grow) 전의 크기를 지정한다는 뜻입니다. 실제 크기(width)를 고정시키는 것이 아닙니다.
  - `flex-direction`의 영향을 받습니다. 주 축의 길이를 지정하기 때문입니다. 기본값 row 일 때는 너비를, column 일 때는 높이를 지정하게 됩니다.
- width, height 로도 줄 수 있기에 자주 사용하지는 않습니다.

#### 1.9 Flexbox Froggy

https://flexboxfroggy.com/#ko 에서 개구리의 위치를 옮겨보세요.
