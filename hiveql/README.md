# git

## 목차
- 가이드


## 유용한 사이트

- 공식문서
  - https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF
- @yeonghoey 님의 패턴 예제
  - https://github.com/yeonghoey/yeonghoey/blob/master/content/spark/README.org
- 예제가 있어서 좀 더 쉬운 설명
  - https://www.tutorialspoint.com/hive/index.htm
- 온라인 실습 사이트
  - http://demo.gethue.com/hue


## 예제

### `limit` 로 일부 출력
스키마가 어떻게 생겼는지 확인하거나, 정렬 후 상위 n개를 출력할 때 사용한다.

```
select
    *
from MoneyTransactions
limit 5
```

| time | player_id | player_level | reason | amount |
| --- | --- | --- | --- | ---: |
| 2018-02-28 | a | 10 | buy_item | -100 |
| 2018-02-28 | b | 28 | sell_item | 1,000 |
| 2018-02-28 | b | 28 | quest | 250 |
| 2018-02-28 | c | 82 | hunt | 28 |
| 2018-02-28 | d | 22 | buy_item | -282 |

### `group by` 로 집계하기

집계하는데 사용하는 함수들을 [UDAF]((https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-Built-inAggregateFunctions(UDAF)))(User Defined Aggregate Function) 이라고 한다.

`group by {필드1}, {필드2}` 처럼 `,` 로 여러 필드로 그루핑 할 수 있다.

```
select
    reason,
    count(*),                   -- 개수
    sum(amount),                -- 합계
    avg(amount),                -- 평균
    percentile(amount, 0.5)     -- 백분위 50%
from MoneyTransactions
group by reason
```

### `cast` 로 타입 변경

경우에 따라 타입을 변경할 필요가 있다. 이를테면 거래 내역을 보다가 숫자가 안맞아서 보니 액수가 `INT` 범위를 넘어서 그랬던 적이 있다. 숫자가 충분히 크다면 `BIGINT` 로 타입을 변환해서 쓰자.

```
select
    sum(cast(amount as BIGINT)),
...
```

### `as` 로 별칭 설정

UDAF를 사용하면 열 이름이 `sum(cast(amount as BIGINT))` 이런 식으로 복잡해진다. 이럴 때 `as` 를 사용하여 별칭을 설정할 수 있다.

```
select
    sum(cast(amount as BIGINT)) as sum,
...
```

### `order by` 로 정렬

- `order by {필드 이름/순서} {asc/desc}` 형태로 사용한다.
- `,` 로 여러 개의 정렬 기준을 사용할 수 있다.

```
select
    player_level,
    reason,
    sum(amount) as amount
from MoneyTransactions
group by player_level, reason
order by player_level asc, amount desc
-- order by 1 asc, 3 desc 와 동일
```

| player_level | reason | amount |
| --- | --- | ---: |
| 1 | hunt | 100,000 |
| 1 | buy_item | -10,000 |
| 2 | quest | 300,000 |
| 2 | hunt | 200,000 |
| 2 | buy_item | -100,000 |

### `where` 로 필터링

10레벨 이상 플레이어 대상으로 보고 싶다면 아래와 같이 조건을 걸면 된다.

```
...
from MoneyTransactions
where player_level >= 10
...
```

만약 아이템/버프/입장권 구매를 각각 `buy_(item/buff/ticket)` 형태로 남겼고, 이들에 대한 조건을 추가로 걸고 싶다면? (물론 별도 필드에 기록하는 게 좋다)

#### `or`

안되는 것은 아니지만 구리다.

```
where player_level >= 10 and (reason = 'buy_item' or reason = 'buy_buff' or reason = 'buy_ticket')
```

#### `in`

`in` 을 사용하면 좀 더 낫다.

```
where player_level >= 10 and reason in ('buy_item', 'buy_buff', 'buy_ticket')
```

#### `like`
`like`는 `%` 를 와일드카드로 하여 텍스트 패턴을 지정할 수 있다. 아래와 같이 하면 `buy_` 로 시작하는 모든 텍스트를 선택한다.

```
where player_level >= 10 and reason like 'buy_%'
```
| | |
| --- | --- |
| `buy` | `= 'buy'` 와 동일 |
| `buy_%` | `buy_` 로 시작 |
| `%_buy` | `_buy` 로 끝남 |
| `%_buy_%` | `_buy_` 가 어디든 있음 |

기타 사용 가능한 [연산자](https://www.tutorialspoint.com/hive/hive_built_in_operators.htm) 목록.
