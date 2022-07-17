### 기록용


Q. customer_stats 테이블의 last_order_date 컬럼을 이용하여 각 고객의 Recency 점수를 계산해봅시다. last_order_date 컬럼과, recency 점수 컬럼을 출력해주세요. 조건 계산에는 CASE를 사용해주세요.
</br>Recency: 한 달 이내에 구매 기록이 있으면 1점 이외는 0점 (현재는 2021년 1월 1일로 가정)

```SQL
SELECT 
      CASE 
        WHEN last_order_date BETWEEN '2020-12-01' AND '2021-01-01' THEN 1
        ELSE 0
      END as recency
    , last_order_date
FROM customer_stats
ORDER BY last_order_date DESC;
```
