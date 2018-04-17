# pandas

## 목차
- 설정
  - 숫자 포맷
  - 출력 개수
  - 출력 크기
- 자주 사용

## 설정
`set_option()`을 사용하거나, 직접 지정하거나 할 수 있다.
```
pd.set_option('display.max_rows', 500)
pd.options.display.max_rows = 500
```

### 숫자 포맷
이유는 모르겠지만 `int` 는 지정할 수 없다.
```
pd.options.display.float_format = '{:.2f}'.format
```

### 출력 개수
```
pd.options.display.max_rows = 500
pd.options.display.max_columns = 500
```

### 출력 크기
```
pd.options.display.height = 1000
pd.options.display.width = 1000
```

### `SettingWithCopyWarning` 안보이게
https://stackoverflow.com/questions/20625582/how-to-deal-with-settingwithcopywarning-in-pandas

```
pd.options.mode.chained_assignment = None
```

## 자주 사용

머지
```
pd.merge(left, right, on='key/[key1, key2]', how='left/right/outer/inner')
```

열 이름 바꾸기
```
df = df.rename(columns={'a': 'aa', 'b': 'bb'})
```

열 순서 바꾸기
```
df = df[['a', 'b', 'c']]
```

열 타입 설정
```
df[c] = df[c].astype(int)
df[c] = df[c].astype('datetime64[ns]')
```

기존 열에 함수 적용하여 새로운 열 만들기
```
df['new'] = df['old'].apply(f)
df['new'] = df['old'].apply(lambda x: df.loc[x, 'other'])
```

### 행/열 변경

행열 스왑
```
df = df.transpose()
```

특정 열의 값을 열 목록으로 만든다.

| name | week | value |
| ---- | ---- | ----- |
| A | 1 | 100 |
| A | 2 | 200 |

| name | 1   | 2   |
| ---- | --- | --- |
| A | 100 | 200 |

```
df = df.pivot(index='name', columns='week', values='value')
```
