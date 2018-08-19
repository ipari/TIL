# Javascript

## 목차

- 크기 계산
- jQuery 벗어나기
  - $(document).ready
  - 요소 선택
  - CSS Property 구하기


## 크기 계산
em 단위로 디자인을 하면 필연적으로 요소의 크기가 소수점 단위가 된다. 때문에 `int` 단위로 리턴하는 `elem.offsetHeight` 같은 것을 사용하여 계산하면 소수점 단위로 어긋나면서 보기 안좋게 된다.

번거롭지만 `elem.getBoundingClientRect().height` 을 사용하면 소수점 단위의 정확한 값을 얻을 수 있다. 이 메소드는 화면 상에서의 요소의 위치를 찾는다거나 할 때도 유용하게 사용할 수 있다.


## jQuery 벗어나기

### $(document).ready
아래 두 가지 정도가 있는 것 같다.

https://stackoverflow.com/questions/9899372/pure-javascript-equivalent-of-jquerys-ready-how-to-call-a-function-when-t/9899701#9899701

```
document.addEventListener('DOMContentLoaded', f);
window.addEventListener('load', f);
```

### 요소 선택
전자는 가장 앞의 한개를 리턴하고, 후자는 목록으로 리턴한다.
```
let elem = document.querySelector("div.menu a");
let elems = documents.querySelectorAll("div.menu a");
```

후자는 이런 식으로 이터레이션 돌리면 된다.
```
for (let elem of elems) {
    elem.style.color = "#cccccc";
}
```

### CSS Property 구하기
```
let style = window.getComputedStyle(elem, null);
let value = style.getPropertyValue("display");
```

만약 페이지 로드 중에 이 값을 사용하고 싶다면 `window.addEventListener('load', f)` 를 사용해야 한다. (`window`의 메소드를 사용하기 때문일 것 같다.)
