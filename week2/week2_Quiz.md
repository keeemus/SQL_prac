# Practice

#### 문제1번) 주문일이 2017-09-02 일에 해당 하는 주문건에 대해서 어떤 고객이 어떠한 상품에 대해서 얼마를 지불하여 상품을 구매했는지 확인해주세요.
```SQL
SELECT o.orderdate
       ,c.customerid
       ,c.custfirstname
       ,c.custlastname
       ,p.productname
       ,p.productnumber
       ,od.quotedprice * od.quantityordered AS price
FROM   orders o
       INNER JOIN customers c
               ON c.customerid = o.customerid
       INNER JOIN order_details od
               ON o.ordernumber = od.ordernumber
       INNER JOIN products p
               ON p.productnumber = od.productnumber
WHERE  date(orderdate) = '2017-09-02'
ORDER BY o.orderdate
         ,o.customerid
         ,price; 
```

#### 문제2번) 헬멧을 주문한 적 없는 고객을 보여주세요.(모르겠음)
- 헬맷은 Products 테이블의 productname 컬럼을 이용해서 확인해주세요.
```SQL
SELECT actor_id
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  rating = 'R')
INTERSECT
SELECT actor_id
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  title = 'Alone Trip'); 
```

#### 문제3번) 모든 제품과 주문 일자를 나열하세요.(주문되지 않은 제품도 포함해서 보여주세요.)
```SQL
SELECT p.productnumber
       ,p.productname
       ,od.ordernumber
       ,o.orderdate
FROM   products p
       LEFT JOIN order_details od
              ON p.productnumber = od.productnumber
       LEFT JOIN orders o
              ON od.ordernumber = o.ordernumber;
```

#### 문제4번) 캘리포니아 주와 캘리포니아 주가 아닌 STATS로 구분하여 각 주문량을 알려주세요.(CASE문 사용)
```SQL
SELECT data_order.stats
       ,count(DISTINCT ordernumber)
FROM   (SELECT c.custstate
               ,o.ordernumber,
               CASE
                 WHEN c.custstate = 'CA' THEN 'CA'
                 ELSE 'NOT CA'
               END AS stats
        FROM   customers c
               INNER JOIN orders o
                       ON c.customerid = o.customerid
               LEFT JOIN order_details od
                      ON o.ordernumber = od.ordernumber) AS data_order
GROUP BY data_order.stats; 
```

#### 문제5번) 공급업체와 판매 제품 수를 나열하세요. 단 판매 제품수가 2개 이상인 곳만 보여주세요.
```SQL
SELECT v.vendname
       ,count(*)
FROM   product_vendors pv
       INNER JOIN vendors v
               ON v.vendorid = pv.vendorid
GROUP BY v.vendname
HAVING count(*) >= 2
ORDER BY 1; 

-- 문제 제대로 읽기, 공급업체 별 판매 제품 수가 아닌 주문 건수로 잘못 읽어서 오래 삽질했음 !
```

#### 문제6번) 가장 높은 주문 금액을 산 고객은 누구인가요?
- 주문일자별, 고객의 아이디별로, 주문번호, 주문 금액도 함께 알려주세요.
```SQL
SELECT orderdate
       ,customerid
       ,ordernumber
       ,sum(price) AS price
FROM   (SELECT o.orderdate
               ,o.customerid
               ,o.ordernumber
               ,od.quotedprice * od.quantityordered AS price
        FROM   orders o
               INNER JOIN order_details od
                       ON o.ordernumber = od.ordernumber) AS temporary_data
GROUP BY orderdate
         ,customerid
         ,ordernumber
ORDER BY 4 DESC; 
```

#### 문제7번) 주문일자별로 주문 개수와 고객수를 알려주세요.
- ex) 하루에 한 고객이 주문을 2번이상했다고 가정했을때 -> 해당의 경우는 고객수는 1명으로 계산해야합니다.
```SQL
SELECT orderdate
       ,count(DISTINCT ordernumber) AS order_count
       ,count(DISTINCT customerid)  AS customer_count
FROM   (SELECT o.orderdate
               ,o.customerid
               ,o.ordernumber
        FROM   orders o
               INNER JOIN order_details od
                       ON o.ordernumber = od.ordernumber) AS temporary_data
GROUP BY orderdate
ORDER BY orderdate; 
```

#### 문제8번) 타이어와 헬멧을 모두 산적이 있는 고객의 ID 를 알려주세요.
- 타이어와 헬멧에 대해서는 Products 테이블의 productname 컬럼을 이용해서 확인해주세요.
```SQL
SELECT customerid
FROM   orders o
       INNER JOIN order_details od
               ON o.ordernumber = od.ordernumber
       INNER JOIN products p
               ON od.productnumber = p.productnumber
WHERE  p.productname LIKE '%Tire%'
INTERSECT
SELECT customerid
FROM   orders o
       INNER JOIN order_details od
               ON o.ordernumber = od.ordernumber
       INNER JOIN products p
               ON od.productnumber = p.productnumber
WHERE  p.productname LIKE '%Helmet%'
ORDER BY 1; 
```

#### 문제9번) 타이어는 샀지만 헬멧을 사지 않은 고객의 ID를 알려주세요. Except 조건을 사용하여 풀이 해주세요.
- 타이어와 헬멧에 대해서는 Products 테이블의 productname 컬럼을 이용해서 확인해주세요.
```SQL
SELECT customerid
FROM   orders o
       INNER JOIN order_details od
               ON o.ordernumber = od.ordernumber
       INNER JOIN products p
               ON od.productnumber = p.productnumber
WHERE  p.productname LIKE '%Tire%'
EXCEPT
SELECT customerid
FROM   orders o
       INNER JOIN order_details od
               ON o.ordernumber = od.ordernumber
       INNER JOIN products p
               ON od.productnumber = p.productnumber
WHERE  p.productname LIKE '%Helmet%'
ORDER BY 1; 
```
