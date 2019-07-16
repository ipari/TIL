# Python

## 목차
- 자주 사용
  - defaultdict to dict

## 자주 사용

### `defaultdict` to `dict`
`dict` 나 `list` 에 있는 `defaultdict`를 recursive 하게 `dict`로 변환한다.

`defaultdict` 를 yaml로 덤프하면 자기가 덤프해놓고 다시 못 불러오는 일이 있는데 이럴 때 사용하면 좋다. 보통은 `yaml.safe_dump()` 을 쓰면 되는데, 커스텀 타입이 있으면 작동 안하는 문제가 있다.

```
def defaultdict_to_dict(d):
    if isinstance(d, dict):
        d = {k: defaultdict_to_dict(v) for k, v in d.iteritems()}
    if isinstance(d, list):
        d = [defaultdict_to_dict(d) for d in d]
    return d
```

## 디버깅

### Warning 잡기
데이터 가공을 하다가 numpy에서 RuntimeWarning을 만났다. 왜 문제가 발생할까 찾아보니 데이터에 예외 케이스가 있어서 `0 / 0` 연산을 수행하면서 에러가 발생하던 것. `ZeroDivisionError`가 아닌 `RuntimeWarning: invalid value encountered in long_scalars` 를 띄워서 한참 헤맸다.

※ 데이터 작업 때는 warning 이라도 무시하지 말고 짚고 넘어가도록 하자.

```
import warnings

  with warnings.catch_warnings(record=True) as w:
      warnings.simplefilter('error', RuntimeWarning)
      
      try:
          func()
      except RuntimeWarning:
          import pdb
          pdb.set_trace()
```
