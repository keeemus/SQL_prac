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

Q. orders 테이블 안에 몇 일 치의 데이터가 들어있나요? order_purchase_timestamp 컬럼을 기준으로 데이터의 시작 시점과 끝 시점을 알려주세요.
```SQL
-- 내 답변
SELECT COUNT(order_purchase_timestamp) as cnt_date
     , MIN(order_purchase_timestamp) as start_date
     , MAX(order_purchase_timestamp) as last_date
from olist_orders_dataset

/*
총 99,441 개
시작 시점 : 2016-09-04 21:15:19
끝 시점 : 2018-10-17 17:30:18
*/
```
Q. 고객들은 어떤 주(state)에 살고 있나요? customers 테이블 안에 customer_state 컬럼을 참고하여 olist 고객들이 어떤 주에 살고 있는지 알아봅시다. 이 정보를 주문 데이터(orders)와 연결하려면 어떤 컬럼을 사용해야 하나요?

```SQL
-- 내 답변
SELECT DISTINCT customer_state
FROM olist_customers_dataset

/*
총 27개의 주에 거주
customer_id 컬럼 사용
*/
```

Q. 고객들의 결제 데이터는 어떤 테이블에 있나요? 주문 하나에 결제 데이터가 하나 붙어있는 1:1 관계인가요, 아니면 주문 하나에 결제 데이터가 여러 개 있을 수 있는 1:N 관계인가요? 결제 금액은 어떤 컬럼을 보고 알 수 있나요? orders 테이블과 결제 데이터는 어떤 컬럼을 기준으로 연결할 수 있나요?

```SQL
-- 내 답변
SELECT order_id
     , COUNT(*) AS cnt_orders
FROM olist_order_payments_dataset
GROUP BY order_id
ORDER BY cnt_orders DESC

/*
1:N 관계이며 결제 금액은 payment_value 컬럼을 보고 파악
order_id 컬럼 사용
*/
```

Q. orders, customers, payments 테이블을 사용하여 2017년에 매출이 많이 나오는 주(customer_state)가 어디인지 알아봅시다. 2017년 매출 Top 3 주를 꼽아주세요.
```SQL
/*
1. olist_orders_dataset에서 날짜(order_purchase_timestamp) 확인 가능
2. olist_customers_dataset에서 주(state) 확인 가능
3. olist_order_payments_dataset에서 매출(payment_value) 확인 가능
4. 1,2테이블 join시 customer_id 활용
5. 1,3테이블 join시 order_id 활용
*/

-- 내 답변
SELECT c.customer_state AS state
     , SUM(p.payment_value) AS total_sales
FROM olist_orders_dataset AS o
     INNER JOIN olist_customers_dataset AS c ON o.customer_id = c.customer_id
     INNER JOIN olist_order_payments_dataset AS p ON o.order_id = p.order_id
WHERE YEAR(o.order_purchase_timestamp) = 2017
GROUP BY c.customer_state
ORDER BY total_sales DESC
LIMIT 3

/*
2017년 매출 TOP 3
1. SP
2. RJ
3. MG
*/
```

Q. 2017년 5월 1일부터 2017년 11월 19일까지 주(state)별 일일 매출액을 알고 싶습니다. **2017년 매출 Top 3 지역이었던 ‘SP’, ‘RJ’, ‘MG’ 주 각각의 일일 매출액과 일일 전체 매출액에서 차지하는 비중을 계산해주세요**. orders, customers, payments 테이블을 이용하고, 쿼리 만으로 계산이 어렵다면 구글 스프레드 시트나 엑셀을 활용하는 것도 좋은 방법입니다.

```SQL
-- 내 답변

SELECT c.customer_state AS state
    , DATE(o.order_purchase_timestamp) AS date
    , ROUND(SUM(p.payment_value), 2) AS revenue_daily
FROM olist_orders_dataset AS o
    INNER JOIN olist_customers_dataset AS c ON o.customer_id = c.customer_id
    INNER JOIN olist_order_payments_dataset AS p ON o.order_id = p.order_id
WHERE o.order_purchase_timestamp BETWEEN '2017-05-01 00:00:00' AND '2017-11-19 23:59:59'
AND c.customer_state IN ('SP', 'RJ', 'MG')
GROUP BY c.customer_state
      , date
ORDER BY date

-- 풀이
SELECT 
    -- c.customer_state AS state
      DATE(o.order_purchase_timestamp) AS date
    , ROUND(SUM(CASE WHEN c.customer_state = 'SP' THEN p.payment_value ELSE 0 END), 2) AS SP_revenue_daily
    , ROUND(SUM(CASE WHEN c.customer_state = 'RJ' THEN p.payment_value ELSE 0 END), 2) AS RJ_revenue_daily
    , ROUND(SUM(CASE WHEN c.customer_state = 'MG' THEN p.payment_value ELSE 0 END), 2) AS MG_revenue_daily
    , ROUND(SUM(p.payment_value), 2) AS revenue_total
    , ROUND(SUM(CASE WHEN c.customer_state = 'SP' THEN p.payment_value ELSE 0 END) / SUM(p.payment_value) * 100, 2) AS SP_revenue_pct
    , ROUND(SUM(CASE WHEN c.customer_state = 'RJ' THEN p.payment_value ELSE 0 END) / SUM(p.payment_value) * 100, 2) AS RJ_revenue_pct
    , ROUND(SUM(CASE WHEN c.customer_state = 'MG' THEN p.payment_value ELSE 0 END) / SUM(p.payment_value) * 100, 2) AS MG_revenue_pct
FROM olist_orders_dataset AS o
    INNER JOIN olist_customers_dataset AS c ON o.customer_id = c.customer_id
    INNER JOIN olist_order_payments_dataset AS p ON o.order_id = p.order_id
WHERE o.order_purchase_timestamp BETWEEN '2017-05-01 00:00:00' AND '2017-11-19 23:59:59'
-- AND c.customer_state IN ('SP', 'RJ', 'MG')
GROUP BY 
        -- c.customer_state
        date
ORDER BY date
```
