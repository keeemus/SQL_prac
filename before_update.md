### 기록용


Q. customer_stats 테이블의 last_order_date 컬럼을 이용하여 각 고객의 Recency 점수를 계산해봅시다. last_order_date 컬럼과, recency 점수 컬럼을 출력해주세요. 조건 계산에는 CASE를 사용해주세요.
</br>Recency: 한 달 이내에 구매 기록이 있으면 1점 이외는 0점 (현재는 2021년 1월 1일로 가정)

```SQL
-- 내 답변
SELECT 
      CASE 
        WHEN last_order_date BETWEEN '2020-12-01' AND '2021-01-01' THEN 1
        ELSE 0
      END as recency
    , last_order_date
FROM customer_stats
ORDER BY last_order_date DESC;

-- 풀이
SELECT 
      CASE 
        WHEN last_order_date >= '2020-12-01' THEN 1
        ELSE 0
      END as recency
    , last_order_date
FROM customer_stats;
```

Q. customer_stats 테이블의 cnt_orders 컬럼을 이용하여 각 고객의 Frequency 점수를 계산해봅시다. cnt_orders 컬럼과, frequency 점수 컬럼을 출력해주세요. 조건 계산에는 IF를 사용해주세요.
</br>Frequency: 3회 이상 구매시 1점, 3회 미만 구매시 0점
```SQL
-- 내 답변
SELECT cnt_orders
      , IF(cnt_orders >= 3, 1, 0) as frequency
FROM customer_stats;

-- 풀이
SELECT IF(cnt_orders >= 3, 1, 0) as frequency
      , cnt_orders
FROM customer_stats;
```

Q. customer_stats 테이블의 last_order_date 컬럼을 이용하여 각 고객의 Recency 점수를 계산해봅시다. Recency 점수가 1점인 사용자, 0점인 사용자는 각각 몇 명인가요? 조건 계산에는 CASE를 사용해주세요.

```SQL
-- 내 답변
SELECT 
      CASE 
          WHEN last_order_date >= '2020-12-01' THEN 1
          ELSE 0 
      END as recency
      , COUNT(*) as cnt_recency
FROM customer_stats
GROUP BY recency;

-- 풀이
SELECT 
      CASE 
          WHEN last_order_date >= '2020-12-01' THEN 1
          ELSE 0 
      END as recency
      , COUNT(customer_id) as customers
FROM customer_stats
GROUP BY recency;
```

Q. customer_stats 테이블의 last_order_date, cnt_orders 컬럼을 이용하여 각 고객의 Recency, Frequency 점수를 계산해봅시다. 조건 계산에는 IF를 사용해주세요.

```SQL
-- 내 답변
SELECT IF(last_order_date >= '2020-12-01',1, 0) as Recency
      , IF(cnt_orders >= 3, 1, 0) as Frequency
      , COUNT(*) as Customers
FROM customer_stats
GROUP BY Recency, Frequency

-- 풀이
SELECT IF(last_order_date >= '2020-12-01',1, 0) as recency
      , IF(cnt_orders >= 3, 1, 0) as frequency
      , COUNT(customer_id) as Customers
FROM customer_stats
GROUP BY recency, frequency
```


