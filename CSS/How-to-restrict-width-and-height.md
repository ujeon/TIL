## 너비와 높이 제한하기

디스플레이 크기는 제각각이기 때문에 웹 페이지의 컨텐츠의 크기 역시 달라질 수 있습니다. CSS는 이러한 문제를 피하기 위해서 엘리먼트 상자의 크기를 제한할 수 있도록 두가지 속성을 제공합니다.

바로 width, height에 min과 max를 사용하는 것입니다. min과 max를 width, height와 함께 사용하면 엘리먼트의 최소 크기와 최대 크기를 제한할 수 있습니다. 

min과 max를 사용하는 방법은 다음과 같습니다:

```css
p {
  min-width: 300px;
  max-width: 600px;
}

p {
  min-height: 150px;
  max-height: 300px;
}
```

