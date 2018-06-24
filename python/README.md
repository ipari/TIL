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
